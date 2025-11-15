# File Documentation: common/log_utils.py

## File Metadata

- **Path**: `common/log_utils.py`
- **Extension**: `.py`
- **Lines**: 83
- **Characters**: 2,851
- **Size**: 2,851 bytes
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

import os
import os.path
import logging
from logging.handlers import RotatingFileHandler
from common.file_utils import get_project_base_directory

initialized_root_logger = False

def init_root_logger(logfile_basename: str, log_format: str = "%(asctime)-15s %(levelname)-8s %(process)d %(message)s"):
    global initialized_root_logger
    if initialized_root_logger:
        return
    initialized_root_logger = True

    logger = logging.getLogger()
    logger.handlers.clear()
    log_path = os.path.abspath(os.path.join(get_project_base_directory(), "logs", f"{logfile_basename}.log"))

    os.makedirs(os.path.dirname(log_path), exist_ok=True)
    formatter = logging.Formatter(log_format)

    handler1 = RotatingFileHandler(log_path, maxBytes=10*1024*1024, backupCount=5)
    handler1.setFormatter(formatter)
    logger.addHandler(handler1)

    handler2 = logging.StreamHandler()
    handler2.setFormatter(formatter)
    logger.addHandler(handler2)

    logging.captureWarnings(True)

    LOG_LEVELS = os.environ.get("LOG_LEVELS", "")
    pkg_levels = {}
    for pkg_name_level in LOG_LEVELS.split(","):
        terms = pkg_name_level.split("=")
        if len(terms)!= 2:
            continue
        pkg_name, pkg_level = terms[0], terms[1]
        pkg_name = pkg_name.strip()
        pkg_level = logging.getLevelName(pkg_level.strip().upper())
        if not isinstance(pkg_level, int):
            pkg_level = logging.INFO
        pkg_levels[pkg_name] = logging.getLevelName(pkg_level)

    for pkg_name in ['peewee', 'pdfminer']:
        if pkg_name not in pkg_levels:
            pkg_levels[pkg_name] = logging.getLevelName(logging.WARNING)
    if 'root' not in pkg_levels:
        pkg_levels['root'] = logging.getLevelName(logging.INFO)

    for pkg_name, pkg_level in pkg_levels.items():
        pkg_logger = logging.getLogger(pkg_name)
        pkg_logger.setLevel(pkg_level)

    msg = f"{logfile_basename} log path: {log_path}, log levels: {pkg_levels}"
    logger.info(msg)


def log_exception(e, *args):
    logging.exception(e)
    for a in args:
        if hasattr(a, "text"):
            logging.error(a.text)
            raise Exception(a.text)
        else:
            logging.error(str(a))
    raise e
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


### Functions (2)

- `init_root_logger()`: Function definition
- `log_exception()`: Function definition

### Imports (5)

- `import os`
- `import os.path`
- `import logging`
- `from logging.handlers import RotatingFileHandler`
- `from common.file_utils import get_project_base_directory`

## Code Structure Analysis

- Total lines: 83
- Blank lines: 14 (16.9%)
- Comment lines: ~15 (18.1%)
- Code lines: ~54


## Dependencies and Imports

- `import os`
- `import os.path`
- `import logging`
- `from logging.handlers import RotatingFileHandler`
- `from common.file_utils import get_project_base_directory`

## Design & Architecture

This file is located in the `common` directory, specifically within `common`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 5 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `common/` directory
- Imports from `logging.handlers`
- Imports from `common.file_utils`
- Potential test file: `test_log_utils.py`

## Keywords

ANY, All, Apache, Authors, BASIS, CONDITIONS, Copyright, Exception, False, Formatter, INFO, InfiniFlow, KIND, LICENSE, LOG_LEVELS, License, Licensed, Python, Reserved, Rights, RotatingFileHandler, See, StreamHandler, The, True, Unless, Version, WARNING, WARRANTIES, WITHOUT, You, init_root_logger, log_exception

---
*Generated by RAGFlow Repository Documentation Generator*
