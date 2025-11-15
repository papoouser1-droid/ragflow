# File Documentation: common/misc_utils.py

## File Metadata

- **Path**: `common/misc_utils.py`
- **Extension**: `.py`
- **Lines**: 109
- **Characters**: 2,988
- **Size**: 2,988 bytes
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


### Functions (6)

- `get_uuid()`: Function definition
- `download_img()`: Function definition
- `hash_str2int()`: Function definition
- `convert_bytes()`: Function definition
- `once()`: Function definition
- `pip_install_torch()`: Function definition

### Imports (9)

- `import base64`
- `import hashlib`
- `import uuid`
- `import requests`
- `import threading`
- `import subprocess`
- `import sys`
- `import os`
- `import logging`

## Code Structure Analysis

- Total lines: 109
- Blank lines: 18 (16.5%)
- Comment lines: ~21 (19.3%)
- Code lines: ~70


## Dependencies and Imports

- `import base64`
- `import hashlib`
- `import uuid`
- `import requests`
- `import threading`
- `import subprocess`
- `import sys`
- `import os`
- `import logging`

## Design & Architecture

This file is located in the `common` directory, specifically within `common`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 3 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `common/` directory
- Potential test file: `test_misc_utils.py`

## Keywords

ANY, All, Apache, Args, Authors, BASIS, CONDITIONS, Computing, Content, Copyright, DEVICE, Example, False, First, Format, InfiniFlow, Installing, KIND, LICENSE, License, Licensed, Lock, None, Python, Reserved, Returns, Rights, See, Subsequent, The, This, True, Type, Unless, Version, WARRANTIES, WITHOUT, You, compute_expensive_value, convert_bytes, download_img, get_uuid, hash_str2int, once, pip_install_torch, runs, that, to, wrapper

---
*Generated by RAGFlow Repository Documentation Generator*
