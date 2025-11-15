# Documentation: agent/tools/searxng.py

## File Metadata

- **Path**: `agent/tools/searxng.py`
- **Size**: 6060 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/tools/searxng.py`.

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

#### Class: `SearXNGParam` (line 25)

**Docstring**: Define the SearXNG component parameters....

**Methods**: __init__, check, get_input_form

#### Class: `SearXNG` (line 77)

**Methods**: _invoke, thoughts

### Functions Defined

This file defines 5 function(s):

#### Function: `__init__` (line 30)

**Parameters**: self

#### Function: `check` (line 53)

**Parameters**: self

#### Function: `get_input_form` (line 63)

**Parameters**: self

#### Function: `_invoke` (line 81)

**Parameters**: self

#### Function: `thoughts` (line 165)

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
from agent.tools.base import ToolMeta, ToolParamBase, ToolBase
from common.connection_utils import timeout


class SearXNGParam(ToolParamBase):
    """
    Define the SearXNG component parameters.
    """

    def __init__(self):
        self.meta: ToolMeta = {
            "name": "searxng_search",
            "description": "SearXNG is a privacy-focused metasearch engine that aggregates results from multiple search engines without tracking users. It provides comprehensive web search capabilities.",
            "parameters": {
                "query": {
                    "type": "string",
                    "description": "The search keywords to execute with SearXNG. The keywords should be the most important words/terms(includes synonyms) from the original request.",
                    "default": "{sys.query}",
                    "required": True
                },
                "searxng_url": {
                    "type": "string",
                    "description": "The base URL of your SearXNG instance (e.g., http://localhost:4000). This is required to connect to your SearXNG server.",
                    "required": False,
                    "default": ""
                }
            }
        }
        super().__init__()
        self.top_n = 10
        self.searxng_url = ""

    def check(self):
        # Keep validation lenient so opening try-run panel won't fail without URL.
        # Coerce top_n to int if it comes as string from UI.
        try:
            if isinstance(self.top_n, str):
                self.top_n = int(self.top_n.strip())
        except Exception:
            pass
        self.check_positive_integer(self.top_n, "Top N")

    def get_input_form(self) -> dict[str, dict]:
        return {
            "query": {
                "name": "Query",
                "type": "line"
            },
            "searxng_url": {
                "name": "SearXNG URL",
                "type": "line",
                "placeholder": "http://localhost:4000"
            }
        }


class SearXNG(ToolBase, ABC):
    component_name = "SearXNG"

    @timeout(int(os.environ.get("COMPONENT_EXEC_TIMEOUT", 12)))
    def _invoke(self, **kwargs):
        if self.check_if_canceled("SearXNG processing"):
            return

        # Gracefully handle try-run without inputs
        query = kwargs.get("query")
        if not query or not isinstance(query, str) or not query.strip():
            self.set_output("formalized_content", "")
            return ""

        searxng_url = (getattr(self._param, "searxng_url", "") or kwargs.get("searxng_url") or "").strip()
        # In try-run, if no URL configured, just return empty instead of raising
        if not searxng_url:
            self.set_output("formalized_content", "")
            return ""

        last_e = ""
        for _ in range(self._param.max_retries+1):
            if self.check_if_canceled("SearXNG processing"):
                return

            try:
                search_params = {
                    'q': query,
                    'format': 'json',
                    'categories': 'general',
                    'language': 'auto',
                    'safesearch': 1,
                    'pageno': 1
                }

                response = requests.get(
                    f"{searxng_url}/search",
                    params=search_params,
                    timeout=10
                )
                response.raise_for_status()

                if self.check_if_canceled("SearXNG processing"):
                    return

                data = response.json()

                if not data or not isinstance(data, dict):
                    raise ValueError("Invalid response from SearXNG")

                results = data.get("results", [])
                if not isinstance(results, list):
                    raise ValueError("Invalid results format from SearXNG")

                results = results[:self._param.top_n]

                if self.check_if_canceled("SearXNG processing"):
                    return

                self._retrieve_chunks(results,
                                      get_title=lambda r: r.get("title", ""),
                                      get_url=lambda r: r.get("url", ""),
                                      get_content=lambda r: r.get("content", ""))

                self.set_output("json", results)
                return self.output("formalized_content")

            except requests.RequestException as e:
                if self.check_if_canceled("SearXNG processing"):
                    return

                last_e = f"Network error: {e}"
                logging.exception(f"SearXNG network error: {e}")
                time.sleep(self._param.delay_after_error)
            except Exception as e:
                if self.check_if_canceled("SearXNG processing"):
                    return

                last_e = str(e)
                logging.exception(f"SearXNG error: {e}")
                time.sleep(self._param.delay_after_error)

        if last_e:
            self.set_output("_ERROR", last_e)
            return f"SearXNG error: {last_e}"

        assert False, self.output()

    def thoughts(self) -> str:
        return """
Keywords: {}
Searching with SearXNG for relevant results...
                """.format(self.get_input().get("query", "-_-!"))

```

## Detailed Analysis

### File Role in Repository

The file `agent/tools/searxng.py` is located in the `agent/tools` directory.

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
