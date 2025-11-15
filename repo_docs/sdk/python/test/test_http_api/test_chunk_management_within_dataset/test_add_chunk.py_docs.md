# File Documentation: sdk/python/test/test_http_api/test_chunk_management_within_dataset/test_add_chunk.py

## File Metadata

- **Path**: `sdk/python/test/test_http_api/test_chunk_management_within_dataset/test_add_chunk.py`
- **Extension**: `.py`
- **Lines**: 251
- **Characters**: 10,729
- **Size**: 10,737 bytes
- **Purpose**: Testing - Contains unit tests, integration tests, or test utilities

## Original Source

```python
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

## High-Level Overview

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

## Detailed Walkthrough

### Classes (2)

- `TestAuthorization`: Class definition
- `TestAddChunk`: Class definition

### Functions (1)

- `validate_chunk_details()`: Function definition

### Imports (4)

- `from concurrent.futures import ThreadPoolExecutor`
- `import pytest`
- `from common import INVALID_API_TOKEN, add_chunk, delete_documnets, list_chunks`
- `from libs.auth import RAGFlowHttpApiAuth`

## Code Structure Analysis

- Total lines: 251
- Blank lines: 17 (6.8%)
- Comment lines: ~16 (6.4%)
- Code lines: ~218


## Dependencies and Imports

- `from concurrent.futures import ThreadPoolExecutor`
- `import pytest`
- `from common import INVALID_API_TOKEN, add_chunk, delete_documnets, list_chunks`
- `from libs.auth import RAGFlowHttpApiAuth`

## Design & Architecture

This file is located in the `sdk` directory, specifically within `sdk/python/test/test_http_api/test_chunk_management_within_dataset`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 7 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `sdk/python/test/test_http_api/test_chunk_management_within_dataset/` directory
- Imports from `concurrent.futures`
- Imports from `common`
- Imports from `libs.auth`

## Keywords

ANY, API, All, Allowed, Apache, Authentication, Authorization, Authors, BASIS, CONDITIONS, Copyright, False, Found, INVALID_API_TOKEN, InfiniFlow, KIND, LICENSE, License, Licensed, Method, MethodNotAllowed, None, NoneType, Not, NotFound, Python, RAGFlowHttpApiAuth, Reserved, Rights, See, TestAddChunk, TestAuthorization, The, ThreadPoolExecutor, TypeError, Unless, Version, WARRANTIES, WITHOUT, You, pytest, test_add_chunk_to_deleted_document, test_concurrent_add_chunk, test_content, test_important_keywords, test_invalid_auth, test_invalid_dataset_id, test_invalid_document_id, test_questions, test_repeated_add_chunk...

---
*Generated by RAGFlow Repository Documentation Generator*
