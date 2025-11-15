# Documentation: test/testcases/test_sdk_api/test_chat_assistant_management/test_delete_chat_assistants.py

## File Metadata

- **Path**: `test/testcases/test_sdk_api/test_chat_assistant_management/test_delete_chat_assistants.py`
- **Size**: 4365 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_sdk_api/test_chat_assistant_management/test_delete_chat_assistants.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `concurrent.futures`
- `pytest`
- `common`

### Classes Defined

This file defines 1 class(es):

#### Class: `TestChatAssistantsDelete` (line 22)

**Methods**: test_basic_scenarios, test_delete_partial_invalid_id, test_repeated_deletion, test_duplicate_deletion, test_concurrent_deletion, test_delete_1k

### Functions Defined

This file defines 6 function(s):

#### Function: `test_basic_scenarios` (line 34)

**Parameters**: self, client, add_chat_assistants_func, payload, expected_message, remaining

#### Function: `test_delete_partial_invalid_id` (line 60)

**Parameters**: self, client, add_chat_assistants_func, payload

#### Function: `test_repeated_deletion` (line 69)

**Parameters**: self, client, add_chat_assistants_func

#### Function: `test_duplicate_deletion` (line 79)

**Parameters**: self, client, add_chat_assistants_func

#### Function: `test_concurrent_deletion` (line 88)

**Parameters**: self, client

#### Function: `test_delete_1k` (line 100)

**Parameters**: self, client

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
from common import batch_create_chat_assistants


class TestChatAssistantsDelete:
    @pytest.mark.parametrize(
        "payload, expected_message, remaining",
        [
            pytest.param(None, "", 0, marks=pytest.mark.p3),
            pytest.param({"ids": []}, "", 0, marks=pytest.mark.p3),
            pytest.param({"ids": ["invalid_id"]}, "Assistant(invalid_id) not found.", 5, marks=pytest.mark.p3),
            pytest.param({"ids": ["\n!?。；！？\"'"]}, """Assistant(\n!?。；！？"\') not found.""", 5, marks=pytest.mark.p3),
            pytest.param(lambda r: {"ids": r[:1]}, "", 4, marks=pytest.mark.p3),
            pytest.param(lambda r: {"ids": r}, "", 0, marks=pytest.mark.p1),
        ],
    )
    def test_basic_scenarios(self, client, add_chat_assistants_func, payload, expected_message, remaining):
        _, _, chat_assistants = add_chat_assistants_func
        if callable(payload):
            payload = payload([chat_assistant.id for chat_assistant in chat_assistants])

        if expected_message:
            with pytest.raises(Exception) as excinfo:
                client.delete_chats(**payload)
            assert expected_message in str(excinfo.value)
        else:
            if payload is None:
                client.delete_chats(payload)
            else:
                client.delete_chats(**payload)

        assistants = client.list_chats()
        assert len(assistants) == remaining

    @pytest.mark.parametrize(
        "payload",
        [
            pytest.param(lambda r: {"ids": ["invalid_id"] + r}, marks=pytest.mark.p3),
            pytest.param(lambda r: {"ids": r[:1] + ["invalid_id"] + r[1:5]}, marks=pytest.mark.p1),
            pytest.param(lambda r: {"ids": r + ["invalid_id"]}, marks=pytest.mark.p3),
        ],
    )
    def test_delete_partial_invalid_id(self, client, add_chat_assistants_func, payload):
        _, _, chat_assistants = add_chat_assistants_func
        payload = payload([chat_assistant.id for chat_assistant in chat_assistants])
        client.delete_chats(**payload)

        assistants = client.list_chats()
        assert len(assistants) == 0

    @pytest.mark.p3
    def test_repeated_deletion(self, client, add_chat_assistants_func):
        _, _, chat_assistants = add_chat_assistants_func
        chat_ids = [chat.id for chat in chat_assistants]
        client.delete_chats(ids=chat_ids)

        with pytest.raises(Exception) as excinfo:
            client.delete_chats(ids=chat_ids)
        assert "not found" in str(excinfo.value)

    @pytest.mark.p3
    def test_duplicate_deletion(self, client, add_chat_assistants_func):
        _, _, chat_assistants = add_chat_assistants_func
        chat_ids = [chat.id for chat in chat_assistants]
        client.delete_chats(ids=chat_ids + chat_ids)

        assistants = client.list_chats()
        assert len(assistants) == 0

    @pytest.mark.p3
    def test_concurrent_deletion(self, client):
        count = 100
        chat_ids = [client.create_chat(name=f"test_{i}").id for i in range(count)]

        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [executor.submit(client.delete_chats, ids=[chat_ids[i]]) for i in range(count)]
            responses = list(as_completed(futures))

        assert len(responses) == count
        assert all(future.exception() is None for future in futures)

    @pytest.mark.p3
    def test_delete_1k(self, client):
        chat_assistants = batch_create_chat_assistants(client, 1_000)
        client.delete_chats(ids=[chat_assistants.id for chat_assistants in chat_assistants])

        assistants = client.list_chats()
        assert len(assistants) == 0

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_sdk_api/test_chat_assistant_management/test_delete_chat_assistants.py` is located in the `test/testcases/test_sdk_api/test_chat_assistant_management` directory.

This file is part of the **Testing** infrastructure.

### Architecture Context

Files in this location typically handle concerns related to test_chat_assistant_management.

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
- [test_create_chat_assistant.py](test_create_chat_assistant.py_docs.md)
- [test_list_chat_assistants.py](test_list_chat_assistants.py_docs.md)
- [test_update_chat_assistant.py](test_update_chat_assistant.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
