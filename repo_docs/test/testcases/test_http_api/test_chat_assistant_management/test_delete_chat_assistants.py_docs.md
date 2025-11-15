# File Documentation: test/testcases/test_http_api/test_chat_assistant_management/test_delete_chat_assistants.py

## File Metadata

- **Path**: `test/testcases/test_http_api/test_chat_assistant_management/test_delete_chat_assistants.py`
- **Extension**: `.py`
- **Lines**: 128
- **Characters**: 5,475
- **Size**: 5,491 bytes
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
from concurrent.futures import ThreadPoolExecutor, as_completed

import pytest
from common import batch_create_chat_assistants, delete_chat_assistants, list_chat_assistants
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
        res = delete_chat_assistants(invalid_auth)
        assert res["code"] == expected_code
        assert res["message"] == expected_message


class TestChatAssistantsDelete:
    @pytest.mark.parametrize(
        "payload, expected_code, expected_message, remaining",
        [
            pytest.param(None, 0, "", 0, marks=pytest.mark.p3),
            pytest.param({"ids": []}, 0, "", 0, marks=pytest.mark.p3),
            pytest.param({"ids": ["invalid_id"]}, 102, "Assistant(invalid_id) not found.", 5, marks=pytest.mark.p3),
            pytest.param({"ids": ["\n!?。；！？\"'"]}, 102, """Assistant(\n!?。；！？"\') not found.""", 5, marks=pytest.mark.p3),
            pytest.param("not json", 100, "AttributeError(\"'str' object has no attribute 'get'\")", 5, marks=pytest.mark.p3),
            pytest.param(lambda r: {"ids": r[:1]}, 0, "", 4, marks=pytest.mark.p3),
            pytest.param(lambda r: {"ids": r}, 0, "", 0, marks=pytest.mark.p1),
        ],
    )
    def test_basic_scenarios(self, HttpApiAuth, add_chat_assistants_func, payload, expected_code, expected_message, remaining):
        _, _, chat_assistant_ids = add_chat_assistants_func
        if callable(payload):
            payload = payload(chat_assistant_ids)
        res = delete_chat_assistants(HttpApiAuth, payload)
        assert res["code"] == expected_code
        if res["code"] != 0:
            assert res["message"] == expected_message

        res = list_chat_assistants(HttpApiAuth)
        assert len(res["data"]) == remaining

    @pytest.mark.parametrize(
        "payload",
        [
            pytest.param(lambda r: {"ids": ["invalid_id"] + r}, marks=pytest.mark.p3),
            pytest.param(lambda r: {"ids": r[:1] + ["invalid_id"] + r[1:5]}, marks=pytest.mark.p1),
            pytest.param(lambda r: {"ids": r + ["invalid_id"]}, marks=pytest.mark.p3),
        ],
    )
    def test_delete_partial_invalid_id(self, HttpApiAuth, add_chat_assistants_func, payload):
        _, _, chat_assistant_ids = add_chat_assistants_func
        if callable(payload):
            payload = payload(chat_assistant_ids)
        res = delete_chat_assistants(HttpApiAuth, payload)
        assert res["code"] == 0
        assert res["data"]["errors"][0] == "Assistant(invalid_id) not found."
        assert res["data"]["success_count"] == 5

        res = list_chat_assistants(HttpApiAuth)
        assert len(res["data"]) == 0

    @pytest.mark.p3
    def test_repeated_deletion(self, HttpApiAuth, add_chat_assistants_func):
        _, _, chat_assistant_ids = add_chat_assistants_func
        res = delete_chat_assistants(HttpApiAuth, {"ids": chat_assistant_ids})
        assert res["code"] == 0

        res = delete_chat_assistants(HttpApiAuth, {"ids": chat_assistant_ids})
        assert res["code"] == 102
        assert "not found" in res["message"]

    @pytest.mark.p3
    def test_duplicate_deletion(self, HttpApiAuth, add_chat_assistants_func):
        _, _, chat_assistant_ids = add_chat_assistants_func
        res = delete_chat_assistants(HttpApiAuth, {"ids": chat_assistant_ids + chat_assistant_ids})
        assert res["code"] == 0
        assert "Duplicate assistant ids" in res["data"]["errors"][0]
        assert res["data"]["success_count"] == 5

        res = list_chat_assistants(HttpApiAuth)
        assert res["code"] == 0

    @pytest.mark.p3
    def test_concurrent_deletion(self, HttpApiAuth):
        count = 100
        ids = batch_create_chat_assistants(HttpApiAuth, count)

        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [executor.submit(delete_chat_assistants, HttpApiAuth, {"ids": ids[i : i + 1]}) for i in range(count)]
        responses = list(as_completed(futures))
        assert len(responses) == count, responses
        assert all(future.result()["code"] == 0 for future in futures)

    @pytest.mark.p3
    def test_delete_10k(self, HttpApiAuth):
        ids = batch_create_chat_assistants(HttpApiAuth, 1_000)
        res = delete_chat_assistants(HttpApiAuth, {"ids": ids})
        assert res["code"] == 0

        res = list_chat_assistants(HttpApiAuth)
        assert len(res["data"]) == 0

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
- `TestChatAssistantsDelete`: Class definition

### Imports (5)

- `from concurrent.futures import ThreadPoolExecutor, as_completed`
- `import pytest`
- `from common import batch_create_chat_assistants, delete_chat_assistants, list_chat_assistants`
- `from configs import INVALID_API_TOKEN`
- `from libs.auth import RAGFlowHttpApiAuth`

## Code Structure Analysis

- Total lines: 128
- Blank lines: 17 (13.3%)
- Comment lines: ~15 (11.7%)
- Code lines: ~96


## Dependencies and Imports

- `from concurrent.futures import ThreadPoolExecutor, as_completed`
- `import pytest`
- `from common import batch_create_chat_assistants, delete_chat_assistants, list_chat_assistants`
- `from configs import INVALID_API_TOKEN`
- `from libs.auth import RAGFlowHttpApiAuth`

## Design & Architecture

This file is located in the `test` directory, specifically within `test/testcases/test_http_api/test_chat_assistant_management`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 3 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `test/testcases/test_http_api/test_chat_assistant_management/` directory
- Imports from `concurrent.futures`
- Imports from `common`
- Imports from `configs`
- Imports from `libs.auth`

## Keywords

ANY, API, All, Apache, Assistant, AttributeError, Authentication, Authorization, Authors, BASIS, CONDITIONS, Copyright, Duplicate, HttpApiAuth, INVALID_API_TOKEN, InfiniFlow, KIND, LICENSE, License, Licensed, None, Python, RAGFlowHttpApiAuth, Reserved, Rights, See, TestAuthorization, TestChatAssistantsDelete, The, ThreadPoolExecutor, Unless, Version, WARRANTIES, WITHOUT, You, pytest, test_basic_scenarios, test_concurrent_deletion, test_delete_10k, test_delete_partial_invalid_id, test_duplicate_deletion, test_invalid_auth, test_repeated_deletion

---
*Generated by RAGFlow Repository Documentation Generator*
