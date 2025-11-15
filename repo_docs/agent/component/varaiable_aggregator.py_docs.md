# File Documentation: agent/component/varaiable_aggregator.py

## File Metadata

- **Path**: `agent/component/varaiable_aggregator.py`
- **Extension**: `.py`
- **Lines**: 85
- **Characters**: 3,064
- **Size**: 3,064 bytes
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

from typing import Any
import os

from common.connection_utils import timeout
from agent.component.base import ComponentBase, ComponentParamBase


class VariableAggregatorParam(ComponentParamBase):
    """
    Parameters for VariableAggregator

    - groups: list of dicts {"group_name": str, "variables": [variable selectors]}
    """

    def __init__(self):
        super().__init__()
        # each group expects: {"group_name": str, "variables": List[str]}
        self.groups = []

    def check(self):
        self.check_empty(self.groups, "[VariableAggregator] groups")
        for g in self.groups:
            if not g.get("group_name"):
                raise ValueError("[VariableAggregator] group_name can not be empty!")
            if not g.get("variables"):
                raise ValueError(
                    f"[VariableAggregator] variables of group `{g.get('group_name')}` can not be empty"
                )
            if not isinstance(g.get("variables"), list):
                raise ValueError(
                    f"[VariableAggregator] variables of group `{g.get('group_name')}` should be a list of strings"
                )

    def get_input_form(self) -> dict[str, dict]:
        return {
            "variables": {
                "name": "Variables",
                "type": "list",
            }
        }


class VariableAggregator(ComponentBase):
    component_name = "VariableAggregator"

    @timeout(int(os.environ.get("COMPONENT_EXEC_TIMEOUT", 3)))
    def _invoke(self, **kwargs):
        # Group mode: for each group, pick the first available variable
        for group in self._param.groups:
            gname = group.get("group_name")

            # record candidate selectors within this group
            self.set_input_value(f"{gname}.variables", list(group.get("variables", [])))
            for selector in group.get("variables", []):
                val = self._canvas.get_variable_value(selector['value'])
                if val:
                    self.set_output(gname, val)
                    break
            
    @staticmethod
    def _to_object(value: Any) -> Any:
        # Try to convert value to serializable object if it has to_object()
        try:
            return value.to_object()  # type: ignore[attr-defined]
        except Exception:
            return value

    def thoughts(self) -> str:
        return "Aggregating variables from canvas and grouping as configured."

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

## Detailed Walkthrough

### Classes (2)

- `VariableAggregatorParam`: Class definition
- `VariableAggregator`: Class definition

### Imports (4)

- `from typing import Any`
- `import os`
- `from common.connection_utils import timeout`
- `from agent.component.base import ComponentBase, ComponentParamBase`

## Code Structure Analysis

- Total lines: 85
- Blank lines: 15 (17.6%)
- Comment lines: ~20 (23.5%)
- Code lines: ~50


## Dependencies and Imports

- `from typing import Any`
- `import os`
- `from common.connection_utils import timeout`
- `from agent.component.base import ComponentBase, ComponentParamBase`

## Design & Architecture

This file is located in the `agent` directory, specifically within `agent/component`.

This appears to be a UI component or frontend module.

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

- Other files in `agent/component/` directory
- Imports from `common.connection_utils`
- Imports from `agent.component.base`
- Potential test file: `test_varaiable_aggregator.py`

## Keywords

ANY, Aggregating, All, Any, Apache, Authors, BASIS, COMPONENT_EXEC_TIMEOUT, CONDITIONS, ComponentBase, ComponentParamBase, Copyright, Exception, Group, InfiniFlow, KIND, LICENSE, License, Licensed, List, Parameters, Python, Reserved, Rights, See, The, Try, Unless, ValueError, VariableAggregator, VariableAggregatorParam, Variables, Version, WARRANTIES, WITHOUT, You, __init__, _invoke, _to_object, check, get_input_form, staticmethod, thoughts, timeout

---
*Generated by RAGFlow Repository Documentation Generator*
