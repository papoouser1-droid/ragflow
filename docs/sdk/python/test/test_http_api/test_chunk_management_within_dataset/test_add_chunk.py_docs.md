# Documentation: sdk/python/test/test_http_api/test_chunk_management_within_dataset/test_add_chunk.py

## File Metadata

- **Path**: `sdk/python/test/test_http_api/test_chunk_management_within_dataset/test_add_chunk.py`
- **Size**: 10737 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/test/test_http_api/test_chunk_management_within_dataset/test_add_chunk.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `concurrent.futures`
- `pytest`
- `common`
- `libs.auth`

### Classes Defined

This file defines 2 class(es):

#### Class: `TestAuthorization` (line 35)

**Methods**: test_invalid_auth

#### Class: `TestAddChunk` (line 53)

**Methods**: test_content, test_important_keywords, test_questions, test_invalid_dataset_id, test_invalid_document_id, test_repeated_add_chunk, test_add_chunk_to_deleted_document, test_concurrent_add_chunk

### Functions Defined

This file defines 10 function(s):

#### Function: `validate_chunk_details` (line 23)

**Parameters**: dataset_id, document_id, payload, res

#### Function: `test_invalid_auth` (line 47)

**Parameters**: self, auth, expected_code, expected_message

#### Function: `test_content` (line 71)

**Parameters**: self, get_http_api_auth, add_document, payload, expected_code, expected_message

#### Function: `test_important_keywords` (line 104)

**Parameters**: self, get_http_api_auth, add_document, payload, expected_code, expected_message

#### Function: `test_questions` (line 133)

**Parameters**: self, get_http_api_auth, add_document, payload, expected_code, expected_message

#### Function: `test_invalid_dataset_id` (line 162)

**Parameters**: self, get_http_api_auth, add_document, dataset_id, expected_code, expected_message

#### Function: `test_invalid_document_id` (line 187)

**Parameters**: self, get_http_api_auth, add_document, document_id, expected_code, expected_message

#### Function: `test_repeated_add_chunk` (line 194)

**Parameters**: self, get_http_api_auth, add_document

#### Function: `test_add_chunk_to_deleted_document` (line 218)

**Parameters**: self, get_http_api_auth, add_document

#### Function: `test_concurrent_add_chunk` (line 226)

**Parameters**: self, get_http_api_auth, add_document

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
from common import INVALID_API_TOKEN, add_chunk, delete_documnets, list_chunks
from libs.auth import RAGFlowHttpApiAuth


def validate_chunk_details(dataset_id, document_id, payload, res):
    chunk = res["data"]["chunk"]
    assert chunk["dataset_id"] == dataset_id
    assert chunk["document_id"] == document_id
    assert chunk["content"] == payload["content"]
    if "important_keywords" in payload:
        assert chunk["important_keywords"] == payload["important_keywords"]
    if "questions" in payload:
        assert chunk["questions"] == [str(q).strip() for q in payload.get("questions", []) if str(q).strip()]


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
        res = add_chunk(auth, "dataset_id", "document_id")
        assert res["code"] == expected_code
        assert res["message"] == expected_message


class TestAddChunk:
    @pytest.mark.p1
    @pytest.mark.parametrize(
        "payload, expected_code, expected_message",
        [
            ({"content": None}, 100, """TypeError("unsupported operand type(s) for +: \'NoneType\' and \'str\'")"""),
            ({"content": ""}, 102, "`content` is required"),
            pytest.param(
                {"content": 1},
                100,
                """TypeError("unsupported operand type(s) for +: \'int\' and \'str\'")""",
                marks=pytest.mark.skip,
            ),
            ({"content": "a"}, 0, ""),
            ({"content": " "}, 102, "`content` is required"),
            ({"content": "\n!?。；！？\"'"}, 0, ""),
        ],
    )
    def test_content(self, get_http_api_auth, add_document, payload, expected_code, expected_message):
        dataset_id, document_id = add_document
        res = list_chunks(get_http_api_auth, dataset_id, document_id)
        if res["code"] != 0:
            assert False, res
        chunks_count = res["data"]["doc"]["chunk_count"]
        res = add_chunk(get_http_api_auth, dataset_id, document_id, payload)
        assert res["code"] == expected_code
        if expected_code == 0:
            validate_chunk_details(dataset_id, document_id, payload, res)
            res = list_chunks(get_http_api_auth, dataset_id, document_id)
            if res["code"] != 0:
                assert False, res
            assert res["data"]["doc"]["chunk_count"] == chunks_count + 1
        else:
            assert res["message"] == expected_message

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "payload, expected_code, expected_message",
        [
            ({"content": "chunk test", "important_keywords": ["a", "b", "c"]}, 0, ""),
            ({"content": "chunk test", "important_keywords": [""]}, 0, ""),
            (
                {"content": "chunk test", "important_keywords": [1]},
                100,
                "TypeError('sequence item 0: expected str instance, int found')",
            ),
            ({"content": "chunk test", "important_keywords": ["a", "a"]}, 0, ""),
            ({"content": "chunk test", "important_keywords": "abc"}, 102, "`important_keywords` is required to be a list"),
            ({"content": "chunk test", "important_keywords": 123}, 102, "`important_keywords` is required to be a list"),
        ],
    )
    def test_important_keywords(self, get_http_api_auth, add_document, payload, expected_code, expected_message):
        dataset_id, document_id = add_document
        res = list_chunks(get_http_api_auth, dataset_id, document_id)
        if res["code"] != 0:
            assert False, res
        chunks_count = res["data"]["doc"]["chunk_count"]
        res = add_chunk(get_http_api_auth, dataset_id, document_id, payload)
        assert res["code"] == expected_code
        if expected_code == 0:
            validate_chunk_details(dataset_id, document_id, payload, res)
            res = list_chunks(get_http_api_auth, dataset_id, document_id)
            if res["code"] != 0:
                assert False, res
            assert res["data"]["doc"]["chunk_count"] == chunks_count + 1
        else:
            assert res["message"] == expected_message

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "payload, expected_code, expected_message",
        [
            ({"content": "chunk test", "questions": ["a", "b", "c"]}, 0, ""),
            ({"content": "chunk test", "questions": [""]}, 0, ""),
            ({"content": "chunk test", "questions": [1]}, 100, "TypeError('sequence item 0: expected str instance, int found')"),
            ({"content": "chunk test", "questions": ["a", "a"]}, 0, ""),
            ({"content": "chunk test", "questions": "abc"}, 102, "`questions` is required to be a list"),
            ({"content": "chunk test", "questions": 123}, 102, "`questions` is required to be a list"),
        ],
    )
    def test_questions(self, get_http_api_auth, add_document, payload, expected_code, expected_message):
        dataset_id, document_id = add_document
        res = list_chunks(get_http_api_auth, dataset_id, document_id)
        if res["code"] != 0:
            assert False, res
        chunks_count = res["data"]["doc"]["chunk_count"]
        res = add_chunk(get_http_api_auth, dataset_id, document_id, payload)
        assert res["code"] == expected_code
        if expected_code == 0:
            validate_chunk_details(dataset_id, document_id, payload, res)
            if res["code"] != 0:
                assert False, res
            res = list_chunks(get_http_api_auth, dataset_id, document_id)
            assert res["data"]["doc"]["chunk_count"] == chunks_count + 1
        else:
            assert res["message"] == expected_message

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
    def test_invalid_dataset_id(
        self,
        get_http_api_auth,
        add_document,
        dataset_id,
        expected_code,
        expected_message,
    ):
        _, document_id = add_document
        res = add_chunk(get_http_api_auth, dataset_id, document_id, {"content": "a"})
        assert res["code"] == expected_code
        assert res["message"] == expected_message

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "document_id, expected_code, expected_message",
        [
            ("", 100, "<MethodNotAllowed '405: Method Not Allowed'>"),
            (
                "invalid_document_id",
                102,
                "You don't own the document invalid_document_id.",
            ),
        ],
    )
    def test_invalid_document_id(self, get_http_api_auth, add_document, document_id, expected_code, expected_message):
        dataset_id, _ = add_document
        res = add_chunk(get_http_api_auth, dataset_id, document_id, {"content": "chunk test"})
        assert res["code"] == expected_code
        assert res["message"] == expected_message

    @pytest.mark.p3
    def test_repeated_add_chunk(self, get_http_api_auth, add_document):
        payload = {"content": "chunk test"}
        dataset_id, document_id = add_document
        res = list_chunks(get_http_api_auth, dataset_id, document_id)
        if res["code"] != 0:
            assert False, res
        chunks_count = res["data"]["doc"]["chunk_count"]
        res = add_chunk(get_http_api_auth, dataset_id, document_id, payload)
        assert res["code"] == 0
        validate_chunk_details(dataset_id, document_id, payload, res)
        res = list_chunks(get_http_api_auth, dataset_id, document_id)
        if res["code"] != 0:
            assert False, res
        assert res["data"]["doc"]["chunk_count"] == chunks_count + 1

        res = add_chunk(get_http_api_auth, dataset_id, document_id, payload)
        assert res["code"] == 0
        validate_chunk_details(dataset_id, document_id, payload, res)
        res = list_chunks(get_http_api_auth, dataset_id, document_id)
        if res["code"] != 0:
            assert False, res
        assert res["data"]["doc"]["chunk_count"] == chunks_count + 2

    @pytest.mark.p2
    def test_add_chunk_to_deleted_document(self, get_http_api_auth, add_document):
        dataset_id, document_id = add_document
        delete_documnets(get_http_api_auth, dataset_id, {"ids": [document_id]})
        res = add_chunk(get_http_api_auth, dataset_id, document_id, {"content": "chunk test"})
        assert res["code"] == 102
        assert res["message"] == f"You don't own the document {document_id}."

    @pytest.mark.skip(reason="issues/6411")
    def test_concurrent_add_chunk(self, get_http_api_auth, add_document):
        chunk_num = 50
        dataset_id, document_id = add_document
        res = list_chunks(get_http_api_auth, dataset_id, document_id)
        if res["code"] != 0:
            assert False, res
        chunks_count = res["data"]["doc"]["chunk_count"]

        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [
                executor.submit(
                    add_chunk,
                    get_http_api_auth,
                    dataset_id,
                    document_id,
                    {"content": f"chunk test {i}"},
                )
                for i in range(chunk_num)
            ]
        responses = [f.result() for f in futures]
        assert all(r["code"] == 0 for r in responses)
        res = list_chunks(get_http_api_auth, dataset_id, document_id)
        if res["code"] != 0:
            assert False, res
        assert res["data"]["doc"]["chunk_count"] == chunks_count + chunk_num

```

## Detailed Analysis

### File Role in Repository

The file `sdk/python/test/test_http_api/test_chunk_management_within_dataset/test_add_chunk.py` is located in the `sdk/python/test/test_http_api/test_chunk_management_within_dataset` directory.

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
- [test_delete_chunks.py](test_delete_chunks.py_docs.md)
- [test_list_chunks.py](test_list_chunks.py_docs.md)
- [test_retrieval_chunks.py](test_retrieval_chunks.py_docs.md)
- [test_update_chunk.py](test_update_chunk.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
