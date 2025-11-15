# File Documentation: rag/flow/base.py

## File Metadata

- **Path**: `rag/flow/base.py`
- **Extension**: `.py`
- **Lines**: 62
- **Characters**: 2,218
- **Size**: 2,218 bytes
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
import logging
import os
import time
from functools import partial
from typing import Any
import trio
from agent.component.base import ComponentBase, ComponentParamBase
from common.connection_utils import timeout


class ProcessParamBase(ComponentParamBase):
    def __init__(self):
        super().__init__()
        self.timeout = 100000000
        self.persist_logs = True


class ProcessBase(ComponentBase):
    def __init__(self, pipeline, id, param: ProcessParamBase):
        super().__init__(pipeline, id, param)
        if hasattr(self._canvas, "callback"):
            self.callback = partial(self._canvas.callback, id)
        else:
            self.callback = partial(lambda *args, **kwargs: None, id)

    async def invoke(self, **kwargs) -> dict[str, Any]:
        self.set_output("_created_time", time.perf_counter())
        for k, v in kwargs.items():
            self.set_output(k, v)
        try:
            with trio.fail_after(self._param.timeout):
                await self._invoke(**kwargs)
                self.callback(1, "Done")
        except Exception as e:
            if self.get_exception_default_value():
                self.set_exception_default_value()
            else:
                self.set_output("_ERROR", str(e))
            logging.exception(e)
            self.callback(-1, str(e))
        self.set_output("_elapsed_time", time.perf_counter() - self.output("_created_time"))
        return self.output()

    @timeout(int(os.environ.get("COMPONENT_EXEC_TIMEOUT", 10 * 60)))
    async def _invoke(self, **kwargs):
        raise NotImplementedError()

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

- `ProcessParamBase`: Class definition
- `ProcessBase`: Class definition

### Imports (8)

- `import logging`
- `import os`
- `import time`
- `from functools import partial`
- `from typing import Any`
- `import trio`
- `from agent.component.base import ComponentBase, ComponentParamBase`
- `from common.connection_utils import timeout`

## Code Structure Analysis

- Total lines: 62
- Blank lines: 7 (11.3%)
- Comment lines: ~15 (24.2%)
- Code lines: ~40


## Dependencies and Imports

- `import logging`
- `import os`
- `import time`
- `from functools import partial`
- `from typing import Any`
- `import trio`
- `from agent.component.base import ComponentBase, ComponentParamBase`
- `from common.connection_utils import timeout`

## Design & Architecture

This file is located in the `rag` directory, specifically within `rag/flow`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 2 loop(s) - consider algorithmic complexity
- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `rag/flow/` directory
- Imports from `functools`
- Imports from `agent.component.base`
- Imports from `common.connection_utils`
- Potential test file: `test_base.py`

## Keywords

ANY, All, Any, Apache, Authors, BASIS, COMPONENT_EXEC_TIMEOUT, CONDITIONS, ComponentBase, ComponentParamBase, Copyright, Done, Exception, InfiniFlow, KIND, LICENSE, License, Licensed, None, NotImplementedError, ProcessBase, ProcessParamBase, Python, Reserved, Rights, See, The, True, Unless, Version, WARRANTIES, WITHOUT, You, __init__, _invoke, invoke, timeout

---
*Generated by RAGFlow Repository Documentation Generator*
