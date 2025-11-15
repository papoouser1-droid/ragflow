# File Documentation: sdk/python/test/test_http_api/test_session_management/test_delete_sessions_with_chat_assistant.py

## File Metadata

- **Path**: `sdk/python/test/test_http_api/test_session_management/test_delete_sessions_with_chat_assistant.py`
- **Extension**: `.py`
- **Lines**: 171
- **Characters**: 7,470
- **Size**: 7,470 bytes
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
from common import INVALID_API_TOKEN, batch_add_sessions_with_chat_assistant, delete_session_with_chat_assistants, list_session_with_chat_assistants
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
        res = delete_session_with_chat_assistants(auth, "chat_assistant_id")
        assert res["code"] == expected_code
        assert res["message"] == expected_message


class TestSessionWithChatAssistantDelete:
    @pytest.mark.p3
    @pytest.mark.parametrize(
        "chat_assistant_id, expected_code, expected_message",
        [
            ("", 100, "<MethodNotAllowed '405: Method Not Allowed'>"),
            (
                "invalid_chat_assistant_id",
                102,
                "You don't own the chat",
            ),
        ],
    )
    def test_invalid_chat_assistant_id(self, get_http_api_auth, add_sessions_with_chat_assistant_func, chat_assistant_id, expected_code, expected_message):
        _, session_ids = add_sessions_with_chat_assistant_func
        res = delete_session_with_chat_assistants(get_http_api_auth, chat_assistant_id, {"ids": session_ids})
        assert res["code"] == expected_code
        assert res["message"] == expected_message

    @pytest.mark.parametrize(
        "payload",
        [
            pytest.param(lambda r: {"ids": ["invalid_id"] + r}, marks=pytest.mark.p3),
            pytest.param(lambda r: {"ids": r[:1] + ["invalid_id"] + r[1:5]}, marks=pytest.mark.p1),
            pytest.param(lambda r: {"ids": r + ["invalid_id"]}, marks=pytest.mark.p3),
        ],
    )
    def test_delete_partial_invalid_id(self, get_http_api_auth, add_sessions_with_chat_assistant_func, payload):
        chat_assistant_id, session_ids = add_sessions_with_chat_assistant_func
        if callable(payload):
            payload = payload(session_ids)
        res = delete_session_with_chat_assistants(get_http_api_auth, chat_assistant_id, payload)
        assert res["code"] == 0
        assert res["data"]["errors"][0] == "The chat doesn't own the session invalid_id"

        res = list_session_with_chat_assistants(get_http_api_auth, chat_assistant_id)
        if res["code"] != 0:
            assert False, res
        assert len(res["data"]) == 0

    @pytest.mark.p3
    def test_repeated_deletion(self, get_http_api_auth, add_sessions_with_chat_assistant_func):
        chat_assistant_id, session_ids = add_sessions_with_chat_assistant_func
        payload = {"ids": session_ids}
        res = delete_session_with_chat_assistants(get_http_api_auth, chat_assistant_id, payload)
        assert res["code"] == 0

        res = delete_session_with_chat_assistants(get_http_api_auth, chat_assistant_id, payload)
        assert res["code"] == 102
        assert "The chat doesn't own the session" in res["message"]

    @pytest.mark.p3
    def test_duplicate_deletion(self, get_http_api_auth, add_sessions_with_chat_assistant_func):
        chat_assistant_id, session_ids = add_sessions_with_chat_assistant_func
        res = delete_session_with_chat_assistants(get_http_api_auth, chat_assistant_id, {"ids": session_ids * 2})
        assert res["code"] == 0
        assert "Duplicate session ids" in res["data"]["errors"][0]
        assert res["data"]["success_count"] == 5

        res = list_session_with_chat_assistants(get_http_api_auth, chat_assistant_id)
        if res["code"] != 0:
            assert False, res
        assert len(res["data"]) == 0

    @pytest.mark.p3
    def test_concurrent_deletion(self, get_http_api_auth, add_chat_assistants):
        sessions_num = 100
        _, _, chat_assistant_ids = add_chat_assistants
        session_ids = batch_add_sessions_with_chat_assistant(get_http_api_auth, chat_assistant_ids[0], sessions_num)

        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [
                executor.submit(
                    delete_session_with_chat_assistants,
                    get_http_api_auth,
                    chat_assistant_ids[0],
                    {"ids": session_ids[i : i + 1]},
                )
                for i in range(sessions_num)
            ]
        responses = [f.result() for f in futures]
        assert all(r["code"] == 0 for r in responses)

    @pytest.mark.p3
    def test_delete_1k(self, get_http_api_auth, add_chat_assistants):
        sessions_num = 1_000
        _, _, chat_assistant_ids = add_chat_assistants
        session_ids = batch_add_sessions_with_chat_assistant(get_http_api_auth, chat_assistant_ids[0], sessions_num)

        res = delete_session_with_chat_assistants(get_http_api_auth, chat_assistant_ids[0], {"ids": session_ids})
        assert res["code"] == 0

        res = list_session_with_chat_assistants(get_http_api_auth, chat_assistant_ids[0])
        if res["code"] != 0:
            assert False, res
        assert len(res["data"]) == 0

    @pytest.mark.parametrize(
        "payload, expected_code, expected_message, remaining",
        [
            pytest.param(None, 0, """TypeError("argument of type \'NoneType\' is not iterable")""", 0, marks=pytest.mark.skip),
            pytest.param({"ids": ["invalid_id"]}, 102, "The chat doesn't own the session invalid_id", 5, marks=pytest.mark.p3),
            pytest.param("not json", 100, """AttributeError("\'str\' object has no attribute \'get\'")""", 5, marks=pytest.mark.skip),
            pytest.param(lambda r: {"ids": r[:1]}, 0, "", 4, marks=pytest.mark.p3),
            pytest.param(lambda r: {"ids": r}, 0, "", 0, marks=pytest.mark.p1),
            pytest.param({"ids": []}, 0, "", 0, marks=pytest.mark.p3),
        ],
    )
    def test_basic_scenarios(
        self,
        get_http_api_auth,
        add_sessions_with_chat_assistant_func,
        payload,
        expected_code,
        expected_message,
        remaining,
    ):
        chat_assistant_id, session_ids = add_sessions_with_chat_assistant_func
        if callable(payload):
            payload = payload(session_ids)
        res = delete_session_with_chat_assistants(get_http_api_auth, chat_assistant_id, payload)
        assert res["code"] == expected_code
        if res["code"] != 0:
            assert res["message"] == expected_message

        res = list_session_with_chat_assistants(get_http_api_auth, chat_assistant_id)
        if res["code"] != 0:
            assert False, res
        assert len(res["data"]) == remaining

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
- `TestSessionWithChatAssistantDelete`: Class definition

### Imports (4)

- `from concurrent.futures import ThreadPoolExecutor`
- `import pytest`
- `from common import INVALID_API_TOKEN, batch_add_sessions_with_chat_assistant, delete_session_with_chat_assistants, list_session_with_chat_assistants`
- `from libs.auth import RAGFlowHttpApiAuth`

## Code Structure Analysis

- Total lines: 171
- Blank lines: 19 (11.1%)
- Comment lines: ~15 (8.8%)
- Code lines: ~137


## Dependencies and Imports

- `from concurrent.futures import ThreadPoolExecutor`
- `import pytest`
- `from common import INVALID_API_TOKEN, batch_add_sessions_with_chat_assistant, delete_session_with_chat_assistants, list_session_with_chat_assistants`
- `from libs.auth import RAGFlowHttpApiAuth`

## Design & Architecture

This file is located in the `sdk` directory, specifically within `sdk/python/test/test_http_api/test_session_management`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 4 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `sdk/python/test/test_http_api/test_session_management/` directory
- Imports from `concurrent.futures`
- Imports from `common`
- Imports from `libs.auth`

## Keywords

ANY, API, All, Allowed, Apache, AttributeError, Authentication, Authorization, Authors, BASIS, CONDITIONS, Copyright, Duplicate, False, INVALID_API_TOKEN, InfiniFlow, KIND, LICENSE, License, Licensed, Method, MethodNotAllowed, None, NoneType, Not, Python, RAGFlowHttpApiAuth, Reserved, Rights, See, TestAuthorization, TestSessionWithChatAssistantDelete, The, ThreadPoolExecutor, TypeError, Unless, Version, WARRANTIES, WITHOUT, You, pytest, test_basic_scenarios, test_concurrent_deletion, test_delete_1k, test_delete_partial_invalid_id, test_duplicate_deletion, test_invalid_auth, test_invalid_chat_assistant_id, test_repeated_deletion

---
*Generated by RAGFlow Repository Documentation Generator*
