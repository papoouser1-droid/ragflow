# File Documentation: rag/flow/tokenizer/schema.py

## File Metadata

- **Path**: `rag/flow/tokenizer/schema.py`
- **Extension**: `.py`
- **Lines**: 54
- **Characters**: 2,536
- **Size**: 2,536 bytes
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
from typing import Any, Literal

from pydantic import BaseModel, ConfigDict, Field, model_validator


class TokenizerFromUpstream(BaseModel):
    created_time: float | None = Field(default=None, alias="_created_time")
    elapsed_time: float | None = Field(default=None, alias="_elapsed_time")

    name: str = ""
    file: dict | None = Field(default=None)

    output_format: Literal["json", "markdown", "text", "html", "chunks"] | None = Field(default=None)

    chunks: list[dict[str, Any]] | None = Field(default=None)

    json_result: list[dict[str, Any]] | None = Field(default=None, alias="json")
    markdown_result: str | None = Field(default=None, alias="markdown")
    text_result: str | None = Field(default=None, alias="text")
    html_result: str | None = Field(default=None, alias="html")

    model_config = ConfigDict(populate_by_name=True, extra="forbid")

    @model_validator(mode="after")
    def _check_payloads(self) -> "TokenizerFromUpstream":
        if self.chunks:
            return self

        if self.output_format in {"markdown", "text", "html"}:
            if self.output_format == "markdown" and not self.markdown_result:
                raise ValueError("output_format=markdown requires a markdown payload (field: 'markdown' or 'markdown_result').")
            if self.output_format == "text" and not self.text_result:
                raise ValueError("output_format=text requires a text payload (field: 'text' or 'text_result').")
            if self.output_format == "html" and not self.html_result:
                raise ValueError("output_format=text requires a html payload (field: 'html' or 'html_result').")
        else:
            if not self.json_result and not self.chunks:
                raise ValueError("When no chunks are provided and output_format is not markdown/text, a JSON list payload is required (field: 'json' or 'json_result').")
        return self

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

### Classes (1)

- `TokenizerFromUpstream`: Class definition

### Imports (2)

- `from typing import Any, Literal`
- `from pydantic import BaseModel, ConfigDict, Field, model_validator`

## Code Structure Analysis

- Total lines: 54
- Blank lines: 11 (20.4%)
- Comment lines: ~14 (25.9%)
- Code lines: ~29


## Dependencies and Imports

- `from typing import Any, Literal`
- `from pydantic import BaseModel, ConfigDict, Field, model_validator`

## Design & Architecture

This file is located in the `rag` directory, specifically within `rag/flow/tokenizer`.

This file likely defines data models or schemas used throughout the application.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `rag/flow/tokenizer/` directory
- Imports from `pydantic`
- Potential test file: `test_schema.py`

## Keywords

ANY, All, Any, Apache, Authors, BASIS, BaseModel, CONDITIONS, ConfigDict, Copyright, Field, InfiniFlow, JSON, KIND, LICENSE, License, Licensed, Literal, None, Python, Reserved, Rights, See, The, TokenizerFromUpstream, True, Unless, ValueError, Version, WARRANTIES, WITHOUT, When, You, _check_payloads, model_validator

---
*Generated by RAGFlow Repository Documentation Generator*
