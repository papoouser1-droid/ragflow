# Documentation: sdk/python/ragflow_sdk/modules/session.py

## File Metadata

- **Path**: `sdk/python/ragflow_sdk/modules/session.py`
- **Size**: 4858 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/ragflow_sdk/modules/session.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `json`
- `base`

### Classes Defined

This file defines 2 class(es):

#### Class: `Session` (line 21)

**Methods**: __init__, ask, _structure_answer, _ask_chat, _ask_agent, update

#### Class: `Message` (line 120)

**Methods**: __init__

### Functions Defined

This file defines 7 function(s):

#### Function: `__init__` (line 22)

**Parameters**: self, rag, res_dict

#### Function: `ask` (line 36)

**Parameters**: self, question, stream

**Docstring**: Ask a question to the session. If stream=True, yields Message objects as they arrive (SSE streaming).
If stream=False, returns a single Message object for the final answer....

#### Function: `_structure_answer` (line 82)

**Parameters**: self, json_data

#### Function: `_ask_chat` (line 98)

**Parameters**: self, question, stream

#### Function: `_ask_agent` (line 105)

**Parameters**: self, question, stream

#### Function: `update` (line 112)

**Parameters**: self, update_message

#### Function: `__init__` (line 121)

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

import json
from .base import Base


class Session(Base):
    def __init__(self, rag, res_dict):
        self.id = None
        self.name = "New session"
        self.messages = [{"role": "assistant", "content": "Hi! I am your assistant, can I help you?"}]
        for key, value in res_dict.items():
            if key == "chat_id" and value is not None:
                self.chat_id = None
                self.__session_type = "chat"
            if key == "agent_id" and value is not None:
                self.agent_id = None
                self.__session_type = "agent"
        super().__init__(rag, res_dict)


    def ask(self, question="", stream=False, **kwargs):
        """
        Ask a question to the session. If stream=True, yields Message objects as they arrive (SSE streaming).
        If stream=False, returns a single Message object for the final answer.
        """
        if self.__session_type == "agent":
            res = self._ask_agent(question, stream, **kwargs)
        elif self.__session_type == "chat":
            res = self._ask_chat(question, stream, **kwargs)
        else:
            raise Exception(f"Unknown session type: {self.__session_type}")

        if stream:
            for line in res.iter_lines(decode_unicode=True):
                if not line:
                    continue  # Skip empty lines
                line = line.strip()
                if line.startswith("data:"):
                    content = line[len("data:"):].strip()
                    if content == "[DONE]":
                        break  # End of stream
                else:
                    content = line

                try:
                    json_data = json.loads(content)
                except json.JSONDecodeError:
                    continue  # Skip lines that are not valid JSON

                if (
                    (self.__session_type == "agent" and json_data.get("event") == "message_end")
                    or (self.__session_type == "chat" and json_data.get("data") is True)
                ):
                    return
                if self.__session_type == "agent":
                    yield self._structure_answer(json_data)
                else:
                    yield self._structure_answer(json_data["data"])
        else:
            try:
                json_data = res.json()
            except ValueError:
                raise Exception(f"Invalid response {res}")
            yield self._structure_answer(json_data["data"])
        

    def _structure_answer(self, json_data):
        if self.__session_type == "agent":
           answer = json_data["data"]["content"]
        elif self.__session_type == "chat":
            answer = json_data["answer"]
        reference = json_data.get("reference", {})
        temp_dict = {
            "content": answer,
            "role": "assistant"
        }
        if reference and "chunks" in reference:
            chunks = reference["chunks"]
            temp_dict["reference"] = chunks
        message = Message(self.rag, temp_dict)
        return message

    def _ask_chat(self, question: str, stream: bool, **kwargs):
        json_data = {"question": question, "stream": stream, "session_id": self.id}
        json_data.update(kwargs)
        res = self.post(f"/chats/{self.chat_id}/completions",
                        json_data, stream=stream)
        return res

    def _ask_agent(self, question: str, stream: bool, **kwargs):
        json_data = {"question": question, "stream": stream, "session_id": self.id}
        json_data.update(kwargs)
        res = self.post(f"/agents/{self.agent_id}/completions",
                        json_data, stream=stream)
        return res

    def update(self, update_message):
        res = self.put(f"/chats/{self.chat_id}/sessions/{self.id}",
                       update_message)
        res = res.json()
        if res.get("code") != 0:
            raise Exception(res.get("message"))


class Message(Base):
    def __init__(self, rag, res_dict):
        self.content = "Hi! I am your assistant, can I help you?"
        self.reference = None
        self.role = "assistant"
        self.prompt = None
        self.id = None
        super().__init__(rag, res_dict)

```

## Detailed Analysis

### File Role in Repository

The file `sdk/python/ragflow_sdk/modules/session.py` is located in the `sdk/python/ragflow_sdk/modules` directory.

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
- [chat.py](chat.py_docs.md)
- [chunk.py](chunk.py_docs.md)
- [dataset.py](dataset.py_docs.md)
- [document.py](document.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
