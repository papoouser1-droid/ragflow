# File Documentation: sandbox/executor_manager/api/handlers.py

## File Metadata

- **Path**: `sandbox/executor_manager/api/handlers.py`
- **Extension**: `.py`
- **Lines**: 50
- **Characters**: 2,119
- **Size**: 2,122 bytes
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

from core.container import _CONTAINER_EXECUTION_SEMAPHORES
from core.logger import logger
from fastapi import Request
from models.enums import ResultStatus, SupportLanguage
from models.schemas import CodeExecutionRequest, CodeExecutionResult
from services.execution import execute_code
from services.limiter import limiter
from services.security import analyze_code_security


async def healthz_handler():
    return {"status": "ok"}


@limiter.limit("5/second")
async def run_code_handler(req: CodeExecutionRequest, request: Request):
    logger.info("🟢 Received /run request")

    async with _CONTAINER_EXECUTION_SEMAPHORES[req.language]:
        code = base64.b64decode(req.code_b64).decode("utf-8")
        if req.language == SupportLanguage.NODEJS:
            code += "\n\nmodule.exports = { main };"
            req.code_b64 = base64.b64encode(code.encode("utf-8")).decode("utf-8")
        is_safe, issues = analyze_code_security(code, language=req.language)
        if not is_safe:
            issue_details = "\n".join([f"Line {lineno}: {issue}" for issue, lineno in issues])
            return CodeExecutionResult(status=ResultStatus.PROGRAM_RUNNER_ERROR, stdout="", stderr=issue_details, exit_code=-999, detail="Code is unsafe")

        try:
            return await execute_code(req)
        except Exception as e:
            return CodeExecutionResult(status=ResultStatus.PROGRAM_RUNNER_ERROR, stdout="", stderr=str(e), exit_code=-999, detail="unhandled_exception")

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


### Imports (9)

- `import base64`
- `from core.container import _CONTAINER_EXECUTION_SEMAPHORES`
- `from core.logger import logger`
- `from fastapi import Request`
- `from models.enums import ResultStatus, SupportLanguage`
- `from models.schemas import CodeExecutionRequest, CodeExecutionResult`
- `from services.execution import execute_code`
- `from services.limiter import limiter`
- `from services.security import analyze_code_security`

## Code Structure Analysis

- Total lines: 50
- Blank lines: 8 (16.0%)
- Comment lines: ~15 (30.0%)
- Code lines: ~27


## Dependencies and Imports

- `import base64`
- `from core.container import _CONTAINER_EXECUTION_SEMAPHORES`
- `from core.logger import logger`
- `from fastapi import Request`
- `from models.enums import ResultStatus, SupportLanguage`
- `from models.schemas import CodeExecutionRequest, CodeExecutionResult`
- `from services.execution import execute_code`
- `from services.limiter import limiter`
- `from services.security import analyze_code_security`

## Design & Architecture

This file is located in the `sandbox` directory, specifically within `sandbox/executor_manager/api`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 2 loop(s) - consider algorithmic complexity
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

- Other files in `sandbox/executor_manager/api/` directory
- Imports from `core.container`
- Imports from `core.logger`
- Imports from `fastapi`
- Imports from `models.enums`
- Imports from `models.schemas`
- Potential test file: `test_handlers.py`

## Keywords

ANY, All, Apache, Authors, BASIS, CONDITIONS, Code, CodeExecutionRequest, CodeExecutionResult, Copyright, Exception, InfiniFlow, KIND, LICENSE, License, Licensed, Line, NODEJS, PROGRAM_RUNNER_ERROR, Python, Received, Request, Reserved, ResultStatus, Rights, See, SupportLanguage, The, Unless, Version, WARRANTIES, WITHOUT, You, healthz_handler, limiter, run_code_handler

---
*Generated by RAGFlow Repository Documentation Generator*
