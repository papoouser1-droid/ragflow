# Documentation: sdk/python/ragflow_sdk/modules/chat.py

## File Metadata

- **Path**: `sdk/python/ragflow_sdk/modules/chat.py`
- **Size**: 3663 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/ragflow_sdk/modules/chat.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `base`
- `session`

### Classes Defined

This file defines 3 class(es):

#### Class: `Chat` (line 22)

**Methods**: __init__, update, create_session, list_sessions, delete_sessions

#### Class: `LLM` (line 31)

**Methods**: __init__

#### Class: `Prompt` (line 41)

**Methods**: __init__

### Functions Defined

This file defines 7 function(s):

#### Function: `__init__` (line 23)

**Parameters**: self, rag, res_dict

#### Function: `update` (line 60)

**Parameters**: self, update_message

#### Function: `create_session` (line 66)

**Parameters**: self, name

#### Function: `list_sessions` (line 73)

**Parameters**: self, page, page_size, orderby, desc, id, name

#### Function: `delete_sessions` (line 83)

**Parameters**: self, ids

#### Function: `__init__` (line 32)

**Parameters**: self, rag, res_dict

#### Function: `__init__` (line 42)

**Parameters**: self, rag, res_dict

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


from .base import Base
from .session import Session


class Chat(Base):
    def __init__(self, rag, res_dict):
        self.id = ""
        self.name = "assistant"
        self.avatar = "path/to/avatar"
        self.llm = Chat.LLM(rag, {})
        self.prompt = Chat.Prompt(rag, {})
        super().__init__(rag, res_dict)

    class LLM(Base):
        def __init__(self, rag, res_dict):
            self.model_name = None
            self.temperature = 0.1
            self.top_p = 0.3
            self.presence_penalty = 0.4
            self.frequency_penalty = 0.7
            self.max_tokens = 512
            super().__init__(rag, res_dict)

    class Prompt(Base):
        def __init__(self, rag, res_dict):
            self.similarity_threshold = 0.2
            self.keywords_similarity_weight = 0.7
            self.top_n = 8
            self.top_k = 1024
            self.variables = [{"key": "knowledge", "optional": True}]
            self.rerank_model = ""
            self.empty_response = None
            self.opener = "Hi! I'm your assistant. What can I do for you?"
            self.show_quote = True
            self.prompt = (
                "You are an intelligent assistant. Please summarize the content of the knowledge base to answer the question. "
                "Please list the data in the knowledge base and answer in detail. When all knowledge base content is irrelevant to the question, "
                "your answer must include the sentence 'The answer you are looking for is not found in the knowledge base!' "
                "Answers need to consider chat history.\nHere is the knowledge base:\n{knowledge}\nThe above is the knowledge base."
            )
            super().__init__(rag, res_dict)

    def update(self, update_message: dict):
        res = self.put(f"/chats/{self.id}", update_message)
        res = res.json()
        if res.get("code") != 0:
            raise Exception(res["message"])

    def create_session(self, name: str = "New session") -> Session:
        res = self.post(f"/chats/{self.id}/sessions", {"name": name})
        res = res.json()
        if res.get("code") == 0:
            return Session(self.rag, res["data"])
        raise Exception(res["message"])

    def list_sessions(self, page: int = 1, page_size: int = 30, orderby: str = "create_time", desc: bool = True, id: str = None, name: str = None) -> list[Session]:
        res = self.get(f"/chats/{self.id}/sessions", {"page": page, "page_size": page_size, "orderby": orderby, "desc": desc, "id": id, "name": name})
        res = res.json()
        if res.get("code") == 0:
            result_list = []
            for data in res["data"]:
                result_list.append(Session(self.rag, data))
            return result_list
        raise Exception(res["message"])

    def delete_sessions(self, ids: list[str] | None = None):
        res = self.rm(f"/chats/{self.id}/sessions", {"ids": ids})
        res = res.json()
        if res.get("code") != 0:
            raise Exception(res.get("message"))

```

## Detailed Analysis

### File Role in Repository

The file `sdk/python/ragflow_sdk/modules/chat.py` is located in the `sdk/python/ragflow_sdk/modules` directory.

### Architecture Context

Files in this location typically handle concerns related to modules.

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

- [__init__.py](__init__.py_docs.md)
- [agent.py](agent.py_docs.md)
- [base.py](base.py_docs.md)
- [chunk.py](chunk.py_docs.md)
- [dataset.py](dataset.py_docs.md)
- [document.py](document.py_docs.md)
- [session.py](session.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
