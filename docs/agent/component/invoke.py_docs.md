# Documentation: agent/component/invoke.py

## File Metadata

- **Path**: `agent/component/invoke.py`
- **Size**: 5481 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/component/invoke.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `json`
- `logging`
- `os`
- `re`
- `time`
- `abc`
- `requests`
- `agent.component.base`
- `common.connection_utils`
- `deepdoc.parser`

### Classes Defined

This file defines 2 class(es):

#### Class: `InvokeParam` (line 30)

**Docstring**: Define the Crawler component parameters....

**Methods**: __init__, check

#### Class: `Invoke` (line 54)

**Methods**: _invoke, thoughts

### Functions Defined

This file defines 5 function(s):

#### Function: `__init__` (line 35)

**Parameters**: self

#### Function: `check` (line 46)

**Parameters**: self

#### Function: `_invoke` (line 58)

**Parameters**: self

#### Function: `thoughts` (line 143)

**Parameters**: self

#### Function: `replace_variable` (line 71)

**Parameters**: match

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
import logging
import os
import re
import time
from abc import ABC

import requests

from agent.component.base import ComponentBase, ComponentParamBase
from common.connection_utils import timeout
from deepdoc.parser import HtmlParser


class InvokeParam(ComponentParamBase):
    """
    Define the Crawler component parameters.
    """

    def __init__(self):
        super().__init__()
        self.proxy = None
        self.headers = ""
        self.method = "get"
        self.variables = []
        self.url = ""
        self.timeout = 60
        self.clean_html = False
        self.datatype = "json"  # New parameter to determine data posting type

    def check(self):
        self.check_valid_value(self.method.lower(), "Type of content from the crawler", ["get", "post", "put"])
        self.check_empty(self.url, "End point URL")
        self.check_positive_integer(self.timeout, "Timeout time in second")
        self.check_boolean(self.clean_html, "Clean HTML")
        self.check_valid_value(self.datatype.lower(), "Data post type", ["json", "formdata"])  # Check for valid datapost value


class Invoke(ComponentBase, ABC):
    component_name = "Invoke"

    @timeout(int(os.environ.get("COMPONENT_EXEC_TIMEOUT", 3)))
    def _invoke(self, **kwargs):
        if self.check_if_canceled("Invoke processing"):
            return

        args = {}
        for para in self._param.variables:
            if para.get("value"):
                args[para["key"]] = para["value"]
            else:
                args[para["key"]] = self._canvas.get_variable_value(para["ref"])

        url = self._param.url.strip()

        def replace_variable(match):
            var_name = match.group(1)
            try:
                value = self._canvas.get_variable_value(var_name)
                return str(value or "")
            except Exception:
                return ""

        # {base_url} or {component_id@variable_name}
        url = re.sub(r"\{([a-zA-Z_][a-zA-Z0-9_.@-]*)\}", replace_variable, url)

        if url.find("http") != 0:
            url = "http://" + url

        method = self._param.method.lower()
        headers = {}
        if self._param.headers:
            headers = json.loads(self._param.headers)
        proxies = None
        if re.sub(r"https?:?/?/?", "", self._param.proxy):
            proxies = {"http": self._param.proxy, "https": self._param.proxy}

        last_e = ""
        for _ in range(self._param.max_retries + 1):
            if self.check_if_canceled("Invoke processing"):
                return

            try:
                if method == "get":
                    response = requests.get(url=url, params=args, headers=headers, proxies=proxies, timeout=self._param.timeout)
                    if self._param.clean_html:
                        sections = HtmlParser()(None, response.content)
                        self.set_output("result", "\n".join(sections))
                    else:
                        self.set_output("result", response.text)

                if method == "put":
                    if self._param.datatype.lower() == "json":
                        response = requests.put(url=url, json=args, headers=headers, proxies=proxies, timeout=self._param.timeout)
                    else:
                        response = requests.put(url=url, data=args, headers=headers, proxies=proxies, timeout=self._param.timeout)
                    if self._param.clean_html:
                        sections = HtmlParser()(None, response.content)
                        self.set_output("result", "\n".join(sections))
                    else:
                        self.set_output("result", response.text)

                if method == "post":
                    if self._param.datatype.lower() == "json":
                        response = requests.post(url=url, json=args, headers=headers, proxies=proxies, timeout=self._param.timeout)
                    else:
                        response = requests.post(url=url, data=args, headers=headers, proxies=proxies, timeout=self._param.timeout)
                    if self._param.clean_html:
                        self.set_output("result", "\n".join(sections))
                    else:
                        self.set_output("result", response.text)

                return self.output("result")
            except Exception as e:
                if self.check_if_canceled("Invoke processing"):
                    return

                last_e = e
                logging.exception(f"Http request error: {e}")
                time.sleep(self._param.delay_after_error)

        if last_e:
            self.set_output("_ERROR", str(last_e))
            return f"Http request error: {last_e}"

        assert False, self.output()

    def thoughts(self) -> str:
        return "Waiting for the server respond..."

```

## Detailed Analysis

### File Role in Repository

The file `agent/component/invoke.py` is located in the `agent/component` directory.

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
- [iteration.py](iteration.py_docs.md)
- [iterationitem.py](iterationitem.py_docs.md)
- [list_operations.py](list_operations.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
