# Documentation: sdk/python/ragflow_sdk/modules/agent.py

## File Metadata

- **Path**: `sdk/python/ragflow_sdk/modules/agent.py`
- **Size**: 3284 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/ragflow_sdk/modules/agent.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `base`
- `session`

### Classes Defined

This file defines 2 class(es):

#### Class: `Agent` (line 21)

**Methods**: __init__, create_session, list_sessions, delete_sessions

#### Class: `Dsl` (line 30)

**Methods**: __init__

### Functions Defined

This file defines 5 function(s):

#### Function: `__init__` (line 22)

**Parameters**: self, rag, res_dict

#### Function: `create_session` (line 69)

**Parameters**: self

#### Function: `list_sessions` (line 77)

**Parameters**: self, page, page_size, orderby, desc, id

#### Function: `delete_sessions` (line 90)

**Parameters**: self, ids

#### Function: `__init__` (line 31)

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


class Agent(Base):
    def __init__(self, rag, res_dict):
        self.id  = None
        self.avatar = None
        self.canvas_type = None
        self.description = None
        self.dsl = None
        super().__init__(rag, res_dict)

    class Dsl(Base):
        def __init__(self, rag, res_dict):
            self.answer = []
            self.components = {
                "begin": {
                    "downstream": ["Answer:China"],
                    "obj": {
                        "component_name": "Begin",
                        "params": {}
                    },
                    "upstream": []
                }
            }
            self.graph = {
                "edges": [],
                "nodes": [
                    {
                        "data": {
                            "label": "Begin",
                            "name": "begin"
                        },
                        "id": "begin",
                        "position": {
                            "x": 50,
                            "y": 200
                        },
                        "sourcePosition": "left",
                        "targetPosition": "right",
                        "type": "beginNode"
                    }
                ]
            }
            self.history =  []
            self.messages =  []
            self.path =  []
            self.reference = []
            super().__init__(rag, res_dict)

    
    def create_session(self, **kwargs) -> Session:
        res = self.post(f"/agents/{self.id}/sessions", json=kwargs)
        res = res.json()
        if res.get("code") == 0:
            return Session(self.rag, res.get("data"))
        raise Exception(res.get("message"))

    
    def list_sessions(self, page: int = 1, page_size: int = 30, orderby: str = "create_time", desc: bool = True,
                      id: str = None) -> list[Session]:
        res = self.get(f"/agents/{self.id}/sessions",
                       {"page": page, "page_size": page_size, "orderby": orderby, "desc": desc, "id": id})
        res = res.json()
        if res.get("code") == 0:
            result_list = []
            for data in res.get("data"):
                temp_agent = Session(self.rag, data)
                result_list.append(temp_agent)
            return result_list
        raise Exception(res.get("message"))
    
    def delete_sessions(self, ids: list[str] | None = None):
        res = self.rm(f"/agents/{self.id}/sessions", {"ids": ids})
        res = res.json()
        if res.get("code") != 0:
            raise Exception(res.get("message"))
```

## Detailed Analysis

### File Role in Repository

The file `sdk/python/ragflow_sdk/modules/agent.py` is located in the `sdk/python/ragflow_sdk/modules` directory.

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
- [base.py](base.py_docs.md)
- [chat.py](chat.py_docs.md)
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
