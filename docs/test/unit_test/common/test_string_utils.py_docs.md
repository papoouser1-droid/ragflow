# Documentation: test/unit_test/common/test_string_utils.py

## File Metadata

- **Path**: `test/unit_test/common/test_string_utils.py`
- **Size**: 14559 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/unit_test/common/test_string_utils.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `pytest`
- `common.string_utils`

### Classes Defined

This file defines 2 class(es):

#### Class: `TestRemoveRedundantSpaces` (line 21)

**Methods**: test_remove_spaces_before_commas, test_remove_spaces_before_periods, test_remove_spaces_before_exclamation, test_remove_spaces_after_opening_parenthesis, test_remove_spaces_before_closing_parenthesis, test_keep_spaces_between_words, test_mixed_punctuation, test_with_numbers, test_decimal_numbers, test_time_format, test_currency_symbols, test_empty_string, test_only_spaces, test_no_redundant_spaces, test_multiple_spaces, test_angle_brackets, test_case_insensitive, test_semicolon_and_colon, test_quotation_marks, test_abbreviations, test_multiple_punctuation, test_email_addresses, test_urls, test_hashtags_and_mentions, test_nested_parentheses, test_math_expressions, test_html_tags, test_programming_code, test_unicode_and_special_symbols, test_mixed_chinese_english, test_special_characters_in_pattern, test_tabs_and_newlines

#### Class: `TestCleanMarkdownBlock` (line 246)

**Methods**: test_standard_markdown_block, test_with_whitespace_variations, test_multiline_content, test_no_opening_newline, test_no_closing_newline, test_empty_markdown_block, test_only_whitespace_content, test_plain_text_without_markdown, test_partial_markdown_syntax, test_mixed_whitespace_characters, test_preserves_internal_whitespace, test_special_characters_content, test_empty_string, test_only_markdown_tags, test_windows_line_endings, test_unix_line_endings, test_nested_code_blocks_preserved, test_multiple_markdown_blocks

### Functions Defined

This file defines 50 function(s):

#### Function: `test_remove_spaces_before_commas` (line 25)

**Parameters**: self

**Docstring**: Test removing spaces before commas...

#### Function: `test_remove_spaces_before_periods` (line 32)

**Parameters**: self

**Docstring**: Test removing spaces before periods...

#### Function: `test_remove_spaces_before_exclamation` (line 38)

**Parameters**: self

**Docstring**: Test removing spaces before exclamation marks...

#### Function: `test_remove_spaces_after_opening_parenthesis` (line 44)

**Parameters**: self

**Docstring**: Test removing spaces after opening parenthesis...

#### Function: `test_remove_spaces_before_closing_parenthesis` (line 50)

**Parameters**: self

**Docstring**: Test removing spaces before closing parenthesis...

#### Function: `test_keep_spaces_between_words` (line 56)

**Parameters**: self

**Docstring**: Test preserving normal spaces between words...

#### Function: `test_mixed_punctuation` (line 63)

**Parameters**: self

**Docstring**: Test mixed punctuation scenarios...

#### Function: `test_with_numbers` (line 71)

**Parameters**: self

**Docstring**: Test handling of numbers...

#### Function: `test_decimal_numbers` (line 78)

**Parameters**: self

**Docstring**: Test decimal numbers...

#### Function: `test_time_format` (line 85)

**Parameters**: self

**Docstring**: Test time format handling...

#### Function: `test_currency_symbols` (line 92)

**Parameters**: self

**Docstring**: Test currency symbols...

#### Function: `test_empty_string` (line 99)

**Parameters**: self

**Docstring**: Test empty string input...

#### Function: `test_only_spaces` (line 103)

**Parameters**: self

**Docstring**: Test input with only spaces...

#### Function: `test_no_redundant_spaces` (line 110)

**Parameters**: self

**Docstring**: Test text without redundant spaces...

#### Function: `test_multiple_spaces` (line 117)

**Parameters**: self

**Docstring**: Test multiple consecutive spaces...

#### Function: `test_angle_brackets` (line 123)

**Parameters**: self

**Docstring**: Test angle brackets handling...

#### Function: `test_case_insensitive` (line 130)

**Parameters**: self

**Docstring**: Test case insensitivity...

#### Function: `test_semicolon_and_colon` (line 138)

**Parameters**: self

**Docstring**: Test semicolon and colon handling...

#### Function: `test_quotation_marks` (line 145)

**Parameters**: self

**Docstring**: Test quotation marks handling...

#### Function: `test_abbreviations` (line 152)

**Parameters**: self

**Docstring**: Test abbreviations...

#### Function: `test_multiple_punctuation` (line 159)

**Parameters**: self

**Docstring**: Test multiple consecutive punctuation marks...

#### Function: `test_email_addresses` (line 167)

**Parameters**: self

**Docstring**: Test email addresses (should not be modified ideally)...

#### Function: `test_urls` (line 174)

**Parameters**: self

**Docstring**: Test URLs (might be modified by current function)...

#### Function: `test_hashtags_and_mentions` (line 181)

**Parameters**: self

**Docstring**: Test hashtags and mentions...

#### Function: `test_nested_parentheses` (line 189)

**Parameters**: self

**Docstring**: Test nested parentheses...

#### Function: `test_math_expressions` (line 196)

**Parameters**: self

**Docstring**: Test mathematical expressions...

#### Function: `test_html_tags` (line 203)

**Parameters**: self

**Docstring**: Test HTML tags...

#### Function: `test_programming_code` (line 210)

**Parameters**: self

**Docstring**: Test programming code snippets...

#### Function: `test_unicode_and_special_symbols` (line 218)

**Parameters**: self

**Docstring**: Test Unicode characters and special symbols...

#### Function: `test_mixed_chinese_english` (line 225)

**Parameters**: self

**Docstring**: Test mixed Chinese and English text...

#### Function: `test_special_characters_in_pattern` (line 232)

**Parameters**: self

**Docstring**: Test special characters in the pattern...

#### Function: `test_tabs_and_newlines` (line 239)

**Parameters**: self

**Docstring**: Test tabs and newlines handling...

#### Function: `test_standard_markdown_block` (line 248)

**Parameters**: self

**Docstring**: Test standard Markdown code block syntax...

#### Function: `test_with_whitespace_variations` (line 254)

**Parameters**: self

**Docstring**: Test markdown blocks with various whitespace patterns...

#### Function: `test_multiline_content` (line 260)

**Parameters**: self

**Docstring**: Test markdown blocks with multiple lines of content...

#### Function: `test_no_opening_newline` (line 266)

**Parameters**: self

**Docstring**: Test markdown block without newline after opening tag...

#### Function: `test_no_closing_newline` (line 272)

**Parameters**: self

**Docstring**: Test markdown block without newline before closing tag...

#### Function: `test_empty_markdown_block` (line 278)

**Parameters**: self

**Docstring**: Test empty Markdown code block...

#### Function: `test_only_whitespace_content` (line 284)

**Parameters**: self

**Docstring**: Test markdown block containing only whitespace...

#### Function: `test_plain_text_without_markdown` (line 290)

**Parameters**: self

**Docstring**: Test text that doesn't contain markdown block syntax...

#### Function: `test_partial_markdown_syntax` (line 296)

**Parameters**: self

**Docstring**: Test text with only opening or closing tags...

#### Function: `test_mixed_whitespace_characters` (line 306)

**Parameters**: self

**Docstring**: Test with tabs, spaces, and mixed whitespace...

#### Function: `test_preserves_internal_whitespace` (line 312)

**Parameters**: self

**Docstring**: Test that internal whitespace is preserved...

#### Function: `test_special_characters_content` (line 318)

**Parameters**: self

**Docstring**: Test markdown block with special characters...

#### Function: `test_empty_string` (line 324)

**Parameters**: self

**Docstring**: Test empty string input...

#### Function: `test_only_markdown_tags` (line 330)

**Parameters**: self

**Docstring**: Test input containing only Markdown tags...

#### Function: `test_windows_line_endings` (line 336)

**Parameters**: self

**Docstring**: Test markdown block with Windows line endings...

#### Function: `test_unix_line_endings` (line 342)

**Parameters**: self

**Docstring**: Test markdown block with Unix line endings...

#### Function: `test_nested_code_blocks_preserved` (line 348)

**Parameters**: self

**Docstring**: Test that nested code blocks within content are preserved...

#### Function: `test_multiple_markdown_blocks` (line 354)

**Parameters**: self

**Docstring**: Test behavior with multiple markdown blocks (takes first and last)...

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

import pytest
from common.string_utils import remove_redundant_spaces, clean_markdown_block


class TestRemoveRedundantSpaces:

    # Basic punctuation tests
    @pytest.mark.skip(reason="Failed")
    def test_remove_spaces_before_commas(self):
        """Test removing spaces before commas"""
        input_text = "Hello , world"
        expected = "Hello, world"
        assert remove_redundant_spaces(input_text) == expected

    @pytest.mark.skip(reason="Failed")
    def test_remove_spaces_before_periods(self):
        """Test removing spaces before periods"""
        input_text = "This is a test ."
        expected = "This is a test."
        assert remove_redundant_spaces(input_text) == expected

    def test_remove_spaces_before_exclamation(self):
        """Test removing spaces before exclamation marks"""
        input_text = "Amazing !"
        expected = "Amazing!"
        assert remove_redundant_spaces(input_text) == expected

    def test_remove_spaces_after_opening_parenthesis(self):
        """Test removing spaces after opening parenthesis"""
        input_text = "This is ( test)"
        expected = "This is (test)"
        assert remove_redundant_spaces(input_text) == expected

    def test_remove_spaces_before_closing_parenthesis(self):
        """Test removing spaces before closing parenthesis"""
        input_text = "This is (test )"
        expected = "This is (test)"
        assert remove_redundant_spaces(input_text) == expected

    def test_keep_spaces_between_words(self):
        """Test preserving normal spaces between words"""
        input_text = "This should remain unchanged"
        expected = "This should remain unchanged"
        assert remove_redundant_spaces(input_text) == expected

    @pytest.mark.skip(reason="Failed")
    def test_mixed_punctuation(self):
        """Test mixed punctuation scenarios"""
        input_text = "Hello , world ! This is ( test ) ."
        expected = "Hello, world! This is (test)."
        assert remove_redundant_spaces(input_text) == expected

    # Numbers and special formats
    @pytest.mark.skip(reason="Failed")
    def test_with_numbers(self):
        """Test handling of numbers"""
        input_text = "I have 100 , 000 dollars ."
        expected = "I have 100, 000 dollars."
        assert remove_redundant_spaces(input_text) == expected

    @pytest.mark.skip(reason="Failed")
    def test_decimal_numbers(self):
        """Test decimal numbers"""
        input_text = "The value is 3 . 14 ."
        expected = "The value is 3.14."
        assert remove_redundant_spaces(input_text) == expected

    @pytest.mark.skip(reason="Failed")
    def test_time_format(self):
        """Test time format handling"""
        input_text = "Time is 12 : 30 PM ."
        expected = "Time is 12:30 PM."
        assert remove_redundant_spaces(input_text) == expected

    @pytest.mark.skip(reason="Failed")
    def test_currency_symbols(self):
        """Test currency symbols"""
        input_text = "Price : € 100 , £ 50 , ¥ 1000 ."
        expected = "Price: €100, £50, ¥1000."
        assert remove_redundant_spaces(input_text) == expected

    # Edge cases and special characters
    def test_empty_string(self):
        """Test empty string input"""
        assert remove_redundant_spaces("") == ""

    def test_only_spaces(self):
        """Test input with only spaces"""
        input_text = "   "
        expected = "   "
        assert remove_redundant_spaces(input_text) == expected

    @pytest.mark.skip(reason="Failed")
    def test_no_redundant_spaces(self):
        """Test text without redundant spaces"""
        input_text = "Hello, world! This is (test)."
        expected = "Hello, world! This is (test)."
        assert remove_redundant_spaces(input_text) == expected

    @pytest.mark.skip(reason="Failed")
    def test_multiple_spaces(self):
        """Test multiple consecutive spaces"""
        input_text = "Hello  ,   world  !"
        expected = "Hello, world!"
        assert remove_redundant_spaces(input_text) == expected

    def test_angle_brackets(self):
        """Test angle brackets handling"""
        input_text = "This is < test >"
        expected = "This is <test>"
        assert remove_redundant_spaces(input_text) == expected

    @pytest.mark.skip(reason="Failed")
    def test_case_insensitive(self):
        """Test case insensitivity"""
        input_text = "HELLO , World !"
        expected = "HELLO, World!"
        assert remove_redundant_spaces(input_text) == expected

    # Additional punctuation marks
    @pytest.mark.skip(reason="Failed")
    def test_semicolon_and_colon(self):
        """Test semicolon and colon handling"""
        input_text = "Items : apple ; banana ; orange ."
        expected = "Items: apple; banana; orange."
        assert remove_redundant_spaces(input_text) == expected

    @pytest.mark.skip(reason="Failed")
    def test_quotation_marks(self):
        """Test quotation marks handling"""
        input_text = 'He said , " Hello " .'
        expected = 'He said, "Hello".'
        assert remove_redundant_spaces(input_text) == expected

    @pytest.mark.skip(reason="Failed")
    def test_abbreviations(self):
        """Test abbreviations"""
        input_text = "Dr . Smith and Mr . Jones ."
        expected = "Dr. Smith and Mr. Jones."
        assert remove_redundant_spaces(input_text) == expected

    @pytest.mark.skip(reason="Failed")
    def test_multiple_punctuation(self):
        """Test multiple consecutive punctuation marks"""
        input_text = "Wow !! ... Really ??"
        expected = "Wow!! ... Really??"
        assert remove_redundant_spaces(input_text) == expected

    # Special text formats
    @pytest.mark.skip(reason="Failed")
    def test_email_addresses(self):
        """Test email addresses (should not be modified ideally)"""
        input_text = "Contact me at test @ example . com ."
        expected = "Contact me at test@example.com."
        assert remove_redundant_spaces(input_text) == expected

    @pytest.mark.skip(reason="Failed")
    def test_urls(self):
        """Test URLs (might be modified by current function)"""
        input_text = "Visit https : //example.com / path ."
        expected = "Visit https://example.com/path."
        assert remove_redundant_spaces(input_text) == expected

    @pytest.mark.skip(reason="Failed")
    def test_hashtags_and_mentions(self):
        """Test hashtags and mentions"""
        input_text = "Check out # topic and @ user ."
        expected = "Check out #topic and @user."
        assert remove_redundant_spaces(input_text) == expected

    # Complex structures
    @pytest.mark.skip(reason="Failed")
    def test_nested_parentheses(self):
        """Test nested parentheses"""
        input_text = "Outer ( inner ( deep ) ) ."
        expected = "Outer (inner (deep))."
        assert remove_redundant_spaces(input_text) == expected

    @pytest.mark.skip(reason="Failed")
    def test_math_expressions(self):
        """Test mathematical expressions"""
        input_text = "Calculate 2 + 2 = 4 ."
        expected = "Calculate 2 + 2 = 4."
        assert remove_redundant_spaces(input_text) == expected

    @pytest.mark.skip(reason="Failed")
    def test_html_tags(self):
        """Test HTML tags"""
        input_text = "< p > This is a paragraph . < / p >"
        expected = "<p> This is a paragraph. </p>"
        assert remove_redundant_spaces(input_text) == expected

    @pytest.mark.skip(reason="Failed")
    def test_programming_code(self):
        """Test programming code snippets"""
        input_text = "Code : if ( x > 0 ) { print ( 'hello' ) ; }"
        expected = "Code: if (x > 0) {print ('hello');}"
        assert remove_redundant_spaces(input_text) == expected

    # Unicode and special symbols
    @pytest.mark.skip(reason="Failed")
    def test_unicode_and_special_symbols(self):
        """Test Unicode characters and special symbols"""
        input_text = "Copyright © 2023 , All rights reserved ."
        expected = "Copyright © 2023, All rights reserved."
        assert remove_redundant_spaces(input_text) == expected

    @pytest.mark.skip(reason="Failed")
    def test_mixed_chinese_english(self):
        """Test mixed Chinese and English text"""
        input_text = "你好 , world ! 这是 ( 测试 ) ."
        expected = "你好, world! 这是 (测试)."
        assert remove_redundant_spaces(input_text) == expected

    @pytest.mark.skip(reason="Failed")
    def test_special_characters_in_pattern(self):
        """Test special characters in the pattern"""
        input_text = "Price is $ 100 . 00 , tax included ."
        expected = "Price is $100.00, tax included."
        assert remove_redundant_spaces(input_text) == expected

    @pytest.mark.skip(reason="Failed")
    def test_tabs_and_newlines(self):
        """Test tabs and newlines handling"""
        input_text = "Hello ,\tworld !\nThis is ( test ) ."
        expected = "Hello,\tworld!\nThis is (test)."
        assert remove_redundant_spaces(input_text) == expected


class TestCleanMarkdownBlock:

    def test_standard_markdown_block(self):
        """Test standard Markdown code block syntax"""
        input_text = "```markdown\nHello world\n```"
        expected = "Hello world"
        assert clean_markdown_block(input_text) == expected

    def test_with_whitespace_variations(self):
        """Test markdown blocks with various whitespace patterns"""
        input_text = "  ```markdown  \n  Content here  \n  ```  "
        expected = "Content here"
        assert clean_markdown_block(input_text) == expected

    def test_multiline_content(self):
        """Test markdown blocks with multiple lines of content"""
        input_text = "```markdown\nLine 1\nLine 2\nLine 3\n```"
        expected = "Line 1\nLine 2\nLine 3"
        assert clean_markdown_block(input_text) == expected

    def test_no_opening_newline(self):
        """Test markdown block without newline after opening tag"""
        input_text = "```markdownHello world\n```"
        expected = "Hello world"
        assert clean_markdown_block(input_text) == expected

    def test_no_closing_newline(self):
        """Test markdown block without newline before closing tag"""
        input_text = "```markdown\nHello world```"
        expected = "Hello world"
        assert clean_markdown_block(input_text) == expected

    def test_empty_markdown_block(self):
        """Test empty Markdown code block"""
        input_text = "```markdown\n```"
        expected = ""
        assert clean_markdown_block(input_text) == expected

    def test_only_whitespace_content(self):
        """Test markdown block containing only whitespace"""
        input_text = "```markdown\n   \n\t\n\n```"
        expected = ""
        assert clean_markdown_block(input_text) == expected

    def test_plain_text_without_markdown(self):
        """Test text that doesn't contain markdown block syntax"""
        input_text = "This is plain text without any code blocks"
        expected = "This is plain text without any code blocks"
        assert clean_markdown_block(input_text) == expected

    def test_partial_markdown_syntax(self):
        """Test text with only opening or closing tags"""
        input_text = "```markdown\nUnclosed block"
        expected = "Unclosed block"
        assert clean_markdown_block(input_text) == expected

        input_text = "Unopened block\n```"
        expected = "Unopened block"
        assert clean_markdown_block(input_text) == expected

    def test_mixed_whitespace_characters(self):
        """Test with tabs, spaces, and mixed whitespace"""
        input_text = "\t```markdown\t\n\tContent with tabs\n\t```\t"
        expected = "Content with tabs"
        assert clean_markdown_block(input_text) == expected

    def test_preserves_internal_whitespace(self):
        """Test that internal whitespace is preserved"""
        input_text = "```markdown\n  Preserve internal  \n  whitespace  \n```"
        expected = "Preserve internal  \n  whitespace"
        assert clean_markdown_block(input_text) == expected

    def test_special_characters_content(self):
        """Test markdown block with special characters"""
        input_text = "```markdown\n# Header\n**Bold** and *italic*\n```"
        expected = "# Header\n**Bold** and *italic*"
        assert clean_markdown_block(input_text) == expected

    def test_empty_string(self):
        """Test empty string input"""
        input_text = ""
        expected = ""
        assert clean_markdown_block(input_text) == expected

    def test_only_markdown_tags(self):
        """Test input containing only Markdown tags"""
        input_text = "```markdown```"
        expected = ""
        assert clean_markdown_block(input_text) == expected

    def test_windows_line_endings(self):
        """Test markdown block with Windows line endings"""
        input_text = "```markdown\r\nHello world\r\n```"
        expected = "Hello world"
        assert clean_markdown_block(input_text) == expected

    def test_unix_line_endings(self):
        """Test markdown block with Unix line endings"""
        input_text = "```markdown\nHello world\n```"
        expected = "Hello world"
        assert clean_markdown_block(input_text) == expected

    def test_nested_code_blocks_preserved(self):
        """Test that nested code blocks within content are preserved"""
        input_text = "```markdown\nText with ```nested``` blocks\n```"
        expected = "Text with ```nested``` blocks"
        assert clean_markdown_block(input_text) == expected

    def test_multiple_markdown_blocks(self):
        """Test behavior with multiple markdown blocks (takes first and last)"""
        input_text = "```markdown\nFirst line\n```\n```markdown\nSecond line\n```"
        expected = "First line\n```\n```markdown\nSecond line"
        assert clean_markdown_block(input_text) == expected


```

## Detailed Analysis

### File Role in Repository

The file `test/unit_test/common/test_string_utils.py` is located in the `test/unit_test/common` directory.

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
- [test_time_utils.py](test_time_utils.py_docs.md)
- [test_token_utils.py](test_token_utils.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
