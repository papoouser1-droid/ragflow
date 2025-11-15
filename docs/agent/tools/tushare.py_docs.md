# Documentation: agent/tools/tushare.py

## File Metadata

- **Path**: `agent/tools/tushare.py`
- **Size**: 2994 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/tools/tushare.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `json`
- `abc`
- `pandas`
- `time`
- `requests`
- `agent.component.base`

### Classes Defined

This file defines 2 class(es):

#### Class: `TuShareParam` (line 24)

**Docstring**: Define the TuShare component parameters....

**Methods**: __init__, check

#### Class: `TuShare` (line 42)

**Methods**: _run

### Functions Defined

This file defines 3 function(s):

#### Function: `__init__` (line 29)

**Parameters**: self

#### Function: `check` (line 37)

**Parameters**: self

#### Function: `_run` (line 45)

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

## Detailed Analysis

### File Role in Repository

The file `agent/tools/tushare.py` is located in the `agent/tools` directory.

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
