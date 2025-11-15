# Documentation: test/unit_test/common/test_misc_utils.py

## File Metadata

- **Path**: `test/unit_test/common/test_misc_utils.py`
- **Size**: 12484 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/unit_test/common/test_misc_utils.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `uuid`
- `hashlib`
- `common.misc_utils`

### Classes Defined

This file defines 4 class(es):

#### Class: `TestGetUuid` (line 21)

**Docstring**: Test cases for get_uuid function...

**Methods**: test_returns_string, test_hex_format, test_no_dashes_in_result, test_unique_results, test_valid_uuid_structure, test_uuid1_specific_characteristics, test_result_length_consistency, test_hex_characters_only

#### Class: `TestDownloadImg` (line 91)

**Docstring**: Test cases for download_img function...

**Methods**: test_empty_url_returns_empty_string, test_none_url_returns_empty_string

#### Class: `TestHashStr2Int` (line 105)

**Docstring**: Test cases for hash_str2int function...

**Methods**: test_basic_hashing, test_default_mod_value, test_custom_mod_value, test_same_input_same_output, test_different_input_different_output, test_empty_string, test_unicode_string, test_special_characters, test_large_string, test_mod_value_1, test_mod_value_2, test_very_large_mod, test_hash_algorithm_sha1, test_utf8_encoding, test_range_with_different_mods, test_hexdigest_conversion, test_consistent_with_direct_calculation, test_numeric_strings, test_whitespace_strings

#### Class: `TestConvertBytes` (line 275)

**Docstring**: Test suite for convert_bytes function...

**Methods**: test_zero_bytes, test_single_byte, test_kilobyte_range, test_megabyte_range, test_gigabyte_range, test_terabyte_range, test_petabyte_range, test_boundary_values, test_precision_transitions, test_large_values_no_overflow

### Functions Defined

This file defines 39 function(s):

#### Function: `test_returns_string` (line 24)

**Parameters**: self

**Docstring**: Test that function returns a string...

#### Function: `test_hex_format` (line 29)

**Parameters**: self

**Docstring**: Test that returned string is in hex format...

#### Function: `test_no_dashes_in_result` (line 37)

**Parameters**: self

**Docstring**: Test that result contains no dashes...

#### Function: `test_unique_results` (line 42)

**Parameters**: self

**Docstring**: Test that multiple calls return different UUIDs...

#### Function: `test_valid_uuid_structure` (line 54)

**Parameters**: self

**Docstring**: Test that the hex string can be converted back to UUID...

#### Function: `test_uuid1_specific_characteristics` (line 65)

**Parameters**: self

**Docstring**: Test that UUID v1 characteristics are present...

#### Function: `test_result_length_consistency` (line 76)

**Parameters**: self

**Docstring**: Test that all generated UUIDs have consistent length...

#### Function: `test_hex_characters_only` (line 82)

**Parameters**: self

**Docstring**: Test that only valid hex characters are used...

#### Function: `test_empty_url_returns_empty_string` (line 94)

**Parameters**: self

**Docstring**: Test that empty URL returns empty string...

#### Function: `test_none_url_returns_empty_string` (line 99)

**Parameters**: self

**Docstring**: Test that None URL returns empty string...

#### Function: `test_basic_hashing` (line 108)

**Parameters**: self

**Docstring**: Test basic string hashing functionality...

#### Function: `test_default_mod_value` (line 114)

**Parameters**: self

**Docstring**: Test that default mod value is 10^8...

#### Function: `test_custom_mod_value` (line 119)

**Parameters**: self

**Docstring**: Test with custom mod value...

#### Function: `test_same_input_same_output` (line 125)

**Parameters**: self

**Docstring**: Test that same input produces same output...

#### Function: `test_different_input_different_output` (line 133)

**Parameters**: self

**Docstring**: Test that different inputs produce different outputs (usually)...

#### Function: `test_empty_string` (line 143)

**Parameters**: self

**Docstring**: Test hashing empty string...

#### Function: `test_unicode_string` (line 149)

**Parameters**: self

**Docstring**: Test hashing unicode strings...

#### Function: `test_special_characters` (line 164)

**Parameters**: self

**Docstring**: Test hashing strings with special characters...

#### Function: `test_large_string` (line 179)

**Parameters**: self

**Docstring**: Test hashing large string...

#### Function: `test_mod_value_1` (line 186)

**Parameters**: self

**Docstring**: Test with mod value 1 (should always return 0)...

#### Function: `test_mod_value_2` (line 191)

**Parameters**: self

**Docstring**: Test with mod value 2 (should return 0 or 1)...

#### Function: `test_very_large_mod` (line 196)

**Parameters**: self

**Docstring**: Test with very large mod value...

#### Function: `test_hash_algorithm_sha1` (line 202)

**Parameters**: self

**Docstring**: Test that SHA1 algorithm is used...

#### Function: `test_utf8_encoding` (line 211)

**Parameters**: self

**Docstring**: Test that UTF-8 encoding is used...

#### Function: `test_range_with_different_mods` (line 217)

**Parameters**: self

**Docstring**: Test that result is always in correct range for different mod values...

#### Function: `test_hexdigest_conversion` (line 230)

**Parameters**: self

**Docstring**: Test the hexdigest to integer conversion...

#### Function: `test_consistent_with_direct_calculation` (line 240)

**Parameters**: self

**Docstring**: Test that function matches direct hashlib usage...

#### Function: `test_numeric_strings` (line 249)

**Parameters**: self

**Docstring**: Test hashing numeric strings...

#### Function: `test_whitespace_strings` (line 258)

**Parameters**: self

**Docstring**: Test hashing strings with various whitespace...

#### Function: `test_zero_bytes` (line 278)

**Parameters**: self

**Docstring**: Test that 0 bytes returns '0 B'...

#### Function: `test_single_byte` (line 282)

**Parameters**: self

**Docstring**: Test single byte values...

#### Function: `test_kilobyte_range` (line 287)

**Parameters**: self

**Docstring**: Test values in kilobyte range with different precisions...

#### Function: `test_megabyte_range` (line 301)

**Parameters**: self

**Docstring**: Test values in megabyte range...

#### Function: `test_gigabyte_range` (line 310)

**Parameters**: self

**Docstring**: Test values in gigabyte range...

#### Function: `test_terabyte_range` (line 318)

**Parameters**: self

**Docstring**: Test values in terabyte range...

#### Function: `test_petabyte_range` (line 322)

**Parameters**: self

**Docstring**: Test values in petabyte range...

#### Function: `test_boundary_values` (line 326)

**Parameters**: self

**Docstring**: Test values at unit boundaries...

#### Function: `test_precision_transitions` (line 338)

**Parameters**: self

**Docstring**: Test the precision formatting transitions...

#### Function: `test_large_values_no_overflow` (line 347)

**Parameters**: self

**Docstring**: Test that very large values don't cause issues...

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
import uuid
import hashlib
from common.misc_utils import get_uuid, download_img, hash_str2int, convert_bytes


class TestGetUuid:
    """Test cases for get_uuid function"""

    def test_returns_string(self):
        """Test that function returns a string"""
        result = get_uuid()
        assert isinstance(result, str)

    def test_hex_format(self):
        """Test that returned string is in hex format"""
        result = get_uuid()
        # UUID v1 hex should be 32 characters (without dashes)
        assert len(result) == 32
        # Should only contain hexadecimal characters
        assert all(c in '0123456789abcdef' for c in result)

    def test_no_dashes_in_result(self):
        """Test that result contains no dashes"""
        result = get_uuid()
        assert '-' not in result

    def test_unique_results(self):
        """Test that multiple calls return different UUIDs"""
        results = [get_uuid() for _ in range(10)]

        # All results should be unique
        assert len(results) == len(set(results))

        # All should be valid hex strings of correct length
        for result in results:
            assert len(result) == 32
            assert all(c in '0123456789abcdef' for c in result)

    def test_valid_uuid_structure(self):
        """Test that the hex string can be converted back to UUID"""
        result = get_uuid()

        # Should be able to create UUID from the hex string
        reconstructed_uuid = uuid.UUID(hex=result)
        assert isinstance(reconstructed_uuid, uuid.UUID)

        # The hex representation should match the original
        assert reconstructed_uuid.hex == result

    def test_uuid1_specific_characteristics(self):
        """Test that UUID v1 characteristics are present"""
        result = get_uuid()
        uuid_obj = uuid.UUID(hex=result)

        # UUID v1 should have version 1
        assert uuid_obj.version == 1

        # Variant should be RFC 4122
        assert uuid_obj.variant == 'specified in RFC 4122'

    def test_result_length_consistency(self):
        """Test that all generated UUIDs have consistent length"""
        for _ in range(100):
            result = get_uuid()
            assert len(result) == 32

    def test_hex_characters_only(self):
        """Test that only valid hex characters are used"""
        for _ in range(100):
            result = get_uuid()
            # Should only contain lowercase hex characters (UUID hex is lowercase)
            assert result.islower()
            assert all(c in '0123456789abcdef' for c in result)


class TestDownloadImg:
    """Test cases for download_img function"""

    def test_empty_url_returns_empty_string(self):
        """Test that empty URL returns empty string"""
        result = download_img("")
        assert result == ""

    def test_none_url_returns_empty_string(self):
        """Test that None URL returns empty string"""
        result = download_img(None)
        assert result == ""


class TestHashStr2Int:
    """Test cases for hash_str2int function"""

    def test_basic_hashing(self):
        """Test basic string hashing functionality"""
        result = hash_str2int("hello")
        assert isinstance(result, int)
        assert 0 <= result < 10 ** 8

    def test_default_mod_value(self):
        """Test that default mod value is 10^8"""
        result = hash_str2int("test")
        assert 0 <= result < 10 ** 8

    def test_custom_mod_value(self):
        """Test with custom mod value"""
        result = hash_str2int("test", mod=1000)
        assert isinstance(result, int)
        assert 0 <= result < 1000

    def test_same_input_same_output(self):
        """Test that same input produces same output"""
        result1 = hash_str2int("consistent")
        result2 = hash_str2int("consistent")
        result3 = hash_str2int("consistent")

        assert result1 == result2 == result3

    def test_different_input_different_output(self):
        """Test that different inputs produce different outputs (usually)"""
        result1 = hash_str2int("hello")
        result2 = hash_str2int("world")
        result3 = hash_str2int("hello world")

        # While hash collisions are possible, they're very unlikely for these inputs
        results = [result1, result2, result3]
        assert len(set(results)) == len(results)

    def test_empty_string(self):
        """Test hashing empty string"""
        result = hash_str2int("")
        assert isinstance(result, int)
        assert 0 <= result < 10 ** 8

    def test_unicode_string(self):
        """Test hashing unicode strings"""
        test_strings = [
            "中文",
            "🚀火箭",
            "café",
            "🎉",
            "Hello 世界"
        ]

        for test_str in test_strings:
            result = hash_str2int(test_str)
            assert isinstance(result, int)
            assert 0 <= result < 10 ** 8

    def test_special_characters(self):
        """Test hashing strings with special characters"""
        test_strings = [
            "hello@world.com",
            "test#123",
            "line\nwith\nnewlines",
            "tab\tcharacter",
            "space in string"
        ]

        for test_str in test_strings:
            result = hash_str2int(test_str)
            assert isinstance(result, int)
            assert 0 <= result < 10 ** 8

    def test_large_string(self):
        """Test hashing large string"""
        large_string = "x" * 10000
        result = hash_str2int(large_string)
        assert isinstance(result, int)
        assert 0 <= result < 10 ** 8

    def test_mod_value_1(self):
        """Test with mod value 1 (should always return 0)"""
        result = hash_str2int("any string", mod=1)
        assert result == 0

    def test_mod_value_2(self):
        """Test with mod value 2 (should return 0 or 1)"""
        result = hash_str2int("test", mod=2)
        assert result in [0, 1]

    def test_very_large_mod(self):
        """Test with very large mod value"""
        result = hash_str2int("test", mod=10 ** 12)
        assert isinstance(result, int)
        assert 0 <= result < 10 ** 12

    def test_hash_algorithm_sha1(self):
        """Test that SHA1 algorithm is used"""
        test_string = "hello"
        expected_hash = hashlib.sha1(test_string.encode("utf-8")).hexdigest()
        expected_int = int(expected_hash, 16) % (10 ** 8)

        result = hash_str2int(test_string)
        assert result == expected_int

    def test_utf8_encoding(self):
        """Test that UTF-8 encoding is used"""
        # This should work without encoding errors
        result = hash_str2int("café 🎉")
        assert isinstance(result, int)

    def test_range_with_different_mods(self):
        """Test that result is always in correct range for different mod values"""
        test_cases = [
            ("test1", 100),
            ("test2", 1000),
            ("test3", 10000),
            ("test4", 999999),
        ]

        for test_str, mod_val in test_cases:
            result = hash_str2int(test_str, mod=mod_val)
            assert 0 <= result < mod_val

    def test_hexdigest_conversion(self):
        """Test the hexdigest to integer conversion"""
        test_string = "hello"
        hash_obj = hashlib.sha1(test_string.encode("utf-8"))
        hex_digest = hash_obj.hexdigest()
        expected_int = int(hex_digest, 16) % (10 ** 8)

        result = hash_str2int(test_string)
        assert result == expected_int

    def test_consistent_with_direct_calculation(self):
        """Test that function matches direct hashlib usage"""
        test_strings = ["a", "b", "abc", "hello world", "12345"]

        for test_str in test_strings:
            direct_result = int(hashlib.sha1(test_str.encode("utf-8")).hexdigest(), 16) % (10 ** 8)
            function_result = hash_str2int(test_str)
            assert function_result == direct_result

    def test_numeric_strings(self):
        """Test hashing numeric strings"""
        test_strings = ["123", "0", "999999", "3.14159", "-42"]

        for test_str in test_strings:
            result = hash_str2int(test_str)
            assert isinstance(result, int)
            assert 0 <= result < 10 ** 8

    def test_whitespace_strings(self):
        """Test hashing strings with various whitespace"""
        test_strings = [
            "  leading",
            "trailing  ",
            "  both  ",
            "\ttab",
            "new\nline",
            "\r\nwindows"
        ]

        for test_str in test_strings:
            result = hash_str2int(test_str)
            assert isinstance(result, int)
            assert 0 <= result < 10 ** 8


class TestConvertBytes:
    """Test suite for convert_bytes function"""

    def test_zero_bytes(self):
        """Test that 0 bytes returns '0 B'"""
        assert convert_bytes(0) == "0 B"

    def test_single_byte(self):
        """Test single byte values"""
        assert convert_bytes(1) == "1 B"
        assert convert_bytes(999) == "999 B"

    def test_kilobyte_range(self):
        """Test values in kilobyte range with different precisions"""
        # Exactly 1 KB
        assert convert_bytes(1024) == "1.00 KB"

        # Values that should show 1 decimal place (10-99.9 range)
        assert convert_bytes(15360) == "15.0 KB"  # 15 KB exactly
        assert convert_bytes(10752) == "10.5 KB"  # 10.5 KB

        # Values that should show 2 decimal places (1-9.99 range)
        assert convert_bytes(2048) == "2.00 KB"  # 2 KB exactly
        assert convert_bytes(3072) == "3.00 KB"  # 3 KB exactly
        assert convert_bytes(5120) == "5.00 KB"  # 5 KB exactly

    def test_megabyte_range(self):
        """Test values in megabyte range"""
        # Exactly 1 MB
        assert convert_bytes(1048576) == "1.00 MB"

        # Values with different precision requirements
        assert convert_bytes(15728640) == "15.0 MB"  # 15.0 MB
        assert convert_bytes(11010048) == "10.5 MB"  # 10.5 MB

    def test_gigabyte_range(self):
        """Test values in gigabyte range"""
        # Exactly 1 GB
        assert convert_bytes(1073741824) == "1.00 GB"

        # Large value that should show 0 decimal places
        assert convert_bytes(3221225472) == "3.00 GB"  # 3 GB exactly

    def test_terabyte_range(self):
        """Test values in terabyte range"""
        assert convert_bytes(1099511627776) == "1.00 TB"  # 1 TB

    def test_petabyte_range(self):
        """Test values in petabyte range"""
        assert convert_bytes(1125899906842624) == "1.00 PB"  # 1 PB

    def test_boundary_values(self):
        """Test values at unit boundaries"""
        # Just below 1 KB
        assert convert_bytes(1023) == "1023 B"

        # Just above 1 KB
        assert convert_bytes(1025) == "1.00 KB"

        # At 100 KB boundary (should switch to 0 decimal places)
        assert convert_bytes(102400) == "100 KB"
        assert convert_bytes(102300) == "99.9 KB"

    def test_precision_transitions(self):
        """Test the precision formatting transitions"""
        # Test transition from 2 decimal places to 1 decimal place
        assert convert_bytes(9216) == "9.00 KB"  # 9.00 KB (2 decimal places)
        assert convert_bytes(10240) == "10.0 KB"  # 10.0 KB (1 decimal place)

        # Test transition from 1 decimal place to 0 decimal places
        assert convert_bytes(102400) == "100 KB"  # 100 KB (0 decimal places)

    def test_large_values_no_overflow(self):
        """Test that very large values don't cause issues"""
        # Very large value that should use PB
        large_value = 10 * 1125899906842624  # 10 PB
        assert "PB" in convert_bytes(large_value)

        # Ensure we don't exceed available units
        huge_value = 100 * 1125899906842624  # 100 PB (still within PB range)
        assert "PB" in convert_bytes(huge_value)

```

## Detailed Analysis

### File Role in Repository

The file `test/unit_test/common/test_misc_utils.py` is located in the `test/unit_test/common` directory.

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
- [test_float_utils.py](test_float_utils.py_docs.md)
- [test_string_utils.py](test_string_utils.py_docs.md)
- [test_time_utils.py](test_time_utils.py_docs.md)
- [test_token_utils.py](test_token_utils.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
