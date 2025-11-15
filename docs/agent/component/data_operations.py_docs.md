# Documentation: agent/component/data_operations.py

## File Metadata

- **Path**: `agent/component/data_operations.py`
- **Size**: 7301 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/component/data_operations.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `abc`
- `ast`
- `os`
- `agent.component.base`
- `api.utils.api_utils`

### Classes Defined

This file defines 2 class(es):

#### Class: `DataOperationsParam` (line 7)

**Docstring**: Define the Data Operations component parameters....

**Methods**: __init__, check

#### Class: `DataOperations` (line 32)

**Methods**: get_input_form, _invoke, _select_keys, _recursive_eval, _literal_eval, _combine, norm, match_rule, _filter_values, _append_or_update, _remove_keys, _rename_keys, thoughts

### Functions Defined

This file defines 15 function(s):

#### Function: `__init__` (line 11)

**Parameters**: self

#### Function: `check` (line 27)

**Parameters**: self

#### Function: `get_input_form` (line 35)

**Parameters**: self

#### Function: `_invoke` (line 43)

**Parameters**: self

#### Function: `_select_keys` (line 74)

**Parameters**: self

#### Function: `_recursive_eval` (line 80)

**Parameters**: self, data

#### Function: `_literal_eval` (line 99)

**Parameters**: self

#### Function: `_combine` (line 102)

**Parameters**: self

#### Function: `norm` (line 119)

**Parameters**: self, v

#### Function: `match_rule` (line 123)

**Parameters**: self, obj, rule

#### Function: `_filter_values` (line 144)

**Parameters**: self

#### Function: `_append_or_update` (line 156)

**Parameters**: self

#### Function: `_remove_keys` (line 171)

**Parameters**: self

#### Function: `_rename_keys` (line 184)

**Parameters**: self

#### Function: `thoughts` (line 202)

**Parameters**: self

## Original Source Code

```py
from abc import ABC
import ast
import os
from agent.component.base import ComponentBase, ComponentParamBase
from api.utils.api_utils import timeout

class DataOperationsParam(ComponentParamBase):
    """
    Define the Data Operations component parameters.
    """
    def __init__(self):
        super().__init__()
        self.query = []
        self.operations = "literal_eval"
        self.select_keys = []
        self.filter_values=[]
        self.updates=[]
        self.remove_keys=[]
        self.rename_keys=[]
        self.outputs = {
            "result": {
                "value": [],
                "type": "Array of Object"
            }
        }
    
    def check(self):
        self.check_valid_value(self.operations, "Support operations", ["select_keys", "literal_eval","combine","filter_values","append_or_update","remove_keys","rename_keys"])
    
    

class DataOperations(ComponentBase,ABC):
    component_name = "DataOperations"

    def get_input_form(self) -> dict[str, dict]:
        return {
            k: {"name": o.get("name", ""), "type": "line"}
            for input_item in (self._param.query or [])
            for k, o in self.get_input_elements_from_text(input_item).items()
        }

    @timeout(int(os.environ.get("COMPONENT_EXEC_TIMEOUT", 10*60)))
    def _invoke(self, **kwargs):
        self.input_objects=[]
        inputs = getattr(self._param, "query", None)
        if not isinstance(inputs, (list, tuple)):
            inputs = [inputs]
        for input_ref in inputs:
            input_object=self._canvas.get_variable_value(input_ref)
            self.set_input_value(input_ref, input_object)
            if input_object is None:
                continue
            if isinstance(input_object,dict):
                self.input_objects.append(input_object)
            elif isinstance(input_object,list):
                self.input_objects.extend(x for x in input_object if isinstance(x, dict))
            else:
                continue
        if self._param.operations == "select_keys":
            self._select_keys()
        elif self._param.operations == "recursive_eval":
            self._literal_eval()
        elif self._param.operations == "combine":
            self._combine()
        elif self._param.operations == "filter_values":
            self._filter_values()
        elif self._param.operations == "append_or_update":
            self._append_or_update()
        elif self._param.operations == "remove_keys":
            self._remove_keys()
        else:
            self._rename_keys()
    
    def _select_keys(self):
        filter_criteria: list[str] = self._param.select_keys
        results = [{key: value for key, value in data_dict.items() if key in filter_criteria} for data_dict in self.input_objects]
        self.set_output("result", results)


    def _recursive_eval(self, data):
        if isinstance(data, dict):
            return {k: self.recursive_eval(v) for k, v in data.items()}
        if isinstance(data, list):
            return [self.recursive_eval(item) for item in data]
        if isinstance(data, str):
            try:
                if (
                    data.strip().startswith(("{", "[", "(", "'", '"'))
                    or data.strip().lower() in ("true", "false", "none")
                    or data.strip().replace(".", "").isdigit()
                ):
                    return ast.literal_eval(data)
            except (ValueError, SyntaxError, TypeError, MemoryError):
                return data
            else:
                return data
        return data
    
    def _literal_eval(self):
        self.set_output("result", self._recursive_eval(self.input_objects))

    def _combine(self):
        result={}
        for obj in self.input_objects:
            for key, value in obj.items():
                if key not in result:
                    result[key] = value
                elif isinstance(result[key], list):
                    if isinstance(value, list):
                        result[key].extend(value)
                    else:
                        result[key].append(value)
                else:
                    result[key] = (
                        [result[key], value] if not isinstance(value, list) else [result[key], *value]
                    )
        self.set_output("result", result)
    
    def norm(self,v):
        s = "" if v is None else str(v)
        return s
    
    def match_rule(self, obj, rule):
        key = rule.get("key")
        op = (rule.get("operator") or "equals").lower()
        target = self.norm(rule.get("value"))
        target = self._canvas.get_value_with_variable(target) or target
        if key not in obj:
            return False
        val = obj.get(key, None)
        v = self.norm(val)
        if op == "=":
            return v == target
        if op == "≠":
            return v != target
        if op == "contains":
            return target in v
        if op == "start with":
            return v.startswith(target)
        if op == "end with":
            return v.endswith(target)
        return False
        
    def _filter_values(self):
        results=[]
        rules = (getattr(self._param, "filter_values", None) or [])
        for obj in self.input_objects:
            if not rules:
                results.append(obj)
                continue
            if all(self.match_rule(obj, r) for r in rules):
                results.append(obj)
        self.set_output("result", results)
            
                
    def _append_or_update(self):
        results=[]
        updates = getattr(self._param, "updates", []) or [] 
        for obj in self.input_objects:
            new_obj = dict(obj)
            for item in updates:
                if not isinstance(item, dict):
                    continue
                k = (item.get("key") or "").strip()
                if not k:
                    continue
                new_obj[k] = self._canvas.get_value_with_variable(item.get("value")) or item.get("value")
            results.append(new_obj)
        self.set_output("result", results)

    def _remove_keys(self):
        results = []
        remove_keys = getattr(self._param, "remove_keys", []) or []

        for obj in (self.input_objects or []):
            new_obj = dict(obj)
            for k in remove_keys:
                if not isinstance(k, str):
                    continue
                new_obj.pop(k, None)
            results.append(new_obj)
        self.set_output("result", results)

    def _rename_keys(self):
        results = []
        rename_pairs = getattr(self._param, "rename_keys", []) or []

        for obj in (self.input_objects or []):
            new_obj = dict(obj)
            for pair in rename_pairs:
                if not isinstance(pair, dict):
                    continue
                old = (pair.get("old_key") or "").strip()
                new = (pair.get("new_key") or "").strip()
                if not old or not new or old == new:
                    continue
                if old in new_obj:
                    new_obj[new] = new_obj.pop(old)
            results.append(new_obj)
        self.set_output("result", results)

    def thoughts(self) -> str:
        return "DataOperation in progress"

```

## Detailed Analysis

### File Role in Repository

The file `agent/component/data_operations.py` is located in the `agent/component` directory.

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
- [fillup.py](fillup.py_docs.md)
- [invoke.py](invoke.py_docs.md)
- [iteration.py](iteration.py_docs.md)
- [iterationitem.py](iterationitem.py_docs.md)
- [list_operations.py](list_operations.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
