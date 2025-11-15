# File Documentation: sdk/python/test/test_sdk_api/t_dataset.py

## File Metadata

- **Path**: `sdk/python/test/test_sdk_api/t_dataset.py`
- **Extension**: `.py`
- **Lines**: 78
- **Characters**: 3,026
- **Size**: 3,026 bytes
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

import random

import pytest
from common import HOST_ADDRESS
from ragflow_sdk import RAGFlow


def test_create_dataset_with_name(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    rag.create_dataset("test_create_dataset_with_name")


def test_create_dataset_with_duplicated_name(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    rag.create_dataset("test_create_dataset_with_duplicated_name")
    with pytest.raises(Exception) as exc_info:
        rag.create_dataset("test_create_dataset_with_duplicated_name")
    assert str(exc_info.value) == "Dataset name 'test_create_dataset_with_duplicated_name' already exists"


def test_create_dataset_with_random_chunk_method(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    valid_chunk_methods = ["naive", "manual", "qa", "table", "paper", "book", "laws", "presentation", "picture", "one", "email"]
    random_chunk_method = random.choice(valid_chunk_methods)
    rag.create_dataset("test_create_dataset_with_random_chunk_method", chunk_method=random_chunk_method)


def test_create_dataset_with_invalid_parameter(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    chunk_method = "invalid_chunk_method"
    with pytest.raises(Exception) as exc_info:
        rag.create_dataset("test_create_dataset_with_invalid_chunk_method", chunk_method=chunk_method)
    assert (
        str(exc_info.value)
        == f"Field: <chunk_method> - Message: <Input should be 'naive', 'book', 'email', 'laws', 'manual', 'one', 'paper', 'picture', 'presentation', 'qa', 'table' or 'tag'> - Value: <{chunk_method}>"
    )


def test_update_dataset_with_name(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset("test_update_dataset")
    ds.update({"name": "updated_dataset"})


def test_delete_datasets_with_success(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset("test_delete_dataset")
    rag.delete_datasets(ids=[ds.id])


def test_list_datasets_with_success(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    rag.create_dataset("test_list_datasets")
    rag.list_datasets()

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


### Functions (7)

- `test_create_dataset_with_name()`: Function definition
- `test_create_dataset_with_duplicated_name()`: Function definition
- `test_create_dataset_with_random_chunk_method()`: Function definition
- `test_create_dataset_with_invalid_parameter()`: Function definition
- `test_update_dataset_with_name()`: Function definition
- `test_delete_datasets_with_success()`: Function definition
- `test_list_datasets_with_success()`: Function definition

### Imports (4)

- `import random`
- `import pytest`
- `from common import HOST_ADDRESS`
- `from ragflow_sdk import RAGFlow`

## Code Structure Analysis

- Total lines: 78
- Blank lines: 17 (21.8%)
- Comment lines: ~15 (19.2%)
- Code lines: ~46


## Dependencies and Imports

- `import random`
- `import pytest`
- `from common import HOST_ADDRESS`
- `from ragflow_sdk import RAGFlow`

## Design & Architecture

This file is located in the `sdk` directory, specifically within `sdk/python/test/test_sdk_api`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `sdk/python/test/test_sdk_api/` directory
- Imports from `common`
- Imports from `ragflow_sdk`

## Keywords

ANY, API_KEY, All, Apache, Authors, BASIS, CONDITIONS, Copyright, Dataset, Exception, Field, HOST_ADDRESS, InfiniFlow, Input, KIND, LICENSE, License, Licensed, Message, Python, RAGFlow, Reserved, Rights, See, The, Unless, Value, Version, WARRANTIES, WITHOUT, You, test_create_dataset_with_duplicated_name, test_create_dataset_with_invalid_parameter, test_create_dataset_with_name, test_create_dataset_with_random_chunk_method, test_delete_datasets_with_success, test_list_datasets_with_success, test_update_dataset_with_name

---
*Generated by RAGFlow Repository Documentation Generator*
