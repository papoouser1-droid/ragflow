# Documentation: sdk/python/test/test_http_api/test_chunk_management_within_dataset/test_update_chunk.py

## File Metadata

- **Path**: `sdk/python/test/test_http_api/test_chunk_management_within_dataset/test_update_chunk.py`
- **Size**: 10602 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/test/test_http_api/test_chunk_management_within_dataset/test_update_chunk.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `os`
- `concurrent.futures`
- `random`
- `pytest`
- `common`
- `libs.auth`

### Classes Defined

This file defines 2 class(es):

#### Class: `TestAuthorization` (line 26)

**Methods**: test_invalid_auth

#### Class: `TestUpdatedChunk` (line 44)

**Methods**: test_content, test_important_keywords, test_questions, test_available, test_invalid_dataset_id, test_invalid_document_id, test_invalid_chunk_id, test_repeated_update_chunk, test_invalid_params, test_concurrent_update_chunk, test_update_chunk_to_deleted_document

### Functions Defined

This file defines 12 function(s):

#### Function: `test_invalid_auth` (line 38)

**Parameters**: self, auth, expected_code, expected_message

#### Function: `test_content` (line 72)

**Parameters**: self, get_http_api_auth, add_chunks, payload, expected_code, expected_message

#### Function: `test_important_keywords` (line 91)

**Parameters**: self, get_http_api_auth, add_chunks, payload, expected_code, expected_message

#### Function: `test_questions` (line 110)

**Parameters**: self, get_http_api_auth, add_chunks, payload, expected_code, expected_message

#### Function: `test_available` (line 129)

**Parameters**: self, get_http_api_auth, add_chunks, payload, expected_code, expected_message

#### Function: `test_invalid_dataset_id` (line 152)

**Parameters**: self, get_http_api_auth, add_chunks, dataset_id, expected_code, expected_message

#### Function: `test_invalid_document_id` (line 170)

**Parameters**: self, get_http_api_auth, add_chunks, document_id, expected_code, expected_message

#### Function: `test_invalid_chunk_id` (line 188)

**Parameters**: self, get_http_api_auth, add_chunks, chunk_id, expected_code, expected_message

#### Function: `test_repeated_update_chunk` (line 195)

**Parameters**: self, get_http_api_auth, add_chunks

#### Function: `test_invalid_params` (line 212)

**Parameters**: self, get_http_api_auth, add_chunks, payload, expected_code, expected_message

#### Function: `test_concurrent_update_chunk` (line 221)

**Parameters**: self, get_http_api_auth, add_chunks

#### Function: `test_update_chunk_to_deleted_document` (line 241)

**Parameters**: self, get_http_api_auth, add_chunks

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
import os
from concurrent.futures import ThreadPoolExecutor
from random import randint

import pytest
from common import INVALID_API_TOKEN, delete_documnets, update_chunk
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
        res = update_chunk(auth, "dataset_id", "document_id", "chunk_id")
        assert res["code"] == expected_code
        assert res["message"] == expected_message


class TestUpdatedChunk:
    @pytest.mark.p1
    @pytest.mark.parametrize(
        "payload, expected_code, expected_message",
        [
            ({"content": None}, 100, "TypeError('expected string or bytes-like object')"),
            pytest.param(
                {"content": ""},
                100,
                """APIRequestFailedError(\'Error code: 400, with error text {"error":{"code":"1213","message":"未正常接收到prompt参数。"}}\')""",
                marks=pytest.mark.skip(reason="issues/6541"),
            ),
            pytest.param(
                {"content": 1},
                100,
                "TypeError('expected string or bytes-like object')",
                marks=pytest.mark.skip,
            ),
            ({"content": "update chunk"}, 0, ""),
            pytest.param(
                {"content": " "},
                100,
                """APIRequestFailedError(\'Error code: 400, with error text {"error":{"code":"1213","message":"未正常接收到prompt参数。"}}\')""",
                marks=pytest.mark.skip(reason="issues/6541"),
            ),
            ({"content": "\n!?。；！？\"'"}, 0, ""),
        ],
    )
    def test_content(self, get_http_api_auth, add_chunks, payload, expected_code, expected_message):
        dataset_id, document_id, chunk_ids = add_chunks
        res = update_chunk(get_http_api_auth, dataset_id, document_id, chunk_ids[0], payload)
        assert res["code"] == expected_code
        if expected_code != 0:
            assert res["message"] == expected_message

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "payload, expected_code, expected_message",
        [
            ({"important_keywords": ["a", "b", "c"]}, 0, ""),
            ({"important_keywords": [""]}, 0, ""),
            ({"important_keywords": [1]}, 100, "TypeError('sequence item 0: expected str instance, int found')"),
            ({"important_keywords": ["a", "a"]}, 0, ""),
            ({"important_keywords": "abc"}, 102, "`important_keywords` should be a list"),
            ({"important_keywords": 123}, 102, "`important_keywords` should be a list"),
        ],
    )
    def test_important_keywords(self, get_http_api_auth, add_chunks, payload, expected_code, expected_message):
        dataset_id, document_id, chunk_ids = add_chunks
        res = update_chunk(get_http_api_auth, dataset_id, document_id, chunk_ids[0], payload)
        assert res["code"] == expected_code
        if expected_code != 0:
            assert res["message"] == expected_message

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "payload, expected_code, expected_message",
        [
            ({"questions": ["a", "b", "c"]}, 0, ""),
            ({"questions": [""]}, 0, ""),
            ({"questions": [1]}, 100, "TypeError('sequence item 0: expected str instance, int found')"),
            ({"questions": ["a", "a"]}, 0, ""),
            ({"questions": "abc"}, 102, "`questions` should be a list"),
            ({"questions": 123}, 102, "`questions` should be a list"),
        ],
    )
    def test_questions(self, get_http_api_auth, add_chunks, payload, expected_code, expected_message):
        dataset_id, document_id, chunk_ids = add_chunks
        res = update_chunk(get_http_api_auth, dataset_id, document_id, chunk_ids[0], payload)
        assert res["code"] == expected_code
        if expected_code != 0:
            assert res["message"] == expected_message

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "payload, expected_code, expected_message",
        [
            ({"available": True}, 0, ""),
            pytest.param({"available": "True"}, 100, """ValueError("invalid literal for int() with base 10: \'True\'")""", marks=pytest.mark.skip),
            ({"available": 1}, 0, ""),
            ({"available": False}, 0, ""),
            pytest.param({"available": "False"}, 100, """ValueError("invalid literal for int() with base 10: \'False\'")""", marks=pytest.mark.skip),
            ({"available": 0}, 0, ""),
        ],
    )
    def test_available(
        self,
        get_http_api_auth,
        add_chunks,
        payload,
        expected_code,
        expected_message,
    ):
        dataset_id, document_id, chunk_ids = add_chunks
        res = update_chunk(get_http_api_auth, dataset_id, document_id, chunk_ids[0], payload)
        assert res["code"] == expected_code
        if expected_code != 0:
            assert res["message"] == expected_message

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "dataset_id, expected_code, expected_message",
        [
            ("", 100, "<NotFound '404: Not Found'>"),
            pytest.param("invalid_dataset_id", 102, "You don't own the dataset invalid_dataset_id.", marks=pytest.mark.skipif(os.getenv("DOC_ENGINE") == "infinity", reason="infinity")),
            pytest.param("invalid_dataset_id", 102, "Can't find this chunk", marks=pytest.mark.skipif(os.getenv("DOC_ENGINE") in [None, "opensearch","elasticsearch"], reason="elasticsearch")),
        ],
    )
    def test_invalid_dataset_id(self, get_http_api_auth, add_chunks, dataset_id, expected_code, expected_message):
        _, document_id, chunk_ids = add_chunks
        res = update_chunk(get_http_api_auth, dataset_id, document_id, chunk_ids[0])
        assert res["code"] == expected_code
        assert expected_message in res["message"]

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "document_id, expected_code, expected_message",
        [
            ("", 100, "<NotFound '404: Not Found'>"),
            (
                "invalid_document_id",
                102,
                "You don't own the document invalid_document_id.",
            ),
        ],
    )
    def test_invalid_document_id(self, get_http_api_auth, add_chunks, document_id, expected_code, expected_message):
        dataset_id, _, chunk_ids = add_chunks
        res = update_chunk(get_http_api_auth, dataset_id, document_id, chunk_ids[0])
        assert res["code"] == expected_code
        assert res["message"] == expected_message

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "chunk_id, expected_code, expected_message",
        [
            ("", 100, "<MethodNotAllowed '405: Method Not Allowed'>"),
            (
                "invalid_document_id",
                102,
                "Can't find this chunk invalid_document_id",
            ),
        ],
    )
    def test_invalid_chunk_id(self, get_http_api_auth, add_chunks, chunk_id, expected_code, expected_message):
        dataset_id, document_id, _ = add_chunks
        res = update_chunk(get_http_api_auth, dataset_id, document_id, chunk_id)
        assert res["code"] == expected_code
        assert res["message"] == expected_message

    @pytest.mark.p3
    def test_repeated_update_chunk(self, get_http_api_auth, add_chunks):
        dataset_id, document_id, chunk_ids = add_chunks
        res = update_chunk(get_http_api_auth, dataset_id, document_id, chunk_ids[0], {"content": "chunk test 1"})
        assert res["code"] == 0

        res = update_chunk(get_http_api_auth, dataset_id, document_id, chunk_ids[0], {"content": "chunk test 2"})
        assert res["code"] == 0

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "payload, expected_code, expected_message",
        [
            ({"unknown_key": "unknown_value"}, 0, ""),
            ({}, 0, ""),
            pytest.param(None, 100, """TypeError("argument of type \'NoneType\' is not iterable")""", marks=pytest.mark.skip),
        ],
    )
    def test_invalid_params(self, get_http_api_auth, add_chunks, payload, expected_code, expected_message):
        dataset_id, document_id, chunk_ids = add_chunks
        res = update_chunk(get_http_api_auth, dataset_id, document_id, chunk_ids[0], payload)
        assert res["code"] == expected_code
        if expected_code != 0:
            assert res["message"] == expected_message

    @pytest.mark.p3
    @pytest.mark.skipif(os.getenv("DOC_ENGINE") == "infinity", reason="issues/6554")
    def test_concurrent_update_chunk(self, get_http_api_auth, add_chunks):
        chunk_num = 50
        dataset_id, document_id, chunk_ids = add_chunks

        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [
                executor.submit(
                    update_chunk,
                    get_http_api_auth,
                    dataset_id,
                    document_id,
                    chunk_ids[randint(0, 3)],
                    {"content": f"update chunk test {i}"},
                )
                for i in range(chunk_num)
            ]
        responses = [f.result() for f in futures]
        assert all(r["code"] == 0 for r in responses)

    @pytest.mark.p3
    def test_update_chunk_to_deleted_document(self, get_http_api_auth, add_chunks):
        dataset_id, document_id, chunk_ids = add_chunks
        delete_documnets(get_http_api_auth, dataset_id, {"ids": [document_id]})
        res = update_chunk(get_http_api_auth, dataset_id, document_id, chunk_ids[0])
        assert res["code"] == 102
        assert res["message"] == f"Can't find this chunk {chunk_ids[0]}"

```

## Detailed Analysis

### File Role in Repository

The file `sdk/python/test/test_http_api/test_chunk_management_within_dataset/test_update_chunk.py` is located in the `sdk/python/test/test_http_api/test_chunk_management_within_dataset` directory.

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
- [test_delete_chunks.py](test_delete_chunks.py_docs.md)
- [test_list_chunks.py](test_list_chunks.py_docs.md)
- [test_retrieval_chunks.py](test_retrieval_chunks.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
