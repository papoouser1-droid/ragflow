# File Documentation: test/testcases/test_http_api/test_session_management/test_create_session_with_chat_assistant.py

## File Metadata

- **Path**: `test/testcases/test_http_api/test_session_management/test_create_session_with_chat_assistant.py`
- **Extension**: `.py`
- **Lines**: 120
- **Characters**: 5,301
- **Size**: 5,301 bytes
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
from common import create_session_with_chat_assistant, delete_chat_assistants, list_session_with_chat_assistants
from configs import INVALID_API_TOKEN, SESSION_WITH_CHAT_NAME_LIMIT
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
        res = create_session_with_chat_assistant(invalid_auth, "chat_assistant_id")
        assert res["code"] == expected_code
        assert res["message"] == expected_message


@pytest.mark.usefixtures("clear_session_with_chat_assistants")
class TestSessionWithChatAssistantCreate:
    @pytest.mark.p1
    @pytest.mark.parametrize(
        "payload, expected_code, expected_message",
        [
            ({"name": "valid_name"}, 0, ""),
            pytest.param({"name": "a" * (SESSION_WITH_CHAT_NAME_LIMIT + 1)}, 102, "", marks=pytest.mark.skip(reason="issues/")),
            pytest.param({"name": 1}, 100, "", marks=pytest.mark.skip(reason="issues/")),
            ({"name": ""}, 102, "`name` can not be empty."),
            ({"name": "duplicated_name"}, 0, ""),
            ({"name": "case insensitive"}, 0, ""),
        ],
    )
    def test_name(self, HttpApiAuth, add_chat_assistants, payload, expected_code, expected_message):
        _, _, chat_assistant_ids = add_chat_assistants
        if payload["name"] == "duplicated_name":
            create_session_with_chat_assistant(HttpApiAuth, chat_assistant_ids[0], payload)
        elif payload["name"] == "case insensitive":
            create_session_with_chat_assistant(HttpApiAuth, chat_assistant_ids[0], {"name": payload["name"].upper()})

        res = create_session_with_chat_assistant(HttpApiAuth, chat_assistant_ids[0], payload)
        assert res["code"] == expected_code, res
        if expected_code == 0:
            assert res["data"]["name"] == payload["name"]
            assert res["data"]["chat_id"] == chat_assistant_ids[0]
        else:
            assert res["message"] == expected_message

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "chat_assistant_id, expected_code, expected_message",
        [
            ("", 100, "<MethodNotAllowed '405: Method Not Allowed'>"),
            ("invalid_chat_assistant_id", 102, "You do not own the assistant."),
        ],
    )
    def test_invalid_chat_assistant_id(self, HttpApiAuth, chat_assistant_id, expected_code, expected_message):
        res = create_session_with_chat_assistant(HttpApiAuth, chat_assistant_id, {"name": "valid_name"})
        assert res["code"] == expected_code
        assert res["message"] == expected_message

    @pytest.mark.p3
    def test_concurrent_create_session(self, HttpApiAuth, add_chat_assistants):
        count = 1000
        _, _, chat_assistant_ids = add_chat_assistants
        res = list_session_with_chat_assistants(HttpApiAuth, chat_assistant_ids[0])
        if res["code"] != 0:
            assert False, res
        sessions_count = len(res["data"])

        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [
                executor.submit(
                    create_session_with_chat_assistant,
                    HttpApiAuth,
                    chat_assistant_ids[0],
                    {"name": f"session with chat assistant test {i}"},
                )
                for i in range(count)
            ]
        responses = list(as_completed(futures))
        assert len(responses) == count, responses
        assert all(future.result()["code"] == 0 for future in futures)
        res = list_session_with_chat_assistants(HttpApiAuth, chat_assistant_ids[0], {"page_size": count * 2})
        if res["code"] != 0:
            assert False, res
        assert len(res["data"]) == sessions_count + count

    @pytest.mark.p3
    def test_add_session_to_deleted_chat_assistant(self, HttpApiAuth, add_chat_assistants):
        _, _, chat_assistant_ids = add_chat_assistants
        res = delete_chat_assistants(HttpApiAuth, {"ids": [chat_assistant_ids[0]]})
        assert res["code"] == 0
        res = create_session_with_chat_assistant(HttpApiAuth, chat_assistant_ids[0], {"name": "valid_name"})
        assert res["code"] == 102
        assert res["message"] == "You do not own the assistant."

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
- `TestSessionWithChatAssistantCreate`: Class definition

### Imports (5)

- `from concurrent.futures import ThreadPoolExecutor, as_completed`
- `import pytest`
- `from common import create_session_with_chat_assistant, delete_chat_assistants, list_session_with_chat_assistants`
- `from configs import INVALID_API_TOKEN, SESSION_WITH_CHAT_NAME_LIMIT`
- `from libs.auth import RAGFlowHttpApiAuth`

## Code Structure Analysis

- Total lines: 120
- Blank lines: 11 (9.2%)
- Comment lines: ~15 (12.5%)
- Code lines: ~94


## Dependencies and Imports

- `from concurrent.futures import ThreadPoolExecutor, as_completed`
- `import pytest`
- `from common import create_session_with_chat_assistant, delete_chat_assistants, list_session_with_chat_assistants`
- `from configs import INVALID_API_TOKEN, SESSION_WITH_CHAT_NAME_LIMIT`
- `from libs.auth import RAGFlowHttpApiAuth`

## Design & Architecture

This file is located in the `test` directory, specifically within `test/testcases/test_http_api/test_session_management`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 3 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `test/testcases/test_http_api/test_session_management/` directory
- Imports from `concurrent.futures`
- Imports from `common`
- Imports from `configs`
- Imports from `libs.auth`

## Keywords

ANY, API, All, Allowed, Apache, Authentication, Authorization, Authors, BASIS, CONDITIONS, Copyright, False, HttpApiAuth, INVALID_API_TOKEN, InfiniFlow, KIND, LICENSE, License, Licensed, Method, MethodNotAllowed, None, Not, Python, RAGFlowHttpApiAuth, Reserved, Rights, SESSION_WITH_CHAT_NAME_LIMIT, See, TestAuthorization, TestSessionWithChatAssistantCreate, The, ThreadPoolExecutor, Unless, Version, WARRANTIES, WITHOUT, You, pytest, test_add_session_to_deleted_chat_assistant, test_concurrent_create_session, test_invalid_auth, test_invalid_chat_assistant_id, test_name

---
*Generated by RAGFlow Repository Documentation Generator*
