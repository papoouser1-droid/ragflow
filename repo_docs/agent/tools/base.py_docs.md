# File Documentation: agent/tools/base.py

## File Metadata

- **Path**: `agent/tools/base.py`
- **Extension**: `.py`
- **Lines**: 177
- **Characters**: 5,799
- **Size**: 5,799 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

## Original Source

```python
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

## High-Level Overview

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

## Detailed Walkthrough

### Classes (5)

- `ToolParameter`: Class definition
- `ToolMeta`: Class definition
- `LLMToolPluginCallSession`: Class definition
- `ToolParamBase`: Class definition
- `ToolBase`: Class definition

### Imports (12)

- `import logging`
- `import re`
- `import time`
- `from copy import deepcopy`
- `from functools import partial`
- `from typing import TypedDict, List, Any`
- `from agent.component.base import ComponentParamBase, ComponentBase`
- `from common.misc_utils import hash_str2int`
- `from rag.llm.chat_model import ToolCallSession`
- `from rag.prompts.generator import kb_prompt`

## Code Structure Analysis

- Total lines: 177
- Blank lines: 26 (14.7%)
- Comment lines: ~16 (9.0%)
- Code lines: ~135


## Dependencies and Imports

- `import logging`
- `import re`
- `import time`
- `from copy import deepcopy`
- `from functools import partial`
- `from typing import TypedDict, List, Any`
- `from agent.component.base import ComponentParamBase, ComponentBase`
- `from common.misc_utils import hash_str2int`
- `from rag.llm.chat_model import ToolCallSession`
- `from rag.prompts.generator import kb_prompt`
- `from rag.utils.mcp_tool_call_conn import MCPToolCallSession`
- `from timeit import default_timer as timer`

## Design & Architecture

This file is located in the `agent` directory, specifically within `agent/tools`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 6 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `agent/tools/` directory
- Imports from `copy`
- Imports from `functools`
- Imports from `agent.component.base`
- Imports from `common.misc_utils`
- Potential test file: `test_base.py`

## Keywords

ANY, All, Any, Apache, Authors, BASIS, CONDITIONS, Canvas, ComponentBase, ComponentParamBase, Copyright, Exception, InfiniFlow, KIND, LICENSE, LLM, LLMToolPluginCallSession, License, Licensed, List, Local, MCPToolCallSession, None, Python, Reserved, Rights, See, The, Tool, ToolBase, ToolCallSession, ToolMeta, ToolParamBase, ToolParameter, True, TypedDict, Unless, Version, WARRANTIES, WITHOUT, You, __init__, _init_attr_by_meta, _init_inputs, _retrieve_chunks, get_meta, get_tool_obj, invoke, thoughts, tool_call

---
*Generated by RAGFlow Repository Documentation Generator*
