# Documentation: sdk/python/test/test_http_api/test_file_management_within_dataset/test_list_documents.py

## File Metadata

- **Path**: `sdk/python/test/test_http_api/test_file_management_within_dataset/test_list_documents.py`
- **Size**: 12903 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/test/test_http_api/test_file_management_within_dataset/test_list_documents.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `concurrent.futures`
- `pytest`
- `common`
- `libs.auth`
- `libs.utils`

### Classes Defined

This file defines 2 class(es):

#### Class: `TestAuthorization` (line 25)

**Methods**: test_invalid_auth

#### Class: `TestDocumentsList` (line 43)

**Methods**: test_default, test_invalid_dataset_id, test_page, test_page_size, test_orderby, test_desc, test_keywords, test_name, test_id, test_name_and_id, test_concurrent_list, test_invalid_params

### Functions Defined

This file defines 13 function(s):

#### Function: `test_invalid_auth` (line 37)

**Parameters**: self, auth, expected_code, expected_message

#### Function: `test_default` (line 45)

**Parameters**: self, get_http_api_auth, add_documents

#### Function: `test_invalid_dataset_id` (line 64)

**Parameters**: self, get_http_api_auth, dataset_id, expected_code, expected_message

#### Function: `test_page` (line 94)

**Parameters**: self, get_http_api_auth, add_documents, params, expected_code, expected_page_size, expected_message

#### Function: `test_page_size` (line 137)

**Parameters**: self, get_http_api_auth, add_documents, params, expected_code, expected_page_size, expected_message

#### Function: `test_orderby` (line 165)

**Parameters**: self, get_http_api_auth, add_documents, params, expected_code, assertions, expected_message

#### Function: `test_desc` (line 198)

**Parameters**: self, get_http_api_auth, add_documents, params, expected_code, assertions, expected_message

#### Function: `test_keywords` (line 227)

**Parameters**: self, get_http_api_auth, add_documents, params, expected_num

#### Function: `test_name` (line 249)

**Parameters**: self, get_http_api_auth, add_documents, params, expected_code, expected_num, expected_message

#### Function: `test_id` (line 279)

**Parameters**: self, get_http_api_auth, add_documents, document_id, expected_code, expected_num, expected_message

#### Function: `test_name_and_id` (line 320)

**Parameters**: self, get_http_api_auth, add_documents, document_id, name, expected_code, expected_num, expected_message

#### Function: `test_concurrent_list` (line 343)

**Parameters**: self, get_http_api_auth, add_documents

#### Function: `test_invalid_params` (line 352)

**Parameters**: self, get_http_api_auth, add_documents

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
from concurrent.futures import ThreadPoolExecutor

import pytest
from common import INVALID_API_TOKEN, list_documnets
from libs.auth import RAGFlowHttpApiAuth
from libs.utils import is_sorted


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
    def test_invalid_auth(self, auth, expected_code, expected_message):
        res = list_documnets(auth, "dataset_id")
        assert res["code"] == expected_code
        assert res["message"] == expected_message


class TestDocumentsList:
    @pytest.mark.p1
    def test_default(self, get_http_api_auth, add_documents):
        dataset_id, _ = add_documents
        res = list_documnets(get_http_api_auth, dataset_id)
        assert res["code"] == 0
        assert len(res["data"]["docs"]) == 5
        assert res["data"]["total"] == 5

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "dataset_id, expected_code, expected_message",
        [
            ("", 100, "<MethodNotAllowed '405: Method Not Allowed'>"),
            (
                "invalid_dataset_id",
                102,
                "You don't own the dataset invalid_dataset_id. ",
            ),
        ],
    )
    def test_invalid_dataset_id(self, get_http_api_auth, dataset_id, expected_code, expected_message):
        res = list_documnets(get_http_api_auth, dataset_id)
        assert res["code"] == expected_code
        assert res["message"] == expected_message

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "params, expected_code, expected_page_size, expected_message",
        [
            ({"page": None, "page_size": 2}, 0, 2, ""),
            ({"page": 0, "page_size": 2}, 0, 2, ""),
            ({"page": 2, "page_size": 2}, 0, 2, ""),
            ({"page": 3, "page_size": 2}, 0, 1, ""),
            ({"page": "3", "page_size": 2}, 0, 1, ""),
            pytest.param(
                {"page": -1, "page_size": 2},
                100,
                0,
                "1064",
                marks=pytest.mark.skip(reason="issues/5851"),
            ),
            pytest.param(
                {"page": "a", "page_size": 2},
                100,
                0,
                """ValueError("invalid literal for int() with base 10: \'a\'")""",
                marks=pytest.mark.skip(reason="issues/5851"),
            ),
        ],
    )
    def test_page(
        self,
        get_http_api_auth,
        add_documents,
        params,
        expected_code,
        expected_page_size,
        expected_message,
    ):
        dataset_id, _ = add_documents
        res = list_documnets(get_http_api_auth, dataset_id, params=params)
        assert res["code"] == expected_code
        if expected_code == 0:
            assert len(res["data"]["docs"]) == expected_page_size
            assert res["data"]["total"] == 5
        else:
            assert res["message"] == expected_message

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "params, expected_code, expected_page_size, expected_message",
        [
            ({"page_size": None}, 0, 5, ""),
            ({"page_size": 0}, 0, 0, ""),
            ({"page_size": 1}, 0, 1, ""),
            ({"page_size": 6}, 0, 5, ""),
            ({"page_size": "1"}, 0, 1, ""),
            pytest.param(
                {"page_size": -1},
                100,
                0,
                "1064",
                marks=pytest.mark.skip(reason="issues/5851"),
            ),
            pytest.param(
                {"page_size": "a"},
                100,
                0,
                """ValueError("invalid literal for int() with base 10: \'a\'")""",
                marks=pytest.mark.skip(reason="issues/5851"),
            ),
        ],
    )
    def test_page_size(
        self,
        get_http_api_auth,
        add_documents,
        params,
        expected_code,
        expected_page_size,
        expected_message,
    ):
        dataset_id, _ = add_documents
        res = list_documnets(get_http_api_auth, dataset_id, params=params)
        assert res["code"] == expected_code
        if expected_code == 0:
            assert len(res["data"]["docs"]) == expected_page_size
        else:
            assert res["message"] == expected_message

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "params, expected_code, assertions, expected_message",
        [
            ({"orderby": None}, 0, lambda r: (is_sorted(r["data"]["docs"], "create_time", True)), ""),
            ({"orderby": "create_time"}, 0, lambda r: (is_sorted(r["data"]["docs"], "create_time", True)), ""),
            ({"orderby": "update_time"}, 0, lambda r: (is_sorted(r["data"]["docs"], "update_time", True)), ""),
            pytest.param({"orderby": "name", "desc": "False"}, 0, lambda r: (is_sorted(r["data"]["docs"], "name", False)), "", marks=pytest.mark.skip(reason="issues/5851")),
            pytest.param({"orderby": "unknown"}, 102, 0, "orderby should be create_time or update_time", marks=pytest.mark.skip(reason="issues/5851")),
        ],
    )
    def test_orderby(
        self,
        get_http_api_auth,
        add_documents,
        params,
        expected_code,
        assertions,
        expected_message,
    ):
        dataset_id, _ = add_documents
        res = list_documnets(get_http_api_auth, dataset_id, params=params)
        assert res["code"] == expected_code
        if expected_code == 0:
            if callable(assertions):
                assert assertions(res)
        else:
            assert res["message"] == expected_message

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "params, expected_code, assertions, expected_message",
        [
            ({"desc": None}, 0, lambda r: (is_sorted(r["data"]["docs"], "create_time", True)), ""),
            ({"desc": "true"}, 0, lambda r: (is_sorted(r["data"]["docs"], "create_time", True)), ""),
            ({"desc": "True"}, 0, lambda r: (is_sorted(r["data"]["docs"], "create_time", True)), ""),
            ({"desc": True}, 0, lambda r: (is_sorted(r["data"]["docs"], "create_time", True)), ""),
            pytest.param({"desc": "false"}, 0, lambda r: (is_sorted(r["data"]["docs"], "create_time", False)), "", marks=pytest.mark.skip(reason="issues/5851")),
            ({"desc": "False"}, 0, lambda r: (is_sorted(r["data"]["docs"], "create_time", False)), ""),
            ({"desc": False}, 0, lambda r: (is_sorted(r["data"]["docs"], "create_time", False)), ""),
            ({"desc": "False", "orderby": "update_time"}, 0, lambda r: (is_sorted(r["data"]["docs"], "update_time", False)), ""),
            pytest.param({"desc": "unknown"}, 102, 0, "desc should be true or false", marks=pytest.mark.skip(reason="issues/5851")),
        ],
    )
    def test_desc(
        self,
        get_http_api_auth,
        add_documents,
        params,
        expected_code,
        assertions,
        expected_message,
    ):
        dataset_id, _ = add_documents
        res = list_documnets(get_http_api_auth, dataset_id, params=params)
        assert res["code"] == expected_code
        if expected_code == 0:
            if callable(assertions):
                assert assertions(res)
        else:
            assert res["message"] == expected_message

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "params, expected_num",
        [
            ({"keywords": None}, 5),
            ({"keywords": ""}, 5),
            ({"keywords": "0"}, 1),
            ({"keywords": "ragflow_test_upload"}, 5),
            ({"keywords": "unknown"}, 0),
        ],
    )
    def test_keywords(self, get_http_api_auth, add_documents, params, expected_num):
        dataset_id, _ = add_documents
        res = list_documnets(get_http_api_auth, dataset_id, params=params)
        assert res["code"] == 0
        assert len(res["data"]["docs"]) == expected_num
        assert res["data"]["total"] == expected_num

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "params, expected_code, expected_num, expected_message",
        [
            ({"name": None}, 0, 5, ""),
            ({"name": ""}, 0, 5, ""),
            ({"name": "ragflow_test_upload_0.txt"}, 0, 1, ""),
            (
                {"name": "unknown.txt"},
                102,
                0,
                "You don't own the document unknown.txt.",
            ),
        ],
    )
    def test_name(
        self,
        get_http_api_auth,
        add_documents,
        params,
        expected_code,
        expected_num,
        expected_message,
    ):
        dataset_id, _ = add_documents
        res = list_documnets(get_http_api_auth, dataset_id, params=params)
        assert res["code"] == expected_code
        if expected_code == 0:
            if params["name"] in [None, ""]:
                assert len(res["data"]["docs"]) == expected_num
            else:
                assert res["data"]["docs"][0]["name"] == params["name"]
        else:
            assert res["message"] == expected_message

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "document_id, expected_code, expected_num, expected_message",
        [
            (None, 0, 5, ""),
            ("", 0, 5, ""),
            (lambda r: r[0], 0, 1, ""),
            ("unknown.txt", 102, 0, "You don't own the document unknown.txt."),
        ],
    )
    def test_id(
        self,
        get_http_api_auth,
        add_documents,
        document_id,
        expected_code,
        expected_num,
        expected_message,
    ):
        dataset_id, document_ids = add_documents
        if callable(document_id):
            params = {"id": document_id(document_ids)}
        else:
            params = {"id": document_id}
        res = list_documnets(get_http_api_auth, dataset_id, params=params)

        assert res["code"] == expected_code
        if expected_code == 0:
            if params["id"] in [None, ""]:
                assert len(res["data"]["docs"]) == expected_num
            else:
                assert res["data"]["docs"][0]["id"] == params["id"]
        else:
            assert res["message"] == expected_message

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "document_id, name, expected_code, expected_num, expected_message",
        [
            (lambda r: r[0], "ragflow_test_upload_0.txt", 0, 1, ""),
            (lambda r: r[0], "ragflow_test_upload_1.txt", 0, 0, ""),
            (lambda r: r[0], "unknown", 102, 0, "You don't own the document unknown."),
            (
                "id",
                "ragflow_test_upload_0.txt",
                102,
                0,
                "You don't own the document id.",
            ),
        ],
    )
    def test_name_and_id(
        self,
        get_http_api_auth,
        add_documents,
        document_id,
        name,
        expected_code,
        expected_num,
        expected_message,
    ):
        dataset_id, document_ids = add_documents
        if callable(document_id):
            params = {"id": document_id(document_ids), "name": name}
        else:
            params = {"id": document_id, "name": name}

        res = list_documnets(get_http_api_auth, dataset_id, params=params)
        if expected_code == 0:
            assert len(res["data"]["docs"]) == expected_num
        else:
            assert res["message"] == expected_message

    @pytest.mark.p3
    def test_concurrent_list(self, get_http_api_auth, add_documents):
        dataset_id, _ = add_documents

        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [executor.submit(list_documnets, get_http_api_auth, dataset_id) for i in range(100)]
        responses = [f.result() for f in futures]
        assert all(r["code"] == 0 for r in responses)

    @pytest.mark.p3
    def test_invalid_params(self, get_http_api_auth, add_documents):
        dataset_id, _ = add_documents
        params = {"a": "b"}
        res = list_documnets(get_http_api_auth, dataset_id, params=params)
        assert res["code"] == 0
        assert len(res["data"]["docs"]) == 5

```

## Detailed Analysis

### File Role in Repository

The file `sdk/python/test/test_http_api/test_file_management_within_dataset/test_list_documents.py` is located in the `sdk/python/test/test_http_api/test_file_management_within_dataset` directory.

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
