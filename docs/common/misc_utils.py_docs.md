# Documentation: common/misc_utils.py

## File Metadata

- **Path**: `common/misc_utils.py`
- **Size**: 2988 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `common/misc_utils.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `base64`
- `hashlib`
- `uuid`
- `requests`
- `threading`
- `subprocess`
- `sys`
- `os`
- `logging`

### Functions Defined

This file defines 7 function(s):

#### Function: `get_uuid` (line 27)

**Parameters**: None

#### Function: `download_img` (line 31)

**Parameters**: url

#### Function: `hash_str2int` (line 40)

**Parameters**: line, mod

#### Function: `convert_bytes` (line 43)

**Parameters**: size_in_bytes

**Docstring**: Format size in bytes....

#### Function: `once` (line 66)

**Parameters**: func

**Docstring**: A thread-safe decorator that ensures the decorated function runs exactly once,
caching and returning its result for all subsequent calls. This prevents
race conditions in multi-thread environments by ...

#### Function: `pip_install_torch` (line 102)

**Parameters**: None

#### Function: `wrapper` (line 92)

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

import base64
import hashlib
import uuid
import requests
import threading
import subprocess
import sys
import os
import logging

def get_uuid():
    return uuid.uuid1().hex


def download_img(url):
    if not url:
        return ""
    response = requests.get(url)
    return "data:" + \
        response.headers.get('Content-Type', 'image/jpg') + ";" + \
        "base64," + base64.b64encode(response.content).decode("utf-8")


def hash_str2int(line: str, mod: int = 10 ** 8) -> int:
    return int(hashlib.sha1(line.encode("utf-8")).hexdigest(), 16) % mod

def convert_bytes(size_in_bytes: int) -> str:
    """
    Format size in bytes.
    """
    if size_in_bytes == 0:
        return "0 B"

    units = ['B', 'KB', 'MB', 'GB', 'TB', 'PB']
    i = 0
    size = float(size_in_bytes)

    while size >= 1024 and i < len(units) - 1:
        size /= 1024
        i += 1

    if i == 0 or size >= 100:
        return f"{size:.0f} {units[i]}"
    elif size >= 10:
        return f"{size:.1f} {units[i]}"
    else:
        return f"{size:.2f} {units[i]}"


def once(func):
    """
    A thread-safe decorator that ensures the decorated function runs exactly once,
    caching and returning its result for all subsequent calls. This prevents
    race conditions in multi-thread environments by using a lock to protect
    the execution state.

    Args:
        func (callable): The function to be executed only once.

    Returns:
        callable: A wrapper function that executes `func` on the first call
                  and returns the cached result thereafter.

    Example:
        @once
        def compute_expensive_value():
            print("Computing...")
            return 42

        # First call: executes and prints
        # Subsequent calls: return 42 without executing
    """
    executed = False
    result = None
    lock = threading.Lock()
    def wrapper(*args, **kwargs):
        nonlocal executed, result
        with lock:
            if not executed:
                executed = True
                result = func(*args, **kwargs)
        return result
    return wrapper

@once
def pip_install_torch():
    device = os.getenv("DEVICE", "cpu")
    if device=="cpu":
        return
    logging.info("Installing pytorch")
    pkg_names = ["torch>=2.5.0,<3.0.0"]
    subprocess.check_call([sys.executable, "-m", "pip", "install", *pkg_names])

```

## Detailed Analysis

### File Role in Repository

The file `common/misc_utils.py` is located in the `common` directory.

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
- [connection_utils.py](connection_utils.py_docs.md)
- [constants.py](constants.py_docs.md)
- [decorator.py](decorator.py_docs.md)
- [exceptions.py](exceptions.py_docs.md)
- [file_utils.py](file_utils.py_docs.md)
- [float_utils.py](float_utils.py_docs.md)
- [log_utils.py](log_utils.py_docs.md)
- [settings.py](settings.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
