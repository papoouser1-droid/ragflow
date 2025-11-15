# Documentation: sdk/python/test/test_http_api/test_chunk_management_within_dataset/test_delete_chunks.py

## File Metadata

- **Path**: `sdk/python/test/test_http_api/test_chunk_management_within_dataset/test_delete_chunks.py`
- **Size**: 7949 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/test/test_http_api/test_chunk_management_within_dataset/test_delete_chunks.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `concurrent.futures`
- `pytest`
- `common`
- `libs.auth`
- `time`

### Classes Defined

This file defines 2 class(es):

#### Class: `TestAuthorization` (line 24)

**Methods**: test_invalid_auth

#### Class: `TestChunksDeletion` (line 42)

**Methods**: test_invalid_dataset_id, test_invalid_document_id, test_delete_partial_invalid_id, test_repeated_deletion, test_duplicate_deletion, test_concurrent_deletion, test_delete_1k, test_basic_scenarios

### Functions Defined

This file defines 9 function(s):

#### Function: `test_invalid_auth` (line 36)

**Parameters**: self, auth, expected_code, expected_message

#### Function: `test_invalid_dataset_id` (line 55)

**Parameters**: self, get_http_api_auth, add_chunks_func, dataset_id, expected_code, expected_message

#### Function: `test_invalid_document_id` (line 69)

**Parameters**: self, get_http_api_auth, add_chunks_func, document_id, expected_code, expected_message

#### Function: `test_delete_partial_invalid_id` (line 83)

**Parameters**: self, get_http_api_auth, add_chunks_func, payload

#### Function: `test_repeated_deletion` (line 98)

**Parameters**: self, get_http_api_auth, add_chunks_func

#### Function: `test_duplicate_deletion` (line 109)

**Parameters**: self, get_http_api_auth, add_chunks_func

#### Function: `test_concurrent_deletion` (line 123)

**Parameters**: self, get_http_api_auth, add_document

#### Function: `test_delete_1k` (line 143)

**Parameters**: self, get_http_api_auth, add_document

#### Function: `test_basic_scenarios` (line 173)

**Parameters**: self, get_http_api_auth, add_chunks_func, payload, expected_code, expected_message, remaining

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
from common import INVALID_API_TOKEN, batch_add_chunks, delete_chunks, list_chunks
from libs.auth import RAGFlowHttpApiAuth


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
        res = delete_chunks(auth, "dataset_id", "document_id")
        assert res["code"] == expected_code
        assert res["message"] == expected_message


class TestChunksDeletion:
    @pytest.mark.p3
    @pytest.mark.parametrize(
        "dataset_id, expected_code, expected_message",
        [
            ("", 100, "<NotFound '404: Not Found'>"),
            (
                "invalid_dataset_id",
                102,
                "You don't own the dataset invalid_dataset_id.",
            ),
        ],
    )
    def test_invalid_dataset_id(self, get_http_api_auth, add_chunks_func, dataset_id, expected_code, expected_message):
        _, document_id, chunk_ids = add_chunks_func
        res = delete_chunks(get_http_api_auth, dataset_id, document_id, {"chunk_ids": chunk_ids})
        assert res["code"] == expected_code
        assert res["message"] == expected_message

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "document_id, expected_code, expected_message",
        [
            ("", 100, "<MethodNotAllowed '405: Method Not Allowed'>"),
            ("invalid_document_id", 100, """LookupError("Can't find the document with ID invalid_document_id!")"""),
        ],
    )
    def test_invalid_document_id(self, get_http_api_auth, add_chunks_func, document_id, expected_code, expected_message):
        dataset_id, _, chunk_ids = add_chunks_func
        res = delete_chunks(get_http_api_auth, dataset_id, document_id, {"chunk_ids": chunk_ids})
        assert res["code"] == expected_code
        assert res["message"] == expected_message

    @pytest.mark.parametrize(
        "payload",
        [
            pytest.param(lambda r: {"chunk_ids": ["invalid_id"] + r}, marks=pytest.mark.p3),
            pytest.param(lambda r: {"chunk_ids": r[:1] + ["invalid_id"] + r[1:4]}, marks=pytest.mark.p1),
            pytest.param(lambda r: {"chunk_ids": r + ["invalid_id"]}, marks=pytest.mark.p3),
        ],
    )
    def test_delete_partial_invalid_id(self, get_http_api_auth, add_chunks_func, payload):
        dataset_id, document_id, chunk_ids = add_chunks_func
        if callable(payload):
            payload = payload(chunk_ids)
        res = delete_chunks(get_http_api_auth, dataset_id, document_id, payload)
        assert res["code"] == 102
        assert res["message"] == "rm_chunk deleted chunks 4, expect 5"

        res = list_chunks(get_http_api_auth, dataset_id, document_id)
        if res["code"] != 0:
            assert False, res
        assert len(res["data"]["chunks"]) == 1
        assert res["data"]["total"] == 1

    @pytest.mark.p3
    def test_repeated_deletion(self, get_http_api_auth, add_chunks_func):
        dataset_id, document_id, chunk_ids = add_chunks_func
        payload = {"chunk_ids": chunk_ids}
        res = delete_chunks(get_http_api_auth, dataset_id, document_id, payload)
        assert res["code"] == 0

        res = delete_chunks(get_http_api_auth, dataset_id, document_id, payload)
        assert res["code"] == 102
        assert res["message"] == "rm_chunk deleted chunks 0, expect 4"

    @pytest.mark.p3
    def test_duplicate_deletion(self, get_http_api_auth, add_chunks_func):
        dataset_id, document_id, chunk_ids = add_chunks_func
        res = delete_chunks(get_http_api_auth, dataset_id, document_id, {"chunk_ids": chunk_ids * 2})
        assert res["code"] == 0
        assert "Duplicate chunk ids" in res["data"]["errors"][0]
        assert res["data"]["success_count"] == 4

        res = list_chunks(get_http_api_auth, dataset_id, document_id)
        if res["code"] != 0:
            assert False, res
        assert len(res["data"]["chunks"]) == 1
        assert res["data"]["total"] == 1

    @pytest.mark.p3
    def test_concurrent_deletion(self, get_http_api_auth, add_document):
        chunks_num = 100
        dataset_id, document_id = add_document
        chunk_ids = batch_add_chunks(get_http_api_auth, dataset_id, document_id, chunks_num)

        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [
                executor.submit(
                    delete_chunks,
                    get_http_api_auth,
                    dataset_id,
                    document_id,
                    {"chunk_ids": chunk_ids[i : i + 1]},
                )
                for i in range(chunks_num)
            ]
        responses = [f.result() for f in futures]
        assert all(r["code"] == 0 for r in responses)

    @pytest.mark.p3
    def test_delete_1k(self, get_http_api_auth, add_document):
        chunks_num = 1_000
        dataset_id, document_id = add_document
        chunk_ids = batch_add_chunks(get_http_api_auth, dataset_id, document_id, chunks_num)

        # issues/6487
        from time import sleep

        sleep(1)

        res = delete_chunks(get_http_api_auth, dataset_id, document_id, {"chunk_ids": chunk_ids})
        assert res["code"] == 0

        res = list_chunks(get_http_api_auth, dataset_id, document_id)
        if res["code"] != 0:
            assert False, res
        assert len(res["data"]["chunks"]) == 1
        assert res["data"]["total"] == 1

    @pytest.mark.parametrize(
        "payload, expected_code, expected_message, remaining",
        [
            pytest.param(None, 100, """TypeError("argument of type \'NoneType\' is not iterable")""", 5, marks=pytest.mark.skip),
            pytest.param({"chunk_ids": ["invalid_id"]}, 102, "rm_chunk deleted chunks 0, expect 1", 5, marks=pytest.mark.p3),
            pytest.param("not json", 100, """UnboundLocalError("local variable \'duplicate_messages\' referenced before assignment")""", 5, marks=pytest.mark.skip(reason="pull/6376")),
            pytest.param(lambda r: {"chunk_ids": r[:1]}, 0, "", 4, marks=pytest.mark.p3),
            pytest.param(lambda r: {"chunk_ids": r}, 0, "", 1, marks=pytest.mark.p1),
            pytest.param({"chunk_ids": []}, 0, "", 0, marks=pytest.mark.p3),
        ],
    )
    def test_basic_scenarios(
        self,
        get_http_api_auth,
        add_chunks_func,
        payload,
        expected_code,
        expected_message,
        remaining,
    ):
        dataset_id, document_id, chunk_ids = add_chunks_func
        if callable(payload):
            payload = payload(chunk_ids)
        res = delete_chunks(get_http_api_auth, dataset_id, document_id, payload)
        assert res["code"] == expected_code
        if res["code"] != 0:
            assert res["message"] == expected_message

        res = list_chunks(get_http_api_auth, dataset_id, document_id)
        if res["code"] != 0:
            assert False, res
        assert len(res["data"]["chunks"]) == remaining
        assert res["data"]["total"] == remaining

```

## Detailed Analysis

### File Role in Repository

The file `sdk/python/test/test_http_api/test_chunk_management_within_dataset/test_delete_chunks.py` is located in the `sdk/python/test/test_http_api/test_chunk_management_within_dataset` directory.

This file is part of the **Testing** infrastructure.

### Architecture Context

Files in this location typically handle concerns related to test_chunk_management_within_dataset.

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
- [test_add_chunk.py](test_add_chunk.py_docs.md)
- [test_list_chunks.py](test_list_chunks.py_docs.md)
- [test_retrieval_chunks.py](test_retrieval_chunks.py_docs.md)
- [test_update_chunk.py](test_update_chunk.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
