# Documentation: test/testcases/test_http_api/test_file_management_within_dataset/test_upload_documents.py

## File Metadata

- **Path**: `test/testcases/test_http_api/test_file_management_within_dataset/test_upload_documents.py`
- **Size**: 8500 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_http_api/test_file_management_within_dataset/test_upload_documents.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `string`
- `concurrent.futures`
- `pytest`
- `requests`
- `common`
- `configs`
- `libs.auth`
- `requests_toolbelt`
- `utils.file_utils`

### Classes Defined

This file defines 2 class(es):

#### Class: `TestAuthorization` (line 30)

**Methods**: test_invalid_auth

#### Class: `TestDocumentsUpload` (line 48)

**Methods**: test_valid_single_upload, test_file_type_validation, test_unsupported_file_type, test_missing_file, test_empty_file, test_filename_empty, test_filename_max_length, test_invalid_dataset_id, test_duplicate_files, test_same_file_repeat, test_filename_special_characters, test_multiple_files, test_concurrent_upload

### Functions Defined

This file defines 14 function(s):

#### Function: `test_invalid_auth` (line 42)

**Parameters**: self, invalid_auth, expected_code, expected_message

#### Function: `test_valid_single_upload` (line 50)

**Parameters**: self, HttpApiAuth, add_dataset_func, tmp_path

#### Function: `test_file_type_validation` (line 75)

**Parameters**: self, HttpApiAuth, add_dataset_func, generate_test_files, request

#### Function: `test_unsupported_file_type` (line 88)

**Parameters**: self, HttpApiAuth, add_dataset_func, tmp_path, file_type

#### Function: `test_missing_file` (line 97)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_empty_file` (line 104)

**Parameters**: self, HttpApiAuth, add_dataset_func, tmp_path

#### Function: `test_filename_empty` (line 114)

**Parameters**: self, HttpApiAuth, add_dataset_func, tmp_path

#### Function: `test_filename_max_length` (line 130)

**Parameters**: self, HttpApiAuth, add_dataset_func, tmp_path

#### Function: `test_invalid_dataset_id` (line 138)

**Parameters**: self, HttpApiAuth, tmp_path

#### Function: `test_duplicate_files` (line 145)

**Parameters**: self, HttpApiAuth, add_dataset_func, tmp_path

#### Function: `test_same_file_repeat` (line 159)

**Parameters**: self, HttpApiAuth, add_dataset_func, tmp_path

#### Function: `test_filename_special_characters` (line 173)

**Parameters**: self, HttpApiAuth, add_dataset_func, tmp_path

#### Function: `test_multiple_files` (line 188)

**Parameters**: self, HttpApiAuth, add_dataset_func, tmp_path

#### Function: `test_concurrent_upload` (line 202)

**Parameters**: self, HttpApiAuth, add_dataset_func, tmp_path

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
import string
from concurrent.futures import ThreadPoolExecutor, as_completed

import pytest
import requests
from common import FILE_API_URL, list_datasets, upload_documents
from configs import DOCUMENT_NAME_LIMIT, HOST_ADDRESS, INVALID_API_TOKEN
from libs.auth import RAGFlowHttpApiAuth
from requests_toolbelt import MultipartEncoder
from utils.file_utils import create_txt_file


@pytest.mark.p1
@pytest.mark.usefixtures("clear_datasets")
class TestAuthorization:
    @pytest.mark.parametrize(
        "invalid_auth, expected_code, expected_message",
        [
            (None, 0, "`Authorization` can't be empty"),
            (
                RAGFlowHttpApiAuth(INVALID_API_TOKEN),
                109,
                "Authentication error: API key is invalid!",
            ),
        ],
    )
    def test_invalid_auth(self, invalid_auth, expected_code, expected_message):
        res = upload_documents(invalid_auth, "dataset_id")
        assert res["code"] == expected_code
        assert res["message"] == expected_message


class TestDocumentsUpload:
    @pytest.mark.p1
    def test_valid_single_upload(self, HttpApiAuth, add_dataset_func, tmp_path):
        dataset_id = add_dataset_func
        fp = create_txt_file(tmp_path / "ragflow_test.txt")
        res = upload_documents(HttpApiAuth, dataset_id, [fp])
        assert res["code"] == 0
        assert res["data"][0]["dataset_id"] == dataset_id
        assert res["data"][0]["name"] == fp.name

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
    def test_file_type_validation(self, HttpApiAuth, add_dataset_func, generate_test_files, request):
        dataset_id = add_dataset_func
        fp = generate_test_files[request.node.callspec.params["generate_test_files"]]
        res = upload_documents(HttpApiAuth, dataset_id, [fp])
        assert res["code"] == 0
        assert res["data"][0]["dataset_id"] == dataset_id
        assert res["data"][0]["name"] == fp.name

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "file_type",
        ["exe", "unknown"],
    )
    def test_unsupported_file_type(self, HttpApiAuth, add_dataset_func, tmp_path, file_type):
        dataset_id = add_dataset_func
        fp = tmp_path / f"ragflow_test.{file_type}"
        fp.touch()
        res = upload_documents(HttpApiAuth, dataset_id, [fp])
        assert res["code"] == 500
        assert res["message"] == f"ragflow_test.{file_type}: This type of file has not been supported yet!"

    @pytest.mark.p2
    def test_missing_file(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        res = upload_documents(HttpApiAuth, dataset_id)
        assert res["code"] == 101
        assert res["message"] == "No file part!"

    @pytest.mark.p3
    def test_empty_file(self, HttpApiAuth, add_dataset_func, tmp_path):
        dataset_id = add_dataset_func
        fp = tmp_path / "empty.txt"
        fp.touch()

        res = upload_documents(HttpApiAuth, dataset_id, [fp])
        assert res["code"] == 0
        assert res["data"][0]["size"] == 0

    @pytest.mark.p3
    def test_filename_empty(self, HttpApiAuth, add_dataset_func, tmp_path):
        dataset_id = add_dataset_func
        fp = create_txt_file(tmp_path / "ragflow_test.txt")
        url = f"{HOST_ADDRESS}{FILE_API_URL}".format(dataset_id=dataset_id)
        fields = (("file", ("", fp.open("rb"))),)
        m = MultipartEncoder(fields=fields)
        res = requests.post(
            url=url,
            headers={"Content-Type": m.content_type},
            auth=HttpApiAuth,
            data=m,
        )
        assert res.json()["code"] == 101
        assert res.json()["message"] == "No file selected!"

    @pytest.mark.p2
    def test_filename_max_length(self, HttpApiAuth, add_dataset_func, tmp_path):
        dataset_id = add_dataset_func
        fp = create_txt_file(tmp_path / f"{'a' * (DOCUMENT_NAME_LIMIT - 4)}.txt")
        res = upload_documents(HttpApiAuth, dataset_id, [fp])
        assert res["code"] == 0
        assert res["data"][0]["name"] == fp.name

    @pytest.mark.p2
    def test_invalid_dataset_id(self, HttpApiAuth, tmp_path):
        fp = create_txt_file(tmp_path / "ragflow_test.txt")
        res = upload_documents(HttpApiAuth, "invalid_dataset_id", [fp])
        assert res["code"] == 100
        assert res["message"] == """LookupError("Can\'t find the dataset with ID invalid_dataset_id!")"""

    @pytest.mark.p2
    def test_duplicate_files(self, HttpApiAuth, add_dataset_func, tmp_path):
        dataset_id = add_dataset_func
        fp = create_txt_file(tmp_path / "ragflow_test.txt")
        res = upload_documents(HttpApiAuth, dataset_id, [fp, fp])
        assert res["code"] == 0
        assert len(res["data"]) == 2
        for i in range(len(res["data"])):
            assert res["data"][i]["dataset_id"] == dataset_id
            expected_name = fp.name
            if i != 0:
                expected_name = f"{fp.stem}({i}){fp.suffix}"
            assert res["data"][i]["name"] == expected_name

    @pytest.mark.p2
    def test_same_file_repeat(self, HttpApiAuth, add_dataset_func, tmp_path):
        dataset_id = add_dataset_func
        fp = create_txt_file(tmp_path / "ragflow_test.txt")
        for i in range(3):
            res = upload_documents(HttpApiAuth, dataset_id, [fp])
            assert res["code"] == 0
            assert len(res["data"]) == 1
            assert res["data"][0]["dataset_id"] == dataset_id
            expected_name = fp.name
            if i != 0:
                expected_name = f"{fp.stem}({i}){fp.suffix}"
            assert res["data"][0]["name"] == expected_name

    @pytest.mark.p3
    def test_filename_special_characters(self, HttpApiAuth, add_dataset_func, tmp_path):
        dataset_id = add_dataset_func
        illegal_chars = '<>:"/\\|?*'
        translation_table = str.maketrans({char: "_" for char in illegal_chars})
        safe_filename = string.punctuation.translate(translation_table)
        fp = tmp_path / f"{safe_filename}.txt"
        fp.write_text("Sample text content")

        res = upload_documents(HttpApiAuth, dataset_id, [fp])
        assert res["code"] == 0
        assert len(res["data"]) == 1
        assert res["data"][0]["dataset_id"] == dataset_id
        assert res["data"][0]["name"] == fp.name

    @pytest.mark.p1
    def test_multiple_files(self, HttpApiAuth, add_dataset_func, tmp_path):
        dataset_id = add_dataset_func
        expected_document_count = 20
        fps = []
        for i in range(expected_document_count):
            fp = create_txt_file(tmp_path / f"ragflow_test_{i}.txt")
            fps.append(fp)
        res = upload_documents(HttpApiAuth, dataset_id, fps)
        assert res["code"] == 0

        res = list_datasets(HttpApiAuth, {"id": dataset_id})
        assert res["data"][0]["document_count"] == expected_document_count

    @pytest.mark.p3
    def test_concurrent_upload(self, HttpApiAuth, add_dataset_func, tmp_path):
        dataset_id = add_dataset_func

        count = 20
        fps = []
        for i in range(count):
            fp = create_txt_file(tmp_path / f"ragflow_test_{i}.txt")
            fps.append(fp)

        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [executor.submit(upload_documents, HttpApiAuth, dataset_id, [fp]) for fp in fps]
        responses = list(as_completed(futures))
        assert len(responses) == count, responses
        assert all(future.result()["code"] == 0 for future in futures)

        res = list_datasets(HttpApiAuth, {"id": dataset_id})
        assert res["data"][0]["document_count"] == count

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_http_api/test_file_management_within_dataset/test_upload_documents.py` is located in the `test/testcases/test_http_api/test_file_management_within_dataset` directory.

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
- [test_download_document.py](test_download_document.py_docs.md)
- [test_list_documents.py](test_list_documents.py_docs.md)
- [test_parse_documents.py](test_parse_documents.py_docs.md)
- [test_stop_parse_documents.py](test_stop_parse_documents.py_docs.md)
- [test_update_document.py](test_update_document.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
