# File Documentation: test/testcases/test_web_api/test_chunk_app/test_list_chunks.py

## File Metadata

- **Path**: `test/testcases/test_web_api/test_chunk_app/test_list_chunks.py`
- **Extension**: `.py`
- **Lines**: 145
- **Characters**: 6,174
- **Size**: 6,174 bytes
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
from common import batch_add_chunks, list_chunks
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
        res = list_chunks(invalid_auth, {"doc_id": "document_id"})
        assert res["code"] == expected_code, res
        assert res["message"] == expected_message, res


class TestChunksList:
    @pytest.mark.p1
    @pytest.mark.parametrize(
        "params, expected_code, expected_page_size, expected_message",
        [
            pytest.param({"page": None, "size": 2}, 100, 0, """TypeError("int() argument must be a string, a bytes-like object or a real number, not 'NoneType'")""", marks=pytest.mark.skip),
            pytest.param({"page": 0, "size": 2}, 100, 0, "ValueError('Search does not support negative slicing.')", marks=pytest.mark.skip),
            ({"page": 2, "size": 2}, 0, 2, ""),
            ({"page": 3, "size": 2}, 0, 1, ""),
            ({"page": "3", "size": 2}, 0, 1, ""),
            pytest.param({"page": -1, "size": 2}, 100, 0, "ValueError('Search does not support negative slicing.')", marks=pytest.mark.skip),
            pytest.param({"page": "a", "size": 2}, 100, 0, """ValueError("invalid literal for int() with base 10: \'a\'")""", marks=pytest.mark.skip),
        ],
    )
    def test_page(self, WebApiAuth, add_chunks, params, expected_code, expected_page_size, expected_message):
        _, doc_id, _ = add_chunks
        payload = {"doc_id": doc_id}
        if params:
            payload.update(params)
        res = list_chunks(WebApiAuth, payload)
        assert res["code"] == expected_code, res
        if expected_code == 0:
            assert len(res["data"]["chunks"]) == expected_page_size, res
        else:
            assert res["message"] == expected_message, res

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "params, expected_code, expected_page_size, expected_message",
        [
            ({"size": None}, 100, 0, """TypeError("int() argument must be a string, a bytes-like object or a real number, not 'NoneType'")"""),
            pytest.param({"size": 0}, 0, 5, ""),
            ({"size": 1}, 0, 1, ""),
            ({"size": 6}, 0, 5, ""),
            ({"size": "1"}, 0, 1, ""),
            pytest.param({"size": -1}, 0, 5, "", marks=pytest.mark.skip),
            pytest.param({"size": "a"}, 100, 0, """ValueError("invalid literal for int() with base 10: \'a\'")""", marks=pytest.mark.skip),
        ],
    )
    def test_page_size(self, WebApiAuth, add_chunks, params, expected_code, expected_page_size, expected_message):
        _, doc_id, _ = add_chunks
        payload = {"doc_id": doc_id}
        if params:
            payload.update(params)
        res = list_chunks(WebApiAuth, payload)
        assert res["code"] == expected_code, res
        if expected_code == 0:
            assert len(res["data"]["chunks"]) == expected_page_size, res
        else:
            assert res["message"] == expected_message, res

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "params, expected_page_size",
        [
            ({"keywords": None}, 5),
            ({"keywords": ""}, 5),
            ({"keywords": "1"}, 1),
            pytest.param({"keywords": "chunk"}, 4, marks=pytest.mark.skipif(os.getenv("DOC_ENGINE") == "infinity", reason="issues/6509")),
            ({"keywords": "content"}, 1),
            ({"keywords": "unknown"}, 0),
        ],
    )
    def test_keywords(self, WebApiAuth, add_chunks, params, expected_page_size):
        _, doc_id, _ = add_chunks
        payload = {"doc_id": doc_id}
        if params:
            payload.update(params)
        res = list_chunks(WebApiAuth, payload)
        assert res["code"] == 0, res
        assert len(res["data"]["chunks"]) == expected_page_size, res

    @pytest.mark.p3
    def test_invalid_params(self, WebApiAuth, add_chunks):
        _, doc_id, _ = add_chunks
        payload = {"doc_id": doc_id, "a": "b"}
        res = list_chunks(WebApiAuth, payload)
        assert res["code"] == 0, res
        assert len(res["data"]["chunks"]) == 5, res

    @pytest.mark.p3
    def test_concurrent_list(self, WebApiAuth, add_chunks):
        _, doc_id, _ = add_chunks
        count = 100
        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [executor.submit(list_chunks, WebApiAuth, {"doc_id": doc_id}) for i in range(count)]
        responses = list(as_completed(futures))
        assert len(responses) == count, responses
        assert all(len(future.result()["data"]["chunks"]) == 5 for future in futures)

    @pytest.mark.p1
    def test_default(self, WebApiAuth, add_document):
        _, doc_id = add_document

        res = list_chunks(WebApiAuth, {"doc_id": doc_id})
        chunks_count = res["data"]["doc"]["chunk_num"]
        batch_add_chunks(WebApiAuth, doc_id, 31)
        # issues/6487
        from time import sleep

        sleep(3)
        res = list_chunks(WebApiAuth, {"doc_id": doc_id})
        assert res["code"] == 0
        assert len(res["data"]["chunks"]) == 30
        assert res["data"]["doc"]["chunk_num"] == chunks_count + 31

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
- `TestChunksList`: Class definition

### Imports (6)

- `import os`
- `from concurrent.futures import ThreadPoolExecutor, as_completed`
- `import pytest`
- `from common import batch_add_chunks, list_chunks`
- `from configs import INVALID_API_TOKEN`
- `from libs.auth import RAGFlowWebApiAuth`

## Code Structure Analysis

- Total lines: 145
- Blank lines: 13 (9.0%)
- Comment lines: ~16 (11.0%)
- Code lines: ~116


## Dependencies and Imports

- `import os`
- `from concurrent.futures import ThreadPoolExecutor, as_completed`
- `import pytest`
- `from common import batch_add_chunks, list_chunks`
- `from configs import INVALID_API_TOKEN`
- `from libs.auth import RAGFlowWebApiAuth`

## Design & Architecture

This file is located in the `test` directory, specifically within `test/testcases/test_web_api/test_chunk_app`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 5 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `test/testcases/test_web_api/test_chunk_app/` directory
- Imports from `concurrent.futures`
- Imports from `common`
- Imports from `configs`
- Imports from `libs.auth`
- Imports from `time`

## Keywords

ANY, All, Apache, Authors, BASIS, CONDITIONS, Copyright, DOC_ENGINE, INVALID_API_TOKEN, InfiniFlow, KIND, LICENSE, License, Licensed, None, NoneType, Python, RAGFlowWebApiAuth, Reserved, Rights, Search, See, TestAuthorization, TestChunksList, The, ThreadPoolExecutor, TypeError, Unauthorized, Unless, ValueError, Version, WARRANTIES, WITHOUT, WebApiAuth, You, pytest, test_concurrent_list, test_default, test_invalid_auth, test_invalid_params, test_keywords, test_page, test_page_size

---
*Generated by RAGFlow Repository Documentation Generator*
