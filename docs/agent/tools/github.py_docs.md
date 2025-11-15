# Documentation: agent/tools/github.py

## File Metadata

- **Path**: `agent/tools/github.py`
- **Size**: 3930 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/tools/github.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `os`
- `time`
- `abc`
- `requests`
- `agent.tools.base`
- `common.connection_utils`

### Classes Defined

This file defines 2 class(es):

#### Class: `GitHubParam` (line 25)

**Docstring**: Define the GitHub component parameters....

**Methods**: __init__, check, get_input_form

#### Class: `GitHub` (line 57)

**Methods**: _invoke, thoughts

### Functions Defined

This file defines 5 function(s):

#### Function: `__init__` (line 30)

**Parameters**: self

#### Function: `check` (line 46)

**Parameters**: self

#### Function: `get_input_form` (line 49)

**Parameters**: self

#### Function: `_invoke` (line 61)

**Parameters**: self

#### Function: `thoughts` (line 103)

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

## Detailed Analysis

### File Role in Repository

The file `agent/tools/github.py` is located in the `agent/tools` directory.

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
