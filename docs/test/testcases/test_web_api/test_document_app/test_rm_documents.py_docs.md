# Documentation: test/testcases/test_web_api/test_document_app/test_rm_documents.py

## File Metadata

- **Path**: `test/testcases/test_web_api/test_document_app/test_rm_documents.py`
- **Size**: 4151 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_web_api/test_document_app/test_rm_documents.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `concurrent.futures`
- `pytest`
- `common`
- `configs`
- `libs.auth`

### Classes Defined

This file defines 2 class(es):

#### Class: `TestAuthorization` (line 25)

**Methods**: test_invalid_auth

#### Class: `TestDocumentsDeletion` (line 39)

**Methods**: test_basic_scenarios, test_repeated_deletion

### Functions Defined

This file defines 5 function(s):

#### Function: `test_concurrent_deletion` (line 79)

**Parameters**: WebApiAuth, add_dataset, tmp_path

#### Function: `test_delete_100` (line 92)

**Parameters**: WebApiAuth, add_dataset, tmp_path

#### Function: `test_invalid_auth` (line 33)

**Parameters**: self, invalid_auth, expected_code, expected_message

#### Function: `test_basic_scenarios` (line 52)

**Parameters**: self, WebApiAuth, add_documents_func, payload, expected_code, expected_message, remaining

#### Function: `test_repeated_deletion` (line 66)

**Parameters**: self, WebApiAuth, add_documents_func

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
from common import bulk_upload_documents, delete_document, list_documents
from configs import INVALID_API_TOKEN
from libs.auth import RAGFlowWebApiAuth


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
        res = delete_document(invalid_auth)
        assert res["code"] == expected_code, res
        assert res["message"] == expected_message, res


class TestDocumentsDeletion:
    @pytest.mark.p1
    @pytest.mark.parametrize(
        "payload, expected_code, expected_message, remaining",
        [
            (None, 101, "required argument are missing: doc_id; ", 3),
            ({"doc_id": ""}, 109, "No authorization.", 3),
            ({"doc_id": "invalid_id"}, 109, "No authorization.", 3),
            ({"doc_id": "\n!?。；！？\"'"}, 109, "No authorization.", 3),
            ("not json", 101, "required argument are missing: doc_id; ", 3),
            (lambda r: {"doc_id": r[0]}, 0, "", 2),
        ],
    )
    def test_basic_scenarios(self, WebApiAuth, add_documents_func, payload, expected_code, expected_message, remaining):
        kb_id, document_ids = add_documents_func
        if callable(payload):
            payload = payload(document_ids)
        res = delete_document(WebApiAuth, payload)
        assert res["code"] == expected_code, res
        if res["code"] != 0:
            assert res["message"] == expected_message, res

        res = list_documents(WebApiAuth, {"kb_id": kb_id})
        assert len(res["data"]["docs"]) == remaining, res
        assert res["data"]["total"] == remaining, res

    @pytest.mark.p2
    def test_repeated_deletion(self, WebApiAuth, add_documents_func):
        _, document_ids = add_documents_func
        for doc_id in document_ids:
            res = delete_document(WebApiAuth, {"doc_id": doc_id})
            assert res["code"] == 0, res

        for doc_id in document_ids:
            res = delete_document(WebApiAuth, {"doc_id": doc_id})
            assert res["code"] == 109, res
            assert res["message"] == "No authorization.", res


@pytest.mark.p3
def test_concurrent_deletion(WebApiAuth, add_dataset, tmp_path):
    count = 100
    kb_id = add_dataset
    document_ids = bulk_upload_documents(WebApiAuth, kb_id, count, tmp_path)

    with ThreadPoolExecutor(max_workers=5) as executor:
        futures = [executor.submit(delete_document, WebApiAuth, {"doc_id": document_ids[i]}) for i in range(count)]
    responses = list(as_completed(futures))
    assert len(responses) == count, responses
    assert all(future.result()["code"] == 0 for future in futures), responses


@pytest.mark.p3
def test_delete_100(WebApiAuth, add_dataset, tmp_path):
    documents_num = 100
    kb_id = add_dataset
    document_ids = bulk_upload_documents(WebApiAuth, kb_id, documents_num, tmp_path)
    res = list_documents(WebApiAuth, {"kb_id": kb_id})
    assert res["data"]["total"] == documents_num, res

    for doc_id in document_ids:
        res = delete_document(WebApiAuth, {"doc_id": doc_id})
        assert res["code"] == 0, res

    res = list_documents(WebApiAuth, {"kb_id": kb_id})
    assert res["data"]["total"] == 0, res

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_web_api/test_document_app/test_rm_documents.py` is located in the `test/testcases/test_web_api/test_document_app` directory.

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
- [test_upload_documents.py](test_upload_documents.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
