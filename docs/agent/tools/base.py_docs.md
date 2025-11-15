# Documentation: agent/tools/base.py

## File Metadata

- **Path**: `agent/tools/base.py`
- **Size**: 5799 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/tools/base.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `re`
- `time`
- `copy`
- `functools`
- `typing`
- `agent.component.base`
- `common.misc_utils`
- `rag.llm.chat_model`
- `rag.prompts.generator`
- `rag.utils.mcp_tool_call_conn`
- `timeit`
- `agent.canvas`

### Classes Defined

This file defines 5 class(es):

#### Class: `ToolParameter` (line 30)

#### Class: `ToolMeta` (line 38)

#### Class: `LLMToolPluginCallSession` (line 46)

**Methods**: __init__, tool_call, get_tool_obj

#### Class: `ToolParamBase` (line 66)

**Methods**: __init__, _init_inputs, _init_attr_by_meta, get_meta

#### Class: `ToolBase` (line 115)

**Methods**: __init__, get_meta, invoke, _retrieve_chunks, thoughts

### Functions Defined

This file defines 12 function(s):

#### Function: `__init__` (line 47)

**Parameters**: self, tools_map, callback

#### Function: `tool_call` (line 51)

**Parameters**: self, name, arguments

#### Function: `get_tool_obj` (line 62)

**Parameters**: self, name

#### Function: `__init__` (line 67)

**Parameters**: self

#### Function: `_init_inputs` (line 73)

**Parameters**: self

#### Function: `_init_attr_by_meta` (line 78)

**Parameters**: self

#### Function: `get_meta` (line 83)

**Parameters**: self

#### Function: `__init__` (line 116)

**Parameters**: self, canvas, id, param

#### Function: `get_meta` (line 124)

**Parameters**: self

#### Function: `invoke` (line 127)

**Parameters**: self

#### Function: `_retrieve_chunks` (line 143)

**Parameters**: self, res_list, get_title, get_url, get_content, get_score

#### Function: `thoughts` (line 175)

**Parameters**: self

## Original Source Code

```py
#
#  Copyright 2024 The InfiniFlow Authors. All Rights Reserved.
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
import logging
import re
import time
from copy import deepcopy
from functools import partial
from typing import TypedDict, List, Any
from agent.component.base import ComponentParamBase, ComponentBase
from common.misc_utils import hash_str2int
from rag.llm.chat_model import ToolCallSession
from rag.prompts.generator import kb_prompt
from rag.utils.mcp_tool_call_conn import MCPToolCallSession
from timeit import default_timer as timer


class ToolParameter(TypedDict):
    type: str
    description: str
    displayDescription: str
    enum: List[str]
    required: bool


class ToolMeta(TypedDict):
    name: str
    displayName: str
    description: str
    displayDescription: str
    parameters: dict[str, ToolParameter]


class LLMToolPluginCallSession(ToolCallSession):
    def __init__(self, tools_map: dict[str, object], callback: partial):
        self.tools_map = tools_map
        self.callback = callback

    def tool_call(self, name: str, arguments: dict[str, Any]) -> Any:
        assert name in self.tools_map, f"LLM tool {name} does not exist"
        st = timer()
        if isinstance(self.tools_map[name], MCPToolCallSession):
            resp = self.tools_map[name].tool_call(name, arguments, 60)
        else:
            resp = self.tools_map[name].invoke(**arguments)

        self.callback(name, arguments, resp, elapsed_time=timer()-st)
        return resp

    def get_tool_obj(self, name):
        return self.tools_map[name]


class ToolParamBase(ComponentParamBase):
    def __init__(self):
        #self.meta:ToolMeta = None
        super().__init__()
        self._init_inputs()
        self._init_attr_by_meta()

    def _init_inputs(self):
        self.inputs = {}
        for k,p in self.meta["parameters"].items():
            self.inputs[k] = deepcopy(p)

    def _init_attr_by_meta(self):
        for k,p in self.meta["parameters"].items():
            if not hasattr(self, k):
                setattr(self, k, p.get("default"))

    def get_meta(self):
        params = {}
        for k, p in self.meta["parameters"].items():
            params[k] = {
                "type": p["type"],
                "description": p["description"]
            }
            if "enum" in p:
                params[k]["enum"] = p["enum"]

        desc = self.meta["description"]
        if hasattr(self, "description"):
            desc = self.description

        function_name = self.meta["name"]
        if hasattr(self, "function_name"):
            function_name = self.function_name

        return {
            "type": "function",
            "function": {
                "name": function_name,
                "description": desc,
                "parameters": {
                    "type": "object",
                    "properties": params,
                    "required": [k for k, p in self.meta["parameters"].items() if p["required"]]
                }
            }
        }


class ToolBase(ComponentBase):
    def __init__(self, canvas, id, param: ComponentParamBase):
        from agent.canvas import Canvas  # Local import to avoid cyclic dependency
        assert isinstance(canvas, Canvas), "canvas must be an instance of Canvas"
        self._canvas = canvas
        self._id = id
        self._param = param
        self._param.check()

    def get_meta(self) -> dict[str, Any]:
        return self._param.get_meta()

    def invoke(self, **kwargs):
        if self.check_if_canceled("Tool processing"):
            return

        self.set_output("_created_time", time.perf_counter())
        try:
            res = self._invoke(**kwargs)
        except Exception as e:
            self._param.outputs["_ERROR"] = {"value": str(e)}
            logging.exception(e)
            res = str(e)
        self._param.debug_inputs = []

        self.set_output("_elapsed_time", time.perf_counter() - self.output("_created_time"))
        return res

    def _retrieve_chunks(self, res_list: list, get_title, get_url, get_content, get_score=None):
        chunks = []
        aggs = []
        for r in res_list:
            content = get_content(r)
            if not content:
                continue
            content = re.sub(r"!?\[[a-z]+\]\(data:image/png;base64,[ 0-9A-Za-z/_=+-]+\)", "", content)
            content = content[:10000]
            if not content:
                continue
            id = str(hash_str2int(content))
            title = get_title(r)
            url = get_url(r)
            score = get_score(r) if get_score else 1
            chunks.append({
                "chunk_id": id,
                "content": content,
                "doc_id": id,
                "docnm_kwd": title,
                "similarity": score,
                "url": url
            })
            aggs.append({
                "doc_name": title,
                "doc_id": id,
                "count": 1,
                "url": url
            })
        self._canvas.add_reference(chunks, aggs)
        self.set_output("formalized_content", "\n".join(kb_prompt({"chunks": chunks, "doc_aggs": aggs}, 200000, True)))

    def thoughts(self) -> str:
        return self._canvas.get_component_name(self._id) + " is running..."

```

## Detailed Analysis

### File Role in Repository

The file `agent/tools/base.py` is located in the `agent/tools` directory.

This file is part of the **Agent System** for workflow management.

### Architecture Context

Files in this location typically handle concerns related to tools.

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
- [akshare.py](akshare.py_docs.md)
- [arxiv.py](arxiv.py_docs.md)
- [code_exec.py](code_exec.py_docs.md)
- [crawler.py](crawler.py_docs.md)
- [deepl.py](deepl.py_docs.md)
- [duckduckgo.py](duckduckgo.py_docs.md)
- [email.py](email.py_docs.md)
- [exesql.py](exesql.py_docs.md)
- [github.py](github.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
