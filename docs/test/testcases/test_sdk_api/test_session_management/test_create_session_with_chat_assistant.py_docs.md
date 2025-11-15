# Documentation: test/testcases/test_sdk_api/test_session_management/test_create_session_with_chat_assistant.py

## File Metadata

- **Path**: `test/testcases/test_sdk_api/test_session_management/test_create_session_with_chat_assistant.py`
- **Size**: 3134 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_sdk_api/test_session_management/test_create_session_with_chat_assistant.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `concurrent.futures`
- `pytest`
- `configs`

### Classes Defined

This file defines 1 class(es):

#### Class: `TestSessionWithChatAssistantCreate` (line 23)

**Methods**: test_name, test_concurrent_create_session, test_add_session_to_deleted_chat_assistant

### Functions Defined

This file defines 3 function(s):

#### Function: `test_name` (line 36)

**Parameters**: self, add_chat_assistants, name, expected_message

#### Function: `test_concurrent_create_session` (line 55)

**Parameters**: self, add_chat_assistants

#### Function: `test_add_session_to_deleted_chat_assistant` (line 69)

**Parameters**: self, client, add_chat_assistants

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
from configs import SESSION_WITH_CHAT_NAME_LIMIT


@pytest.mark.usefixtures("clear_session_with_chat_assistants")
class TestSessionWithChatAssistantCreate:
    @pytest.mark.p1
    @pytest.mark.parametrize(
        "name, expected_message",
        [
            ("valid_name", ""),
            pytest.param("a" * (SESSION_WITH_CHAT_NAME_LIMIT + 1), "", marks=pytest.mark.skip(reason="issues/")),
            pytest.param(1, "", marks=pytest.mark.skip(reason="issues/")),
            ("", "`name` can not be empty."),
            ("duplicated_name", ""),
            ("case insensitive", ""),
        ],
    )
    def test_name(self, add_chat_assistants, name, expected_message):
        _, _, chat_assistants = add_chat_assistants
        chat_assistant = chat_assistants[0]

        if name == "duplicated_name":
            chat_assistant.create_session(name=name)
        elif name == "case insensitive":
            chat_assistant.create_session(name=name.upper())

        if expected_message:
            with pytest.raises(Exception) as excinfo:
                chat_assistant.create_session(name=name)
            assert expected_message in str(excinfo.value)
        else:
            session = chat_assistant.create_session(name=name)
            assert session.name == name, str(session)
            assert session.chat_id == chat_assistant.id, str(session)

    @pytest.mark.p3
    def test_concurrent_create_session(self, add_chat_assistants):
        count = 1000
        _, _, chat_assistants = add_chat_assistants
        chat_assistant = chat_assistants[0]

        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [executor.submit(chat_assistant.create_session, name=f"session with chat assistant test {i}") for i in range(count)]
        responses = list(as_completed(futures))
        assert len(responses) == count, responses

        updated_sessions = chat_assistant.list_sessions(page_size=count * 2)
        assert len(updated_sessions) == count

    @pytest.mark.p3
    def test_add_session_to_deleted_chat_assistant(self, client, add_chat_assistants):
        _, _, chat_assistants = add_chat_assistants
        chat_assistant = chat_assistants[0]

        client.delete_chats(ids=[chat_assistant.id])
        with pytest.raises(Exception) as excinfo:
            chat_assistant.create_session(name="valid_name")
        assert "You do not own the assistant" in str(excinfo.value)

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_sdk_api/test_session_management/test_create_session_with_chat_assistant.py` is located in the `test/testcases/test_sdk_api/test_session_management` directory.

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
- [test_delete_sessions_with_chat_assistant.py](test_delete_sessions_with_chat_assistant.py_docs.md)
- [test_list_sessions_with_chat_assistant.py](test_list_sessions_with_chat_assistant.py_docs.md)
- [test_update_session_with_chat_assistant.py](test_update_session_with_chat_assistant.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
