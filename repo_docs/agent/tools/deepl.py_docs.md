# File Documentation: agent/tools/deepl.py

## File Metadata

- **Path**: `agent/tools/deepl.py`
- **Extension**: `.py`
- **Lines**: 69
- **Characters**: 2,668
- **Size**: 2,668 bytes
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
from abc import ABC
from agent.component.base import ComponentBase, ComponentParamBase
import deepl


class DeepLParam(ComponentParamBase):
    """
    Define the DeepL component parameters.
    """

    def __init__(self):
        super().__init__()
        self.auth_key = "xxx"
        self.parameters = []
        self.source_lang = 'ZH'
        self.target_lang = 'EN-GB'

    def check(self):
        self.check_positive_integer(self.top_n, "Top N")
        self.check_valid_value(self.source_lang, "Source language",
                               ['AR', 'BG', 'CS', 'DA', 'DE', 'EL', 'EN', 'ES', 'ET', 'FI', 'FR', 'HU', 'ID', 'IT',
                                'JA', 'KO', 'LT', 'LV', 'NB', 'NL', 'PL', 'PT', 'RO', 'RU', 'SK', 'SL', 'SV', 'TR',
                                'UK', 'ZH'])
        self.check_valid_value(self.target_lang, "Target language",
                               ['AR', 'BG', 'CS', 'DA', 'DE', 'EL', 'EN-GB', 'EN-US', 'ES', 'ET', 'FI', 'FR', 'HU',
                                'ID', 'IT', 'JA', 'KO', 'LT', 'LV', 'NB', 'NL', 'PL', 'PT-BR', 'PT-PT', 'RO', 'RU',
                                'SK', 'SL', 'SV', 'TR', 'UK', 'ZH'])


class DeepL(ComponentBase, ABC):
    component_name = "DeepL"

    def _run(self, history, **kwargs):
        if self.check_if_canceled("DeepL processing"):
            return
        ans = self.get_input()
        ans = " - ".join(ans["content"]) if "content" in ans else ""
        if not ans:
            return DeepL.be_output("")

        if self.check_if_canceled("DeepL processing"):
            return

        try:
            translator = deepl.Translator(self._param.auth_key)
            result = translator.translate_text(ans, source_lang=self._param.source_lang,
                                               target_lang=self._param.target_lang)

            return DeepL.be_output(result.text)
        except Exception as e:
            if self.check_if_canceled("DeepL processing"):
                return
            DeepL.be_output("**Error**:" + str(e))

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

- `DeepLParam`: Class definition
- `DeepL`: Class definition

### Imports (3)

- `from abc import ABC`
- `from agent.component.base import ComponentBase, ComponentParamBase`
- `import deepl`

## Code Structure Analysis

- Total lines: 69
- Blank lines: 11 (15.9%)
- Comment lines: ~17 (24.6%)
- Code lines: ~41


## Dependencies and Imports

- `from abc import ABC`
- `from agent.component.base import ComponentBase, ComponentParamBase`
- `import deepl`

## Design & Architecture

This file is located in the `agent` directory, specifically within `agent/tools`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity

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
- Imports from `agent.component.base`
- Potential test file: `test_deepl.py`

## Keywords

ABC, ANY, All, Apache, Authors, BASIS, CONDITIONS, ComponentBase, ComponentParamBase, Copyright, DeepL, DeepLParam, Define, Error, Exception, InfiniFlow, KIND, LICENSE, License, Licensed, Python, Reserved, Rights, See, Source, Target, The, Top, Translator, Unless, Version, WARRANTIES, WITHOUT, You, __init__, _run, check

---
*Generated by RAGFlow Repository Documentation Generator*
