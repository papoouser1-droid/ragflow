# File Documentation: agent/tools/tushare.py

## File Metadata

- **Path**: `agent/tools/tushare.py`
- **Extension**: `.py`
- **Lines**: 85
- **Characters**: 2,994
- **Size**: 2,994 bytes
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
import time
import requests
from agent.component.base import ComponentBase, ComponentParamBase


class TuShareParam(ComponentParamBase):
    """
    Define the TuShare component parameters.
    """

    def __init__(self):
        super().__init__()
        self.token = "xxx"
        self.src = "eastmoney"
        self.start_date = "2024-01-01 09:00:00"
        self.end_date = time.strftime("%Y-%m-%d %H:%M:%S", time.localtime())
        self.keyword = ""

    def check(self):
        self.check_valid_value(self.src, "Quick News Source",
                               ["sina", "wallstreetcn", "10jqka", "eastmoney", "yuncaijing", "fenghuang", "jinrongjie"])


class TuShare(ComponentBase, ABC):
    component_name = "TuShare"

    def _run(self, history, **kwargs):
        if self.check_if_canceled("TuShare processing"):
            return

        ans = self.get_input()
        ans = ",".join(ans["content"]) if "content" in ans else ""
        if not ans:
            return TuShare.be_output("")

        try:
            if self.check_if_canceled("TuShare processing"):
                return

            tus_res = []
            params = {
                "api_name": "news",
                "token": self._param.token,
                "params": {"src": self._param.src, "start_date": self._param.start_date,
                           "end_date": self._param.end_date}
            }
            response = requests.post(url="http://api.tushare.pro", data=json.dumps(params).encode('utf-8'))
            response = response.json()
            if self.check_if_canceled("TuShare processing"):
                return
            if response['code'] != 0:
                return TuShare.be_output(response['msg'])
            df = pd.DataFrame(response['data']['items'])
            df.columns = response['data']['fields']
            if self.check_if_canceled("TuShare processing"):
                return
            tus_res.append({"content": (df[df['content'].str.contains(self._param.keyword, case=False)]).to_markdown()})
        except Exception as e:
            if self.check_if_canceled("TuShare processing"):
                return
            return TuShare.be_output("**ERROR**: " + str(e))

        if not tus_res:
            return TuShare.be_output("")

        return pd.DataFrame(tus_res)

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

- `TuShareParam`: Class definition
- `TuShare`: Class definition

### Imports (6)

- `import json`
- `from abc import ABC`
- `import pandas as pd`
- `import time`
- `import requests`
- `from agent.component.base import ComponentBase, ComponentParamBase`

## Code Structure Analysis

- Total lines: 85
- Blank lines: 13 (15.3%)
- Comment lines: ~17 (20.0%)
- Code lines: ~55


## Dependencies and Imports

- `import json`
- `from abc import ABC`
- `import pandas as pd`
- `import time`
- `import requests`
- `from agent.component.base import ComponentBase, ComponentParamBase`

## Design & Architecture

This file is located in the `agent` directory, specifically within `agent/tools`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity

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
- Potential test file: `test_tushare.py`

## Keywords

ABC, ANY, All, Apache, Authors, BASIS, CONDITIONS, ComponentBase, ComponentParamBase, Copyright, DataFrame, Define, ERROR, Exception, False, InfiniFlow, KIND, LICENSE, License, Licensed, News, Python, Quick, Reserved, Rights, See, Source, The, TuShare, TuShareParam, Unless, Version, WARRANTIES, WITHOUT, You, __init__, _run, check

---
*Generated by RAGFlow Repository Documentation Generator*
