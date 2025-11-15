# Documentation: test/testcases/test_http_api/test_file_management_within_dataset/test_delete_documents.py

## File Metadata

- **Path**: `test/testcases/test_http_api/test_file_management_within_dataset/test_delete_documents.py`
- **Size**: 6543 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_http_api/test_file_management_within_dataset/test_delete_documents.py`.

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

#### Class: `TestDocumentsDeletion` (line 43)

**Methods**: test_basic_scenarios, test_invalid_dataset_id, test_delete_partial_invalid_id, test_repeated_deletion, test_duplicate_deletion

### Functions Defined

This file defines 8 function(s):

#### Function: `test_concurrent_deletion` (line 151)

**Parameters**: HttpApiAuth, add_dataset, tmp_path

#### Function: `test_delete_1k` (line 172)

**Parameters**: HttpApiAuth, add_dataset, tmp_path

#### Function: `test_invalid_auth` (line 37)

**Parameters**: self, invalid_auth, expected_code, expected_message

#### Function: `test_basic_scenarios` (line 67)

**Parameters**: self, HttpApiAuth, add_documents_func, payload, expected_code, expected_message, remaining

#### Function: `test_invalid_dataset_id` (line 100)

**Parameters**: self, HttpApiAuth, add_documents_func, dataset_id, expected_code, expected_message

#### Function: `test_delete_partial_invalid_id` (line 115)

**Parameters**: self, HttpApiAuth, add_documents_func, payload

#### Function: `test_repeated_deletion` (line 128)

**Parameters**: self, HttpApiAuth, add_documents_func

#### Function: `test_duplicate_deletion` (line 138)

**Parameters**: self, HttpApiAuth, add_documents_func

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
from common import bulk_upload_documents, delete_documents, list_documents
from configs import INVALID_API_TOKEN
from libs.auth import RAGFlowHttpApiAuth


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
        res = delete_documents(invalid_auth, "dataset_id")
        assert res["code"] == expected_code
        assert res["message"] == expected_message


class TestDocumentsDeletion:
    @pytest.mark.p1
    @pytest.mark.parametrize(
        "payload, expected_code, expected_message, remaining",
        [
            (None, 0, "", 0),
            ({"ids": []}, 0, "", 0),
            ({"ids": ["invalid_id"]}, 102, "Documents not found: ['invalid_id']", 3),
            (
                {"ids": ["\n!?。；！？\"'"]},
                102,
                """Documents not found: [\'\\n!?。；！？"\\\'\']""",
                3,
            ),
            (
                "not json",
                100,
                "AttributeError(\"'str' object has no attribute 'get'\")",
                3,
            ),
            (lambda r: {"ids": r[:1]}, 0, "", 2),
            (lambda r: {"ids": r}, 0, "", 0),
        ],
    )
    def test_basic_scenarios(
        self,
        HttpApiAuth,
        add_documents_func,
        payload,
        expected_code,
        expected_message,
        remaining,
    ):
        dataset_id, document_ids = add_documents_func
        if callable(payload):
            payload = payload(document_ids)
        res = delete_documents(HttpApiAuth, dataset_id, payload)
        assert res["code"] == expected_code
        if res["code"] != 0:
            assert res["message"] == expected_message

        res = list_documents(HttpApiAuth, dataset_id)
        assert len(res["data"]["docs"]) == remaining
        assert res["data"]["total"] == remaining

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
    def test_invalid_dataset_id(self, HttpApiAuth, add_documents_func, dataset_id, expected_code, expected_message):
        _, document_ids = add_documents_func
        res = delete_documents(HttpApiAuth, dataset_id, {"ids": document_ids[:1]})
        assert res["code"] == expected_code
        assert res["message"] == expected_message

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "payload",
        [
            lambda r: {"ids": ["invalid_id"] + r},
            lambda r: {"ids": r[:1] + ["invalid_id"] + r[1:3]},
            lambda r: {"ids": r + ["invalid_id"]},
        ],
    )
    def test_delete_partial_invalid_id(self, HttpApiAuth, add_documents_func, payload):
        dataset_id, document_ids = add_documents_func
        if callable(payload):
            payload = payload(document_ids)
        res = delete_documents(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 102
        assert res["message"] == "Documents not found: ['invalid_id']"

        res = list_documents(HttpApiAuth, dataset_id)
        assert len(res["data"]["docs"]) == 0
        assert res["data"]["total"] == 0

    @pytest.mark.p2
    def test_repeated_deletion(self, HttpApiAuth, add_documents_func):
        dataset_id, document_ids = add_documents_func
        res = delete_documents(HttpApiAuth, dataset_id, {"ids": document_ids})
        assert res["code"] == 0

        res = delete_documents(HttpApiAuth, dataset_id, {"ids": document_ids})
        assert res["code"] == 102
        assert "Documents not found" in res["message"]

    @pytest.mark.p2
    def test_duplicate_deletion(self, HttpApiAuth, add_documents_func):
        dataset_id, document_ids = add_documents_func
        res = delete_documents(HttpApiAuth, dataset_id, {"ids": document_ids + document_ids})
        assert res["code"] == 0
        assert "Duplicate document ids" in res["data"]["errors"][0]
        assert res["data"]["success_count"] == 3

        res = list_documents(HttpApiAuth, dataset_id)
        assert len(res["data"]["docs"]) == 0
        assert res["data"]["total"] == 0


@pytest.mark.p3
def test_concurrent_deletion(HttpApiAuth, add_dataset, tmp_path):
    count = 100
    dataset_id = add_dataset
    document_ids = bulk_upload_documents(HttpApiAuth, dataset_id, count, tmp_path)

    with ThreadPoolExecutor(max_workers=5) as executor:
        futures = [
            executor.submit(
                delete_documents,
                HttpApiAuth,
                dataset_id,
                {"ids": document_ids[i : i + 1]},
            )
            for i in range(count)
        ]
    responses = list(as_completed(futures))
    assert len(responses) == count, responses
    assert all(future.result()["code"] == 0 for future in futures)


@pytest.mark.p3
def test_delete_1k(HttpApiAuth, add_dataset, tmp_path):
    documents_num = 1_000
    dataset_id = add_dataset
    document_ids = bulk_upload_documents(HttpApiAuth, dataset_id, documents_num, tmp_path)
    res = list_documents(HttpApiAuth, dataset_id)
    assert res["data"]["total"] == documents_num

    res = delete_documents(HttpApiAuth, dataset_id, {"ids": document_ids})
    assert res["code"] == 0

    res = list_documents(HttpApiAuth, dataset_id)
    assert res["data"]["total"] == 0

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_http_api/test_file_management_within_dataset/test_delete_documents.py` is located in the `test/testcases/test_http_api/test_file_management_within_dataset` directory.

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
- [test_download_document.py](test_download_document.py_docs.md)
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
