# File Documentation: sdk/python/test/test_sdk_api/t_chat.py

## File Metadata

- **Path**: `sdk/python/test/test_sdk_api/t_chat.py`
- **Extension**: `.py`
- **Lines**: 132
- **Characters**: 4,371
- **Size**: 4,371 bytes
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

from common import HOST_ADDRESS
from ragflow_sdk import RAGFlow
from ragflow_sdk.modules.chat import Chat


def test_create_chat_with_name(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    kb = rag.create_dataset(name="test_create_chat")
    display_name = "ragflow.txt"
    with open("test_data/ragflow.txt", "rb") as file:
        blob = file.read()
    document = {"display_name": display_name, "blob": blob}
    documents = []
    documents.append(document)
    docs = kb.upload_documents(documents)
    for doc in docs:
        doc.add_chunk("This is a test to add chunk")
    llm = Chat.LLM(
        rag,
        {
            "model_name": "glm-4-flash@ZHIPU-AI",
            "temperature": 0.1,
            "top_p": 0.3,
            "presence_penalty": 0.4,
            "frequency_penalty": 0.7,
            "max_tokens": 512,
        },
    )
    rag.create_chat("test_create_chat", dataset_ids=[kb.id], llm=llm)


def test_update_chat_with_name(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    kb = rag.create_dataset(name="test_update_chat")
    display_name = "ragflow.txt"
    with open("test_data/ragflow.txt", "rb") as file:
        blob = file.read()
    document = {"display_name": display_name, "blob": blob}
    documents = []
    documents.append(document)
    docs = kb.upload_documents(documents)
    for doc in docs:
        doc.add_chunk("This is a test to add chunk")
    llm = Chat.LLM(
        rag,
        {
            "model_name": "glm-4-flash@ZHIPU-AI",
            "temperature": 0.1,
            "top_p": 0.3,
            "presence_penalty": 0.4,
            "frequency_penalty": 0.7,
            "max_tokens": 512,
        },
    )
    chat = rag.create_chat("test_update_chat", dataset_ids=[kb.id], llm=llm)
    chat.update({"name": "new_chat"})


def test_delete_chats_with_success(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    kb = rag.create_dataset(name="test_delete_chat")
    display_name = "ragflow.txt"
    with open("test_data/ragflow.txt", "rb") as file:
        blob = file.read()
    document = {"display_name": display_name, "blob": blob}
    documents = []
    documents.append(document)
    docs = kb.upload_documents(documents)
    for doc in docs:
        doc.add_chunk("This is a test to add chunk")
    llm = Chat.LLM(
        rag,
        {
            "model_name": "glm-4-flash@ZHIPU-AI",
            "temperature": 0.1,
            "top_p": 0.3,
            "presence_penalty": 0.4,
            "frequency_penalty": 0.7,
            "max_tokens": 512,
        },
    )
    chat = rag.create_chat("test_delete_chat", dataset_ids=[kb.id], llm=llm)
    rag.delete_chats(ids=[chat.id])


def test_list_chats_with_success(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    kb = rag.create_dataset(name="test_list_chats")
    display_name = "ragflow.txt"
    with open("test_data/ragflow.txt", "rb") as file:
        blob = file.read()
    document = {"display_name": display_name, "blob": blob}
    documents = []
    documents.append(document)
    docs = kb.upload_documents(documents)
    for doc in docs:
        doc.add_chunk("This is a test to add chunk")
    llm = Chat.LLM(
        rag,
        {
            "model_name": "glm-4-flash@ZHIPU-AI",
            "temperature": 0.1,
            "top_p": 0.3,
            "presence_penalty": 0.4,
            "frequency_penalty": 0.7,
            "max_tokens": 512,
        },
    )
    rag.create_chat("test_list_1", dataset_ids=[kb.id], llm=llm)
    rag.create_chat("test_list_2", dataset_ids=[kb.id], llm=llm)
    rag.list_chats()

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


### Functions (4)

- `test_create_chat_with_name()`: Function definition
- `test_update_chat_with_name()`: Function definition
- `test_delete_chats_with_success()`: Function definition
- `test_list_chats_with_success()`: Function definition

### Imports (3)

- `from common import HOST_ADDRESS`
- `from ragflow_sdk import RAGFlow`
- `from ragflow_sdk.modules.chat import Chat`

## Code Structure Analysis

- Total lines: 132
- Blank lines: 10 (7.6%)
- Comment lines: ~15 (11.4%)
- Code lines: ~107


## Dependencies and Imports

- `from common import HOST_ADDRESS`
- `from ragflow_sdk import RAGFlow`
- `from ragflow_sdk.modules.chat import Chat`

## Design & Architecture

This file is located in the `sdk` directory, specifically within `sdk/python/test/test_sdk_api`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 5 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `sdk/python/test/test_sdk_api/` directory
- Imports from `common`
- Imports from `ragflow_sdk`
- Imports from `ragflow_sdk.modules.chat`

## Keywords

ANY, API_KEY, All, Apache, Authors, BASIS, CONDITIONS, Chat, Copyright, HOST_ADDRESS, InfiniFlow, KIND, LICENSE, LLM, License, Licensed, Python, RAGFlow, Reserved, Rights, See, The, This, Unless, Version, WARRANTIES, WITHOUT, You, ZHIPU, test_create_chat_with_name, test_delete_chats_with_success, test_list_chats_with_success, test_update_chat_with_name

---
*Generated by RAGFlow Repository Documentation Generator*
