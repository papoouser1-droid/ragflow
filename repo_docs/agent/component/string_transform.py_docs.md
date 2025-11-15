# File Documentation: agent/component/string_transform.py

## File Metadata

- **Path**: `agent/component/string_transform.py`
- **Extension**: `.py`
- **Lines**: 116
- **Characters**: 3,704
- **Size**: 3,704 bytes
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

- `StringTransformParam`: Class definition
- `StringTransform`: Class definition

### Imports (8)

- `import os`
- `import re`
- `from abc import ABC`
- `from typing import Any`
- `from jinja2 import Template as Jinja2Template`
- `from agent.component.base import ComponentParamBase`
- `from common.connection_utils import timeout`
- `from .message import Message`

## Code Structure Analysis

- Total lines: 116
- Blank lines: 23 (19.8%)
- Comment lines: ~17 (14.7%)
- Code lines: ~76


## Dependencies and Imports

- `import os`
- `import re`
- `from abc import ABC`
- `from typing import Any`
- `from jinja2 import Template as Jinja2Template`
- `from agent.component.base import ComponentParamBase`
- `from common.connection_utils import timeout`
- `from .message import Message`

## Design & Architecture

This file is located in the `agent` directory, specifically within `agent/component`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 5 loop(s) - consider algorithmic complexity

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

- Other files in `agent/component/` directory
- Imports from `abc`
- Imports from `jinja2`
- Imports from `agent.component.base`
- Imports from `common.connection_utils`
- Potential test file: `test_string_transform.py`

## Keywords

ABC, ANY, All, Any, Apache, Authors, BASIS, COMPONENT_EXEC_TIMEOUT, CONDITIONS, ComponentParamBase, Copyright, DOTALL, Define, Exception, InfiniFlow, Jinja2Template, KIND, LICENSE, License, Licensed, Message, None, Python, Reserved, Rights, See, String, StringTransform, StringTransformParam, Support, Template, The, Unless, Version, WARRANTIES, WITHOUT, You, __init__, _invoke, _merge, _split, check, get_input_elements, get_input_form, thoughts, timeout

---
*Generated by RAGFlow Repository Documentation Generator*
