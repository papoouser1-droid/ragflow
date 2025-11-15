# File Documentation: agent/tools/jin10.py

## File Metadata

- **Path**: `agent/tools/jin10.py`
- **Extension**: `.py`
- **Lines**: 151
- **Characters**: 6,598
- **Size**: 6,600 bytes
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

### Classes (2)

- `Jin10Param`: Class definition
- `Jin10`: Class definition

### Imports (5)

- `import json`
- `from abc import ABC`
- `import pandas as pd`
- `import requests`
- `from agent.component.base import ComponentBase, ComponentParamBase`

## Code Structure Analysis

- Total lines: 151
- Blank lines: 14 (9.3%)
- Comment lines: ~17 (11.3%)
- Code lines: ~120


## Dependencies and Imports

- `import json`
- `from abc import ABC`
- `import pandas as pd`
- `import requests`
- `from agent.component.base import ComponentBase, ComponentParamBase`

## Design & Architecture

This file is located in the `agent` directory, specifically within `agent/tools`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 4 loop(s) - consider algorithmic complexity

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
- Imports from `abc`
- Imports from `agent.component.base`
- Potential test file: `test_jin10.py`

## Keywords

ABC, ANY, All, Apache, Authors, BASIS, BTCUSD, Buying, CONDITIONS, CRYPTO, Calendar, Closing, Code, Commodity, ComponentBase, ComponentParamBase, Copyright, DataFrame, DataType, Define, ERROR, Exception, Exchange, FOREX, FUTURE, Flash, GOODS, Highest, InfiniFlow, Jin10, Jin10Param, KIND, LICENSE, Latest, License, Licensed, Lowest, Market, Name, Opening, Price, Python, Quote, Reserved, Rights, See, Selling, Stock, Symbols, The...

---
*Generated by RAGFlow Repository Documentation Generator*
