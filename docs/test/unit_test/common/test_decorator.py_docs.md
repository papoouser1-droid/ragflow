# Documentation: test/unit_test/common/test_decorator.py

## File Metadata

- **Path**: `test/unit_test/common/test_decorator.py`
- **Size**: 2255 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/unit_test/common/test_decorator.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `common.decorator`

### Classes Defined

This file defines 3 class(es):

#### Class: `TestClass` (line 22)

**Methods**: __init__, increment

#### Class: `TestSingleton` (line 32)

**Methods**: test_state_persistence, test_multiple_calls_consistency, test_instance_methods_work

#### Class: `PlainClass` (line 69)

### Functions Defined

This file defines 6 function(s):

#### Function: `test_singleton_decorator_returns_callable` (line 66)

**Parameters**: None

**Docstring**: Test that the decorator returns a callable...

#### Function: `__init__` (line 23)

**Parameters**: self

#### Function: `increment` (line 26)

**Parameters**: self

#### Function: `test_state_persistence` (line 34)

**Parameters**: self

**Docstring**: Test that instance state persists across multiple calls...

#### Function: `test_multiple_calls_consistency` (line 43)

**Parameters**: self

**Docstring**: Test consistency across multiple calls...

#### Function: `test_instance_methods_work` (line 52)

**Parameters**: self

**Docstring**: Test that instance methods work correctly...

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

from common.decorator import singleton


# Test class for demonstration
@singleton
class TestClass:
    def __init__(self):
        self.counter = 0

    def increment(self):
        self.counter += 1
        return self.counter


# Test cases
class TestSingleton:

    def test_state_persistence(self):
        """Test that instance state persists across multiple calls"""
        instance1 = TestClass()
        instance1.increment()
        instance1.increment()

        instance2 = TestClass()
        assert instance2.counter == 2  # State should persist

    def test_multiple_calls_consistency(self):
        """Test consistency across multiple calls"""
        instances = [TestClass() for _ in range(5)]

        # All references should point to the same object
        first_instance = instances[0]
        for instance in instances:
            assert instance is first_instance

    def test_instance_methods_work(self):
        """Test that instance methods work correctly"""
        instance = TestClass()

        # Test method calls
        result1 = instance.increment()
        result2 = instance.increment()

        assert result1 == 3
        assert result2 == 4
        assert instance.counter == 4


# Test decorator itself
def test_singleton_decorator_returns_callable():
    """Test that the decorator returns a callable"""

    class PlainClass:
        pass

    decorated_class = singleton(PlainClass)

    # Should return a function
    assert callable(decorated_class)

    # Calling should return an instance of PlainClass
    instance = decorated_class()
    assert isinstance(instance, PlainClass)

```

## Detailed Analysis

### File Role in Repository

The file `test/unit_test/common/test_decorator.py` is located in the `test/unit_test/common` directory.

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

- [test_file_utils.py](test_file_utils.py_docs.md)
- [test_float_utils.py](test_float_utils.py_docs.md)
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
