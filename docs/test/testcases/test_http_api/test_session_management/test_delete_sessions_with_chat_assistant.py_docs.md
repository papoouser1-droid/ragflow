# Documentation: test/testcases/test_http_api/test_session_management/test_delete_sessions_with_chat_assistant.py

## File Metadata

- **Path**: `test/testcases/test_http_api/test_session_management/test_delete_sessions_with_chat_assistant.py`
- **Size**: 7445 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_http_api/test_session_management/test_delete_sessions_with_chat_assistant.py`.

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

#### Class: `TestSessionWithChatAssistantDelete` (line 43)

**Methods**: test_invalid_chat_assistant_id, test_delete_partial_invalid_id, test_repeated_deletion, test_duplicate_deletion, test_concurrent_deletion, test_delete_1k, test_basic_scenarios

### Functions Defined

This file defines 8 function(s):

#### Function: `test_invalid_auth` (line 37)

**Parameters**: self, invalid_auth, expected_code, expected_message

#### Function: `test_invalid_chat_assistant_id` (line 56)

**Parameters**: self, HttpApiAuth, add_sessions_with_chat_assistant_func, chat_assistant_id, expected_code, expected_message

#### Function: `test_delete_partial_invalid_id` (line 70)

**Parameters**: self, HttpApiAuth, add_sessions_with_chat_assistant_func, payload

#### Function: `test_repeated_deletion` (line 84)

**Parameters**: self, HttpApiAuth, add_sessions_with_chat_assistant_func

#### Function: `test_duplicate_deletion` (line 95)

**Parameters**: self, HttpApiAuth, add_sessions_with_chat_assistant_func

#### Function: `test_concurrent_deletion` (line 108)

**Parameters**: self, HttpApiAuth, add_chat_assistants

#### Function: `test_delete_1k` (line 128)

**Parameters**: self, HttpApiAuth, add_chat_assistants

#### Function: `test_basic_scenarios` (line 152)

**Parameters**: self, HttpApiAuth, add_sessions_with_chat_assistant_func, payload, expected_code, expected_message, remaining

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
from common import batch_add_sessions_with_chat_assistant, delete_session_with_chat_assistants, list_session_with_chat_assistants
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
        res = delete_session_with_chat_assistants(invalid_auth, "chat_assistant_id")
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
    def test_invalid_chat_assistant_id(self, HttpApiAuth, add_sessions_with_chat_assistant_func, chat_assistant_id, expected_code, expected_message):
        _, session_ids = add_sessions_with_chat_assistant_func
        res = delete_session_with_chat_assistants(HttpApiAuth, chat_assistant_id, {"ids": session_ids})
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
    def test_delete_partial_invalid_id(self, HttpApiAuth, add_sessions_with_chat_assistant_func, payload):
        chat_assistant_id, session_ids = add_sessions_with_chat_assistant_func
        if callable(payload):
            payload = payload(session_ids)
        res = delete_session_with_chat_assistants(HttpApiAuth, chat_assistant_id, payload)
        assert res["code"] == 0
        assert res["data"]["errors"][0] == "The chat doesn't own the session invalid_id"

        res = list_session_with_chat_assistants(HttpApiAuth, chat_assistant_id)
        if res["code"] != 0:
            assert False, res
        assert len(res["data"]) == 0

    @pytest.mark.p3
    def test_repeated_deletion(self, HttpApiAuth, add_sessions_with_chat_assistant_func):
        chat_assistant_id, session_ids = add_sessions_with_chat_assistant_func
        payload = {"ids": session_ids}
        res = delete_session_with_chat_assistants(HttpApiAuth, chat_assistant_id, payload)
        assert res["code"] == 0

        res = delete_session_with_chat_assistants(HttpApiAuth, chat_assistant_id, payload)
        assert res["code"] == 102
        assert "The chat doesn't own the session" in res["message"]

    @pytest.mark.p3
    def test_duplicate_deletion(self, HttpApiAuth, add_sessions_with_chat_assistant_func):
        chat_assistant_id, session_ids = add_sessions_with_chat_assistant_func
        res = delete_session_with_chat_assistants(HttpApiAuth, chat_assistant_id, {"ids": session_ids * 2})
        assert res["code"] == 0
        assert "Duplicate session ids" in res["data"]["errors"][0]
        assert res["data"]["success_count"] == 5

        res = list_session_with_chat_assistants(HttpApiAuth, chat_assistant_id)
        if res["code"] != 0:
            assert False, res
        assert len(res["data"]) == 0

    @pytest.mark.p3
    def test_concurrent_deletion(self, HttpApiAuth, add_chat_assistants):
        count = 100
        _, _, chat_assistant_ids = add_chat_assistants
        session_ids = batch_add_sessions_with_chat_assistant(HttpApiAuth, chat_assistant_ids[0], count)

        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [
                executor.submit(
                    delete_session_with_chat_assistants,
                    HttpApiAuth,
                    chat_assistant_ids[0],
                    {"ids": session_ids[i : i + 1]},
                )
                for i in range(count)
            ]
        responses = list(as_completed(futures))
        assert len(responses) == count, responses
        assert all(future.result()["code"] == 0 for future in futures)

    @pytest.mark.p3
    def test_delete_1k(self, HttpApiAuth, add_chat_assistants):
        sessions_num = 1_000
        _, _, chat_assistant_ids = add_chat_assistants
        session_ids = batch_add_sessions_with_chat_assistant(HttpApiAuth, chat_assistant_ids[0], sessions_num)

        res = delete_session_with_chat_assistants(HttpApiAuth, chat_assistant_ids[0], {"ids": session_ids})
        assert res["code"] == 0

        res = list_session_with_chat_assistants(HttpApiAuth, chat_assistant_ids[0])
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
        HttpApiAuth,
        add_sessions_with_chat_assistant_func,
        payload,
        expected_code,
        expected_message,
        remaining,
    ):
        chat_assistant_id, session_ids = add_sessions_with_chat_assistant_func
        if callable(payload):
            payload = payload(session_ids)
        res = delete_session_with_chat_assistants(HttpApiAuth, chat_assistant_id, payload)
        assert res["code"] == expected_code
        if res["code"] != 0:
            assert res["message"] == expected_message

        res = list_session_with_chat_assistants(HttpApiAuth, chat_assistant_id)
        if res["code"] != 0:
            assert False, res
        assert len(res["data"]) == remaining

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_http_api/test_session_management/test_delete_sessions_with_chat_assistant.py` is located in the `test/testcases/test_http_api/test_session_management` directory.

This file is part of the **Testing** infrastructure.

### Architecture Context

Files in this location typically handle concerns related to test_session_management.

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
- [test_create_session_with_chat_assistant.py](test_create_session_with_chat_assistant.py_docs.md)
- [test_list_sessions_with_chat_assistant.py](test_list_sessions_with_chat_assistant.py_docs.md)
- [test_update_session_with_chat_assistant.py](test_update_session_with_chat_assistant.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
