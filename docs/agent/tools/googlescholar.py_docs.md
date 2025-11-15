# Documentation: agent/tools/googlescholar.py

## File Metadata

- **Path**: `agent/tools/googlescholar.py`
- **Size**: 4513 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/tools/googlescholar.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `os`
- `time`
- `abc`
- `scholarly`
- `agent.tools.base`
- `common.connection_utils`

### Classes Defined

This file defines 2 class(es):

#### Class: `GoogleScholarParam` (line 25)

**Docstring**: Define the GoogleScholar component parameters....

**Methods**: __init__, check, get_input_form

#### Class: `GoogleScholar` (line 63)

**Methods**: _invoke, thoughts

### Functions Defined

This file defines 5 function(s):

#### Function: `__init__` (line 30)

**Parameters**: self

#### Function: `check` (line 50)

**Parameters**: self

#### Function: `get_input_form` (line 55)

**Parameters**: self

#### Function: `_invoke` (line 67)

**Parameters**: self

#### Function: `thoughts` (line 108)

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
from scholarly import scholarly
from agent.tools.base import ToolMeta, ToolParamBase, ToolBase
from common.connection_utils import timeout


class GoogleScholarParam(ToolParamBase):
    """
    Define the GoogleScholar component parameters.
    """

    def __init__(self):
        self.meta:ToolMeta = {
            "name": "google_scholar_search",
            "description": """Google Scholar provides a simple way to broadly search for scholarly literature. From one place, you can search across many disciplines and sources: articles, theses, books, abstracts and court opinions, from academic publishers, professional societies, online repositories, universities and other web sites. Google Scholar helps you find relevant work across the world of scholarly research.""",
            "parameters": {
                "query": {
                    "type": "string",
                    "description": "The search keyword to execute with Google Scholar. The keywords should be the most important words/terms(includes synonyms) from the original request.",
                    "default": "{sys.query}",
                    "required": True
                }
            }
        }
        super().__init__()
        self.top_n = 12
        self.sort_by = 'relevance'
        self.year_low = None
        self.year_high = None
        self.patents = True

    def check(self):
        self.check_positive_integer(self.top_n, "Top N")
        self.check_valid_value(self.sort_by, "GoogleScholar Sort_by", ['date', 'relevance'])
        self.check_boolean(self.patents, "Whether or not to include patents, defaults to True")

    def get_input_form(self) -> dict[str, dict]:
        return {
            "query": {
                "name": "Query",
                "type": "line"
            }
        }

class GoogleScholar(ToolBase, ABC):
    component_name = "GoogleScholar"

    @timeout(int(os.environ.get("COMPONENT_EXEC_TIMEOUT", 12)))
    def _invoke(self, **kwargs):
        if self.check_if_canceled("GoogleScholar processing"):
            return

        if not kwargs.get("query"):
            self.set_output("formalized_content", "")
            return ""

        last_e = ""
        for _ in range(self._param.max_retries+1):
            if self.check_if_canceled("GoogleScholar processing"):
                return

            try:
                scholar_client = scholarly.search_pubs(kwargs["query"], patents=self._param.patents, year_low=self._param.year_low,
                                                       year_high=self._param.year_high, sort_by=self._param.sort_by)

                if self.check_if_canceled("GoogleScholar processing"):
                    return

                self._retrieve_chunks(scholar_client,
                                      get_title=lambda r: r['bib']['title'],
                                      get_url=lambda r: r["pub_url"],
                                      get_content=lambda r: "\n author: " + ",".join(r['bib']['author']) + '\n Abstract: ' + r['bib'].get('abstract', 'no abstract')
                                      )
                self.set_output("json", list(scholar_client))
                return self.output("formalized_content")
            except Exception as e:
                if self.check_if_canceled("GoogleScholar processing"):
                    return

                last_e = e
                logging.exception(f"GoogleScholar error: {e}")
                time.sleep(self._param.delay_after_error)

        if last_e:
            self.set_output("_ERROR", str(last_e))
            return f"GoogleScholar error: {last_e}"

        assert False, self.output()

    def thoughts(self) -> str:
        return "Looking for scholarly papers on `{}`,” prioritising reputable sources.".format(self.get_input().get("query", "-_-!"))

```

## Detailed Analysis

### File Role in Repository

The file `agent/tools/googlescholar.py` is located in the `agent/tools` directory.

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
