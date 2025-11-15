# File Documentation: agent/component/invoke.py

## File Metadata

- **Path**: `agent/component/invoke.py`
- **Extension**: `.py`
- **Lines**: 145
- **Characters**: 5,481
- **Size**: 5,481 bytes
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

- `InvokeParam`: Class definition
- `Invoke`: Class definition

### Imports (10)

- `import json`
- `import logging`
- `import os`
- `import re`
- `import time`
- `from abc import ABC`
- `import requests`
- `from agent.component.base import ComponentBase, ComponentParamBase`
- `from common.connection_utils import timeout`
- `from deepdoc.parser import HtmlParser`

## Code Structure Analysis

- Total lines: 145
- Blank lines: 25 (17.2%)
- Comment lines: ~18 (12.4%)
- Code lines: ~102


## Dependencies and Imports

- `import json`
- `import logging`
- `import os`
- `import re`
- `import time`
- `from abc import ABC`
- `import requests`
- `from agent.component.base import ComponentBase, ComponentParamBase`
- `from common.connection_utils import timeout`
- `from deepdoc.parser import HtmlParser`

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
- Imports from `agent.component.base`
- Imports from `common.connection_utils`
- Imports from `deepdoc.parser`
- Potential test file: `test_invoke.py`

## Keywords

ABC, ANY, All, Apache, Authors, BASIS, COMPONENT_EXEC_TIMEOUT, CONDITIONS, Check, Clean, ComponentBase, ComponentParamBase, Copyright, Crawler, Data, Define, End, Exception, False, HTML, HtmlParser, Http, InfiniFlow, Invoke, InvokeParam, KIND, LICENSE, License, Licensed, New, None, Python, Reserved, Rights, See, The, Timeout, Type, URL, Unless, Version, WARRANTIES, WITHOUT, Waiting, You, __init__, _invoke, check, def, replace_variable...

---
*Generated by RAGFlow Repository Documentation Generator*
