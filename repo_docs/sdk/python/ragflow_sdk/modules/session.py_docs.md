# File Documentation: sdk/python/ragflow_sdk/modules/session.py

## File Metadata

- **Path**: `sdk/python/ragflow_sdk/modules/session.py`
- **Extension**: `.py`
- **Lines**: 128
- **Characters**: 4,858
- **Size**: 4,858 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

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

### Classes (2)

- `Session`: Class definition
- `Message`: Class definition

### Imports (2)

- `import json`
- `from .base import Base`

## Code Structure Analysis

- Total lines: 128
- Blank lines: 16 (12.5%)
- Comment lines: ~17 (13.3%)
- Code lines: ~95


## Dependencies and Imports

- `import json`
- `from .base import Base`

## Design & Architecture

This file is located in the `sdk` directory, specifically within `sdk/python/ragflow_sdk/modules`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 4 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `sdk/python/ragflow_sdk/modules/` directory
- Imports from `.base`
- Potential test file: `test_session.py`

## Keywords

ANY, All, Apache, Ask, Authors, BASIS, Base, CONDITIONS, Copyright, DONE, End, Exception, False, InfiniFlow, Invalid, JSON, JSONDecodeError, KIND, LICENSE, License, Licensed, Message, New, None, Python, Reserved, Rights, SSE, See, Session, Skip, The, True, Unknown, Unless, ValueError, Version, WARRANTIES, WITHOUT, You, __init__, _ask_agent, _ask_chat, _structure_answer, ask, update

---
*Generated by RAGFlow Repository Documentation Generator*
