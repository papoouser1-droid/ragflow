# File Documentation: test/unit_test/common/test_file_utils.py

## File Metadata

- **Path**: `test/unit_test/common/test_file_utils.py`
- **Extension**: `.py`
- **Lines**: 124
- **Characters**: 4,711
- **Size**: 4,711 bytes
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

import os
import pytest
from unittest.mock import patch
from common import file_utils
from common.file_utils import get_project_base_directory


class TestGetProjectBaseDirectory:
    """Test cases for get_project_base_directory function"""

    def test_returns_project_base_when_no_args(self):
        """Test that function returns project base directory when no arguments provided"""
        result = get_project_base_directory()

        assert result is not None
        assert isinstance(result, str)
        assert os.path.isabs(result)  # Should return absolute path

    def test_returns_path_with_single_argument(self):
        """Test that function joins project base with single additional path component"""
        result = get_project_base_directory("subfolder")

        assert result is not None
        assert "subfolder" in result
        assert result.endswith("subfolder")

    def test_returns_path_with_multiple_arguments(self):
        """Test that function joins project base with multiple path components"""
        result = get_project_base_directory("folder1", "folder2", "file.txt")

        assert result is not None
        assert "folder1" in result
        assert "folder2" in result
        assert "file.txt" in result
        assert os.path.basename(result) == "file.txt"

    def test_uses_environment_variable_when_available(self):
        """Test that function uses RAG_PROJECT_BASE environment variable when set"""
        test_path = "/custom/project/path"

        file_utils.PROJECT_BASE = test_path

        result = get_project_base_directory()
        assert result == test_path

    def test_calculates_default_path_when_no_env_vars(self):
        """Test that function calculates default path when no environment variables are set"""
        with patch.dict(os.environ, {}, clear=True):  # Clear all environment variables
            # Reset the global variable to force re-initialization

            result = get_project_base_directory()

            # Should return a valid absolute path
            assert result is not None
            assert os.path.isabs(result)
            assert os.path.basename(result) != ""  # Should not be root directory

    def test_caches_project_base_value(self):
        """Test that PROJECT_BASE is cached after first calculation"""
        # Reset the global variable

        # First call should calculate the value
        first_result = get_project_base_directory()

        # Store the current value
        cached_value = file_utils.PROJECT_BASE

        # Second call should use cached value
        second_result = get_project_base_directory()

        assert first_result == second_result
        assert file_utils.PROJECT_BASE == cached_value

    def test_path_components_joined_correctly(self):
        """Test that path components are properly joined with the base directory"""
        base_path = get_project_base_directory()
        expected_path = os.path.join(base_path, "data", "files", "document.txt")

        result = get_project_base_directory("data", "files", "document.txt")

        assert result == expected_path

    def test_handles_empty_string_arguments(self):
        """Test that function handles empty string arguments correctly"""
        result = get_project_base_directory("")

        # Should still return a valid path (base directory)
        assert result is not None
        assert os.path.isabs(result)


# Parameterized tests for different path combinations
@pytest.mark.parametrize("path_args,expected_suffix", [
    ((), ""),  # No additional arguments
    (("src",), "src"),
    (("data", "models"), os.path.join("data", "models")),
    (("config", "app", "settings.json"), os.path.join("config", "app", "settings.json")),
])
def test_various_path_combinations(path_args, expected_suffix):
    """Test various combinations of path arguments"""
    base_path = get_project_base_directory()
    result = get_project_base_directory(*path_args)

    if expected_suffix:
        assert result.endswith(expected_suffix)
    else:
        assert result == base_path

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

- `TestGetProjectBaseDirectory`: Class definition

### Functions (1)

- `test_various_path_combinations()`: Function definition

### Imports (5)

- `import os`
- `import pytest`
- `from unittest.mock import patch`
- `from common import file_utils`
- `from common.file_utils import get_project_base_directory`

## Code Structure Analysis

- Total lines: 124
- Blank lines: 29 (23.4%)
- Comment lines: ~33 (26.6%)
- Code lines: ~62


## Dependencies and Imports

- `import os`
- `import pytest`
- `from unittest.mock import patch`
- `from common import file_utils`
- `from common.file_utils import get_project_base_directory`

## Design & Architecture

This file is located in the `test` directory, specifically within `test/unit_test/common`.

This is a test file, contributing to the quality assurance and validation of the codebase.

## Performance & Complexity

- Contains 3 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `test/unit_test/common/` directory
- Imports from `unittest.mock`
- Imports from `common`
- Imports from `common.file_utils`

## Keywords

ANY, All, Apache, Authors, BASIS, CONDITIONS, Clear, Copyright, First, InfiniFlow, KIND, LICENSE, License, Licensed, None, PROJECT_BASE, Parameterized, Python, RAG_PROJECT_BASE, Reserved, Reset, Rights, Second, See, Should, Store, Test, TestGetProjectBaseDirectory, The, True, Unless, Version, WARRANTIES, WITHOUT, You, calculates, handles, joins, pytest, returns, test_caches_project_base_value, test_calculates_default_path_when_no_env_vars, test_handles_empty_string_arguments, test_path_components_joined_correctly, test_returns_path_with_multiple_arguments, test_returns_path_with_single_argument, test_returns_project_base_when_no_args, test_uses_environment_variable_when_available, test_various_path_combinations, uses

---
*Generated by RAGFlow Repository Documentation Generator*
