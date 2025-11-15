# Documentation: agent/tools/wencai.py

## File Metadata

- **Path**: `agent/tools/wencai.py`
- **Size**: 5286 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/tools/wencai.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `os`
- `time`
- `abc`
- `pandas`
- `pywencai`
- `agent.tools.base`
- `common.connection_utils`

### Classes Defined

This file defines 2 class(es):

#### Class: `WenCaiParam` (line 27)

**Docstring**: Define the WenCai component parameters....

**Methods**: __init__, check, get_input_form

#### Class: `WenCai` (line 68)

**Methods**: _invoke, thoughts

### Functions Defined

This file defines 5 function(s):

#### Function: `__init__` (line 32)

**Parameters**: self

#### Function: `check` (line 53)

**Parameters**: self

#### Function: `get_input_form` (line 60)

**Parameters**: self

#### Function: `_invoke` (line 72)

**Parameters**: self

#### Function: `thoughts` (line 128)

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
import logging
import os
import time
from abc import ABC
import pandas as pd
import pywencai

from agent.tools.base import ToolParamBase, ToolMeta, ToolBase
from common.connection_utils import timeout


class WenCaiParam(ToolParamBase):
    """
    Define the WenCai component parameters.
    """

    def __init__(self):
        self.meta:ToolMeta = {
            "name": "iwencai",
            "description": """
iwencai search: search platform is committed to providing hundreds of millions of investors with the most timely, accurate and comprehensive information, covering news, announcements, research reports, blogs, forums, Weibo, characters, etc.
robo-advisor intelligent stock selection platform: through AI technology, is committed to providing investors with intelligent stock selection, quantitative investment, main force tracking, value investment, technical analysis and other types of stock selection technologies.
fund selection platform: through AI technology, is committed to providing excellent fund, value investment, quantitative analysis and other fund selection technologies for foundation citizens.
""",
            "parameters": {
                "query": {
                    "type": "string",
                    "description": "The question/conditions to select stocks.",
                    "default": "{sys.query}",
                    "required": True
                }
            }
        }
        super().__init__()
        self.top_n = 10
        self.query_type = "stock"

    def check(self):
        self.check_positive_integer(self.top_n, "Top N")
        self.check_valid_value(self.query_type, "Query type",
                               ['stock', 'zhishu', 'fund', 'hkstock', 'usstock', 'threeboard', 'conbond', 'insurance',
                                'futures', 'lccp',
                                'foreign_exchange'])

    def get_input_form(self) -> dict[str, dict]:
        return {
            "query": {
                "name": "Query",
                "type": "line"
            }
        }

class WenCai(ToolBase, ABC):
    component_name = "WenCai"

    @timeout(int(os.environ.get("COMPONENT_EXEC_TIMEOUT", 12)))
    def _invoke(self, **kwargs):
        if self.check_if_canceled("WenCai processing"):
            return

        if not kwargs.get("query"):
            self.set_output("report", "")
            return ""

        last_e = ""
        for _ in range(self._param.max_retries+1):
            if self.check_if_canceled("WenCai processing"):
                return

            try:
                wencai_res = []
                res = pywencai.get(query=kwargs["query"], query_type=self._param.query_type, perpage=self._param.top_n)
                if self.check_if_canceled("WenCai processing"):
                    return

                if isinstance(res, pd.DataFrame):
                    wencai_res.append(res.to_markdown())
                elif isinstance(res, dict):
                    for item in res.items():
                        if self.check_if_canceled("WenCai processing"):
                            return

                        if isinstance(item[1], list):
                            wencai_res.append(item[0] + "\n" + pd.DataFrame(item[1]).to_markdown())
                        elif isinstance(item[1], str):
                            wencai_res.append(item[0] + "\n" + item[1])
                        elif isinstance(item[1], dict):
                            if "meta" in item[1].keys():
                                continue
                            wencai_res.append(pd.DataFrame.from_dict(item[1], orient='index').to_markdown())
                        elif isinstance(item[1], pd.DataFrame):
                            if "image_url" in item[1].columns:
                                continue
                            wencai_res.append(item[1].to_markdown())
                        else:
                            wencai_res.append(item[0] + "\n" + str(item[1]))
                self.set_output("report", "\n\n".join(wencai_res))
                return self.output("report")
            except Exception as e:
                if self.check_if_canceled("WenCai processing"):
                    return

                last_e = e
                logging.exception(f"WenCai error: {e}")
                time.sleep(self._param.delay_after_error)

        if last_e:
            self.set_output("_ERROR", str(last_e))
            return f"WenCai error: {last_e}"

        assert False, self.output()

    def thoughts(self) -> str:
        return "Pulling live financial data for `{}`.".format(self.get_input().get("query", "-_-!"))

```

## Detailed Analysis

### File Role in Repository

The file `agent/tools/wencai.py` is located in the `agent/tools` directory.

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
