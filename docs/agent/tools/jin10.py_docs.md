# Documentation: agent/tools/jin10.py

## File Metadata

- **Path**: `agent/tools/jin10.py`
- **Size**: 6600 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/tools/jin10.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `json`
- `abc`
- `pandas`
- `requests`
- `agent.component.base`

### Classes Defined

This file defines 2 class(es):

#### Class: `Jin10Param` (line 23)

**Docstring**: Define the Jin10 component parameters....

**Methods**: __init__, check

#### Class: `Jin10` (line 49)

**Methods**: _run

### Functions Defined

This file defines 3 function(s):

#### Function: `__init__` (line 28)

**Parameters**: self

#### Function: `check` (line 40)

**Parameters**: self

#### Function: `_run` (line 52)

**Parameters**: self, history

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
import json
from abc import ABC
import pandas as pd
import requests
from agent.component.base import ComponentBase, ComponentParamBase


class Jin10Param(ComponentParamBase):
    """
    Define the Jin10 component parameters.
    """

    def __init__(self):
        super().__init__()
        self.type = "flash"
        self.secret_key = "xxx"
        self.flash_type = '1'
        self.calendar_type = 'cj'
        self.calendar_datatype = 'data'
        self.symbols_type = 'GOODS'
        self.symbols_datatype = 'symbols'
        self.contain = ""
        self.filter = ""

    def check(self):
        self.check_valid_value(self.type, "Type", ['flash', 'calendar', 'symbols', 'news'])
        self.check_valid_value(self.flash_type, "Flash Type", ['1', '2', '3', '4', '5'])
        self.check_valid_value(self.calendar_type, "Calendar Type", ['cj', 'qh', 'hk', 'us'])
        self.check_valid_value(self.calendar_datatype, "Calendar DataType", ['data', 'event', 'holiday'])
        self.check_valid_value(self.symbols_type, "Symbols Type", ['GOODS', 'FOREX', 'FUTURE', 'CRYPTO'])
        self.check_valid_value(self.symbols_datatype, 'Symbols DataType', ['symbols', 'quotes'])


class Jin10(ComponentBase, ABC):
    component_name = "Jin10"

    def _run(self, history, **kwargs):
        if self.check_if_canceled("Jin10 processing"):
            return

        ans = self.get_input()
        ans = " - ".join(ans["content"]) if "content" in ans else ""
        if not ans:
            return Jin10.be_output("")

        jin10_res = []
        headers = {'secret-key': self._param.secret_key}
        try:
            if self.check_if_canceled("Jin10 processing"):
                return

            if self._param.type == "flash":
                params = {
                    'category': self._param.flash_type,
                    'contain': self._param.contain,
                    'filter': self._param.filter
                }
                response = requests.get(
                    url='https://open-data-api.jin10.com/data-api/flash?category=' + self._param.flash_type,
                    headers=headers, data=json.dumps(params))
                response = response.json()
                for i in response['data']:
                    if self.check_if_canceled("Jin10 processing"):
                        return
                    jin10_res.append({"content": i['data']['content']})
            if self._param.type == "calendar":
                params = {
                    'category': self._param.calendar_type
                }
                response = requests.get(
                    url='https://open-data-api.jin10.com/data-api/calendar/' + self._param.calendar_datatype + '?category=' + self._param.calendar_type,
                    headers=headers, data=json.dumps(params))

                response = response.json()
                if self.check_if_canceled("Jin10 processing"):
                    return
                jin10_res.append({"content": pd.DataFrame(response['data']).to_markdown()})
            if self._param.type == "symbols":
                params = {
                    'type': self._param.symbols_type
                }
                if self._param.symbols_datatype == "quotes":
                    params['codes'] = 'BTCUSD'
                response = requests.get(
                    url='https://open-data-api.jin10.com/data-api/' + self._param.symbols_datatype + '?type=' + self._param.symbols_type,
                    headers=headers, data=json.dumps(params))
                response = response.json()
                if self.check_if_canceled("Jin10 processing"):
                    return
                if self._param.symbols_datatype == "symbols":
                    for i in response['data']:
                        if self.check_if_canceled("Jin10 processing"):
                            return
                        i['Commodity Code'] = i['c']
                        i['Stock Exchange'] = i['e']
                        i['Commodity Name'] = i['n']
                        i['Commodity Type'] = i['t']
                        del i['c'], i['e'], i['n'], i['t']
                if self._param.symbols_datatype == "quotes":
                    for i in response['data']:
                        if self.check_if_canceled("Jin10 processing"):
                            return
                        i['Selling Price'] = i['a']
                        i['Buying Price'] = i['b']
                        i['Commodity Code'] = i['c']
                        i['Stock Exchange'] = i['e']
                        i['Highest Price'] = i['h']
                        i['Yesterday’s Closing Price'] = i['hc']
                        i['Lowest Price'] = i['l']
                        i['Opening Price'] = i['o']
                        i['Latest Price'] = i['p']
                        i['Market Quote Time'] = i['t']
                        del i['a'], i['b'], i['c'], i['e'], i['h'], i['hc'], i['l'], i['o'], i['p'], i['t']
                jin10_res.append({"content": pd.DataFrame(response['data']).to_markdown()})
            if self._param.type == "news":
                params = {
                    'contain': self._param.contain,
                    'filter': self._param.filter
                }
                response = requests.get(
                    url='https://open-data-api.jin10.com/data-api/news',
                    headers=headers, data=json.dumps(params))
                response = response.json()
                if self.check_if_canceled("Jin10 processing"):
                    return
                jin10_res.append({"content": pd.DataFrame(response['data']).to_markdown()})
        except Exception as e:
            if self.check_if_canceled("Jin10 processing"):
                return
            return Jin10.be_output("**ERROR**: " + str(e))

        if not jin10_res:
            return Jin10.be_output("")

        return pd.DataFrame(jin10_res)

```

## Detailed Analysis

### File Role in Repository

The file `agent/tools/jin10.py` is located in the `agent/tools` directory.

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
- [base.py](base.py_docs.md)
- [code_exec.py](code_exec.py_docs.md)
- [crawler.py](crawler.py_docs.md)
- [deepl.py](deepl.py_docs.md)
- [duckduckgo.py](duckduckgo.py_docs.md)
- [email.py](email.py_docs.md)
- [exesql.py](exesql.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
