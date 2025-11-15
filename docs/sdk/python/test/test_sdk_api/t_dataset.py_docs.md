# Documentation: sdk/python/test/test_sdk_api/t_dataset.py

## File Metadata

- **Path**: `sdk/python/test/test_sdk_api/t_dataset.py`
- **Size**: 3026 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/test/test_sdk_api/t_dataset.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `random`
- `pytest`
- `common`
- `ragflow_sdk`

### Functions Defined

This file defines 7 function(s):

#### Function: `test_create_dataset_with_name` (line 24)

**Parameters**: get_api_key_fixture

#### Function: `test_create_dataset_with_duplicated_name` (line 30)

**Parameters**: get_api_key_fixture

#### Function: `test_create_dataset_with_random_chunk_method` (line 39)

**Parameters**: get_api_key_fixture

#### Function: `test_create_dataset_with_invalid_parameter` (line 47)

**Parameters**: get_api_key_fixture

#### Function: `test_update_dataset_with_name` (line 59)

**Parameters**: get_api_key_fixture

#### Function: `test_delete_datasets_with_success` (line 66)

**Parameters**: get_api_key_fixture

#### Function: `test_list_datasets_with_success` (line 73)

**Parameters**: get_api_key_fixture

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

## Detailed Analysis

### File Role in Repository

The file `sdk/python/test/test_sdk_api/t_dataset.py` is located in the `sdk/python/test/test_sdk_api` directory.

This file is part of the **Testing** infrastructure.

### Architecture Context

Files in this location typically handle concerns related to test_sdk_api.

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

- [common.py](common.py_docs.md)
- [get_email.py](get_email.py_docs.md)
- [t_agent.py](t_agent.py_docs.md)
- [t_chat.py](t_chat.py_docs.md)
- [t_chunk.py](t_chunk.py_docs.md)
- [t_document.py](t_document.py_docs.md)
- [t_session.py](t_session.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
