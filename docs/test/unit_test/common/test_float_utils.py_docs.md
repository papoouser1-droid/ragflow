# Documentation: test/unit_test/common/test_float_utils.py

## File Metadata

- **Path**: `test/unit_test/common/test_float_utils.py`
- **Size**: 2980 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/unit_test/common/test_float_utils.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `math`
- `common.float_utils`

### Classes Defined

This file defines 1 class(es):

#### Class: `TestGetFloat` (line 20)

**Methods**: test_valid_float_string, test_valid_integer_string, test_valid_numbers, test_none_input, test_invalid_strings, test_boolean_input, test_special_float_strings, test_very_large_numbers, test_whitespace_strings

### Functions Defined

This file defines 9 function(s):

#### Function: `test_valid_float_string` (line 22)

**Parameters**: self

**Docstring**: Test conversion of valid float strings...

#### Function: `test_valid_integer_string` (line 29)

**Parameters**: self

**Docstring**: Test conversion of valid integer strings...

#### Function: `test_valid_numbers` (line 35)

**Parameters**: self

**Docstring**: Test conversion of actual number types...

#### Function: `test_none_input` (line 42)

**Parameters**: self

**Docstring**: Test handling of None input...

#### Function: `test_invalid_strings` (line 48)

**Parameters**: self

**Docstring**: Test handling of invalid string inputs...

#### Function: `test_boolean_input` (line 62)

**Parameters**: self

**Docstring**: Test conversion of boolean values...

#### Function: `test_special_float_strings` (line 67)

**Parameters**: self

**Docstring**: Test handling of special float strings...

#### Function: `test_very_large_numbers` (line 76)

**Parameters**: self

**Docstring**: Test very large number strings...

#### Function: `test_whitespace_strings` (line 83)

**Parameters**: self

**Docstring**: Test strings with whitespace...

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

## Detailed Analysis

### File Role in Repository

The file `test/unit_test/common/test_float_utils.py` is located in the `test/unit_test/common` directory.

This file is part of the **Testing** infrastructure.

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

- [test_decorator.py](test_decorator.py_docs.md)
- [test_file_utils.py](test_file_utils.py_docs.md)
- [test_misc_utils.py](test_misc_utils.py_docs.md)
- [test_string_utils.py](test_string_utils.py_docs.md)
- [test_time_utils.py](test_time_utils.py_docs.md)
- [test_token_utils.py](test_token_utils.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
