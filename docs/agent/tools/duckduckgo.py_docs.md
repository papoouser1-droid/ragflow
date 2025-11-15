# Documentation: agent/tools/duckduckgo.py

## File Metadata

- **Path**: `agent/tools/duckduckgo.py`
- **Size**: 5850 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/tools/duckduckgo.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `os`
- `time`
- `abc`
- `duckduckgo_search`
- `agent.tools.base`
- `common.connection_utils`

### Classes Defined

This file defines 2 class(es):

#### Class: `DuckDuckGoParam` (line 25)

**Docstring**: Define the DuckDuckGo component parameters....

**Methods**: __init__, check, get_input_form

#### Class: `DuckDuckGo` (line 73)

**Methods**: _invoke, thoughts

### Functions Defined

This file defines 5 function(s):

#### Function: `__init__` (line 30)

**Parameters**: self

#### Function: `check` (line 54)

**Parameters**: self

#### Function: `get_input_form` (line 58)

**Parameters**: self

#### Function: `_invoke` (line 77)

**Parameters**: self

#### Function: `thoughts` (line 139)

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
from duckduckgo_search import DDGS
from agent.tools.base import ToolMeta, ToolParamBase, ToolBase
from common.connection_utils import timeout


class DuckDuckGoParam(ToolParamBase):
    """
    Define the DuckDuckGo component parameters.
    """

    def __init__(self):
        self.meta:ToolMeta = {
            "name": "duckduckgo_search",
            "description": "DuckDuckGo is a search engine focused on privacy. It offers search capabilities for web pages, images, and provides translation services. DuckDuckGo also features a private AI chat interface, providing users with an AI assistant that prioritizes data protection.",
            "parameters": {
                "query": {
                    "type": "string",
                    "description": "The search keywords to execute with DuckDuckGo. The keywords should be the most important words/terms(includes synonyms) from the original request.",
                    "default": "{sys.query}",
                    "required": True
                },
                "channel": {
                    "type": "string",
                    "description": "default:general. The category of the search. `news` is useful for retrieving real-time updates, particularly about politics, sports, and major current events covered by mainstream media sources. `general` is for broader, more general-purpose searches that may include a wide range of sources.",
                    "enum": ["general", "news"],
                    "default": "general",
                    "required": False,
                },
            }
        }
        super().__init__()
        self.top_n = 10
        self.channel = "text"

    def check(self):
        self.check_positive_integer(self.top_n, "Top N")
        self.check_valid_value(self.channel, "Web Search or News", ["text", "news"])

    def get_input_form(self) -> dict[str, dict]:
        return {
            "query": {
                "name": "Query",
                "type": "line"
            },
            "channel": {
                "name": "Channel",
                "type": "options",
                "value": "general",
                "options": ["general", "news"]
            }
        }


class DuckDuckGo(ToolBase, ABC):
    component_name = "DuckDuckGo"

    @timeout(int(os.environ.get("COMPONENT_EXEC_TIMEOUT", 12)))
    def _invoke(self, **kwargs):
        if self.check_if_canceled("DuckDuckGo processing"):
            return

        if not kwargs.get("query"):
            self.set_output("formalized_content", "")
            return ""

        last_e = ""
        for _ in range(self._param.max_retries+1):
            if self.check_if_canceled("DuckDuckGo processing"):
                return

            try:
                if kwargs.get("topic", "general") == "general":
                    with DDGS() as ddgs:
                        if self.check_if_canceled("DuckDuckGo processing"):
                            return

                        # {'title': '', 'href': '', 'body': ''}
                        duck_res = ddgs.text(kwargs["query"], max_results=self._param.top_n)

                        if self.check_if_canceled("DuckDuckGo processing"):
                            return

                        self._retrieve_chunks(duck_res,
                                              get_title=lambda r: r["title"],
                                              get_url=lambda r: r.get("href", r.get("url")),
                                              get_content=lambda r: r["body"])
                        self.set_output("json", duck_res)
                        return self.output("formalized_content")
                else:
                    with DDGS() as ddgs:
                        if self.check_if_canceled("DuckDuckGo processing"):
                            return

                        # {'date': '', 'title': '', 'body': '', 'url': '', 'image': '', 'source': ''}
                        duck_res = ddgs.news(kwargs["query"], max_results=self._param.top_n)

                        if self.check_if_canceled("DuckDuckGo processing"):
                            return

                        self._retrieve_chunks(duck_res,
                                              get_title=lambda r: r["title"],
                                              get_url=lambda r: r.get("href", r.get("url")),
                                              get_content=lambda r: r["body"])
                        self.set_output("json", duck_res)
                        return self.output("formalized_content")
            except Exception as e:
                if self.check_if_canceled("DuckDuckGo processing"):
                    return

                last_e = e
                logging.exception(f"DuckDuckGo error: {e}")
                time.sleep(self._param.delay_after_error)

        if last_e:
            self.set_output("_ERROR", str(last_e))
            return f"DuckDuckGo error: {last_e}"

        assert False, self.output()

    def thoughts(self) -> str:
        return """
Keywords: {}
Looking for the most relevant articles.
                """.format(self.get_input().get("query", "-_-!"))

```

## Detailed Analysis

### File Role in Repository

The file `agent/tools/duckduckgo.py` is located in the `agent/tools` directory.

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
- [email.py](email.py_docs.md)
- [exesql.py](exesql.py_docs.md)
- [github.py](github.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
