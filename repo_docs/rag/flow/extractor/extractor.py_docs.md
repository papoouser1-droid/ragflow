# File Documentation: rag/flow/extractor/extractor.py

## File Metadata

- **Path**: `rag/flow/extractor/extractor.py`
- **Extension**: `.py`
- **Lines**: 64
- **Characters**: 2,305
- **Size**: 2,305 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

## Original Source

```python
#
#  Copyright 2025 The InfiniFlow Authors. All Rights Reserved.
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
import random
from copy import deepcopy
from agent.component.llm import LLMParam, LLM
from rag.flow.base import ProcessBase, ProcessParamBase


class ExtractorParam(ProcessParamBase, LLMParam):
    def __init__(self):
        super().__init__()
        self.field_name = ""

    def check(self):
        super().check()
        self.check_empty(self.field_name, "Result Destination")


class Extractor(ProcessBase, LLM):
    component_name = "Extractor"

    async def _invoke(self, **kwargs):
        self.set_output("output_format", "chunks")
        self.callback(random.randint(1, 5) / 100.0, "Start to generate.")
        inputs = self.get_input_elements()
        chunks = []
        chunks_key = ""
        args = {}
        for k, v in inputs.items():
            args[k] = v["value"]
            if isinstance(args[k], list):
                chunks = deepcopy(args[k])
                chunks_key = k

        if chunks:
            prog = 0
            for i, ck in enumerate(chunks):
                args[chunks_key] = ck["text"]
                msg, sys_prompt = self._sys_prompt_and_msg([], args)
                msg.insert(0, {"role": "system", "content": sys_prompt})
                ck[self._param.field_name] = self._generate(msg)
                prog += 1./len(chunks)
                if i % (len(chunks)//100+1) == 1:
                    self.callback(prog, f"{i+1} / {len(chunks)}")
            self.set_output("chunks", chunks)
        else:
            msg, sys_prompt = self._sys_prompt_and_msg([], args)
            msg.insert(0, {"role": "system", "content": sys_prompt})
            self.set_output("chunks", [{self._param.field_name: self._generate(msg)}])



```

## High-Level Overview

#
#  Copyright 2025 The InfiniFlow Authors. All Rights Reserved.
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

## Detailed Walkthrough

### Classes (2)

- `ExtractorParam`: Class definition
- `Extractor`: Class definition

### Imports (4)

- `import random`
- `from copy import deepcopy`
- `from agent.component.llm import LLMParam, LLM`
- `from rag.flow.base import ProcessBase, ProcessParamBase`

## Code Structure Analysis

- Total lines: 64
- Blank lines: 10 (15.6%)
- Comment lines: ~14 (21.9%)
- Code lines: ~40


## Dependencies and Imports

- `import random`
- `from copy import deepcopy`
- `from agent.component.llm import LLMParam, LLM`
- `from rag.flow.base import ProcessBase, ProcessParamBase`

## Design & Architecture

This file is located in the `rag` directory, specifically within `rag/flow/extractor`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 3 loop(s) - consider algorithmic complexity
- Uses asynchronous patterns for better performance

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

- Other files in `rag/flow/extractor/` directory
- Imports from `copy`
- Imports from `agent.component.llm`
- Imports from `rag.flow.base`
- Potential test file: `test_extractor.py`

## Keywords

ANY, All, Apache, Authors, BASIS, CONDITIONS, Copyright, Destination, Extractor, ExtractorParam, InfiniFlow, KIND, LICENSE, LLM, LLMParam, License, Licensed, ProcessBase, ProcessParamBase, Python, Reserved, Result, Rights, See, Start, The, Unless, Version, WARRANTIES, WITHOUT, You, __init__, _invoke, check

---
*Generated by RAGFlow Repository Documentation Generator*
