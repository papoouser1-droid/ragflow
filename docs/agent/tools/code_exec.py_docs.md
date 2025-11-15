# Documentation: agent/tools/code_exec.py

## File Metadata

- **Path**: `agent/tools/code_exec.py`
- **Size**: 8156 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/tools/code_exec.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `base64`
- `logging`
- `os`
- `abc`
- `strenum`
- `typing`
- `pydantic`
- `agent.tools.base`
- `common.connection_utils`
- `common`
- `requests`

### Classes Defined

This file defines 4 class(es):

#### Class: `Language` (line 28)

#### Class: `CodeExecutionRequest` (line 33)

**Methods**: validate_base64, normalize_language

#### Class: `CodeExecParam` (line 59)

**Docstring**: Define the code sandbox component parameters....

**Methods**: __init__, check, get_input_form

#### Class: `CodeExec` (line 129)

**Methods**: _invoke, _execute_code, _encode_code, thoughts

### Functions Defined

This file defines 9 function(s):

#### Function: `validate_base64` (line 40)

**Parameters**: cls, v

#### Function: `normalize_language` (line 49)

**Parameters**: cls, v

#### Function: `__init__` (line 64)

**Parameters**: self

#### Function: `check` (line 115)

**Parameters**: self

#### Function: `get_input_form` (line 119)

**Parameters**: self

#### Function: `_invoke` (line 133)

**Parameters**: self

#### Function: `_execute_code` (line 153)

**Parameters**: self, language, code, arguments

#### Function: `_encode_code` (line 226)

**Parameters**: self, code

#### Function: `thoughts` (line 229)

**Parameters**: self

## Original Source Code

```py
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
import logging
import os
from abc import ABC
from strenum import StrEnum
from typing import Optional
from pydantic import BaseModel, Field, field_validator
from agent.tools.base import ToolParamBase, ToolBase, ToolMeta
from common.connection_utils import timeout
from common import settings


class Language(StrEnum):
    PYTHON = "python"
    NODEJS = "nodejs"


class CodeExecutionRequest(BaseModel):
    code_b64: str = Field(..., description="Base64 encoded code string")
    language: str = Field(default=Language.PYTHON.value, description="Programming language")
    arguments: Optional[dict] = Field(default={}, description="Arguments")

    @field_validator("code_b64")
    @classmethod
    def validate_base64(cls, v: str) -> str:
        try:
            base64.b64decode(v, validate=True)
            return v
        except Exception as e:
            raise ValueError(f"Invalid base64 encoding: {str(e)}")

    @field_validator("language", mode="before")
    @classmethod
    def normalize_language(cls, v) -> str:
        if isinstance(v, str):
            low = v.lower()
            if low in ("python", "python3"):
                return "python"
            elif low in ("javascript", "nodejs"):
                return "nodejs"
        raise ValueError(f"Unsupported language: {v}")


class CodeExecParam(ToolParamBase):
    """
    Define the code sandbox component parameters.
    """

    def __init__(self):
        self.meta:ToolMeta = {
            "name": "execute_code",
            "description": """
This tool has a sandbox that can execute code written in 'Python'/'Javascript'. It recieves a piece of code and return a Json string.
Here's a code example for Python(`main` function MUST be included):
def main() -> dict:
    \"\"\"
    Generate Fibonacci numbers within 100.
    \"\"\"
    def fibonacci_recursive(n):
        if n <= 1:
            return n
        else:
            return fibonacci_recursive(n-1) + fibonacci_recursive(n-2)
    return {
        "result": fibonacci_recursive(100),
    }

Here's a code example for Javascript(`main` function MUST be included and exported):
const axios = require('axios');
async function main(args) {
  try {
    const response = await axios.get('https://github.com/infiniflow/ragflow');
    console.log('Body:', response.data);
  } catch (error) {
    console.error('Error:', error.message);
  }
}
module.exports = { main };
            """,
            "parameters": {
                "lang": {
                    "type": "string",
                    "description": "The programming language of this piece of code.",
                    "enum": ["python", "javascript"],
                    "required": True,
                },
                "script": {
                    "type": "string",
                    "description": "A piece of code in right format. There MUST be main function.",
                    "required": True
                }
            }
        }
        super().__init__()
        self.lang = Language.PYTHON.value
        self.script = "def main(arg1: str, arg2: str) -> dict: return {\"result\": arg1 + arg2}"
        self.arguments = {}
        self.outputs = {"result": {"value": "", "type": "string"}}

    def check(self):
        self.check_valid_value(self.lang, "Support languages", ["python", "python3", "nodejs", "javascript"])
        self.check_empty(self.script, "Script")

    def get_input_form(self) -> dict[str, dict]:
        res = {}
        for k, v in self.arguments.items():
            res[k] = {
                "type": "line",
                "name": k
            }
        return res


class CodeExec(ToolBase, ABC):
    component_name = "CodeExec"

    @timeout(int(os.environ.get("COMPONENT_EXEC_TIMEOUT", 10*60)))
    def _invoke(self, **kwargs):
        if self.check_if_canceled("CodeExec processing"):
            return

        lang = kwargs.get("lang", self._param.lang)
        script = kwargs.get("script", self._param.script)
        arguments = {}
        for k, v in self._param.arguments.items():

            if kwargs.get(k):
                arguments[k] = kwargs[k]
                continue
            arguments[k] = self._canvas.get_variable_value(v) if v else None

        self._execute_code(
            language=lang,
            code=script,
            arguments=arguments
        )

    def _execute_code(self, language: str, code: str, arguments: dict):
        import requests

        if self.check_if_canceled("CodeExec execution"):
            return

        try:
            code_b64 = self._encode_code(code)
            code_req = CodeExecutionRequest(code_b64=code_b64, language=language, arguments=arguments).model_dump()
        except Exception as e:
            if self.check_if_canceled("CodeExec execution"):
                return

            self.set_output("_ERROR", "construct code request error: " + str(e))

        try:
            if self.check_if_canceled("CodeExec execution"):
                return "Task has been canceled"

            resp = requests.post(url=f"http://{settings.SANDBOX_HOST}:9385/run", json=code_req, timeout=int(os.environ.get("COMPONENT_EXEC_TIMEOUT", 10*60)))
            logging.info(f"http://{settings.SANDBOX_HOST}:9385/run,  code_req: {code_req}, resp.status_code {resp.status_code}:")

            if self.check_if_canceled("CodeExec execution"):
                return "Task has been canceled"

            if resp.status_code != 200:
                resp.raise_for_status()
            body = resp.json()
            if body:
                stderr = body.get("stderr")
                if stderr:
                    self.set_output("_ERROR", stderr)
                    return
                try:
                    rt = eval(body.get("stdout", ""))
                except Exception:
                    rt = body.get("stdout", "")
                logging.info(f"http://{settings.SANDBOX_HOST}:9385/run -> {rt}")
                if isinstance(rt, tuple):
                    for i, (k, o) in enumerate(self._param.outputs.items()):
                        if self.check_if_canceled("CodeExec execution"):
                            return

                        if k.find("_") == 0:
                            continue
                        o["value"] = rt[i]
                elif isinstance(rt, dict):
                    for i, (k, o) in enumerate(self._param.outputs.items()):
                        if self.check_if_canceled("CodeExec execution"):
                            return

                        if k not in rt or k.find("_") == 0:
                            continue
                        o["value"] = rt[k]
                else:
                    for i, (k, o) in enumerate(self._param.outputs.items()):
                        if self.check_if_canceled("CodeExec execution"):
                            return

                        if k.find("_") == 0:
                            continue
                        o["value"] = rt
            else:
                self.set_output("_ERROR", "There is no response from sandbox")

        except Exception as e:
            if self.check_if_canceled("CodeExec execution"):
                return

            self.set_output("_ERROR", "Exception executing code: " + str(e))

        return self.output()

    def _encode_code(self, code: str) -> str:
        return base64.b64encode(code.encode("utf-8")).decode("utf-8")

    def thoughts(self) -> str:
        return "Running a short script to process data."

```

## Detailed Analysis

### File Role in Repository

The file `agent/tools/code_exec.py` is located in the `agent/tools` directory.

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
- [crawler.py](crawler.py_docs.md)
- [deepl.py](deepl.py_docs.md)
- [duckduckgo.py](duckduckgo.py_docs.md)
- [email.py](email.py_docs.md)
- [exesql.py](exesql.py_docs.md)
- [github.py](github.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
