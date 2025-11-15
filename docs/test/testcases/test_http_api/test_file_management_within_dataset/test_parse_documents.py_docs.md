# Documentation: test/testcases/test_http_api/test_file_management_within_dataset/test_parse_documents.py

## File Metadata

- **Path**: `test/testcases/test_http_api/test_file_management_within_dataset/test_parse_documents.py`
- **Size**: 8493 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_http_api/test_file_management_within_dataset/test_parse_documents.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `concurrent.futures`
- `pytest`
- `common`
- `configs`
- `libs.auth`
- `utils`

### Classes Defined

This file defines 2 class(es):

#### Class: `TestAuthorization` (line 56)

**Methods**: test_invalid_auth

#### Class: `TestDocumentsParse` (line 74)

**Methods**: test_basic_scenarios, test_invalid_dataset_id, test_parse_partial_invalid_document_id, test_repeated_parse, test_duplicate_parse

### Functions Defined

This file defines 12 function(s):

#### Function: `condition` (line 26)

**Parameters**: _auth, _dataset_id, _document_ids

#### Function: `validate_document_details` (line 44)

**Parameters**: auth, dataset_id, document_ids

#### Function: `test_parse_100_files` (line 169)

**Parameters**: HttpApiAuth, add_dataset_func, tmp_path

#### Function: `test_concurrent_parse` (line 190)

**Parameters**: HttpApiAuth, add_dataset_func, tmp_path

#### Function: `test_invalid_auth` (line 68)

**Parameters**: self, invalid_auth, expected_code, expected_message

#### Function: `test_basic_scenarios` (line 87)

**Parameters**: self, HttpApiAuth, add_documents_func, payload, expected_code, expected_message

#### Function: `test_invalid_dataset_id` (line 111)

**Parameters**: self, HttpApiAuth, add_documents_func, dataset_id, expected_code, expected_message

#### Function: `test_parse_partial_invalid_document_id` (line 132)

**Parameters**: self, HttpApiAuth, add_documents_func, payload

#### Function: `test_repeated_parse` (line 145)

**Parameters**: self, HttpApiAuth, add_documents_func

#### Function: `test_duplicate_parse` (line 156)

**Parameters**: self, HttpApiAuth, add_documents_func

#### Function: `condition` (line 171)

**Parameters**: _auth, _dataset_id, _document_num

#### Function: `condition` (line 192)

**Parameters**: _auth, _dataset_id, _document_num

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
from concurrent.futures import ThreadPoolExecutor, as_completed

import pytest
from common import bulk_upload_documents, list_documents, parse_documents
from configs import INVALID_API_TOKEN
from libs.auth import RAGFlowHttpApiAuth
from utils import wait_for


@wait_for(30, 1, "Document parsing timeout")
def condition(_auth, _dataset_id, _document_ids=None):
    res = list_documents(_auth, _dataset_id)
    target_docs = res["data"]["docs"]

    if _document_ids is None:
        for doc in target_docs:
            if doc["run"] != "DONE":
                return False
        return True

    target_ids = set(_document_ids)
    for doc in target_docs:
        if doc["id"] in target_ids:
            if doc.get("run") != "DONE":
                return False
    return True


def validate_document_details(auth, dataset_id, document_ids):
    for document_id in document_ids:
        res = list_documents(auth, dataset_id, params={"id": document_id})
        doc = res["data"]["docs"][0]
        assert doc["run"] == "DONE"
        assert len(doc["process_begin_at"]) > 0
        assert doc["process_duration"] > 0
        assert doc["progress"] > 0
        assert "Task done" in doc["progress_msg"]


@pytest.mark.p1
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
        res = parse_documents(invalid_auth, "dataset_id")
        assert res["code"] == expected_code
        assert res["message"] == expected_message


class TestDocumentsParse:
    @pytest.mark.parametrize(
        "payload, expected_code, expected_message",
        [
            pytest.param(None, 102, """AttributeError("\'NoneType\' object has no attribute \'get\'")""", marks=pytest.mark.skip),
            pytest.param({"document_ids": []}, 102, "`document_ids` is required", marks=pytest.mark.p1),
            pytest.param({"document_ids": ["invalid_id"]}, 102, "Documents not found: ['invalid_id']", marks=pytest.mark.p3),
            pytest.param({"document_ids": ["\n!?。；！？\"'"]}, 102, """Documents not found: [\'\\n!?。；！？"\\\'\']""", marks=pytest.mark.p3),
            pytest.param("not json", 102, "AttributeError(\"'str' object has no attribute 'get'\")", marks=pytest.mark.skip),
            pytest.param(lambda r: {"document_ids": r[:1]}, 0, "", marks=pytest.mark.p1),
            pytest.param(lambda r: {"document_ids": r}, 0, "", marks=pytest.mark.p1),
        ],
    )
    def test_basic_scenarios(self, HttpApiAuth, add_documents_func, payload, expected_code, expected_message):
        dataset_id, document_ids = add_documents_func
        if callable(payload):
            payload = payload(document_ids)
        res = parse_documents(HttpApiAuth, dataset_id, payload)
        assert res["code"] == expected_code
        if expected_code != 0:
            assert res["message"] == expected_message
        if expected_code == 0:
            condition(HttpApiAuth, dataset_id, payload["document_ids"])
            validate_document_details(HttpApiAuth, dataset_id, payload["document_ids"])

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "dataset_id, expected_code, expected_message",
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
        HttpApiAuth,
        add_documents_func,
        dataset_id,
        expected_code,
        expected_message,
    ):
        _, document_ids = add_documents_func
        res = parse_documents(HttpApiAuth, dataset_id, {"document_ids": document_ids})
        assert res["code"] == expected_code
        assert res["message"] == expected_message

    @pytest.mark.parametrize(
        "payload",
        [
            pytest.param(lambda r: {"document_ids": ["invalid_id"] + r}, marks=pytest.mark.p3),
            pytest.param(lambda r: {"document_ids": r[:1] + ["invalid_id"] + r[1:3]}, marks=pytest.mark.p1),
            pytest.param(lambda r: {"document_ids": r + ["invalid_id"]}, marks=pytest.mark.p3),
        ],
    )
    def test_parse_partial_invalid_document_id(self, HttpApiAuth, add_documents_func, payload):
        dataset_id, document_ids = add_documents_func
        if callable(payload):
            payload = payload(document_ids)
        res = parse_documents(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 102
        assert res["message"] == "Documents not found: ['invalid_id']"

        condition(HttpApiAuth, dataset_id)

        validate_document_details(HttpApiAuth, dataset_id, document_ids)

    @pytest.mark.p3
    def test_repeated_parse(self, HttpApiAuth, add_documents_func):
        dataset_id, document_ids = add_documents_func
        res = parse_documents(HttpApiAuth, dataset_id, {"document_ids": document_ids})
        assert res["code"] == 0

        condition(HttpApiAuth, dataset_id)

        res = parse_documents(HttpApiAuth, dataset_id, {"document_ids": document_ids})
        assert res["code"] == 0

    @pytest.mark.p3
    def test_duplicate_parse(self, HttpApiAuth, add_documents_func):
        dataset_id, document_ids = add_documents_func
        res = parse_documents(HttpApiAuth, dataset_id, {"document_ids": document_ids + document_ids})
        assert res["code"] == 0
        assert "Duplicate document ids" in res["data"]["errors"][0]
        assert res["data"]["success_count"] == 3

        condition(HttpApiAuth, dataset_id)

        validate_document_details(HttpApiAuth, dataset_id, document_ids)


@pytest.mark.p3
def test_parse_100_files(HttpApiAuth, add_dataset_func, tmp_path):
    @wait_for(200, 1, "Document parsing timeout")
    def condition(_auth, _dataset_id, _document_num):
        res = list_documents(_auth, _dataset_id, {"page_size": _document_num})
        for doc in res["data"]["docs"]:
            if doc["run"] != "DONE":
                return False
        return True

    document_num = 100
    dataset_id = add_dataset_func
    document_ids = bulk_upload_documents(HttpApiAuth, dataset_id, document_num, tmp_path)
    res = parse_documents(HttpApiAuth, dataset_id, {"document_ids": document_ids})
    assert res["code"] == 0

    condition(HttpApiAuth, dataset_id, document_num)

    validate_document_details(HttpApiAuth, dataset_id, document_ids)


@pytest.mark.p3
def test_concurrent_parse(HttpApiAuth, add_dataset_func, tmp_path):
    @wait_for(200, 1, "Document parsing timeout")
    def condition(_auth, _dataset_id, _document_num):
        res = list_documents(_auth, _dataset_id, {"page_size": _document_num})
        for doc in res["data"]["docs"]:
            if doc["run"] != "DONE":
                return False
        return True

    count = 100
    dataset_id = add_dataset_func
    document_ids = bulk_upload_documents(HttpApiAuth, dataset_id, count, tmp_path)

    with ThreadPoolExecutor(max_workers=5) as executor:
        futures = [
            executor.submit(
                parse_documents,
                HttpApiAuth,
                dataset_id,
                {"document_ids": document_ids[i : i + 1]},
            )
            for i in range(count)
        ]
    responses = list(as_completed(futures))
    assert len(responses) == count, responses
    assert all(future.result()["code"] == 0 for future in futures)

    condition(HttpApiAuth, dataset_id, count)

    validate_document_details(HttpApiAuth, dataset_id, document_ids)

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_http_api/test_file_management_within_dataset/test_parse_documents.py` is located in the `test/testcases/test_http_api/test_file_management_within_dataset` directory.

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
- [test_stop_parse_documents.py](test_stop_parse_documents.py_docs.md)
- [test_update_document.py](test_update_document.py_docs.md)
- [test_upload_documents.py](test_upload_documents.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
