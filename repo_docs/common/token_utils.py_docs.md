# File Documentation: common/token_utils.py

## File Metadata

- **Path**: `common/token_utils.py`
- **Extension**: `.py`
- **Lines**: 77
- **Characters**: 2,493
- **Size**: 2,493 bytes
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
import tiktoken

from common.file_utils import get_project_base_directory

tiktoken_cache_dir = get_project_base_directory()
os.environ["TIKTOKEN_CACHE_DIR"] = tiktoken_cache_dir
# encoder = tiktoken.encoding_for_model("gpt-3.5-turbo")
encoder = tiktoken.get_encoding("cl100k_base")


def num_tokens_from_string(string: str) -> int:
    """Returns the number of tokens in a text string."""
    try:
        code_list = encoder.encode(string)
        return len(code_list)
    except Exception:
        return 0

def total_token_count_from_response(resp):
    if resp is None:
        return 0

    if hasattr(resp, "usage") and hasattr(resp.usage, "total_tokens"):
        try:
            return resp.usage.total_tokens
        except Exception:
            pass

    if hasattr(resp, "usage_metadata") and hasattr(resp.usage_metadata, "total_tokens"):
        try:
            return resp.usage_metadata.total_tokens
        except Exception:
            pass

    if 'usage' in resp and 'total_tokens' in resp['usage']:
        try:
            return resp["usage"]["total_tokens"]
        except Exception:
            pass

    if 'usage' in resp and 'input_tokens' in resp['usage'] and 'output_tokens' in resp['usage']:
        try:
            return resp["usage"]["input_tokens"] + resp["usage"]["output_tokens"]
        except Exception:
            pass

    if 'meta' in resp and 'tokens' in resp['meta'] and 'input_tokens' in resp['meta']['tokens'] and 'output_tokens' in resp['meta']['tokens']:
        try:
            return resp["meta"]["tokens"]["input_tokens"] + resp["meta"]["tokens"]["output_tokens"]
        except Exception:
            pass
    return 0


def truncate(string: str, max_len: int) -> str:
    """Returns truncated text if the length of text exceed max_len."""
    return encoder.decode(encoder.encode(string)[:max_len])


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


### Functions (3)

- `num_tokens_from_string()`: Function definition
- `total_token_count_from_response()`: Function definition
- `truncate()`: Function definition

### Imports (3)

- `import os`
- `import tiktoken`
- `from common.file_utils import get_project_base_directory`

## Code Structure Analysis

- Total lines: 77
- Blank lines: 16 (20.8%)
- Comment lines: ~18 (23.4%)
- Code lines: ~43


## Dependencies and Imports

- `import os`
- `import tiktoken`
- `from common.file_utils import get_project_base_directory`

## Design & Architecture

This file is located in the `common` directory, specifically within `common`.

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

- Other files in `common/` directory
- Imports from `common.file_utils`
- Potential test file: `test_token_utils.py`

## Keywords

ANY, All, Apache, Authors, BASIS, CONDITIONS, Copyright, Exception, InfiniFlow, KIND, LICENSE, License, Licensed, None, Python, Reserved, Returns, Rights, See, TIKTOKEN_CACHE_DIR, The, Unless, Version, WARRANTIES, WITHOUT, You, num_tokens_from_string, total_token_count_from_response, truncate

---
*Generated by RAGFlow Repository Documentation Generator*
