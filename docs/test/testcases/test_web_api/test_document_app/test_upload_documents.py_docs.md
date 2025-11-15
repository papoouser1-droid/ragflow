# Documentation: test/testcases/test_web_api/test_document_app/test_upload_documents.py

## File Metadata

- **Path**: `test/testcases/test_web_api/test_document_app/test_upload_documents.py`
- **Size**: 7871 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_web_api/test_document_app/test_upload_documents.py`.

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

#### Class: `TestDocumentsUpload` (line 44)

**Methods**: test_valid_single_upload, test_file_type_validation, test_unsupported_file_type, test_missing_file, test_empty_file, test_filename_empty, test_filename_exceeds_max_length, test_invalid_kb_id, test_duplicate_files, test_filename_special_characters, test_multiple_files, test_concurrent_upload

### Functions Defined

This file defines 13 function(s):

#### Function: `test_invalid_auth` (line 38)

**Parameters**: self, invalid_auth, expected_code, expected_message

#### Function: `test_valid_single_upload` (line 46)

**Parameters**: self, WebApiAuth, add_dataset_func, tmp_path

#### Function: `test_file_type_validation` (line 71)

**Parameters**: self, WebApiAuth, add_dataset_func, generate_test_files, request

#### Function: `test_unsupported_file_type` (line 84)

**Parameters**: self, WebApiAuth, add_dataset_func, tmp_path, file_type

#### Function: `test_missing_file` (line 93)

**Parameters**: self, WebApiAuth, add_dataset_func

#### Function: `test_empty_file` (line 100)

**Parameters**: self, WebApiAuth, add_dataset_func, tmp_path

#### Function: `test_filename_empty` (line 110)

**Parameters**: self, WebApiAuth, add_dataset_func, tmp_path

#### Function: `test_filename_exceeds_max_length` (line 127)

**Parameters**: self, WebApiAuth, add_dataset_func, tmp_path

#### Function: `test_invalid_kb_id` (line 135)

**Parameters**: self, WebApiAuth, tmp_path

#### Function: `test_duplicate_files` (line 142)

**Parameters**: self, WebApiAuth, add_dataset_func, tmp_path

#### Function: `test_filename_special_characters` (line 156)

**Parameters**: self, WebApiAuth, add_dataset_func, tmp_path

#### Function: `test_multiple_files` (line 171)

**Parameters**: self, WebApiAuth, add_dataset_func, tmp_path

#### Function: `test_concurrent_upload` (line 185)

**Parameters**: self, WebApiAuth, add_dataset_func, tmp_path

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
from common import DOCUMENT_APP_URL, list_kbs, upload_documents
from configs import DOCUMENT_NAME_LIMIT, HOST_ADDRESS, INVALID_API_TOKEN
from libs.auth import RAGFlowWebApiAuth
from requests_toolbelt import MultipartEncoder
from utils.file_utils import create_txt_file


@pytest.mark.p1
@pytest.mark.usefixtures("clear_datasets")
class TestAuthorization:
    @pytest.mark.parametrize(
        "invalid_auth, expected_code, expected_message",
        [
            (None, 401, "<Unauthorized '401: Unauthorized'>"),
            (RAGFlowWebApiAuth(INVALID_API_TOKEN), 401, "<Unauthorized '401: Unauthorized'>"),
        ],
    )
    def test_invalid_auth(self, invalid_auth, expected_code, expected_message):
        res = upload_documents(invalid_auth)
        assert res["code"] == expected_code, res
        assert res["message"] == expected_message, res


class TestDocumentsUpload:
    @pytest.mark.p1
    def test_valid_single_upload(self, WebApiAuth, add_dataset_func, tmp_path):
        kb_id = add_dataset_func
        fp = create_txt_file(tmp_path / "ragflow_test.txt")
        res = upload_documents(WebApiAuth, {"kb_id": kb_id}, [fp])
        assert res["code"] == 0, res
        assert res["data"][0]["kb_id"] == kb_id, res
        assert res["data"][0]["name"] == fp.name, res

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
    def test_file_type_validation(self, WebApiAuth, add_dataset_func, generate_test_files, request):
        kb_id = add_dataset_func
        fp = generate_test_files[request.node.callspec.params["generate_test_files"]]
        res = upload_documents(WebApiAuth, {"kb_id": kb_id}, [fp])
        assert res["code"] == 0, res
        assert res["data"][0]["kb_id"] == kb_id, res
        assert res["data"][0]["name"] == fp.name, res

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "file_type",
        ["exe", "unknown"],
    )
    def test_unsupported_file_type(self, WebApiAuth, add_dataset_func, tmp_path, file_type):
        kb_id = add_dataset_func
        fp = tmp_path / f"ragflow_test.{file_type}"
        fp.touch()
        res = upload_documents(WebApiAuth, {"kb_id": kb_id}, [fp])
        assert res["code"] == 500, res
        assert res["message"] == f"ragflow_test.{file_type}: This type of file has not been supported yet!", res

    @pytest.mark.p2
    def test_missing_file(self, WebApiAuth, add_dataset_func):
        kb_id = add_dataset_func
        res = upload_documents(WebApiAuth, {"kb_id": kb_id})
        assert res["code"] == 101, res
        assert res["message"] == "No file part!", res

    @pytest.mark.p3
    def test_empty_file(self, WebApiAuth, add_dataset_func, tmp_path):
        kb_id = add_dataset_func
        fp = tmp_path / "empty.txt"
        fp.touch()

        res = upload_documents(WebApiAuth, {"kb_id": kb_id}, [fp])
        assert res["code"] == 0, res
        assert res["data"][0]["size"] == 0, res

    @pytest.mark.p3
    def test_filename_empty(self, WebApiAuth, add_dataset_func, tmp_path):
        kb_id = add_dataset_func

        fp = create_txt_file(tmp_path / "ragflow_test.txt")
        url = f"{HOST_ADDRESS}{DOCUMENT_APP_URL}/upload"
        fields = [("file", ("", fp.open("rb"))), ("kb_id", kb_id)]
        m = MultipartEncoder(fields=fields)
        res = requests.post(
            url=url,
            headers={"Content-Type": m.content_type},
            auth=WebApiAuth,
            data=m,
        )
        assert res.json()["code"] == 101, res
        assert res.json()["message"] == "No file selected!", res

    @pytest.mark.p2
    def test_filename_exceeds_max_length(self, WebApiAuth, add_dataset_func, tmp_path):
        kb_id = add_dataset_func
        fp = create_txt_file(tmp_path / f"{'a' * (DOCUMENT_NAME_LIMIT - 4)}.txt")
        res = upload_documents(WebApiAuth, {"kb_id": kb_id}, [fp])
        assert res["code"] == 0, res
        assert res["data"][0]["name"] == fp.name, res

    @pytest.mark.p2
    def test_invalid_kb_id(self, WebApiAuth, tmp_path):
        fp = create_txt_file(tmp_path / "ragflow_test.txt")
        res = upload_documents(WebApiAuth, {"kb_id": "invalid_kb_id"}, [fp])
        assert res["code"] == 100, res
        assert res["message"] == """LookupError("Can't find this knowledgebase!")""", res

    @pytest.mark.p2
    def test_duplicate_files(self, WebApiAuth, add_dataset_func, tmp_path):
        kb_id = add_dataset_func
        fp = create_txt_file(tmp_path / "ragflow_test.txt")
        res = upload_documents(WebApiAuth, {"kb_id": kb_id}, [fp, fp])
        assert res["code"] == 0, res
        assert len(res["data"]) == 2, res
        for i in range(len(res["data"])):
            assert res["data"][i]["kb_id"] == kb_id, res
            expected_name = fp.name
            if i != 0:
                expected_name = f"{fp.stem}({i}){fp.suffix}"
            assert res["data"][i]["name"] == expected_name, res

    @pytest.mark.p3
    def test_filename_special_characters(self, WebApiAuth, add_dataset_func, tmp_path):
        kb_id = add_dataset_func
        illegal_chars = '<>:"/\\|?*'
        translation_table = str.maketrans({char: "_" for char in illegal_chars})
        safe_filename = string.punctuation.translate(translation_table)
        fp = tmp_path / f"{safe_filename}.txt"
        fp.write_text("Sample text content")

        res = upload_documents(WebApiAuth, {"kb_id": kb_id}, [fp])
        assert res["code"] == 0, res
        assert len(res["data"]) == 1, res
        assert res["data"][0]["kb_id"] == kb_id, res
        assert res["data"][0]["name"] == fp.name, res

    @pytest.mark.p1
    def test_multiple_files(self, WebApiAuth, add_dataset_func, tmp_path):
        kb_id = add_dataset_func
        expected_document_count = 20
        fps = []
        for i in range(expected_document_count):
            fp = create_txt_file(tmp_path / f"ragflow_test_{i}.txt")
            fps.append(fp)
        res = upload_documents(WebApiAuth, {"kb_id": kb_id}, fps)
        assert res["code"] == 0, res

        res = list_kbs(WebApiAuth)
        assert res["data"]["kbs"][0]["doc_num"] == expected_document_count, res

    @pytest.mark.p3
    def test_concurrent_upload(self, WebApiAuth, add_dataset_func, tmp_path):
        kb_id = add_dataset_func

        count = 20
        fps = []
        for i in range(count):
            fp = create_txt_file(tmp_path / f"ragflow_test_{i}.txt")
            fps.append(fp)

        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [executor.submit(upload_documents, WebApiAuth, {"kb_id": kb_id}, fps[i : i + 1]) for i in range(count)]
        responses = list(as_completed(futures))
        assert len(responses) == count, responses
        assert all(future.result()["code"] == 0 for future in futures), responses

        res = list_kbs(WebApiAuth)
        assert res["data"]["kbs"][0]["doc_num"] == count, res

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_web_api/test_document_app/test_upload_documents.py` is located in the `test/testcases/test_web_api/test_document_app` directory.

This file is part of the **Testing** infrastructure.

### Architecture Context

Files in this location typically handle concerns related to test_document_app.

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
- [test_create_document.py](test_create_document.py_docs.md)
- [test_list_documents.py](test_list_documents.py_docs.md)
- [test_paser_documents.py](test_paser_documents.py_docs.md)
- [test_rm_documents.py](test_rm_documents.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
