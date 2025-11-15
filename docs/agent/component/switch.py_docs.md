# Documentation: agent/component/switch.py

## File Metadata

- **Path**: `agent/component/switch.py`
- **Size**: 5457 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/component/switch.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `numbers`
- `os`
- `abc`
- `typing`
- `agent.component.base`
- `common.connection_utils`

### Classes Defined

This file defines 2 class(es):

#### Class: `SwitchParam` (line 25)

**Docstring**: Define the Switch component parameters....

**Methods**: __init__, check, get_input_form

#### Class: `Switch` (line 61)

**Methods**: _invoke, process_operator, thoughts

### Functions Defined

This file defines 6 function(s):

#### Function: `__init__` (line 30)

**Parameters**: self

#### Function: `check` (line 46)

**Parameters**: self

#### Function: `get_input_form` (line 53)

**Parameters**: self

#### Function: `_invoke` (line 65)

**Parameters**: self

#### Function: `process_operator` (line 99)

**Parameters**: self, input, operator, value

#### Function: `thoughts` (line 139)

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
import numbers
import os
from abc import ABC
from typing import Any

from agent.component.base import ComponentBase, ComponentParamBase
from common.connection_utils import timeout


class SwitchParam(ComponentParamBase):
    """
    Define the Switch component parameters.
    """

    def __init__(self):
        super().__init__()
        """
        {
            "logical_operator" : "and | or"
            "items" : [
                            {"cpn_id": "categorize:0", "operator": "contains", "value": ""},
                            {"cpn_id": "categorize:0", "operator": "contains", "value": ""},...],
            "to": ""
        }
        """
        self.conditions = []
        self.end_cpn_ids = []
        self.operators = ['contains', 'not contains', 'start with', 'end with', 'empty', 'not empty', '=', '≠', '>',
                          '<', '≥', '≤']

    def check(self):
        self.check_empty(self.conditions, "[Switch] conditions")
        for cond in self.conditions:
            if not cond["to"]:
                raise ValueError("[Switch] 'To' can not be empty!")
        self.check_empty(self.end_cpn_ids, "[Switch] the ELSE/Other destination can not be empty.")

    def get_input_form(self) -> dict[str, dict]:
        return {
            "urls": {
                "name": "URLs",
                "type": "line"
            }
        }

class Switch(ComponentBase, ABC):
    component_name = "Switch"

    @timeout(int(os.environ.get("COMPONENT_EXEC_TIMEOUT", 3)))
    def _invoke(self, **kwargs):
        if self.check_if_canceled("Switch processing"):
            return

        for cond in self._param.conditions:
            if self.check_if_canceled("Switch processing"):
                return

            res = []
            for item in cond["items"]:
                if self.check_if_canceled("Switch processing"):
                    return

                if not item["cpn_id"]:
                    continue
                cpn_v = self._canvas.get_variable_value(item["cpn_id"])
                self.set_input_value(item["cpn_id"], cpn_v)
                operatee = item.get("value", "")
                if isinstance(cpn_v, numbers.Number):
                    operatee = float(operatee)
                res.append(self.process_operator(cpn_v, item["operator"], operatee))
                if cond["logical_operator"] != "and" and any(res):
                    self.set_output("next", [self._canvas.get_component_name(cpn_id) for cpn_id in cond["to"]])
                    self.set_output("_next", cond["to"])
                    return

            if all(res):
                self.set_output("next", [self._canvas.get_component_name(cpn_id) for cpn_id in cond["to"]])
                self.set_output("_next", cond["to"])
                return

        self.set_output("next", [self._canvas.get_component_name(cpn_id) for cpn_id in self._param.end_cpn_ids])
        self.set_output("_next", self._param.end_cpn_ids)

    def process_operator(self, input: Any, operator: str, value: Any) -> bool:
        if operator == "contains":
            return True if value.lower() in input.lower() else False
        elif operator == "not contains":
            return True if value.lower() not in input.lower() else False
        elif operator == "start with":
            return True if input.lower().startswith(value.lower()) else False
        elif operator == "end with":
            return True if input.lower().endswith(value.lower()) else False
        elif operator == "empty":
            return True if not input else False
        elif operator == "not empty":
            return True if input else False
        elif operator == "=":
            return True if input == value else False
        elif operator == "≠":
            return True if input != value else False
        elif operator == ">":
            try:
                return True if float(input) > float(value) else False
            except Exception:
                return True if input > value else False
        elif operator == "<":
            try:
                return True if float(input) < float(value) else False
            except Exception:
                return True if input < value else False
        elif operator == "≥":
            try:
                return True if float(input) >= float(value) else False
            except Exception:
                return True if input >= value else False
        elif operator == "≤":
            try:
                return True if float(input) <= float(value) else False
            except Exception:
                return True if input <= value else False

        raise ValueError('Not supported operator' + operator)

    def thoughts(self) -> str:
        return "I’m weighing a few options and will pick the next step shortly."

```

## Detailed Analysis

### File Role in Repository

The file `agent/component/switch.py` is located in the `agent/component` directory.

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
