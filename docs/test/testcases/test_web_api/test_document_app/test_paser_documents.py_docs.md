# Documentation: test/testcases/test_web_api/test_document_app/test_paser_documents.py

## File Metadata

- **Path**: `test/testcases/test_web_api/test_document_app/test_paser_documents.py`
- **Size**: 10510 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_web_api/test_document_app/test_paser_documents.py`.

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

This file defines 3 class(es):

#### Class: `TestAuthorization` (line 67)

**Methods**: test_invalid_auth

#### Class: `TestDocumentsParse` (line 81)

**Methods**: test_basic_scenarios, test_parse_partial_invalid_document_id, test_repeated_parse, test_duplicate_parse

#### Class: `TestDocumentsParseStop` (line 198)

**Methods**: test_basic_scenarios, test_stop_parse_partial_invalid_document_id

### Functions Defined

This file defines 15 function(s):

#### Function: `condition` (line 26)

**Parameters**: _auth, _kb_id, _document_ids

#### Function: `validate_document_parse_done` (line 44)

**Parameters**: auth, _kb_id, _document_ids

#### Function: `validate_document_parse_cancel` (line 56)

**Parameters**: auth, _kb_id, _document_ids

#### Function: `test_parse_100_files` (line 145)

**Parameters**: WebApiAuth, add_dataset_func, tmp_path

#### Function: `test_concurrent_parse` (line 166)

**Parameters**: WebApiAuth, add_dataset_func, tmp_path

#### Function: `test_invalid_auth` (line 75)

**Parameters**: self, invalid_auth, expected_code, expected_message

#### Function: `test_basic_scenarios` (line 94)

**Parameters**: self, WebApiAuth, add_documents_func, payload, expected_code, expected_message

#### Function: `test_parse_partial_invalid_document_id` (line 114)

**Parameters**: self, WebApiAuth, add_documents_func, payload

#### Function: `test_repeated_parse` (line 123)

**Parameters**: self, WebApiAuth, add_documents_func

#### Function: `test_duplicate_parse` (line 134)

**Parameters**: self, WebApiAuth, add_documents_func

#### Function: `condition` (line 147)

**Parameters**: _auth, _kb_id, _document_num

#### Function: `condition` (line 168)

**Parameters**: _auth, _kb_id, _document_num

#### Function: `test_basic_scenarios` (line 211)

**Parameters**: self, WebApiAuth, add_documents_func, payload, expected_code, expected_message

#### Function: `test_stop_parse_partial_invalid_document_id` (line 246)

**Parameters**: self, WebApiAuth, add_documents_func, payload

#### Function: `condition` (line 213)

**Parameters**: _auth, _kb_id, _doc_ids

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
from libs.auth import RAGFlowWebApiAuth
from utils import wait_for


@wait_for(30, 1, "Document parsing timeout")
def condition(_auth, _kb_id, _document_ids=None):
    res = list_documents(_auth, {"kb_id": _kb_id})
    target_docs = res["data"]["docs"]

    if _document_ids is None:
        for doc in target_docs:
            if doc["run"] != "3":
                return False
        return True

    target_ids = set(_document_ids)
    for doc in target_docs:
        if doc["id"] in target_ids:
            if doc.get("run") != "3":
                return False
    return True


def validate_document_parse_done(auth, _kb_id, _document_ids):
    res = list_documents(auth, {"kb_id": _kb_id})
    for doc in res["data"]["docs"]:
        if doc["id"] not in _document_ids:
            continue
        assert doc["run"] == "3"
        assert len(doc["process_begin_at"]) > 0
        assert doc["process_duration"] > 0
        assert doc["progress"] > 0
        assert "Task done" in doc["progress_msg"]


def validate_document_parse_cancel(auth, _kb_id, _document_ids):
    res = list_documents(auth, {"kb_id": _kb_id})
    for doc in res["data"]["docs"]:
        if doc["id"] not in _document_ids:
            continue
        assert doc["run"] == "2"
        assert len(doc["process_begin_at"]) > 0
        assert doc["progress"] == 0.0


@pytest.mark.p1
class TestAuthorization:
    @pytest.mark.parametrize(
        "invalid_auth, expected_code, expected_message",
        [
            (None, 401, "<Unauthorized '401: Unauthorized'>"),
            (RAGFlowWebApiAuth(INVALID_API_TOKEN), 401, "<Unauthorized '401: Unauthorized'>"),
        ],
    )
    def test_invalid_auth(self, invalid_auth, expected_code, expected_message):
        res = parse_documents(invalid_auth)
        assert res["code"] == expected_code, res
        assert res["message"] == expected_message, res


class TestDocumentsParse:
    @pytest.mark.parametrize(
        "payload, expected_code, expected_message",
        [
            pytest.param(None, 101, "required argument are missing: doc_ids, run; ", marks=pytest.mark.skip),
            pytest.param({"doc_ids": [], "run": "1"}, 0, "", marks=pytest.mark.p1),
            pytest.param({"doc_ids": ["invalid_id"], "run": "1"}, 109, "No authorization.", marks=pytest.mark.p3),
            pytest.param({"doc_ids": ["\n!?。；！？\"'"], "run": "1"}, 109, "No authorization.", marks=pytest.mark.p3),
            pytest.param("not json", 101, "required argument are missing: doc_ids, run; ", marks=pytest.mark.skip),
            pytest.param(lambda r: {"doc_ids": r[:1], "run": "1"}, 0, "", marks=pytest.mark.p1),
            pytest.param(lambda r: {"doc_ids": r, "run": "1"}, 0, "", marks=pytest.mark.p1),
        ],
    )
    def test_basic_scenarios(self, WebApiAuth, add_documents_func, payload, expected_code, expected_message):
        kb_id, document_ids = add_documents_func
        if callable(payload):
            payload = payload(document_ids)
        res = parse_documents(WebApiAuth, payload)
        assert res["code"] == expected_code, res
        if expected_code == 0:
            condition(WebApiAuth, kb_id, payload["doc_ids"])
            validate_document_parse_done(WebApiAuth, kb_id, payload["doc_ids"])
        else:
            assert res["message"] == expected_message, res

    @pytest.mark.parametrize(
        "payload",
        [
            pytest.param(lambda r: {"doc_ids": ["invalid_id"] + r, "run": "1"}, marks=pytest.mark.p3),
            pytest.param(lambda r: {"doc_ids": r[:1] + ["invalid_id"] + r[1:3], "run": "1"}, marks=pytest.mark.p1),
            pytest.param(lambda r: {"doc_ids": r + ["invalid_id"], "run": "1"}, marks=pytest.mark.p3),
        ],
    )
    def test_parse_partial_invalid_document_id(self, WebApiAuth, add_documents_func, payload):
        _, document_ids = add_documents_func
        if callable(payload):
            payload = payload(document_ids)
        res = parse_documents(WebApiAuth, payload)
        assert res["code"] == 109, res
        assert res["message"] == "No authorization.", res

    @pytest.mark.p3
    def test_repeated_parse(self, WebApiAuth, add_documents_func):
        kb_id, document_ids = add_documents_func
        res = parse_documents(WebApiAuth, {"doc_ids": document_ids, "run": "1"})
        assert res["code"] == 0, res

        condition(WebApiAuth, kb_id, document_ids)

        res = parse_documents(WebApiAuth, {"doc_ids": document_ids, "run": "1"})
        assert res["code"] == 0, res

    @pytest.mark.p3
    def test_duplicate_parse(self, WebApiAuth, add_documents_func):
        kb_id, document_ids = add_documents_func
        res = parse_documents(WebApiAuth, {"doc_ids": document_ids + document_ids, "run": "1"})
        assert res["code"] == 0, res
        assert res["message"] == "success", res

        condition(WebApiAuth, kb_id, document_ids)
        validate_document_parse_done(WebApiAuth, kb_id, document_ids)


@pytest.mark.p3
def test_parse_100_files(WebApiAuth, add_dataset_func, tmp_path):
    @wait_for(100, 1, "Document parsing timeout")
    def condition(_auth, _kb_id, _document_num):
        res = list_documents(_auth, {"kb_id": _kb_id, "page_size": _document_num})
        for doc in res["data"]["docs"]:
            if doc["run"] != "3":
                return False
        return True

    document_num = 100
    kb_id = add_dataset_func
    document_ids = bulk_upload_documents(WebApiAuth, kb_id, document_num, tmp_path)
    res = parse_documents(WebApiAuth, {"doc_ids": document_ids, "run": "1"})
    assert res["code"] == 0, res

    condition(WebApiAuth, kb_id, document_num)

    validate_document_parse_done(WebApiAuth, kb_id, document_ids)


@pytest.mark.p3
def test_concurrent_parse(WebApiAuth, add_dataset_func, tmp_path):
    @wait_for(120, 1, "Document parsing timeout")
    def condition(_auth, _kb_id, _document_num):
        res = list_documents(_auth, {"kb_id": _kb_id, "page_size": _document_num})
        for doc in res["data"]["docs"]:
            if doc["run"] != "3":
                return False
        return True

    count = 100
    kb_id = add_dataset_func
    document_ids = bulk_upload_documents(WebApiAuth, kb_id, count, tmp_path)

    with ThreadPoolExecutor(max_workers=5) as executor:
        futures = [
            executor.submit(
                parse_documents,
                WebApiAuth,
                {"doc_ids": [document_ids[i]], "run": "1"},
            )
            for i in range(count)
        ]
    responses = list(as_completed(futures))
    assert len(responses) == count, responses
    assert all(future.result()["code"] == 0 for future in futures)

    condition(WebApiAuth, kb_id, count)

    validate_document_parse_done(WebApiAuth, kb_id, document_ids)


# @pytest.mark.skip
class TestDocumentsParseStop:
    @pytest.mark.parametrize(
        "payload, expected_code, expected_message",
        [
            pytest.param(None, 101, "required argument are missing: doc_ids, run; ", marks=pytest.mark.skip),
            pytest.param({"doc_ids": [], "run": "2"}, 0, "", marks=pytest.mark.p1),
            pytest.param({"doc_ids": ["invalid_id"], "run": "2"}, 109, "No authorization.", marks=pytest.mark.p3),
            pytest.param({"doc_ids": ["\n!?。；！？\"'"], "run": "2"}, 109, "No authorization.", marks=pytest.mark.p3),
            pytest.param("not json", 101, "required argument are missing: doc_ids, run; ", marks=pytest.mark.skip),
            pytest.param(lambda r: {"doc_ids": r[:1], "run": "2"}, 0, "", marks=pytest.mark.p1),
            pytest.param(lambda r: {"doc_ids": r, "run": "2"}, 0, "", marks=pytest.mark.p1),
        ],
    )
    def test_basic_scenarios(self, WebApiAuth, add_documents_func, payload, expected_code, expected_message):
        @wait_for(10, 1, "Document parsing timeout")
        def condition(_auth, _kb_id, _doc_ids):
            res = list_documents(_auth, {"kb_id": _kb_id})
            for doc in res["data"]["docs"]:
                if doc["id"] in _doc_ids:
                    if doc["run"] != "3":
                        return False
            return True

        kb_id, document_ids = add_documents_func
        parse_documents(WebApiAuth, {"doc_ids": document_ids, "run": "1"})

        if callable(payload):
            payload = payload(document_ids)

        res = parse_documents(WebApiAuth, payload)
        assert res["code"] == expected_code, res
        if expected_code == 0:
            completed_document_ids = list(set(document_ids) - set(payload["doc_ids"]))
            condition(WebApiAuth, kb_id, completed_document_ids)
            validate_document_parse_cancel(WebApiAuth, kb_id, payload["doc_ids"])
            validate_document_parse_done(WebApiAuth, kb_id, completed_document_ids)
        else:
            assert res["message"] == expected_message, res

    @pytest.mark.skip
    @pytest.mark.parametrize(
        "payload",
        [
            lambda r: {"doc_ids": ["invalid_id"] + r, "run": "2"},
            lambda r: {"doc_ids": r[:1] + ["invalid_id"] + r[1:3], "run": "2"},
            lambda r: {"doc_ids": r + ["invalid_id"], "run": "2"},
        ],
    )
    def test_stop_parse_partial_invalid_document_id(self, WebApiAuth, add_documents_func, payload):
        kb_id, document_ids = add_documents_func
        parse_documents(WebApiAuth, {"doc_ids": document_ids, "run": "1"})

        if callable(payload):
            payload = payload(document_ids)
        res = parse_documents(WebApiAuth, payload)
        assert res["code"] == 109, res
        assert res["message"] == "No authorization.", res

        validate_document_parse_cancel(WebApiAuth, kb_id, document_ids)

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_web_api/test_document_app/test_paser_documents.py` is located in the `test/testcases/test_web_api/test_document_app` directory.

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
- [test_rm_documents.py](test_rm_documents.py_docs.md)
- [test_upload_documents.py](test_upload_documents.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
