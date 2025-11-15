# Documentation: sdk/python/test/test_http_api/test_session_management/test_list_sessions_with_chat_assistant.py

## File Metadata

- **Path**: `sdk/python/test/test_http_api/test_session_management/test_list_sessions_with_chat_assistant.py`
- **Size**: 11352 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/test/test_http_api/test_session_management/test_list_sessions_with_chat_assistant.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `concurrent.futures`
- `pytest`
- `common`
- `libs.auth`
- `libs.utils`

### Classes Defined

This file defines 2 class(es):

#### Class: `TestAuthorization` (line 25)

**Methods**: test_invalid_auth

#### Class: `TestSessionsWithChatAssistantList` (line 43)

**Methods**: test_page, test_page_size, test_orderby, test_desc, test_name, test_id, test_name_and_id, test_concurrent_list, test_invalid_params, test_list_chats_after_deleting_associated_chat_assistant

### Functions Defined

This file defines 11 function(s):

#### Function: `test_invalid_auth` (line 37)

**Parameters**: self, auth, expected_code, expected_message

#### Function: `test_page` (line 57)

**Parameters**: self, get_http_api_auth, add_sessions_with_chat_assistant, params, expected_code, expected_page_size, expected_message

#### Function: `test_page_size` (line 79)

**Parameters**: self, get_http_api_auth, add_sessions_with_chat_assistant, params, expected_code, expected_page_size, expected_message

#### Function: `test_orderby` (line 99)

**Parameters**: self, get_http_api_auth, add_sessions_with_chat_assistant, params, expected_code, assertions, expected_message

#### Function: `test_desc` (line 132)

**Parameters**: self, get_http_api_auth, add_sessions_with_chat_assistant, params, expected_code, assertions, expected_message

#### Function: `test_name` (line 160)

**Parameters**: self, get_http_api_auth, add_sessions_with_chat_assistant, params, expected_code, expected_num, expected_message

#### Function: `test_id` (line 182)

**Parameters**: self, get_http_api_auth, add_sessions_with_chat_assistant, session_id, expected_code, expected_num, expected_message

#### Function: `test_name_and_id` (line 209)

**Parameters**: self, get_http_api_auth, add_sessions_with_chat_assistant, session_id, name, expected_code, expected_num, expected_message

#### Function: `test_concurrent_list` (line 224)

**Parameters**: self, get_http_api_auth, add_sessions_with_chat_assistant

#### Function: `test_invalid_params` (line 232)

**Parameters**: self, get_http_api_auth, add_sessions_with_chat_assistant

#### Function: `test_list_chats_after_deleting_associated_chat_assistant` (line 240)

**Parameters**: self, get_http_api_auth, add_sessions_with_chat_assistant

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
from common import INVALID_API_TOKEN, delete_chat_assistants, list_session_with_chat_assistants
from libs.auth import RAGFlowHttpApiAuth
from libs.utils import is_sorted


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
        res = list_session_with_chat_assistants(auth, "chat_assistant_id")
        assert res["code"] == expected_code
        assert res["message"] == expected_message


class TestSessionsWithChatAssistantList:
    @pytest.mark.p1
    @pytest.mark.parametrize(
        "params, expected_code, expected_page_size, expected_message",
        [
            ({"page": None, "page_size": 2}, 0, 2, ""),
            pytest.param({"page": 0, "page_size": 2}, 100, 0, "ValueError('Search does not support negative slicing.')", marks=pytest.mark.skip),
            ({"page": 2, "page_size": 2}, 0, 2, ""),
            ({"page": 3, "page_size": 2}, 0, 1, ""),
            ({"page": "3", "page_size": 2}, 0, 1, ""),
            pytest.param({"page": -1, "page_size": 2}, 100, 0, "ValueError('Search does not support negative slicing.')", marks=pytest.mark.skip),
            pytest.param({"page": "a", "page_size": 2}, 100, 0, """ValueError("invalid literal for int() with base 10: \'a\'")""", marks=pytest.mark.skip),
        ],
    )
    def test_page(self, get_http_api_auth, add_sessions_with_chat_assistant, params, expected_code, expected_page_size, expected_message):
        chat_assistant_id, _ = add_sessions_with_chat_assistant
        res = list_session_with_chat_assistants(get_http_api_auth, chat_assistant_id, params=params)
        assert res["code"] == expected_code
        if expected_code == 0:
            assert len(res["data"]) == expected_page_size
        else:
            assert res["message"] == expected_message

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "params, expected_code, expected_page_size, expected_message",
        [
            ({"page_size": None}, 0, 5, ""),
            ({"page_size": 0}, 0, 0, ""),
            ({"page_size": 1}, 0, 1, ""),
            ({"page_size": 6}, 0, 5, ""),
            ({"page_size": "1"}, 0, 1, ""),
            pytest.param({"page_size": -1}, 0, 5, "", marks=pytest.mark.skip),
            pytest.param({"page_size": "a"}, 100, 0, """ValueError("invalid literal for int() with base 10: \'a\'")""", marks=pytest.mark.skip),
        ],
    )
    def test_page_size(self, get_http_api_auth, add_sessions_with_chat_assistant, params, expected_code, expected_page_size, expected_message):
        chat_assistant_id, _ = add_sessions_with_chat_assistant
        res = list_session_with_chat_assistants(get_http_api_auth, chat_assistant_id, params=params)
        assert res["code"] == expected_code
        if expected_code == 0:
            assert len(res["data"]) == expected_page_size
        else:
            assert res["message"] == expected_message

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "params, expected_code, assertions, expected_message",
        [
            ({"orderby": None}, 0, lambda r: (is_sorted(r["data"], "create_time", True)), ""),
            ({"orderby": "create_time"}, 0, lambda r: (is_sorted(r["data"], "create_time", True)), ""),
            ({"orderby": "update_time"}, 0, lambda r: (is_sorted(r["data"], "update_time", True)), ""),
            ({"orderby": "name", "desc": "False"}, 0, lambda r: (is_sorted(r["data"], "name", False)), ""),
            pytest.param({"orderby": "unknown"}, 102, 0, "orderby should be create_time or update_time", marks=pytest.mark.skip(reason="issues/")),
        ],
    )
    def test_orderby(
        self,
        get_http_api_auth,
        add_sessions_with_chat_assistant,
        params,
        expected_code,
        assertions,
        expected_message,
    ):
        chat_assistant_id, _ = add_sessions_with_chat_assistant
        res = list_session_with_chat_assistants(get_http_api_auth, chat_assistant_id, params=params)
        assert res["code"] == expected_code
        if expected_code == 0:
            if callable(assertions):
                assert assertions(res)
        else:
            assert res["message"] == expected_message

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "params, expected_code, assertions, expected_message",
        [
            ({"desc": None}, 0, lambda r: (is_sorted(r["data"], "create_time", True)), ""),
            ({"desc": "true"}, 0, lambda r: (is_sorted(r["data"], "create_time", True)), ""),
            ({"desc": "True"}, 0, lambda r: (is_sorted(r["data"], "create_time", True)), ""),
            ({"desc": True}, 0, lambda r: (is_sorted(r["data"], "create_time", True)), ""),
            ({"desc": "false"}, 0, lambda r: (is_sorted(r["data"], "create_time", False)), ""),
            ({"desc": "False"}, 0, lambda r: (is_sorted(r["data"], "create_time", False)), ""),
            ({"desc": False}, 0, lambda r: (is_sorted(r["data"], "create_time", False)), ""),
            ({"desc": "False", "orderby": "update_time"}, 0, lambda r: (is_sorted(r["data"], "update_time", False)), ""),
            pytest.param({"desc": "unknown"}, 102, 0, "desc should be true or false", marks=pytest.mark.skip(reason="issues/")),
        ],
    )
    def test_desc(
        self,
        get_http_api_auth,
        add_sessions_with_chat_assistant,
        params,
        expected_code,
        assertions,
        expected_message,
    ):
        chat_assistant_id, _ = add_sessions_with_chat_assistant
        res = list_session_with_chat_assistants(get_http_api_auth, chat_assistant_id, params=params)
        assert res["code"] == expected_code
        if expected_code == 0:
            if callable(assertions):
                assert assertions(res)
        else:
            assert res["message"] == expected_message

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "params, expected_code, expected_num, expected_message",
        [
            ({"name": None}, 0, 5, ""),
            ({"name": ""}, 0, 5, ""),
            ({"name": "session_with_chat_assistant_1"}, 0, 1, ""),
            ({"name": "unknown"}, 0, 0, ""),
        ],
    )
    def test_name(self, get_http_api_auth, add_sessions_with_chat_assistant, params, expected_code, expected_num, expected_message):
        chat_assistant_id, _ = add_sessions_with_chat_assistant
        res = list_session_with_chat_assistants(get_http_api_auth, chat_assistant_id, params=params)
        assert res["code"] == expected_code
        if expected_code == 0:
            if params["name"] == "session_with_chat_assistant_1":
                assert res["data"][0]["name"] == params["name"]
            else:
                assert len(res["data"]) == expected_num
        else:
            assert res["message"] == expected_message

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "session_id, expected_code, expected_num, expected_message",
        [
            (None, 0, 5, ""),
            ("", 0, 5, ""),
            (lambda r: r[0], 0, 1, ""),
            ("unknown", 0, 0, "The chat doesn't exist"),
        ],
    )
    def test_id(self, get_http_api_auth, add_sessions_with_chat_assistant, session_id, expected_code, expected_num, expected_message):
        chat_assistant_id, session_ids = add_sessions_with_chat_assistant
        if callable(session_id):
            params = {"id": session_id(session_ids)}
        else:
            params = {"id": session_id}

        res = list_session_with_chat_assistants(get_http_api_auth, chat_assistant_id, params=params)
        assert res["code"] == expected_code
        if expected_code == 0:
            if params["id"] == session_ids[0]:
                assert res["data"][0]["id"] == params["id"]
            else:
                assert len(res["data"]) == expected_num
        else:
            assert res["message"] == expected_message

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "session_id, name, expected_code, expected_num, expected_message",
        [
            (lambda r: r[0], "session_with_chat_assistant_0", 0, 1, ""),
            (lambda r: r[0], "session_with_chat_assistant_100", 0, 0, ""),
            (lambda r: r[0], "unknown", 0, 0, ""),
            ("id", "session_with_chat_assistant_0", 0, 0, ""),
        ],
    )
    def test_name_and_id(self, get_http_api_auth, add_sessions_with_chat_assistant, session_id, name, expected_code, expected_num, expected_message):
        chat_assistant_id, session_ids = add_sessions_with_chat_assistant
        if callable(session_id):
            params = {"id": session_id(session_ids), "name": name}
        else:
            params = {"id": session_id, "name": name}

        res = list_session_with_chat_assistants(get_http_api_auth, chat_assistant_id, params=params)
        assert res["code"] == expected_code
        if expected_code == 0:
            assert len(res["data"]) == expected_num
        else:
            assert res["message"] == expected_message

    @pytest.mark.p3
    def test_concurrent_list(self, get_http_api_auth, add_sessions_with_chat_assistant):
        chat_assistant_id, _ = add_sessions_with_chat_assistant
        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [executor.submit(list_session_with_chat_assistants, get_http_api_auth, chat_assistant_id) for i in range(100)]
        responses = [f.result() for f in futures]
        assert all(r["code"] == 0 for r in responses)

    @pytest.mark.p3
    def test_invalid_params(self, get_http_api_auth, add_sessions_with_chat_assistant):
        chat_assistant_id, _ = add_sessions_with_chat_assistant
        params = {"a": "b"}
        res = list_session_with_chat_assistants(get_http_api_auth, chat_assistant_id, params=params)
        assert res["code"] == 0
        assert len(res["data"]) == 5

    @pytest.mark.p3
    def test_list_chats_after_deleting_associated_chat_assistant(self, get_http_api_auth, add_sessions_with_chat_assistant):
        chat_assistant_id, _ = add_sessions_with_chat_assistant
        res = delete_chat_assistants(get_http_api_auth, {"ids": [chat_assistant_id]})
        assert res["code"] == 0

        res = list_session_with_chat_assistants(get_http_api_auth, chat_assistant_id)
        assert res["code"] == 102
        assert "You don't own the assistant" in res["message"]

```

## Detailed Analysis

### File Role in Repository

The file `sdk/python/test/test_http_api/test_session_management/test_list_sessions_with_chat_assistant.py` is located in the `sdk/python/test/test_http_api/test_session_management` directory.

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
- [test_delete_sessions_with_chat_assistant.py](test_delete_sessions_with_chat_assistant.py_docs.md)
- [test_update_session_with_chat_assistant.py](test_update_session_with_chat_assistant.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
