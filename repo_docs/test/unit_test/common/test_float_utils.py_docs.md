# File Documentation: test/unit_test/common/test_float_utils.py

## File Metadata

- **Path**: `test/unit_test/common/test_float_utils.py`
- **Extension**: `.py`
- **Lines**: 88
- **Characters**: 2,980
- **Size**: 2,980 bytes
- **Purpose**: Testing - Contains unit tests, integration tests, or test utilities

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

import math
from common.float_utils import get_float

class TestGetFloat:

    def test_valid_float_string(self):
        """Test conversion of valid float strings"""
        assert get_float("3.14") == 3.14
        assert get_float("-2.5") == -2.5
        assert get_float("0.0") == 0.0
        assert get_float("123.456") == 123.456

    def test_valid_integer_string(self):
        """Test conversion of valid integer strings"""
        assert get_float("42") == 42.0
        assert get_float("-100") == -100.0
        assert get_float("0") == 0.0

    def test_valid_numbers(self):
        """Test conversion of actual number types"""
        assert get_float(3.14) == 3.14
        assert get_float(-2.5) == -2.5
        assert get_float(42) == 42.0
        assert get_float(0) == 0.0

    def test_none_input(self):
        """Test handling of None input"""
        result = get_float(None)
        assert math.isinf(result)
        assert result < 0  # Should be negative infinity

    def test_invalid_strings(self):
        """Test handling of invalid string inputs"""
        result = get_float("invalid")
        assert math.isinf(result)
        assert result < 0

        result = get_float("12.34.56")
        assert math.isinf(result)
        assert result < 0

        result = get_float("")
        assert math.isinf(result)
        assert result < 0

    def test_boolean_input(self):
        """Test conversion of boolean values"""
        assert get_float(True) == 1.0
        assert get_float(False) == 0.0

    def test_special_float_strings(self):
        """Test handling of special float strings"""
        assert get_float("inf") == float('inf')
        assert get_float("-inf") == float('-inf')

        # NaN should return -inf according to our function's design
        result = get_float("nan")
        assert math.isnan(result)

    def test_very_large_numbers(self):
        """Test very large number strings"""
        assert get_float("1e308") == 1e308
        # This will become inf in Python, but let's test it
        large_result = get_float("1e500")
        assert math.isinf(large_result)

    def test_whitespace_strings(self):
        """Test strings with whitespace"""
        assert get_float("  3.14  ") == 3.14
        result = get_float("  invalid  ")
        assert math.isinf(result)
        assert result < 0
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

### Classes (1)

- `TestGetFloat`: Class definition

### Imports (2)

- `import math`
- `from common.float_utils import get_float`

## Code Structure Analysis

- Total lines: 88
- Blank lines: 14 (15.9%)
- Comment lines: ~26 (29.5%)
- Code lines: ~48


## Dependencies and Imports

- `import math`
- `from common.float_utils import get_float`

## Design & Architecture

This file is located in the `test` directory, specifically within `test/unit_test/common`.

This is a test file, contributing to the quality assurance and validation of the codebase.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `test/unit_test/common/` directory
- Imports from `common.float_utils`

## Keywords

ANY, All, Apache, Authors, BASIS, CONDITIONS, Copyright, False, InfiniFlow, KIND, LICENSE, License, Licensed, NaN, None, Python, Reserved, Rights, See, Should, Test, TestGetFloat, The, This, True, Unless, Version, WARRANTIES, WITHOUT, You, test_boolean_input, test_invalid_strings, test_none_input, test_special_float_strings, test_valid_float_string, test_valid_integer_string, test_valid_numbers, test_very_large_numbers, test_whitespace_strings

---
*Generated by RAGFlow Repository Documentation Generator*
