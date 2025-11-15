# Documentation: common/connection_utils.py

## File Metadata

- **Path**: `common/connection_utils.py`
- **Size**: 4693 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `common/connection_utils.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `os`
- `queue`
- `threading`
- `typing`
- `asyncio`
- `trio`
- `functools`
- `flask`
- `common.constants`

### Functions Defined

This file defines 5 function(s):

#### Function: `timeout` (line 31)

**Parameters**: seconds, attempts

#### Function: `construct_response` (line 106)

**Parameters**: code, message, data, auth

#### Function: `decorator` (line 36)

**Parameters**: func

#### Function: `wrapper` (line 38)

**Parameters**: None

#### Function: `target` (line 41)

**Parameters**: None

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

import os
import queue
import threading
from typing import Any, Callable, Coroutine, Optional, Type, Union
import asyncio
import trio
from functools import wraps
from flask import make_response, jsonify
from common.constants import RetCode

TimeoutException = Union[Type[BaseException], BaseException]
OnTimeoutCallback = Union[Callable[..., Any], Coroutine[Any, Any, Any]]


def timeout(seconds: float | int | str = None, attempts: int = 2, *, exception: Optional[TimeoutException] = None,
            on_timeout: Optional[OnTimeoutCallback] = None):
    if isinstance(seconds, str):
        seconds = float(seconds)

    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            result_queue = queue.Queue(maxsize=1)

            def target():
                try:
                    result = func(*args, **kwargs)
                    result_queue.put(result)
                except Exception as e:
                    result_queue.put(e)

            thread = threading.Thread(target=target)
            thread.daemon = True
            thread.start()

            for a in range(attempts):
                try:
                    if os.environ.get("ENABLE_TIMEOUT_ASSERTION"):
                        result = result_queue.get(timeout=seconds)
                    else:
                        result = result_queue.get()
                    if isinstance(result, Exception):
                        raise result
                    return result
                except queue.Empty:
                    pass
            raise TimeoutError(f"Function '{func.__name__}' timed out after {seconds} seconds and {attempts} attempts.")

        @wraps(func)
        async def async_wrapper(*args, **kwargs) -> Any:
            if seconds is None:
                return await func(*args, **kwargs)

            for a in range(attempts):
                try:
                    if os.environ.get("ENABLE_TIMEOUT_ASSERTION"):
                        with trio.fail_after(seconds):
                            return await func(*args, **kwargs)
                    else:
                        return await func(*args, **kwargs)
                except trio.TooSlowError:
                    if a < attempts - 1:
                        continue
                    if on_timeout is not None:
                        if callable(on_timeout):
                            result = on_timeout()
                            if isinstance(result, Coroutine):
                                return await result
                            return result
                        return on_timeout

                    if exception is None:
                        raise TimeoutError(f"Operation timed out after {seconds} seconds and {attempts} attempts.")

                    if isinstance(exception, BaseException):
                        raise exception

                    if isinstance(exception, type) and issubclass(exception, BaseException):
                        raise exception(f"Operation timed out after {seconds} seconds and {attempts} attempts.")

                    raise RuntimeError("Invalid exception type provided")

        if asyncio.iscoroutinefunction(func):
            return async_wrapper
        return wrapper

    return decorator


def construct_response(code=RetCode.SUCCESS, message="success", data=None, auth=None):
    result_dict = {"code": code, "message": message, "data": data}
    response_dict = {}
    for key, value in result_dict.items():
        if value is None and key != "code":
            continue
        else:
            response_dict[key] = value
    response = make_response(jsonify(response_dict))
    if auth:
        response.headers["Authorization"] = auth
    response.headers["Access-Control-Allow-Origin"] = "*"
    response.headers["Access-Control-Allow-Method"] = "*"
    response.headers["Access-Control-Allow-Headers"] = "*"
    response.headers["Access-Control-Allow-Headers"] = "*"
    response.headers["Access-Control-Expose-Headers"] = "Authorization"
    return response

```

## Detailed Analysis

### File Role in Repository

The file `common/connection_utils.py` is located in the `common` directory.

### Architecture Context

Files in this location typically handle concerns related to common.

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
- [config_utils.py](config_utils.py_docs.md)
- [constants.py](constants.py_docs.md)
- [decorator.py](decorator.py_docs.md)
- [exceptions.py](exceptions.py_docs.md)
- [file_utils.py](file_utils.py_docs.md)
- [float_utils.py](float_utils.py_docs.md)
- [log_utils.py](log_utils.py_docs.md)
- [misc_utils.py](misc_utils.py_docs.md)
- [settings.py](settings.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
