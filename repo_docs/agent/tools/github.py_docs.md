# File Documentation: agent/tools/github.py

## File Metadata

- **Path**: `agent/tools/github.py`
- **Extension**: `.py`
- **Lines**: 105
- **Characters**: 3,930
- **Size**: 3,930 bytes
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
import logging
import os
import time
from abc import ABC
import requests
from agent.tools.base import ToolParamBase, ToolMeta, ToolBase
from common.connection_utils import timeout


class GitHubParam(ToolParamBase):
    """
    Define the GitHub component parameters.
    """

    def __init__(self):
        self.meta:ToolMeta = {
            "name": "github_search",
            "description": """GitHub repository search is a feature that enables users to find specific repositories on the GitHub platform. This search functionality allows users to locate projects, codebases, and other content hosted on GitHub based on various criteria.""",
            "parameters": {
                "query": {
                    "type": "string",
                    "description": "The search keywords to execute with GitHub. The keywords should be the most important words/terms(includes synonyms) from the original request.",
                    "default": "{sys.query}",
                    "required": True
                }
            }
        }
        super().__init__()
        self.top_n = 10

    def check(self):
        self.check_positive_integer(self.top_n, "Top N")

    def get_input_form(self) -> dict[str, dict]:
        return {
            "query": {
                "name": "Query",
                "type": "line"
            }
        }

class GitHub(ToolBase, ABC):
    component_name = "GitHub"

    @timeout(int(os.environ.get("COMPONENT_EXEC_TIMEOUT", 12)))
    def _invoke(self, **kwargs):
        if self.check_if_canceled("GitHub processing"):
            return

        if not kwargs.get("query"):
            self.set_output("formalized_content", "")
            return ""

        last_e = ""
        for _ in range(self._param.max_retries+1):
            if self.check_if_canceled("GitHub processing"):
                return

            try:
                url = 'https://api.github.com/search/repositories?q=' + kwargs["query"] + '&sort=stars&order=desc&per_page=' + str(
                    self._param.top_n)
                headers = {"Content-Type": "application/vnd.github+json", "X-GitHub-Api-Version": '2022-11-28'}
                response = requests.get(url=url, headers=headers).json()

                if self.check_if_canceled("GitHub processing"):
                    return

                self._retrieve_chunks(response['items'],
                                      get_title=lambda r: r["name"],
                                      get_url=lambda r: r["html_url"],
                                      get_content=lambda r: str(r["description"]) + '\n stars:' + str(r['watchers']))
                self.set_output("json", response['items'])
                return self.output("formalized_content")
            except Exception as e:
                if self.check_if_canceled("GitHub processing"):
                    return

                last_e = e
                logging.exception(f"GitHub error: {e}")
                time.sleep(self._param.delay_after_error)

        if last_e:
            self.set_output("_ERROR", str(last_e))
            return f"GitHub error: {last_e}"

        assert False, self.output()

    def thoughts(self) -> str:
        return "Scanning GitHub repos related to `{}`.".format(self.get_input().get("query", "-_-!"))

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

- `GitHubParam`: Class definition
- `GitHub`: Class definition

### Imports (7)

- `import logging`
- `import os`
- `import time`
- `from abc import ABC`
- `import requests`
- `from agent.tools.base import ToolParamBase, ToolMeta, ToolBase`
- `from common.connection_utils import timeout`

## Code Structure Analysis

- Total lines: 105
- Blank lines: 17 (16.2%)
- Comment lines: ~17 (16.2%)
- Code lines: ~71


## Dependencies and Imports

- `import logging`
- `import os`
- `import time`
- `from abc import ABC`
- `import requests`
- `from agent.tools.base import ToolParamBase, ToolMeta, ToolBase`
- `from common.connection_utils import timeout`

## Design & Architecture

This file is located in the `agent` directory, specifically within `agent/tools`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 2 loop(s) - consider algorithmic complexity
- Contains database queries - ensure proper indexing and query optimization

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
- Imports from `agent.tools.base`
- Imports from `common.connection_utils`
- Potential test file: `test_github.py`

## Keywords

ABC, ANY, All, Apache, Api, Authors, BASIS, COMPONENT_EXEC_TIMEOUT, CONDITIONS, Content, Copyright, Define, Exception, False, GitHub, GitHubParam, InfiniFlow, KIND, LICENSE, License, Licensed, Python, Query, Reserved, Rights, Scanning, See, The, This, ToolBase, ToolMeta, ToolParamBase, Top, True, Type, Unless, Version, WARRANTIES, WITHOUT, You, __init__, _invoke, check, get_input_form, thoughts, timeout

---
*Generated by RAGFlow Repository Documentation Generator*
