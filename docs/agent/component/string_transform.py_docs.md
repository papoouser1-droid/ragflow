# Documentation: agent/component/string_transform.py

## File Metadata

- **Path**: `agent/component/string_transform.py`
- **Size**: 3704 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/component/string_transform.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `os`
- `re`
- `abc`
- `typing`
- `jinja2`
- `agent.component.base`
- `common.connection_utils`
- `message`

### Classes Defined

This file defines 2 class(es):

#### Class: `StringTransformParam` (line 27)

**Docstring**: Define the code sandbox component parameters....

**Methods**: __init__, check

#### Class: `StringTransform` (line 45)

**Methods**: get_input_elements, get_input_form, _invoke, _split, _merge, thoughts

### Functions Defined

This file defines 8 function(s):

#### Function: `__init__` (line 32)

**Parameters**: self

#### Function: `check` (line 40)

**Parameters**: self

#### Function: `get_input_elements` (line 48)

**Parameters**: self

#### Function: `get_input_form` (line 51)

**Parameters**: self

#### Function: `_invoke` (line 65)

**Parameters**: self

#### Function: `_split` (line 74)

**Parameters**: self, line

#### Function: `_merge` (line 91)

**Parameters**: self, kwargs

#### Function: `thoughts` (line 112)

**Parameters**: self

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
import os
import re
from abc import ABC
from typing import Any

from jinja2 import Template as Jinja2Template
from agent.component.base import ComponentParamBase
from common.connection_utils import timeout
from .message import Message


class StringTransformParam(ComponentParamBase):
    """
    Define the code sandbox component parameters.
    """

    def __init__(self):
        super().__init__()
        self.method = "split"
        self.script = ""
        self.split_ref = ""
        self.delimiters = [","]
        self.outputs = {"result": {"value": "", "type": "string"}}

    def check(self):
        self.check_valid_value(self.method, "Support method", ["split", "merge"])
        self.check_empty(self.delimiters, "delimiters")


class StringTransform(Message, ABC):
    component_name = "StringTransform"

    def get_input_elements(self) -> dict[str, Any]:
        return self.get_input_elements_from_text(self._param.script)

    def get_input_form(self) -> dict[str, dict]:
        if self._param.method == "split":
            return {
                "line": {
                    "name": "String",
                    "type": "line"
                }
            }
        return {k: {
            "name": o["name"],
            "type": "line"
        } for k, o in self.get_input_elements_from_text(self._param.script).items()}

    @timeout(int(os.environ.get("COMPONENT_EXEC_TIMEOUT", 10*60)))
    def _invoke(self, **kwargs):
        if self.check_if_canceled("StringTransform processing"):
            return

        if self._param.method == "split":
            self._split(kwargs.get("line"))
        else:
            self._merge(kwargs)

    def _split(self, line:str|None = None):
        if self.check_if_canceled("StringTransform split processing"):
            return

        var = self._canvas.get_variable_value(self._param.split_ref) if not line else line
        if not var:
            var = ""
        assert isinstance(var, str), "The input variable is not a string: {}".format(type(var))
        self.set_input_value(self._param.split_ref, var)

        res = []
        for i,s in enumerate(re.split(r"(%s)"%("|".join([re.escape(d) for d in self._param.delimiters])), var, flags=re.DOTALL)):
            if i % 2 == 1:
                continue
            res.append(s)
        self.set_output("result", res)

    def _merge(self, kwargs:dict[str, str] = {}):
        if self.check_if_canceled("StringTransform merge processing"):
            return

        script = self._param.script
        script, kwargs = self.get_kwargs(script, kwargs, self._param.delimiters[0])

        if self._is_jinjia2(script):
            template = Jinja2Template(script)
            try:
                script = template.render(kwargs)
            except Exception:
                pass

        for k,v in kwargs.items():
            if not v:
                v = ""
            script = re.sub(k, lambda match: v, script)

        self.set_output("result", script)

    def thoughts(self) -> str:
        return f"It's {self._param.method}ing."



```

## Detailed Analysis

### File Role in Repository

The file `agent/component/string_transform.py` is located in the `agent/component` directory.

This file is part of the **Agent System** for workflow management.

### Architecture Context

Files in this location typically handle concerns related to component.

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
- [agent_with_tools.py](agent_with_tools.py_docs.md)
- [base.py](base.py_docs.md)
- [begin.py](begin.py_docs.md)
- [categorize.py](categorize.py_docs.md)
- [data_operations.py](data_operations.py_docs.md)
- [fillup.py](fillup.py_docs.md)
- [invoke.py](invoke.py_docs.md)
- [iteration.py](iteration.py_docs.md)
- [iterationitem.py](iterationitem.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
