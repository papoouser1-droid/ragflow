# File Documentation: test/testcases/test_web_api/test_chunk_app/test_retrieval_chunks.py

## File Metadata

- **Path**: `test/testcases/test_web_api/test_chunk_app/test_retrieval_chunks.py`
- **Extension**: `.py`
- **Lines**: 309
- **Characters**: 12,560
- **Size**: 12,560 bytes
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
import os
from concurrent.futures import ThreadPoolExecutor, as_completed

import pytest
from common import retrieval_chunks
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
        res = retrieval_chunks(invalid_auth, {"kb_id": "dummy_kb_id", "question": "dummy question"})
        assert res["code"] == expected_code, res
        assert res["message"] == expected_message, res


class TestChunksRetrieval:
    @pytest.mark.p1
    @pytest.mark.parametrize(
        "payload, expected_code, expected_page_size, expected_message",
        [
            ({"question": "chunk", "kb_id": None}, 0, 4, ""),
            ({"question": "chunk", "doc_ids": None}, 101, 0, "required argument are missing: kb_id; "),
            ({"question": "chunk", "kb_id": None, "doc_ids": None}, 0, 4, ""),
            ({"question": "chunk"}, 101, 0, "required argument are missing: kb_id; "),
        ],
    )
    def test_basic_scenarios(self, WebApiAuth, add_chunks, payload, expected_code, expected_page_size, expected_message):
        dataset_id, document_id, _ = add_chunks
        if "kb_id" in payload:
            payload["kb_id"] = [dataset_id]
        if "doc_ids" in payload:
            payload["doc_ids"] = [document_id]
        res = retrieval_chunks(WebApiAuth, payload)
        assert res["code"] == expected_code, res
        if expected_code == 0:
            assert len(res["data"]["chunks"]) == expected_page_size, res
        else:
            assert res["message"] == expected_message, res

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "payload, expected_code, expected_page_size, expected_message",
        [
            pytest.param(
                {"page": None, "size": 2},
                100,
                0,
                """TypeError("int() argument must be a string, a bytes-like object or a real number, not 'NoneType'")""",
                marks=pytest.mark.skip,
            ),
            pytest.param(
                {"page": 0, "size": 2},
                100,
                0,
                "ValueError('Search does not support negative slicing.')",
                marks=pytest.mark.skip,
            ),
            pytest.param({"page": 2, "size": 2}, 0, 2, "", marks=pytest.mark.skip(reason="issues/6646")),
            ({"page": 3, "size": 2}, 0, 0, ""),
            ({"page": "3", "size": 2}, 0, 0, ""),
            pytest.param(
                {"page": -1, "size": 2},
                100,
                0,
                "ValueError('Search does not support negative slicing.')",
                marks=pytest.mark.skip,
            ),
            pytest.param(
                {"page": "a", "size": 2},
                100,
                0,
                """ValueError("invalid literal for int() with base 10: 'a'")""",
                marks=pytest.mark.skip,
            ),
        ],
    )
    def test_page(self, WebApiAuth, add_chunks, payload, expected_code, expected_page_size, expected_message):
        dataset_id, _, _ = add_chunks
        payload.update({"question": "chunk", "kb_id": [dataset_id]})
        res = retrieval_chunks(WebApiAuth, payload)
        assert res["code"] == expected_code, res
        if expected_code == 0:
            assert len(res["data"]["chunks"]) == expected_page_size, res
        else:
            assert res["message"] == expected_message, res

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "payload, expected_code, expected_page_size, expected_message",
        [
            pytest.param(
                {"size": None},
                100,
                0,
                """TypeError("int() argument must be a string, a bytes-like object or a real number, not 'NoneType'")""",
                marks=pytest.mark.skip,
            ),
            # ({"size": 0}, 0, 0, ""),
            ({"size": 1}, 0, 1, ""),
            ({"size": 5}, 0, 4, ""),
            ({"size": "1"}, 0, 1, ""),
            # ({"size": -1}, 0, 0, ""),
            pytest.param(
                {"size": "a"},
                100,
                0,
                """ValueError("invalid literal for int() with base 10: 'a'")""",
                marks=pytest.mark.skip,
            ),
        ],
    )
    def test_page_size(self, WebApiAuth, add_chunks, payload, expected_code, expected_page_size, expected_message):
        dataset_id, _, _ = add_chunks
        payload.update({"question": "chunk", "kb_id": [dataset_id]})

        res = retrieval_chunks(WebApiAuth, payload)
        assert res["code"] == expected_code, res
        if expected_code == 0:
            assert len(res["data"]["chunks"]) == expected_page_size, res
        else:
            assert res["message"] == expected_message, res

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "payload, expected_code, expected_page_size, expected_message",
        [
            ({"vector_similarity_weight": 0}, 0, 4, ""),
            ({"vector_similarity_weight": 0.5}, 0, 4, ""),
            ({"vector_similarity_weight": 10}, 0, 4, ""),
            pytest.param(
                {"vector_similarity_weight": "a"},
                100,
                0,
                """ValueError("could not convert string to float: 'a'")""",
                marks=pytest.mark.skip,
            ),
        ],
    )
    def test_vector_similarity_weight(self, WebApiAuth, add_chunks, payload, expected_code, expected_page_size, expected_message):
        dataset_id, _, _ = add_chunks
        payload.update({"question": "chunk", "kb_id": [dataset_id]})
        res = retrieval_chunks(WebApiAuth, payload)
        assert res["code"] == expected_code, res
        if expected_code == 0:
            assert len(res["data"]["chunks"]) == expected_page_size, res
        else:
            assert res["message"] == expected_message, res

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "payload, expected_code, expected_page_size, expected_message",
        [
            ({"top_k": 10}, 0, 4, ""),
            pytest.param(
                {"top_k": 1},
                0,
                4,
                "",
                marks=pytest.mark.skipif(os.getenv("DOC_ENGINE") in ["infinity", "opensearch"], reason="Infinity"),
            ),
            pytest.param(
                {"top_k": 1},
                0,
                1,
                "",
                marks=pytest.mark.skipif(os.getenv("DOC_ENGINE") in [None, "opensearch", "elasticsearch"], reason="elasticsearch"),
            ),
            pytest.param(
                {"top_k": -1},
                100,
                4,
                "must be greater than 0",
                marks=pytest.mark.skipif(os.getenv("DOC_ENGINE") in ["infinity", "opensearch"], reason="Infinity"),
            ),
            pytest.param(
                {"top_k": -1},
                100,
                4,
                "3014",
                marks=pytest.mark.skipif(os.getenv("DOC_ENGINE") in [None, "opensearch", "elasticsearch"], reason="elasticsearch"),
            ),
            pytest.param(
                {"top_k": "a"},
                100,
                0,
                """ValueError("invalid literal for int() with base 10: 'a'")""",
                marks=pytest.mark.skip,
            ),
        ],
    )
    def test_top_k(self, WebApiAuth, add_chunks, payload, expected_code, expected_page_size, expected_message):
        dataset_id, _, _ = add_chunks
        payload.update({"question": "chunk", "kb_id": [dataset_id]})
        res = retrieval_chunks(WebApiAuth, payload)
        assert res["code"] == expected_code, res
        if expected_code == 0:
            assert len(res["data"]["chunks"]) == expected_page_size, res
        else:
            assert expected_message in res["message"], res

    @pytest.mark.skip
    @pytest.mark.parametrize(
        "payload, expected_code, expected_message",
        [
            ({"rerank_id": "BAAI/bge-reranker-v2-m3"}, 0, ""),
            pytest.param({"rerank_id": "unknown"}, 100, "LookupError('Model(unknown) not authorized')", marks=pytest.mark.skip),
        ],
    )
    def test_rerank_id(self, WebApiAuth, add_chunks, payload, expected_code, expected_message):
        dataset_id, _, _ = add_chunks
        payload.update({"question": "chunk", "kb_id": [dataset_id]})
        res = retrieval_chunks(WebApiAuth, payload)
        assert res["code"] == expected_code, res
        if expected_code == 0:
            assert len(res["data"]["chunks"]) > 0, res
        else:
            assert expected_message in res["message"], res

    @pytest.mark.skip
    @pytest.mark.parametrize(
        "payload, expected_code, expected_page_size, expected_message",
        [
            ({"keyword": True}, 0, 5, ""),
            ({"keyword": "True"}, 0, 5, ""),
            ({"keyword": False}, 0, 5, ""),
            ({"keyword": "False"}, 0, 5, ""),
            ({"keyword": None}, 0, 5, ""),
        ],
    )
    def test_keyword(self, WebApiAuth, add_chunks, payload, expected_code, expected_page_size, expected_message):
        dataset_id, _, _ = add_chunks
        payload.update({"question": "chunk test", "kb_id": [dataset_id]})
        res = retrieval_chunks(WebApiAuth, payload)
        assert res["code"] == expected_code, res
        if expected_code == 0:
            assert len(res["data"]["chunks"]) == expected_page_size, res
        else:
            assert res["message"] == expected_message, res

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "payload, expected_code, expected_highlight, expected_message",
        [
            ({"highlight": True}, 0, True, ""),
            ({"highlight": "True"}, 0, True, ""),
            pytest.param({"highlight": False}, 0, False, "", marks=pytest.mark.skip(reason="issues/6648")),
            pytest.param({"highlight": "False"}, 0, False, "", marks=pytest.mark.skip(reason="issues/6648")),
            pytest.param({"highlight": None}, 0, False, "", marks=pytest.mark.skip(reason="issues/6648")),
        ],
    )
    def test_highlight(self, WebApiAuth, add_chunks, payload, expected_code, expected_highlight, expected_message):
        dataset_id, _, _ = add_chunks
        payload.update({"question": "chunk", "kb_id": [dataset_id]})
        res = retrieval_chunks(WebApiAuth, payload)
        assert res["code"] == expected_code, res
        if expected_highlight:
            for chunk in res["data"]["chunks"]:
                assert "highlight" in chunk, res
        else:
            for chunk in res["data"]["chunks"]:
                assert "highlight" not in chunk, res

        if expected_code != 0:
            assert res["message"] == expected_message, res

    @pytest.mark.p3
    def test_invalid_params(self, WebApiAuth, add_chunks):
        dataset_id, _, _ = add_chunks
        payload = {"question": "chunk", "kb_id": [dataset_id], "a": "b"}
        res = retrieval_chunks(WebApiAuth, payload)
        assert res["code"] == 0, res
        assert len(res["data"]["chunks"]) == 4, res

    @pytest.mark.p3
    def test_concurrent_retrieval(self, WebApiAuth, add_chunks):
        dataset_id, _, _ = add_chunks
        count = 100
        payload = {"question": "chunk", "kb_id": [dataset_id]}

        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [executor.submit(retrieval_chunks, WebApiAuth, payload) for i in range(count)]
        responses = list(as_completed(futures))
        assert len(responses) == count, responses
        assert all(future.result()["code"] == 0 for future in futures)

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
- `TestChunksRetrieval`: Class definition

### Imports (6)

- `import os`
- `from concurrent.futures import ThreadPoolExecutor, as_completed`
- `import pytest`
- `from common import retrieval_chunks`
- `from configs import INVALID_API_TOKEN`
- `from libs.auth import RAGFlowWebApiAuth`

## Code Structure Analysis

- Total lines: 309
- Blank lines: 18 (5.8%)
- Comment lines: ~23 (7.4%)
- Code lines: ~268


## Dependencies and Imports

- `import os`
- `from concurrent.futures import ThreadPoolExecutor, as_completed`
- `import pytest`
- `from common import retrieval_chunks`
- `from configs import INVALID_API_TOKEN`
- `from libs.auth import RAGFlowWebApiAuth`

## Design & Architecture

This file is located in the `test` directory, specifically within `test/testcases/test_web_api/test_chunk_app`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 8 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal
- **Code Execution**: Avoid eval/exec with user input - potential code injection

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `test/testcases/test_web_api/test_chunk_app/` directory
- Imports from `concurrent.futures`
- Imports from `common`
- Imports from `configs`
- Imports from `libs.auth`

## Keywords

ANY, All, Apache, Authors, BAAI, BASIS, CONDITIONS, Copyright, DOC_ENGINE, False, INVALID_API_TOKEN, InfiniFlow, Infinity, KIND, LICENSE, License, Licensed, LookupError, Model, None, NoneType, Python, RAGFlowWebApiAuth, Reserved, Rights, Search, See, TestAuthorization, TestChunksRetrieval, The, ThreadPoolExecutor, True, TypeError, Unauthorized, Unless, ValueError, Version, WARRANTIES, WITHOUT, WebApiAuth, You, pytest, test_basic_scenarios, test_concurrent_retrieval, test_highlight, test_invalid_auth, test_invalid_params, test_keyword, test_page, test_page_size...

---
*Generated by RAGFlow Repository Documentation Generator*
