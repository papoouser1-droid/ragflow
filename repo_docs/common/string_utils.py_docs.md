# File Documentation: common/string_utils.py

## File Metadata

- **Path**: `common/string_utils.py`
- **Extension**: `.py`
- **Lines**: 74
- **Characters**: 2,859
- **Size**: 2,863 bytes
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

import re


def remove_redundant_spaces(txt: str):
    """
    Remove redundant spaces around punctuation marks while preserving meaningful spaces.

    This function performs two main operations:
    1. Remove spaces after left-boundary characters (opening brackets, etc.)
    2. Remove spaces before right-boundary characters (closing brackets, punctuation, etc.)

    Args:
        txt (str): Input text to process

    Returns:
        str: Text with redundant spaces removed
    """
    # First pass: Remove spaces after left-boundary characters
    # Matches: [non-alphanumeric-and-specific-right-punctuation] + [non-space]
    # Removes spaces after characters like '(', '<', and other non-alphanumeric chars
    # Examples:
    #   "( test" → "(test"
    txt = re.sub(r"([^a-z0-9.,\)>]) +([^ ])", r"\1\2", txt, flags=re.IGNORECASE)

    # Second pass: Remove spaces before right-boundary characters
    # Matches: [non-space] + [non-alphanumeric-and-specific-left-punctuation]
    # Removes spaces before characters like non-')', non-',', non-'.', and non-alphanumeric chars
    # Examples:
    #   "world !" → "world!"
    return re.sub(r"([^ ]) +([^a-z0-9.,\(<])", r"\1\2", txt, flags=re.IGNORECASE)


def clean_markdown_block(text):
    """
    Remove Markdown code block syntax from the beginning and end of text.

    This function cleans Markdown code blocks by removing:
    - Opening ```Markdown tags (with optional whitespace and newlines)
    - Closing ``` tags (with optional whitespace and newlines)

    Args:
        text (str): Input text that may be wrapped in Markdown code blocks

    Returns:
        str: Cleaned text with Markdown code block syntax removed, and stripped of surrounding whitespace

    """
    # Remove opening ```markdown tag with optional whitespace and newlines
    # Matches: optional whitespace + ```markdown + optional whitespace + optional newline
    text = re.sub(r'^\s*```markdown\s*\n?', '', text)

    # Remove closing ``` tag with optional whitespace and newlines
    # Matches: optional newline + optional whitespace + ``` + optional whitespace at end
    text = re.sub(r'\n?\s*```\s*$', '', text)

    # Return text with surrounding whitespace removed
    return text.strip()

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

- `remove_redundant_spaces()`: Function definition
- `clean_markdown_block()`: Function definition

### Imports (1)

- `import re`

## Code Structure Analysis

- Total lines: 74
- Blank lines: 16 (21.6%)
- Comment lines: ~34 (45.9%)
- Code lines: ~24


## Dependencies and Imports

- `import re`

## Design & Architecture

This file is located in the `common` directory, specifically within `common`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 2 loop(s) - consider algorithmic complexity

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
- Potential test file: `test_string_utils.py`

## Keywords

ANY, All, Apache, Args, Authors, BASIS, CONDITIONS, Cleaned, Closing, Copyright, Examples, First, IGNORECASE, InfiniFlow, Input, KIND, LICENSE, License, Licensed, Markdown, Matches, Opening, Python, Remove, Removes, Reserved, Return, Returns, Rights, Second, See, Text, The, This, Unless, Version, WARRANTIES, WITHOUT, You, clean_markdown_block, cleans, performs, remove_redundant_spaces

---
*Generated by RAGFlow Repository Documentation Generator*
