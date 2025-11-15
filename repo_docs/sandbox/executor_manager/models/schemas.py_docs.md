# File Documentation: sandbox/executor_manager/models/schemas.py

## File Metadata

- **Path**: `sandbox/executor_manager/models/schemas.py`
- **Extension**: `.py`
- **Lines**: 54
- **Characters**: 1,871
- **Size**: 1,871 bytes
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
#
import base64
from typing import Optional

from pydantic import BaseModel, Field, field_validator

from models.enums import ResourceLimitType, ResultStatus, RuntimeErrorType, SupportLanguage, UnauthorizedAccessType


class CodeExecutionResult(BaseModel):
    status: ResultStatus
    stdout: str
    stderr: str
    exit_code: int
    detail: Optional[str] = None

    # Resource usage
    time_used_ms: Optional[float] = None
    memory_used_kb: Optional[float] = None

    # Error details
    resource_limit_type: Optional[ResourceLimitType] = None
    unauthorized_access_type: Optional[UnauthorizedAccessType] = None
    runtime_error_type: Optional[RuntimeErrorType] = None


class CodeExecutionRequest(BaseModel):
    code_b64: str = Field(..., description="Base64 encoded code string")
    language: SupportLanguage = Field(default=SupportLanguage.PYTHON, description="Programming language")
    arguments: Optional[dict] = Field(default={}, description="Arguments")

    @field_validator("code_b64")
    @classmethod
    def validate_base64(cls, v: str) -> str:
        try:
            base64.b64decode(v, validate=True)
            return v
        except Exception as e:
            raise ValueError(f"Invalid base64 encoding: {str(e)}")

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
#

## Detailed Walkthrough

### Classes (2)

- `CodeExecutionResult`: Class definition
- `CodeExecutionRequest`: Class definition

### Imports (4)

- `import base64`
- `from typing import Optional`
- `from pydantic import BaseModel, Field, field_validator`
- `from models.enums import ResourceLimitType, ResultStatus, RuntimeErrorType, SupportLanguage, UnauthorizedAccessType`

## Code Structure Analysis

- Total lines: 54
- Blank lines: 10 (18.5%)
- Comment lines: ~17 (31.5%)
- Code lines: ~27


## Dependencies and Imports

- `import base64`
- `from typing import Optional`
- `from pydantic import BaseModel, Field, field_validator`
- `from models.enums import ResourceLimitType, ResultStatus, RuntimeErrorType, SupportLanguage, UnauthorizedAccessType`

## Design & Architecture

This file is located in the `sandbox` directory, specifically within `sandbox/executor_manager/models`.

This file likely defines data models or schemas used throughout the application.

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

- Other files in `sandbox/executor_manager/models/` directory
- Imports from `pydantic`
- Imports from `models.enums`
- Potential test file: `test_schemas.py`

## Keywords

ANY, All, Apache, Arguments, Authors, BASIS, Base64, BaseModel, CONDITIONS, CodeExecutionRequest, CodeExecutionResult, Copyright, Error, Exception, Field, InfiniFlow, Invalid, KIND, LICENSE, License, Licensed, None, Optional, PYTHON, Programming, Python, Reserved, Resource, ResourceLimitType, ResultStatus, Rights, RuntimeErrorType, See, SupportLanguage, The, True, UnauthorizedAccessType, Unless, ValueError, Version, WARRANTIES, WITHOUT, You, classmethod, field_validator, validate_base64

---
*Generated by RAGFlow Repository Documentation Generator*
