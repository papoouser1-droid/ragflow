# File Documentation: sdk/python/test/test_http_api/test_file_management_within_dataset/test_download_document.py

## File Metadata

- **Path**: `sdk/python/test/test_http_api/test_file_management_within_dataset/test_download_document.py`
- **Extension**: `.py`
- **Lines**: 179
- **Characters**: 6,009
- **Size**: 6,009 bytes
- **Purpose**: Testing - Contains unit tests, integration tests, or test utilities

## Original Source

```python
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

## High-Level Overview

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

## Detailed Walkthrough

### Classes (2)

- `TestAuthorization`: Class definition
- `TestDocumentDownload`: Class definition

### Functions (2)

- `test_file_type_validation()`: Function definition
- `test_concurrent_download()`: Function definition

### Imports (7)

- `import json`
- `from concurrent.futures import ThreadPoolExecutor`
- `import pytest`
- `from common import INVALID_API_TOKEN, bulk_upload_documents, download_document, upload_documnets`
- `from libs.auth import RAGFlowHttpApiAuth`
- `from libs.utils import compare_by_hash`
- `from requests import codes`

## Code Structure Analysis

- Total lines: 179
- Blank lines: 15 (8.4%)
- Comment lines: ~15 (8.4%)
- Code lines: ~149


## Dependencies and Imports

- `import json`
- `from concurrent.futures import ThreadPoolExecutor`
- `import pytest`
- `from common import INVALID_API_TOKEN, bulk_upload_documents, download_document, upload_documnets`
- `from libs.auth import RAGFlowHttpApiAuth`
- `from libs.utils import compare_by_hash`
- `from requests import codes`

## Design & Architecture

This file is located in the `sdk` directory, specifically within `sdk/python/test/test_http_api/test_file_management_within_dataset`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 6 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `sdk/python/test/test_http_api/test_file_management_within_dataset/` directory
- Imports from `concurrent.futures`
- Imports from `common`
- Imports from `libs.auth`
- Imports from `libs.utils`

## Keywords

ANY, API, All, Apache, Authentication, Authorization, Authors, BASIS, CONDITIONS, Copyright, Found, INVALID_API_TOKEN, InfiniFlow, KIND, LICENSE, License, Licensed, None, Not, NotFound, Python, RAGFlowHttpApiAuth, Reserved, Rights, See, TestAuthorization, TestDocumentDownload, The, ThreadPoolExecutor, True, Unless, Version, WARRANTIES, WITHOUT, You, pytest, test_concurrent_download, test_file_type_validation, test_invalid_auth, test_invalid_dataset_id, test_invalid_document_id, test_same_file_repeat

---
*Generated by RAGFlow Repository Documentation Generator*
