# Documentation: test/unit_test/common/test_token_utils.py

## File Metadata

- **Path**: `test/unit_test/common/test_token_utils.py`
- **Size**: 13295 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/unit_test/common/test_token_utils.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `common.token_utils`
- `pytest`

### Classes Defined

This file defines 3 class(es):

#### Class: `TestNumTokensFromString` (line 21)

**Docstring**: Test cases for num_tokens_from_string function...

**Methods**: test_empty_string, test_single_word, test_multiple_words, test_special_characters, test_hanzi_characters, test_unicode_characters, test_long_text, test_whitespace_only, test_numbers, test_mixed_content, test_encoding_error_handling

#### Class: `TestTotalTokenCountFromResponse` (line 115)

**Docstring**: Test cases for total_token_count_from_response function...

**Methods**: test_dict_with_usage_total_tokens, test_dict_with_usage_input_output_tokens, test_dict_with_meta_tokens_input_output, test_priority_order_dict_usage_total_tokens_third, test_priority_order_dict_usage_input_output_fourth, test_priority_order_meta_tokens_last, test_no_token_info_returns_zero, test_partial_dict_usage_missing_output_tokens, test_partial_meta_tokens_missing_input_tokens, test_none_response, test_invalid_response_type

#### Class: `TestTruncate` (line 252)

**Docstring**: Test cases for truncate function...

**Methods**: test_empty_string, test_string_shorter_than_max_len, test_string_equal_to_max_len, test_string_longer_than_max_len, test_truncation_preserves_beginning, test_unicode_characters, test_special_characters, test_whitespace_string, test_max_len_zero, test_max_len_one, test_preserves_decoding_encoding_consistency, test_multibyte_characters_truncation, test_mixed_english_chinese_text, test_numbers_and_symbols

### Functions Defined

This file defines 38 function(s):

#### Function: `test_token_count_ranges` (line 99)

**Parameters**: input_string, expected_min_tokens

**Docstring**: Parameterized test for various input strings...

#### Function: `test_consistency` (line 105)

**Parameters**: None

**Docstring**: Test that the same input produces consistent results...

#### Function: `test_empty_string` (line 24)

**Parameters**: self

**Docstring**: Test that empty string returns zero tokens...

#### Function: `test_single_word` (line 29)

**Parameters**: self

**Docstring**: Test token count for a single word...

#### Function: `test_multiple_words` (line 35)

**Parameters**: self

**Docstring**: Test token count for multiple words...

#### Function: `test_special_characters` (line 41)

**Parameters**: self

**Docstring**: Test token count with special characters...

#### Function: `test_hanzi_characters` (line 47)

**Parameters**: self

**Docstring**: Test token count with special characters...

#### Function: `test_unicode_characters` (line 53)

**Parameters**: self

**Docstring**: Test token count with unicode characters...

#### Function: `test_long_text` (line 59)

**Parameters**: self

**Docstring**: Test token count for longer text...

#### Function: `test_whitespace_only` (line 66)

**Parameters**: self

**Docstring**: Test token count for whitespace-only strings...

#### Function: `test_numbers` (line 72)

**Parameters**: self

**Docstring**: Test token count with numerical values...

#### Function: `test_mixed_content` (line 77)

**Parameters**: self

**Docstring**: Test token count with mixed content types...

#### Function: `test_encoding_error_handling` (line 83)

**Parameters**: self

**Docstring**: Test that function handles encoding errors gracefully...

#### Function: `test_dict_with_usage_total_tokens` (line 118)

**Parameters**: self

**Docstring**: Test dictionary response with usage['total_tokens']...

#### Function: `test_dict_with_usage_input_output_tokens` (line 129)

**Parameters**: self

**Docstring**: Test dictionary response with input_tokens and output_tokens in usage...

#### Function: `test_dict_with_meta_tokens_input_output` (line 141)

**Parameters**: self

**Docstring**: Test dictionary response with meta.tokens.input_tokens and output_tokens...

#### Function: `test_priority_order_dict_usage_total_tokens_third` (line 155)

**Parameters**: self

**Docstring**: Test that dict['usage']['total_tokens'] is third in priority...

#### Function: `test_priority_order_dict_usage_input_output_fourth` (line 174)

**Parameters**: self

**Docstring**: Test that dict['usage']['input_tokens'] + output_tokens is fourth in priority...

#### Function: `test_priority_order_meta_tokens_last` (line 192)

**Parameters**: self

**Docstring**: Test that meta.tokens is the last option in priority...

#### Function: `test_no_token_info_returns_zero` (line 206)

**Parameters**: self

**Docstring**: Test that function returns 0 when no token information is found...

#### Function: `test_partial_dict_usage_missing_output_tokens` (line 212)

**Parameters**: self

**Docstring**: Test dictionary with usage but missing output_tokens...

#### Function: `test_partial_meta_tokens_missing_input_tokens` (line 224)

**Parameters**: self

**Docstring**: Test dictionary with meta.tokens but missing input_tokens...

#### Function: `test_none_response` (line 238)

**Parameters**: self

**Docstring**: Test that function handles None response gracefully...

#### Function: `test_invalid_response_type` (line 243)

**Parameters**: self

**Docstring**: Test that function handles invalid response types gracefully...

#### Function: `test_empty_string` (line 255)

**Parameters**: self

**Docstring**: Test truncation of empty string...

#### Function: `test_string_shorter_than_max_len` (line 261)

**Parameters**: self

**Docstring**: Test string that is shorter than max_len...

#### Function: `test_string_equal_to_max_len` (line 268)

**Parameters**: self

**Docstring**: Test string that exactly equals max_len in tokens...

#### Function: `test_string_longer_than_max_len` (line 279)

**Parameters**: self

**Docstring**: Test string that is longer than max_len...

#### Function: `test_truncation_preserves_beginning` (line 288)

**Parameters**: self

**Docstring**: Test that truncation preserves the beginning of the string...

#### Function: `test_unicode_characters` (line 300)

**Parameters**: self

**Docstring**: Test truncation with unicode characters...

#### Function: `test_special_characters` (line 310)

**Parameters**: self

**Docstring**: Test truncation with special characters...

#### Function: `test_whitespace_string` (line 318)

**Parameters**: self

**Docstring**: Test truncation of whitespace-only string...

#### Function: `test_max_len_zero` (line 327)

**Parameters**: self

**Docstring**: Test truncation with max_len = 0...

#### Function: `test_max_len_one` (line 334)

**Parameters**: self

**Docstring**: Test truncation with max_len = 1...

#### Function: `test_preserves_decoding_encoding_consistency` (line 340)

**Parameters**: self

**Docstring**: Test that truncation preserves encoding-decoding consistency...

#### Function: `test_multibyte_characters_truncation` (line 350)

**Parameters**: self

**Docstring**: Test truncation with multibyte characters that span multiple tokens...

#### Function: `test_mixed_english_chinese_text` (line 359)

**Parameters**: self

**Docstring**: Test truncation with mixed English and Chinese text...

#### Function: `test_numbers_and_symbols` (line 367)

**Parameters**: self

**Docstring**: Test truncation with numbers and symbols...

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

from common.token_utils import num_tokens_from_string, total_token_count_from_response, truncate, encoder
import pytest


class TestNumTokensFromString:
    """Test cases for num_tokens_from_string function"""

    def test_empty_string(self):
        """Test that empty string returns zero tokens"""
        result = num_tokens_from_string("")
        assert result == 0

    def test_single_word(self):
        """Test token count for a single word"""
        # "hello" should be 1 token with cl100k_base encoding
        result = num_tokens_from_string("hello")
        assert result == 1

    def test_multiple_words(self):
        """Test token count for multiple words"""
        # "hello world" typically becomes 2 tokens
        result = num_tokens_from_string("hello world")
        assert result == 2

    def test_special_characters(self):
        """Test token count with special characters"""
        result = num_tokens_from_string("hello, world!")
        # Special characters may be separate tokens
        assert result == 4

    def test_hanzi_characters(self):
        """Test token count with special characters"""
        result = num_tokens_from_string("世界")
        # Special characters may be separate tokens
        assert result > 0

    def test_unicode_characters(self):
        """Test token count with unicode characters"""
        result = num_tokens_from_string("Hello 世界 🌍")
        # Unicode characters typically require multiple tokens
        assert result > 0

    def test_long_text(self):
        """Test token count for longer text"""
        long_text = "This is a longer piece of text that should contain multiple sentences. " \
                    "It will help verify that the token counting works correctly for substantial input."
        result = num_tokens_from_string(long_text)
        assert result > 10

    def test_whitespace_only(self):
        """Test token count for whitespace-only strings"""
        result = num_tokens_from_string("   \n\t   ")
        # Whitespace may or may not be tokens depending on the encoding
        assert result >= 0

    def test_numbers(self):
        """Test token count with numerical values"""
        result = num_tokens_from_string("12345 678.90")
        assert result > 0

    def test_mixed_content(self):
        """Test token count with mixed content types"""
        mixed_text = "Hello! 123 Main St. Price: $19.99 🎉"
        result = num_tokens_from_string(mixed_text)
        assert result > 0

    def test_encoding_error_handling(self):
        """Test that function handles encoding errors gracefully"""
        # This test verifies the exception handling in the function.
        # The function should return 0 when encoding fails
        # Note: We can't easily simulate encoding errors without mocking
        pass


# Additional parameterized tests for efficiency
@pytest.mark.parametrize("input_string,expected_min_tokens", [
    ("a", 1),  # Single character
    ("test", 1),  # Single word
    ("hello world", 2),  # Two words
    ("This is a sentence.", 4),  # Short sentence
    # ("A" * 100, 100),  # Repeated characters
])
def test_token_count_ranges(input_string, expected_min_tokens):
    """Parameterized test for various input strings"""
    result = num_tokens_from_string(input_string)
    assert result >= expected_min_tokens


def test_consistency():
    """Test that the same input produces consistent results"""
    test_string = "Consistent token counting"
    first_result = num_tokens_from_string(test_string)
    second_result = num_tokens_from_string(test_string)

    assert first_result == second_result
    assert first_result > 0


class TestTotalTokenCountFromResponse:
    """Test cases for total_token_count_from_response function"""

    def test_dict_with_usage_total_tokens(self):
        """Test dictionary response with usage['total_tokens']"""
        resp_dict = {
            'usage': {
                'total_tokens': 175
            }
        }

        result = total_token_count_from_response(resp_dict)
        assert result == 175

    def test_dict_with_usage_input_output_tokens(self):
        """Test dictionary response with input_tokens and output_tokens in usage"""
        resp_dict = {
            'usage': {
                'input_tokens': 100,
                'output_tokens': 50
            }
        }

        result = total_token_count_from_response(resp_dict)
        assert result == 150

    def test_dict_with_meta_tokens_input_output(self):
        """Test dictionary response with meta.tokens.input_tokens and output_tokens"""
        resp_dict = {
            'meta': {
                'tokens': {
                    'input_tokens': 80,
                    'output_tokens': 40
                }
            }
        }

        result = total_token_count_from_response(resp_dict)
        assert result == 120

    def test_priority_order_dict_usage_total_tokens_third(self):
        """Test that dict['usage']['total_tokens'] is third in priority"""
        resp_dict = {
            'usage': {
                'total_tokens': 180,
                'input_tokens': 100,
                'output_tokens': 80
            },
            'meta': {
                'tokens': {
                    'input_tokens': 200,
                    'output_tokens': 100
                }
            }
        }

        result = total_token_count_from_response(resp_dict)
        assert result == 180  # Should use total_tokens from usage

    def test_priority_order_dict_usage_input_output_fourth(self):
        """Test that dict['usage']['input_tokens'] + output_tokens is fourth in priority"""
        resp_dict = {
            'usage': {
                'input_tokens': 120,
                'output_tokens': 60
            },
            'meta': {
                'tokens': {
                    'input_tokens': 200,
                    'output_tokens': 100
                }
            }
        }

        result = total_token_count_from_response(resp_dict)
        assert result == 180  # Should sum input_tokens + output_tokens from usage

    def test_priority_order_meta_tokens_last(self):
        """Test that meta.tokens is the last option in priority"""
        resp_dict = {
            'meta': {
                'tokens': {
                    'input_tokens': 90,
                    'output_tokens': 30
                }
            }
        }

        result = total_token_count_from_response(resp_dict)
        assert result == 120

    def test_no_token_info_returns_zero(self):
        """Test that function returns 0 when no token information is found"""
        empty_resp = {}
        result = total_token_count_from_response(empty_resp)
        assert result == 0

    def test_partial_dict_usage_missing_output_tokens(self):
        """Test dictionary with usage but missing output_tokens"""
        resp_dict = {
            'usage': {
                'input_tokens': 100
                # Missing output_tokens
            }
        }

        result = total_token_count_from_response(resp_dict)
        assert result == 0  # Should not match the condition and return 0

    def test_partial_meta_tokens_missing_input_tokens(self):
        """Test dictionary with meta.tokens but missing input_tokens"""
        resp_dict = {
            'meta': {
                'tokens': {
                    'output_tokens': 50
                    # Missing input_tokens
                }
            }
        }

        result = total_token_count_from_response(resp_dict)
        assert result == 0  # Should not match the condition and return 0

    def test_none_response(self):
        """Test that function handles None response gracefully"""
        result = total_token_count_from_response(None)
        assert result == 0

    def test_invalid_response_type(self):
        """Test that function handles invalid response types gracefully"""
        result = total_token_count_from_response("invalid response")
        assert result == 0

        # result = total_token_count_from_response(123)
        # assert result == 0


class TestTruncate:
    """Test cases for truncate function"""

    def test_empty_string(self):
        """Test truncation of empty string"""
        result = truncate("", 5)
        assert result == ""
        assert isinstance(result, str)

    def test_string_shorter_than_max_len(self):
        """Test string that is shorter than max_len"""
        original_string = "hello"
        result = truncate(original_string, 10)
        assert result == original_string
        assert len(encoder.encode(result)) <= 10

    def test_string_equal_to_max_len(self):
        """Test string that exactly equals max_len in tokens"""
        # Create a string that encodes to exactly 5 tokens
        test_string = "hello world test"
        encoded = encoder.encode(test_string)
        exact_length = len(encoded)

        result = truncate(test_string, exact_length)
        assert result == test_string
        assert len(encoder.encode(result)) == exact_length

    def test_string_longer_than_max_len(self):
        """Test string that is longer than max_len"""
        long_string = "This is a longer string that will be truncated"
        max_len = 5

        result = truncate(long_string, max_len)
        assert len(encoder.encode(result)) == max_len
        assert result != long_string

    def test_truncation_preserves_beginning(self):
        """Test that truncation preserves the beginning of the string"""
        test_string = "The quick brown fox jumps over the lazy dog"
        max_len = 3

        result = truncate(test_string, max_len)
        encoded_result = encoder.encode(result)

        # The truncated result should match the beginning of the original encoding
        original_encoded = encoder.encode(test_string)
        assert encoded_result == original_encoded[:max_len]

    def test_unicode_characters(self):
        """Test truncation with unicode characters"""
        unicode_string = "Hello 世界 🌍 测试"
        max_len = 4

        result = truncate(unicode_string, max_len)
        assert len(encoder.encode(result)) == max_len
        # Should be a valid string
        assert isinstance(result, str)

    def test_special_characters(self):
        """Test truncation with special characters"""
        special_string = "Hello, world! @#$%^&*()"
        max_len = 3

        result = truncate(special_string, max_len)
        assert len(encoder.encode(result)) == max_len

    def test_whitespace_string(self):
        """Test truncation of whitespace-only string"""
        whitespace_string = "   \n\t   "
        max_len = 2

        result = truncate(whitespace_string, max_len)
        assert len(encoder.encode(result)) <= max_len
        assert isinstance(result, str)

    def test_max_len_zero(self):
        """Test truncation with max_len = 0"""
        test_string = "hello world"
        result = truncate(test_string, 0)
        assert result == ""
        assert len(encoder.encode(result)) == 0

    def test_max_len_one(self):
        """Test truncation with max_len = 1"""
        test_string = "hello world"
        result = truncate(test_string, 1)
        assert len(encoder.encode(result)) == 1

    def test_preserves_decoding_encoding_consistency(self):
        """Test that truncation preserves encoding-decoding consistency"""
        test_string = "This is a test string for encoding consistency"
        max_len = 6

        result = truncate(test_string, max_len)
        # Re-encoding the result should give the same token count
        re_encoded = encoder.encode(result)
        assert len(re_encoded) == max_len

    def test_multibyte_characters_truncation(self):
        """Test truncation with multibyte characters that span multiple tokens"""
        # Some unicode characters may require multiple tokens
        multibyte_string = "🚀🌟🎉✨🔥💫"
        max_len = 3

        result = truncate(multibyte_string, max_len)
        assert len(encoder.encode(result)) == max_len

    def test_mixed_english_chinese_text(self):
        """Test truncation with mixed English and Chinese text"""
        mixed_string = "Hello 世界, this is a test 测试"
        max_len = 5

        result = truncate(mixed_string, max_len)
        assert len(encoder.encode(result)) == max_len

    def test_numbers_and_symbols(self):
        """Test truncation with numbers and symbols"""
        number_string = "12345 678.90 $100.00 @username #tag"
        max_len = 4

        result = truncate(number_string, max_len)
        assert len(encoder.encode(result)) == max_len

```

## Detailed Analysis

### File Role in Repository

The file `test/unit_test/common/test_token_utils.py` is located in the `test/unit_test/common` directory.

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
- [test_misc_utils.py](test_misc_utils.py_docs.md)
- [test_string_utils.py](test_string_utils.py_docs.md)
- [test_time_utils.py](test_time_utils.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
