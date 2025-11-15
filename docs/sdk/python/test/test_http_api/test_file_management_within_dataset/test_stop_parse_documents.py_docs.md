# Documentation: sdk/python/test/test_http_api/test_file_management_within_dataset/test_stop_parse_documents.py

## File Metadata

- **Path**: `sdk/python/test/test_http_api/test_file_management_within_dataset/test_stop_parse_documents.py`
- **Size**: 8822 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/test/test_http_api/test_file_management_within_dataset/test_stop_parse_documents.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `concurrent.futures`
- `time`
- `pytest`
- `common`
- `libs.auth`
- `libs.utils`

### Classes Defined

This file defines 2 class(es):

#### Class: `TestAuthorization` (line 46)

**Methods**: test_invalid_auth

#### Class: `TestDocumentsParseStop` (line 65)

**Methods**: test_basic_scenarios, test_invalid_dataset_id, test_stop_parse_partial_invalid_document_id, test_repeated_stop_parse, test_duplicate_stop_parse

### Functions Defined

This file defines 11 function(s):

#### Function: `validate_document_parse_done` (line 25)

**Parameters**: auth, dataset_id, document_ids

#### Function: `validate_document_parse_cancel` (line 36)

**Parameters**: auth, dataset_id, document_ids

#### Function: `test_stop_parse_100_files` (line 172)

**Parameters**: get_http_api_auth, add_dataset_func, tmp_path

#### Function: `test_concurrent_parse` (line 184)

**Parameters**: get_http_api_auth, add_dataset_func, tmp_path

#### Function: `test_invalid_auth` (line 58)

**Parameters**: self, auth, expected_code, expected_message

#### Function: `test_basic_scenarios` (line 78)

**Parameters**: self, get_http_api_auth, add_documents_func, payload, expected_code, expected_message

#### Function: `test_invalid_dataset_id` (line 115)

**Parameters**: self, get_http_api_auth, add_documents_func, invalid_dataset_id, expected_code, expected_message

#### Function: `test_stop_parse_partial_invalid_document_id` (line 138)

**Parameters**: self, get_http_api_auth, add_documents_func, payload

#### Function: `test_repeated_stop_parse` (line 151)

**Parameters**: self, get_http_api_auth, add_documents_func

#### Function: `test_duplicate_stop_parse` (line 162)

**Parameters**: self, get_http_api_auth, add_documents_func

#### Function: `condition` (line 80)

**Parameters**: _auth, _dataset_id, _document_ids

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
from time import sleep

import pytest
from common import INVALID_API_TOKEN, bulk_upload_documents, list_documnets, parse_documnets, stop_parse_documnets
from libs.auth import RAGFlowHttpApiAuth
from libs.utils import wait_for


def validate_document_parse_done(auth, dataset_id, document_ids):
    for document_id in document_ids:
        res = list_documnets(auth, dataset_id, params={"id": document_id})
        doc = res["data"]["docs"][0]
        assert doc["run"] == "DONE"
        assert len(doc["process_begin_at"]) > 0
        assert doc["process_duration"] > 0
        assert doc["progress"] > 0
        assert "Task done" in doc["progress_msg"]


def validate_document_parse_cancel(auth, dataset_id, document_ids):
    for document_id in document_ids:
        res = list_documnets(auth, dataset_id, params={"id": document_id})
        doc = res["data"]["docs"][0]
        assert doc["run"] == "CANCEL"
        assert len(doc["process_begin_at"]) > 0
        assert doc["progress"] == 0.0


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
        res = stop_parse_documnets(auth, "dataset_id")
        assert res["code"] == expected_code
        assert res["message"] == expected_message


@pytest.mark.skip
class TestDocumentsParseStop:
    @pytest.mark.parametrize(
        "payload, expected_code, expected_message",
        [
            pytest.param(None, 102, """AttributeError("\'NoneType\' object has no attribute \'get\'")""", marks=pytest.mark.skip),
            pytest.param({"document_ids": []}, 102, "`document_ids` is required", marks=pytest.mark.p1),
            pytest.param({"document_ids": ["invalid_id"]}, 102, "You don't own the document invalid_id.", marks=pytest.mark.p3),
            pytest.param({"document_ids": ["\n!?。；！？\"'"]}, 102, """You don\'t own the document \n!?。；！？"\'.""", marks=pytest.mark.p3),
            pytest.param("not json", 102, "AttributeError(\"'str' object has no attribute 'get'\")", marks=pytest.mark.skip),
            pytest.param(lambda r: {"document_ids": r[:1]}, 0, "", marks=pytest.mark.p1),
            pytest.param(lambda r: {"document_ids": r}, 0, "", marks=pytest.mark.p1),
        ],
    )
    def test_basic_scenarios(self, get_http_api_auth, add_documents_func, payload, expected_code, expected_message):
        @wait_for(10, 1, "Document parsing timeout")
        def condition(_auth, _dataset_id, _document_ids):
            for _document_id in _document_ids:
                res = list_documnets(_auth, _dataset_id, {"id": _document_id})
                if res["data"]["docs"][0]["run"] != "DONE":
                    return False
            return True

        dataset_id, document_ids = add_documents_func
        parse_documnets(get_http_api_auth, dataset_id, {"document_ids": document_ids})

        if callable(payload):
            payload = payload(document_ids)

        res = stop_parse_documnets(get_http_api_auth, dataset_id, payload)
        assert res["code"] == expected_code
        if expected_code == 0:
            completed_document_ids = list(set(document_ids) - set(payload["document_ids"]))
            condition(get_http_api_auth, dataset_id, completed_document_ids)
            validate_document_parse_cancel(get_http_api_auth, dataset_id, payload["document_ids"])
            validate_document_parse_done(get_http_api_auth, dataset_id, completed_document_ids)
        else:
            assert res["message"] == expected_message

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "invalid_dataset_id, expected_code, expected_message",
        [
            ("", 100, "<MethodNotAllowed '405: Method Not Allowed'>"),
            (
                "invalid_dataset_id",
                102,
                "You don't own the dataset invalid_dataset_id.",
            ),
        ],
    )
    def test_invalid_dataset_id(
        self,
        get_http_api_auth,
        add_documents_func,
        invalid_dataset_id,
        expected_code,
        expected_message,
    ):
        dataset_id, document_ids = add_documents_func
        parse_documnets(get_http_api_auth, dataset_id, {"document_ids": document_ids})
        res = stop_parse_documnets(get_http_api_auth, invalid_dataset_id, {"document_ids": document_ids})
        assert res["code"] == expected_code
        assert res["message"] == expected_message

    @pytest.mark.skip
    @pytest.mark.parametrize(
        "payload",
        [
            lambda r: {"document_ids": ["invalid_id"] + r},
            lambda r: {"document_ids": r[:1] + ["invalid_id"] + r[1:3]},
            lambda r: {"document_ids": r + ["invalid_id"]},
        ],
    )
    def test_stop_parse_partial_invalid_document_id(self, get_http_api_auth, add_documents_func, payload):
        dataset_id, document_ids = add_documents_func
        parse_documnets(get_http_api_auth, dataset_id, {"document_ids": document_ids})

        if callable(payload):
            payload = payload(document_ids)
        res = stop_parse_documnets(get_http_api_auth, dataset_id, payload)
        assert res["code"] == 102
        assert res["message"] == "You don't own the document invalid_id."

        validate_document_parse_cancel(get_http_api_auth, dataset_id, document_ids)

    @pytest.mark.p3
    def test_repeated_stop_parse(self, get_http_api_auth, add_documents_func):
        dataset_id, document_ids = add_documents_func
        parse_documnets(get_http_api_auth, dataset_id, {"document_ids": document_ids})
        res = stop_parse_documnets(get_http_api_auth, dataset_id, {"document_ids": document_ids})
        assert res["code"] == 0

        res = stop_parse_documnets(get_http_api_auth, dataset_id, {"document_ids": document_ids})
        assert res["code"] == 102
        assert res["message"] == "Can't stop parsing document with progress at 0 or 1"

    @pytest.mark.p3
    def test_duplicate_stop_parse(self, get_http_api_auth, add_documents_func):
        dataset_id, document_ids = add_documents_func
        parse_documnets(get_http_api_auth, dataset_id, {"document_ids": document_ids})
        res = stop_parse_documnets(get_http_api_auth, dataset_id, {"document_ids": document_ids + document_ids})
        assert res["code"] == 0
        assert res["data"]["success_count"] == 3
        assert f"Duplicate document ids: {document_ids[0]}" in res["data"]["errors"]


@pytest.mark.skip(reason="unstable")
def test_stop_parse_100_files(get_http_api_auth, add_dataset_func, tmp_path):
    document_num = 100
    dataset_id = add_dataset_func
    document_ids = bulk_upload_documents(get_http_api_auth, dataset_id, document_num, tmp_path)
    parse_documnets(get_http_api_auth, dataset_id, {"document_ids": document_ids})
    sleep(1)
    res = stop_parse_documnets(get_http_api_auth, dataset_id, {"document_ids": document_ids})
    assert res["code"] == 0
    validate_document_parse_cancel(get_http_api_auth, dataset_id, document_ids)


@pytest.mark.skip(reason="unstable")
def test_concurrent_parse(get_http_api_auth, add_dataset_func, tmp_path):
    document_num = 50
    dataset_id = add_dataset_func
    document_ids = bulk_upload_documents(get_http_api_auth, dataset_id, document_num, tmp_path)
    parse_documnets(get_http_api_auth, dataset_id, {"document_ids": document_ids})

    with ThreadPoolExecutor(max_workers=5) as executor:
        futures = [
            executor.submit(
                stop_parse_documnets,
                get_http_api_auth,
                dataset_id,
                {"document_ids": document_ids[i : i + 1]},
            )
            for i in range(document_num)
        ]
    responses = [f.result() for f in futures]
    assert all(r["code"] == 0 for r in responses)
    validate_document_parse_cancel(get_http_api_auth, dataset_id, document_ids)

```

## Detailed Analysis

### File Role in Repository

The file `sdk/python/test/test_http_api/test_file_management_within_dataset/test_stop_parse_documents.py` is located in the `sdk/python/test/test_http_api/test_file_management_within_dataset` directory.

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
- [test_update_document.py](test_update_document.py_docs.md)
- [test_upload_documents.py](test_upload_documents.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
