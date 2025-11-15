# File Documentation: sdk/python/ragflow_sdk/modules/agent.py

## File Metadata

- **Path**: `sdk/python/ragflow_sdk/modules/agent.py`
- **Extension**: `.py`
- **Lines**: 94
- **Characters**: 3,284
- **Size**: 3,284 bytes
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

- `Agent`: Class definition

### Imports (2)

- `from .base import Base`
- `from .session import Session`

## Code Structure Analysis

- Total lines: 94
- Blank lines: 9 (9.6%)
- Comment lines: ~15 (16.0%)
- Code lines: ~70


## Dependencies and Imports

- `from .base import Base`
- `from .session import Session`

## Design & Architecture

This file is located in the `sdk` directory, specifically within `sdk/python/ragflow_sdk/modules`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 2 loop(s) - consider algorithmic complexity

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
- Imports from `.session`
- Potential test file: `test_agent.py`

## Keywords

ANY, Agent, All, Answer, Apache, Authors, BASIS, Base, Begin, CONDITIONS, China, Copyright, Dsl, Exception, InfiniFlow, KIND, LICENSE, License, Licensed, None, Python, Reserved, Rights, See, Session, The, True, Unless, Version, WARRANTIES, WITHOUT, You, __init__, create_session, delete_sessions, list_sessions

---
*Generated by RAGFlow Repository Documentation Generator*
