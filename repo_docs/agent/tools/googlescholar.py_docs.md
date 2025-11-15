# File Documentation: agent/tools/googlescholar.py

## File Metadata

- **Path**: `agent/tools/googlescholar.py`
- **Extension**: `.py`
- **Lines**: 110
- **Characters**: 4,511
- **Size**: 4,513 bytes
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

- `GoogleScholarParam`: Class definition
- `GoogleScholar`: Class definition

### Imports (7)

- `import logging`
- `import os`
- `import time`
- `from abc import ABC`
- `from scholarly import scholarly`
- `from agent.tools.base import ToolMeta, ToolParamBase, ToolBase`
- `from common.connection_utils import timeout`

## Code Structure Analysis

- Total lines: 110
- Blank lines: 17 (15.5%)
- Comment lines: ~17 (15.5%)
- Code lines: ~76


## Dependencies and Imports

- `import logging`
- `import os`
- `import time`
- `from abc import ABC`
- `from scholarly import scholarly`
- `from agent.tools.base import ToolMeta, ToolParamBase, ToolBase`
- `from common.connection_utils import timeout`

## Design & Architecture

This file is located in the `agent` directory, specifically within `agent/tools`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 4 loop(s) - consider algorithmic complexity
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
- Imports from `scholarly`
- Imports from `agent.tools.base`
- Imports from `common.connection_utils`
- Potential test file: `test_googlescholar.py`

## Keywords

ABC, ANY, Abstract, All, Apache, Authors, BASIS, COMPONENT_EXEC_TIMEOUT, CONDITIONS, Copyright, Define, Exception, False, From, Google, GoogleScholar, GoogleScholarParam, InfiniFlow, KIND, LICENSE, License, Licensed, Looking, None, Python, Query, Reserved, Rights, Scholar, See, Sort_by, The, ToolBase, ToolMeta, ToolParamBase, Top, True, Unless, Version, WARRANTIES, WITHOUT, Whether, You, __init__, _invoke, check, get_input_form, thoughts, timeout

---
*Generated by RAGFlow Repository Documentation Generator*
