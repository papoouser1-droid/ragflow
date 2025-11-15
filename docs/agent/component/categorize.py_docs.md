# Documentation: agent/component/categorize.py

## File Metadata

- **Path**: `agent/component/categorize.py`
- **Size**: 5488 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/component/categorize.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `os`
- `re`
- `abc`
- `common.constants`
- `api.db.services.llm_service`
- `agent.component.llm`
- `common.connection_utils`
- `rag.llm.chat_model`

### Classes Defined

This file defines 2 class(es):

#### Class: `CategorizeParam` (line 28)

**Docstring**: Define the categorize component parameters....

**Methods**: __init__, check, get_input_form, update_prompt

#### Class: `Categorize` (line 96)

**Methods**: _invoke, thoughts

### Functions Defined

This file defines 6 function(s):

#### Function: `__init__` (line 33)

**Parameters**: self

#### Function: `check` (line 40)

**Parameters**: self

#### Function: `get_input_form` (line 49)

**Parameters**: self

#### Function: `update_prompt` (line 57)

**Parameters**: self

#### Function: `_invoke` (line 100)

**Parameters**: self

#### Function: `thoughts` (line 147)

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
import re
from abc import ABC

from common.constants import LLMType
from api.db.services.llm_service import LLMBundle
from agent.component.llm import LLMParam, LLM
from common.connection_utils import timeout
from rag.llm.chat_model import ERROR_PREFIX


class CategorizeParam(LLMParam):

    """
    Define the categorize component parameters.
    """
    def __init__(self):
        super().__init__()
        self.category_description = {}
        self.query = "sys.query"
        self.message_history_window_size = 1
        self.update_prompt()

    def check(self):
        self.check_positive_integer(self.message_history_window_size, "[Categorize] Message window size > 0")
        self.check_empty(self.category_description, "[Categorize] Category examples")
        for k, v in self.category_description.items():
            if not k:
                raise ValueError("[Categorize] Category name can not be empty!")
            if not v.get("to"):
                raise ValueError(f"[Categorize] 'To' of category {k} can not be empty!")

    def get_input_form(self) -> dict[str, dict]:
        return {
            "query": {
                "type": "line",
                "name": "Query"
            }
        }

    def update_prompt(self):
        cate_lines = []
        for c, desc in self.category_description.items():
            for line in desc.get("examples", []):
                if not line:
                    continue
                cate_lines.append("USER: \"" + re.sub(r"\n", "    ", line, flags=re.DOTALL) + "\" → "+c)

        descriptions = []
        for c, desc in self.category_description.items():
            if desc.get("description"):
                descriptions.append(
                    "\n------\nCategory: {}\nDescription: {}".format(c, desc["description"]))

        self.sys_prompt = """
You are an advanced classification system that categorizes user questions into specific types. Analyze the input question and classify it into ONE of the following categories:
{}

Here's description of each category:
 - {}

---- Instructions ----
 - Consider both explicit mentions and implied context
 - Prioritize the most specific applicable category
 - Return only the category name without explanations
 - Use "Other" only when no other category fits

 """.format(
            "\n - ".join(list(self.category_description.keys())),
            "\n".join(descriptions)
        )

        if cate_lines:
            self.sys_prompt += """
---- Examples ----
{}
""".format("\n".join(cate_lines))


class Categorize(LLM, ABC):
    component_name = "Categorize"

    @timeout(int(os.environ.get("COMPONENT_EXEC_TIMEOUT", 10*60)))
    def _invoke(self, **kwargs):
        if self.check_if_canceled("Categorize processing"):
            return

        msg = self._canvas.get_history(self._param.message_history_window_size)
        if not msg:
            msg = [{"role": "user", "content": ""}]
        if kwargs.get("sys.query"):
            msg[-1]["content"] = kwargs["sys.query"]
            self.set_input_value("sys.query", kwargs["sys.query"])
        else:
            msg[-1]["content"] = self._canvas.get_variable_value(self._param.query)
            self.set_input_value(self._param.query, msg[-1]["content"])
        self._param.update_prompt()
        chat_mdl = LLMBundle(self._canvas.get_tenant_id(), LLMType.CHAT, self._param.llm_id)

        user_prompt = """
---- Real Data ----
{} →
""".format(" | ".join(["{}: \"{}\"".format(c["role"].upper(), re.sub(r"\n", "", c["content"], flags=re.DOTALL)) for c in msg]))

        if self.check_if_canceled("Categorize processing"):
            return

        ans = chat_mdl.chat(self._param.sys_prompt, [{"role": "user", "content": user_prompt}], self._param.gen_conf())
        logging.info(f"input: {user_prompt}, answer: {str(ans)}")
        if ERROR_PREFIX in ans:
            raise Exception(ans)

        if self.check_if_canceled("Categorize processing"):
            return

        # Count the number of times each category appears in the answer.
        category_counts = {}
        for c in self._param.category_description.keys():
            count = ans.lower().count(c.lower())
            category_counts[c] = count

        cpn_ids = list(self._param.category_description.items())[-1][1]["to"]
        max_category = list(self._param.category_description.keys())[0]
        if any(category_counts.values()):
            max_category = max(category_counts.items(), key=lambda x: x[1])[0]
            cpn_ids = self._param.category_description[max_category]["to"]

        self.set_output("category_name", max_category)
        self.set_output("_next", cpn_ids)

    def thoughts(self) -> str:
        return "Which should it falls into {}? ...".format(",".join([f"`{c}`" for c, _ in self._param.category_description.items()]))

```

## Detailed Analysis

### File Role in Repository

The file `agent/component/categorize.py` is located in the `agent/component` directory.

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
- [data_operations.py](data_operations.py_docs.md)
- [fillup.py](fillup.py_docs.md)
- [invoke.py](invoke.py_docs.md)
- [iteration.py](iteration.py_docs.md)
- [iterationitem.py](iterationitem.py_docs.md)
- [list_operations.py](list_operations.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
