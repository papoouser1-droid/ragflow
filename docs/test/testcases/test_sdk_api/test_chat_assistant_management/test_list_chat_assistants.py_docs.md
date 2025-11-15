# Documentation: test/testcases/test_sdk_api/test_chat_assistant_management/test_list_chat_assistants.py

## File Metadata

- **Path**: `test/testcases/test_sdk_api/test_chat_assistant_management/test_list_chat_assistants.py`
- **Size**: 8587 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_sdk_api/test_chat_assistant_management/test_list_chat_assistants.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `concurrent.futures`
- `pytest`

### Classes Defined

This file defines 1 class(es):

#### Class: `TestChatAssistantsList` (line 22)

**Methods**: test_default, test_page, test_page_size, test_orderby, test_desc, test_name, test_id, test_name_and_id, test_concurrent_list, test_list_chats_after_deleting_associated_dataset

### Functions Defined

This file defines 10 function(s):

#### Function: `test_default` (line 24)

**Parameters**: self, client

#### Function: `test_page` (line 50)

**Parameters**: self, client, params, expected_page_size, expected_message

#### Function: `test_page_size` (line 81)

**Parameters**: self, client, params, expected_page_size, expected_message

#### Function: `test_orderby` (line 100)

**Parameters**: self, client, params, expected_message

#### Function: `test_desc` (line 127)

**Parameters**: self, client, params, expected_message

#### Function: `test_name` (line 145)

**Parameters**: self, client, params, expected_num, expected_message

#### Function: `test_id` (line 167)

**Parameters**: self, client, add_chat_assistants, chat_assistant_id, expected_num, expected_message

#### Function: `test_name_and_id` (line 195)

**Parameters**: self, client, add_chat_assistants, chat_assistant_id, name, expected_num, expected_message

#### Function: `test_concurrent_list` (line 211)

**Parameters**: self, client

#### Function: `test_list_chats_after_deleting_associated_dataset` (line 219)

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


@pytest.mark.usefixtures("add_chat_assistants")
class TestChatAssistantsList:
    @pytest.mark.p1
    def test_default(self, client):
        assistants = client.list_chats()
        assert len(assistants) == 5

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "params, expected_page_size, expected_message",
        [
            ({"page": 0, "page_size": 2}, 2, ""),
            ({"page": 2, "page_size": 2}, 2, ""),
            ({"page": 3, "page_size": 2}, 1, ""),
            ({"page": "3", "page_size": 2}, 0, "not instance of"),
            pytest.param(
                {"page": -1, "page_size": 2},
                0,
                "1064",
                marks=pytest.mark.skip(reason="issues/5851"),
            ),
            pytest.param(
                {"page": "a", "page_size": 2},
                0,
                """ValueError("invalid literal for int() with base 10: \'a\'")""",
                marks=pytest.mark.skip(reason="issues/5851"),
            ),
        ],
    )
    def test_page(self, client, params, expected_page_size, expected_message):
        if expected_message:
            with pytest.raises(Exception) as excinfo:
                client.list_chats(**params)
            assert expected_message in str(excinfo.value)
        else:
            assistants = client.list_chats(**params)
            assert len(assistants) == expected_page_size

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "params, expected_page_size, expected_message",
        [
            ({"page_size": 0}, 0, ""),
            ({"page_size": 1}, 1, ""),
            ({"page_size": 6}, 5, ""),
            ({"page_size": "1"}, 0, "not instance of"),
            pytest.param(
                {"page_size": -1},
                0,
                "1064",
                marks=pytest.mark.skip(reason="issues/5851"),
            ),
            pytest.param(
                {"page_size": "a"},
                0,
                """ValueError("invalid literal for int() with base 10: \'a\'")""",
                marks=pytest.mark.skip(reason="issues/5851"),
            ),
        ],
    )
    def test_page_size(self, client, params, expected_page_size, expected_message):
        if expected_message:
            with pytest.raises(Exception) as excinfo:
                client.list_chats(**params)
            assert expected_message in str(excinfo.value)
        else:
            assistants = client.list_chats(**params)
            assert len(assistants) == expected_page_size

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "params, expected_message",
        [
            ({"orderby": "create_time"}, ""),
            ({"orderby": "update_time"}, ""),
            pytest.param({"orderby": "name", "desc": "False"}, "", marks=pytest.mark.skip(reason="issues/5851")),
            pytest.param({"orderby": "unknown"}, "orderby should be create_time or update_time", marks=pytest.mark.skip(reason="issues/5851")),
        ],
    )
    def test_orderby(self, client, params, expected_message):
        if expected_message:
            with pytest.raises(Exception) as excinfo:
                client.list_chats(**params)
            assert expected_message in str(excinfo.value)
        else:
            client.list_chats(**params)

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
            pytest.param(
                {"desc": "unknown"},
                "desc should be true or false",
                marks=pytest.mark.skip(reason="issues/5851"),
            ),
        ],
    )
    def test_desc(self, client, params, expected_message):
        if expected_message:
            with pytest.raises(Exception) as excinfo:
                client.list_chats(**params)
            assert expected_message in str(excinfo.value)
        else:
            client.list_chats(**params)

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "params, expected_num, expected_message",
        [
            ({"name": None}, 5, ""),
            ({"name": ""}, 5, ""),
            ({"name": "test_chat_assistant_1"}, 1, ""),
            ({"name": "unknown"}, 0, "The chat doesn't exist"),
        ],
    )
    def test_name(self, client, params, expected_num, expected_message):
        if expected_message:
            with pytest.raises(Exception) as excinfo:
                client.list_chats(**params)
            assert expected_message in str(excinfo.value)
        else:
            assistants = client.list_chats(**params)
            if params["name"] in [None, ""]:
                assert len(assistants) == expected_num
            else:
                assert assistants[0].name == params["name"]

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "chat_assistant_id, expected_num, expected_message",
        [
            (None, 5, ""),
            ("", 5, ""),
            (lambda r: r[0], 1, ""),
            ("unknown", 0, "The chat doesn't exist"),
        ],
    )
    def test_id(self, client, add_chat_assistants, chat_assistant_id, expected_num, expected_message):
        _, _, chat_assistants = add_chat_assistants
        if callable(chat_assistant_id):
            params = {"id": chat_assistant_id([chat.id for chat in chat_assistants])}
        else:
            params = {"id": chat_assistant_id}

        if expected_message:
            with pytest.raises(Exception) as excinfo:
                client.list_chats(**params)
            assert expected_message in str(excinfo.value)
        else:
            assistants = client.list_chats(**params)
            if params["id"] in [None, ""]:
                assert len(assistants) == expected_num
            else:
                assert assistants[0].id == params["id"]

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "chat_assistant_id, name, expected_num, expected_message",
        [
            (lambda r: r[0], "test_chat_assistant_0", 1, ""),
            (lambda r: r[0], "test_chat_assistant_1", 0, "The chat doesn't exist"),
            (lambda r: r[0], "unknown", 0, "The chat doesn't exist"),
            ("id", "chat_assistant_0", 0, "The chat doesn't exist"),
        ],
    )
    def test_name_and_id(self, client, add_chat_assistants, chat_assistant_id, name, expected_num, expected_message):
        _, _, chat_assistants = add_chat_assistants
        if callable(chat_assistant_id):
            params = {"id": chat_assistant_id([chat.id for chat in chat_assistants]), "name": name}
        else:
            params = {"id": chat_assistant_id, "name": name}

        if expected_message:
            with pytest.raises(Exception) as excinfo:
                client.list_chats(**params)
            assert expected_message in str(excinfo.value)
        else:
            assistants = client.list_chats(**params)
            assert len(assistants) == expected_num

    @pytest.mark.p3
    def test_concurrent_list(self, client):
        count = 100
        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [executor.submit(client.list_chats) for _ in range(count)]
        responses = list(as_completed(futures))
        assert len(responses) == count, responses

    @pytest.mark.p2
    def test_list_chats_after_deleting_associated_dataset(self, client, add_chat_assistants):
        dataset, _, _ = add_chat_assistants
        client.delete_datasets(ids=[dataset.id])

        assistants = client.list_chats()
        assert len(assistants) == 5

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_sdk_api/test_chat_assistant_management/test_list_chat_assistants.py` is located in the `test/testcases/test_sdk_api/test_chat_assistant_management` directory.

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
- [test_delete_chat_assistants.py](test_delete_chat_assistants.py_docs.md)
- [test_update_chat_assistant.py](test_update_chat_assistant.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
