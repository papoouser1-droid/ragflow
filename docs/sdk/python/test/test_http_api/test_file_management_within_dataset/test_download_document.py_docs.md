# Documentation: sdk/python/test/test_http_api/test_file_management_within_dataset/test_download_document.py

## File Metadata

- **Path**: `sdk/python/test/test_http_api/test_file_management_within_dataset/test_download_document.py`
- **Size**: 6009 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/test/test_http_api/test_file_management_within_dataset/test_download_document.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `json`
- `concurrent.futures`
- `pytest`
- `common`
- `libs.auth`
- `libs.utils`
- `requests`

### Classes Defined

This file defines 2 class(es):

#### Class: `TestAuthorization` (line 28)

**Methods**: test_invalid_auth

#### Class: `TestDocumentDownload` (line 85)

**Methods**: test_invalid_document_id, test_invalid_dataset_id, test_same_file_repeat

### Functions Defined

This file defines 6 function(s):

#### Function: `test_file_type_validation` (line 66)

**Parameters**: get_http_api_auth, add_dataset, generate_test_files, request

#### Function: `test_concurrent_download` (line 156)

**Parameters**: get_http_api_auth, add_dataset, tmp_path

#### Function: `test_invalid_auth` (line 40)

**Parameters**: self, tmp_path, auth, expected_code, expected_message

#### Function: `test_invalid_document_id` (line 97)

**Parameters**: self, get_http_api_auth, add_documents, tmp_path, document_id, expected_code, expected_message

#### Function: `test_invalid_dataset_id` (line 123)

**Parameters**: self, get_http_api_auth, add_documents, tmp_path, dataset_id, expected_code, expected_message

#### Function: `test_same_file_repeat` (line 138)

**Parameters**: self, get_http_api_auth, add_documents, tmp_path, ragflow_tmp_dir

## Original Source Code

```py
#
#  Copyright 2025 The InfiniFlow Authors. All Rights Reserved.
#
#  Licensed under the Apache License, Version 2.0 (the "License");
#  you may not use this file except in compliance with the License.
#  You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
#  Unless required by applicable law or agreed to in writing, software
#  distributed under the License is distributed on an "AS IS" BASIS,
#  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#  See the License for the specific language governing permissions and
#  limitations under the License.
#

import json
from concurrent.futures import ThreadPoolExecutor

import pytest
from common import INVALID_API_TOKEN, bulk_upload_documents, download_document, upload_documnets
from libs.auth import RAGFlowHttpApiAuth
from libs.utils import compare_by_hash
from requests import codes


@pytest.mark.p1
class TestAuthorization:
    @pytest.mark.parametrize(
        "auth, expected_code, expected_message",
        [
            (None, 0, "`Authorization` can't be empty"),
            (
                RAGFlowHttpApiAuth(INVALID_API_TOKEN),
                109,
                "Authentication error: API key is invalid!",
            ),
        ],
    )
    def test_invalid_auth(self, tmp_path, auth, expected_code, expected_message):
        res = download_document(auth, "dataset_id", "document_id", tmp_path / "ragflow_tes.txt")
        assert res.status_code == codes.ok
        with (tmp_path / "ragflow_tes.txt").open("r") as f:
            response_json = json.load(f)
        assert response_json["code"] == expected_code
        assert response_json["message"] == expected_message


@pytest.mark.p1
@pytest.mark.parametrize(
    "generate_test_files",
    [
        "docx",
        "excel",
        "ppt",
        "image",
        "pdf",
        "txt",
        "md",
        "json",
        "eml",
        "html",
    ],
    indirect=True,
)
def test_file_type_validation(get_http_api_auth, add_dataset, generate_test_files, request):
    dataset_id = add_dataset
    fp = generate_test_files[request.node.callspec.params["generate_test_files"]]
    res = upload_documnets(get_http_api_auth, dataset_id, [fp])
    document_id = res["data"][0]["id"]

    res = download_document(
        get_http_api_auth,
        dataset_id,
        document_id,
        fp.with_stem("ragflow_test_download"),
    )
    assert res.status_code == codes.ok
    assert compare_by_hash(
        fp,
        fp.with_stem("ragflow_test_download"),
    )


class TestDocumentDownload:
    @pytest.mark.p3
    @pytest.mark.parametrize(
        "document_id, expected_code, expected_message",
        [
            (
                "invalid_document_id",
                102,
                "The dataset not own the document invalid_document_id.",
            ),
        ],
    )
    def test_invalid_document_id(self, get_http_api_auth, add_documents, tmp_path, document_id, expected_code, expected_message):
        dataset_id, _ = add_documents
        res = download_document(
            get_http_api_auth,
            dataset_id,
            document_id,
            tmp_path / "ragflow_test_download_1.txt",
        )
        assert res.status_code == codes.ok
        with (tmp_path / "ragflow_test_download_1.txt").open("r") as f:
            response_json = json.load(f)
        assert response_json["code"] == expected_code
        assert response_json["message"] == expected_message

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "dataset_id, expected_code, expected_message",
        [
            ("", 100, "<NotFound '404: Not Found'>"),
            (
                "invalid_dataset_id",
                102,
                "You do not own the dataset invalid_dataset_id.",
            ),
        ],
    )
    def test_invalid_dataset_id(self, get_http_api_auth, add_documents, tmp_path, dataset_id, expected_code, expected_message):
        _, document_ids = add_documents
        res = download_document(
            get_http_api_auth,
            dataset_id,
            document_ids[0],
            tmp_path / "ragflow_test_download_1.txt",
        )
        assert res.status_code == codes.ok
        with (tmp_path / "ragflow_test_download_1.txt").open("r") as f:
            response_json = json.load(f)
        assert response_json["code"] == expected_code
        assert response_json["message"] == expected_message

    @pytest.mark.p3
    def test_same_file_repeat(self, get_http_api_auth, add_documents, tmp_path, ragflow_tmp_dir):
        num = 5
        dataset_id, document_ids = add_documents
        for i in range(num):
            res = download_document(
                get_http_api_auth,
                dataset_id,
                document_ids[0],
                tmp_path / f"ragflow_test_download_{i}.txt",
            )
            assert res.status_code == codes.ok
            assert compare_by_hash(
                ragflow_tmp_dir / "ragflow_test_upload_0.txt",
                tmp_path / f"ragflow_test_download_{i}.txt",
            )


@pytest.mark.p3
def test_concurrent_download(get_http_api_auth, add_dataset, tmp_path):
    document_count = 20
    dataset_id = add_dataset
    document_ids = bulk_upload_documents(get_http_api_auth, dataset_id, document_count, tmp_path)

    with ThreadPoolExecutor(max_workers=5) as executor:
        futures = [
            executor.submit(
                download_document,
                get_http_api_auth,
                dataset_id,
                document_ids[i],
                tmp_path / f"ragflow_test_download_{i}.txt",
            )
            for i in range(document_count)
        ]
    responses = [f.result() for f in futures]
    assert all(r.status_code == codes.ok for r in responses)
    for i in range(document_count):
        assert compare_by_hash(
            tmp_path / f"ragflow_test_upload_{i}.txt",
            tmp_path / f"ragflow_test_download_{i}.txt",
        )

```

## Detailed Analysis

### File Role in Repository

The file `sdk/python/test/test_http_api/test_file_management_within_dataset/test_download_document.py` is located in the `sdk/python/test/test_http_api/test_file_management_within_dataset` directory.

This file is part of the **Testing** infrastructure.

### Architecture Context

Files in this location typically handle concerns related to test_file_management_within_dataset.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

- Ensure all user inputs are validated
- Check for SQL injection vulnerabilities
- Verify authentication and authorization

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [conftest.py](conftest.py_docs.md)
- [test_delete_documents.py](test_delete_documents.py_docs.md)
- [test_list_documents.py](test_list_documents.py_docs.md)
- [test_parse_documents.py](test_parse_documents.py_docs.md)
- [test_stop_parse_documents.py](test_stop_parse_documents.py_docs.md)
- [test_update_document.py](test_update_document.py_docs.md)
- [test_upload_documents.py](test_upload_documents.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
