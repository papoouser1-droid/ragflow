# File Documentation: test/testcases/test_sdk_api/test_session_management/test_list_sessions_with_chat_assistant.py

## File Metadata

- **Path**: `test/testcases/test_sdk_api/test_session_management/test_list_sessions_with_chat_assistant.py`
- **Extension**: `.py`
- **Lines**: 204
- **Characters**: 8,973
- **Size**: 8,973 bytes
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
import pytest
from concurrent.futures import ThreadPoolExecutor, as_completed


class TestSessionsWithChatAssistantList:
    @pytest.mark.p1
    @pytest.mark.parametrize(
        "params, expected_page_size, expected_message",
        [
            ({"page": None, "page_size": 2}, 0, "not instance of"),
            pytest.param({"page": 0, "page_size": 2}, 0, "ValueError('Search does not support negative slicing.')", marks=pytest.mark.skip),
            ({"page": 2, "page_size": 2}, 2, ""),
            ({"page": 3, "page_size": 2}, 1, ""),
            ({"page": "3", "page_size": 2}, 0, "not instance of"),
            pytest.param({"page": -1, "page_size": 2}, 0, "ValueError('Search does not support negative slicing.')", marks=pytest.mark.skip),
            pytest.param({"page": "a", "page_size": 2}, 0, """ValueError("invalid literal for int() with base 10: \'a\'")""", marks=pytest.mark.skip),
        ],
    )
    def test_page(self, add_sessions_with_chat_assistant, params, expected_page_size, expected_message):
        chat_assistant, _ = add_sessions_with_chat_assistant
        if expected_message:
            with pytest.raises(Exception) as excinfo:
                chat_assistant.list_sessions(**params)
            assert expected_message in str(excinfo.value)
        else:
            sessions = chat_assistant.list_sessions(**params)
            assert len(sessions) == expected_page_size

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "params, expected_page_size, expected_message",
        [
            ({"page_size": None}, 0, "not instance of"),
            ({"page_size": 0}, 0, ""),
            ({"page_size": 1}, 1, ""),
            ({"page_size": 6}, 5, ""),
            ({"page_size": "1"}, 0, "not instance of"),
            pytest.param({"page_size": -1}, 5, "", marks=pytest.mark.skip),
            pytest.param({"page_size": "a"}, 0, """ValueError("invalid literal for int() with base 10: \'a\'")""", marks=pytest.mark.skip),
        ],
    )
    def test_page_size(self, add_sessions_with_chat_assistant, params, expected_page_size, expected_message):
        chat_assistant, _ = add_sessions_with_chat_assistant
        if expected_message:
            with pytest.raises(Exception) as excinfo:
                chat_assistant.list_sessions(**params)
            assert expected_message in str(excinfo.value)
        else:
            sessions = chat_assistant.list_sessions(**params)
            assert len(sessions) == expected_page_size

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "params, expected_message",
        [
            ({"orderby": None}, "not instance of"),
            ({"orderby": "create_time"}, ""),
            ({"orderby": "update_time"}, ""),
            ({"orderby": "name", "desc": "False"}, "not instance of"),
            pytest.param({"orderby": "unknown"}, "orderby should be create_time or update_time", marks=pytest.mark.skip(reason="issues/")),
        ],
    )
    def test_orderby(self, add_sessions_with_chat_assistant, params, expected_message):
        chat_assistant, _ = add_sessions_with_chat_assistant
        if expected_message:
            with pytest.raises(Exception) as excinfo:
                chat_assistant.list_sessions(**params)
            assert expected_message in str(excinfo.value)
        else:
            chat_assistant.list_sessions(**params)

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "params, expected_message",
        [
            ({"desc": None}, "not instance of"),
            ({"desc": "true"}, "not instance of"),
            ({"desc": "True"}, "not instance of"),
            ({"desc": True}, ""),
            ({"desc": "false"}, "not instance of"),
            ({"desc": "False"}, "not instance of"),
            ({"desc": False}, ""),
            ({"desc": "False", "orderby": "update_time"}, "not instance of"),
            pytest.param({"desc": "unknown"}, "desc should be true or false", marks=pytest.mark.skip(reason="issues/")),
        ],
    )
    def test_desc(self, add_sessions_with_chat_assistant, params, expected_message):
        chat_assistant, _ = add_sessions_with_chat_assistant
        if expected_message:
            with pytest.raises(Exception) as excinfo:
                chat_assistant.list_sessions(**params)
            assert expected_message in str(excinfo.value)
        else:
            chat_assistant.list_sessions(**params)

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "params, expected_num, expected_message",
        [
            ({"name": None}, 0, "not instance of"),
            ({"name": ""}, 5, ""),
            ({"name": "session_with_chat_assistant_1"}, 1, ""),
            ({"name": "unknown"}, 0, ""),
        ],
    )
    def test_name(self, add_sessions_with_chat_assistant, params, expected_num, expected_message):
        chat_assistant, _ = add_sessions_with_chat_assistant
        if expected_message:
            with pytest.raises(Exception) as excinfo:
                chat_assistant.list_sessions(**params)
            assert expected_message in str(excinfo.value)
        else:
            sessions = chat_assistant.list_sessions(**params)
            if params["name"] == "session_with_chat_assistant_1":
                assert sessions[0].name == params["name"]
            else:
                assert len(sessions) == expected_num

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "session_id, expected_num, expected_message",
        [
            (None, 0, "not instance of"),
            ("", 5, ""),
            (lambda r: r[0], 1, ""),
            ("unknown", 0, ""),
        ],
    )
    def test_id(self, add_sessions_with_chat_assistant, session_id, expected_num, expected_message):
        chat_assistant, sessions = add_sessions_with_chat_assistant
        if callable(session_id):
            params = {"id": session_id([s.id for s in sessions])}
        else:
            params = {"id": session_id}

        if expected_message:
            with pytest.raises(Exception) as excinfo:
                chat_assistant.list_sessions(**params)
            assert expected_message in str(excinfo.value)
        else:
            list_sessions = chat_assistant.list_sessions(**params)
            if "id" in params and params["id"] == sessions[0].id:
                assert list_sessions[0].id == params["id"]
            else:
                assert len(list_sessions) == expected_num

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "session_id, name, expected_num, expected_message",
        [
            (lambda r: r[0], "session_with_chat_assistant_0", 1, ""),
            (lambda r: r[0], "session_with_chat_assistant_100", 0, ""),
            (lambda r: r[0], "unknown", 0, ""),
            ("id", "session_with_chat_assistant_0", 0, ""),
        ],
    )
    def test_name_and_id(self, add_sessions_with_chat_assistant, session_id, name, expected_num, expected_message):
        chat_assistant, sessions = add_sessions_with_chat_assistant
        if callable(session_id):
            params = {"id": session_id([s.id for s in sessions]), "name": name}
        else:
            params = {"id": session_id, "name": name}

        if expected_message:
            with pytest.raises(Exception) as excinfo:
                chat_assistant.list_sessions(**params)
            assert expected_message in str(excinfo.value)
        else:
            list_sessions = chat_assistant.list_sessions(**params)
            assert len(list_sessions) == expected_num

    @pytest.mark.p3
    def test_concurrent_list(self, add_sessions_with_chat_assistant):
        count = 100
        chat_assistant, _ = add_sessions_with_chat_assistant
        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [executor.submit(chat_assistant.list_sessions) for i in range(count)]
        responses = list(as_completed(futures))
        assert len(responses) == count, responses

    @pytest.mark.p3
    def test_list_chats_after_deleting_associated_chat_assistant(self, client, add_sessions_with_chat_assistant):
        chat_assistant, _ = add_sessions_with_chat_assistant
        client.delete_chats(ids=[chat_assistant.id])

        with pytest.raises(Exception) as excinfo:
            chat_assistant.list_sessions()
        assert "You don't own the assistant" in str(excinfo.value)

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

### Classes (1)

- `TestSessionsWithChatAssistantList`: Class definition

### Imports (2)

- `import pytest`
- `from concurrent.futures import ThreadPoolExecutor, as_completed`

## Code Structure Analysis

- Total lines: 204
- Blank lines: 14 (6.9%)
- Comment lines: ~15 (7.4%)
- Code lines: ~175


## Dependencies and Imports

- `import pytest`
- `from concurrent.futures import ThreadPoolExecutor, as_completed`

## Design & Architecture

This file is located in the `test` directory, specifically within `test/testcases/test_sdk_api/test_session_management`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 6 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `test/testcases/test_sdk_api/test_session_management/` directory
- Imports from `concurrent.futures`

## Keywords

ANY, All, Apache, Authors, BASIS, CONDITIONS, Copyright, Exception, False, InfiniFlow, KIND, LICENSE, License, Licensed, None, Python, Reserved, Rights, Search, See, TestSessionsWithChatAssistantList, The, ThreadPoolExecutor, True, Unless, ValueError, Version, WARRANTIES, WITHOUT, You, pytest, test_concurrent_list, test_desc, test_id, test_list_chats_after_deleting_associated_chat_assistant, test_name, test_name_and_id, test_orderby, test_page, test_page_size

---
*Generated by RAGFlow Repository Documentation Generator*
