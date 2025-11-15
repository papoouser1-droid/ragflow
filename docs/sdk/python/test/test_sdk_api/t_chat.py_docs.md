# Documentation: sdk/python/test/test_sdk_api/t_chat.py

## File Metadata

- **Path**: `sdk/python/test/test_sdk_api/t_chat.py`
- **Size**: 4371 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/test/test_sdk_api/t_chat.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `common`
- `ragflow_sdk`
- `ragflow_sdk.modules.chat`

### Functions Defined

This file defines 4 function(s):

#### Function: `test_create_chat_with_name` (line 22)

**Parameters**: get_api_key_fixture

#### Function: `test_update_chat_with_name` (line 49)

**Parameters**: get_api_key_fixture

#### Function: `test_delete_chats_with_success` (line 77)

**Parameters**: get_api_key_fixture

#### Function: `test_list_chats_with_success` (line 105)

**Parameters**: get_api_key_fixture

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

## Detailed Analysis

### File Role in Repository

The file `sdk/python/test/test_sdk_api/t_chat.py` is located in the `sdk/python/test/test_sdk_api` directory.

This file is part of the **Testing** infrastructure.

### Architecture Context

Files in this location typically handle concerns related to test_sdk_api.

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

- [common.py](common.py_docs.md)
- [get_email.py](get_email.py_docs.md)
- [t_agent.py](t_agent.py_docs.md)
- [t_chunk.py](t_chunk.py_docs.md)
- [t_dataset.py](t_dataset.py_docs.md)
- [t_document.py](t_document.py_docs.md)
- [t_session.py](t_session.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
