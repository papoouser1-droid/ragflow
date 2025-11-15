# Documentation: test/testcases/test_sdk_api/test_dataset_mangement/test_delete_datasets.py

## File Metadata

- **Path**: `test/testcases/test_sdk_api/test_dataset_mangement/test_delete_datasets.py`
- **Size**: 6690 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_sdk_api/test_dataset_mangement/test_delete_datasets.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `uuid`
- `concurrent.futures`
- `pytest`
- `common`
- `configs`
- `ragflow_sdk`

### Classes Defined

This file defines 3 class(es):

#### Class: `TestAuthorization` (line 25)

**Methods**: test_auth_invalid

#### Class: `TestCapability` (line 41)

**Methods**: test_delete_dataset_1k, test_concurrent_deletion

#### Class: `TestDatasetsDelete` (line 63)

**Methods**: test_ids, test_ids_empty, test_ids_none, test_id_not_uuid, test_id_not_uuid1, test_id_wrong_uuid, test_ids_partial_invalid, test_ids_duplicate, test_repeated_delete, test_field_unsupported

### Functions Defined

This file defines 13 function(s):

#### Function: `test_auth_invalid` (line 34)

**Parameters**: self, invalid_auth, expected_message

#### Function: `test_delete_dataset_1k` (line 43)

**Parameters**: self, client

#### Function: `test_concurrent_deletion` (line 51)

**Parameters**: self, client

#### Function: `test_ids` (line 73)

**Parameters**: self, client, add_datasets_func, func, remaining

#### Function: `test_ids_empty` (line 83)

**Parameters**: self, client

#### Function: `test_ids_none` (line 92)

**Parameters**: self, client

#### Function: `test_id_not_uuid` (line 101)

**Parameters**: self, client

#### Function: `test_id_not_uuid1` (line 112)

**Parameters**: self, client

#### Function: `test_id_wrong_uuid` (line 120)

**Parameters**: self, client

#### Function: `test_ids_partial_invalid` (line 138)

**Parameters**: self, client, add_datasets_func, func

#### Function: `test_ids_duplicate` (line 149)

**Parameters**: self, client, add_datasets_func

#### Function: `test_repeated_delete` (line 160)

**Parameters**: self, client, add_datasets_func

#### Function: `test_field_unsupported` (line 171)

**Parameters**: self, client

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
from concurrent.futures import ThreadPoolExecutor, as_completed

import pytest
from common import batch_create_datasets
from configs import HOST_ADDRESS, INVALID_API_TOKEN
from ragflow_sdk import RAGFlow


class TestAuthorization:
    @pytest.mark.p1
    @pytest.mark.parametrize(
        "invalid_auth, expected_message",
        [
            (None, "Authentication error: API key is invalid!"),
            (INVALID_API_TOKEN, "Authentication error: API key is invalid!"),
        ],
    )
    def test_auth_invalid(self, invalid_auth, expected_message):
        client = RAGFlow(invalid_auth, HOST_ADDRESS)
        with pytest.raises(Exception) as excinfo:
            client.delete_datasets()
        assert str(excinfo.value) == expected_message


class TestCapability:
    @pytest.mark.p3
    def test_delete_dataset_1k(self, client):
        datasets = batch_create_datasets(client, 1_000)
        client.delete_datasets(**{"ids": [dataset.id for dataset in datasets]})

        datasets = client.list_datasets()
        assert len(datasets) == 0, datasets

    @pytest.mark.p3
    def test_concurrent_deletion(self, client):
        count = 1_000
        datasets = batch_create_datasets(client, count)
        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [executor.submit(client.delete_datasets, **{"ids": [dataset.id for dataset in datasets][i : i + 1]}) for i in range(count)]
        responses = list(as_completed(futures))
        assert len(responses) == count, responses

        datasets = client.list_datasets()
        assert len(datasets) == 0, datasets


class TestDatasetsDelete:
    @pytest.mark.p1
    @pytest.mark.parametrize(
        "func, remaining",
        [
            (lambda r: {"ids": r[:1]}, 2),
            (lambda r: {"ids": r}, 0),
        ],
        ids=["single_dataset", "multiple_datasets"],
    )
    def test_ids(self, client, add_datasets_func, func, remaining):
        if callable(func):
            payload = func([dataset.id for dataset in add_datasets_func])
        client.delete_datasets(**payload)

        datasets = client.list_datasets()
        assert len(datasets) == remaining, str(datasets)

    @pytest.mark.p1
    @pytest.mark.usefixtures("add_dataset_func")
    def test_ids_empty(self, client):
        payload = {"ids": []}
        client.delete_datasets(**payload)

        datasets = client.list_datasets()
        assert len(datasets) == 1, str(datasets)

    @pytest.mark.p1
    @pytest.mark.usefixtures("add_datasets_func")
    def test_ids_none(self, client):
        payload = {"ids": None}
        client.delete_datasets(**payload)

        datasets = client.list_datasets()
        assert len(datasets) == 0, str(datasets)

    @pytest.mark.p2
    @pytest.mark.usefixtures("add_dataset_func")
    def test_id_not_uuid(self, client):
        payload = {"ids": ["not_uuid"]}
        with pytest.raises(Exception) as excinfo:
            client.delete_datasets(**payload)
        assert "Invalid UUID1 format" in str(excinfo.value), str(excinfo.value)

        datasets = client.list_datasets()
        assert len(datasets) == 1, str(datasets)

    @pytest.mark.p3
    @pytest.mark.usefixtures("add_dataset_func")
    def test_id_not_uuid1(self, client):
        payload = {"ids": [uuid.uuid4().hex]}
        with pytest.raises(Exception) as excinfo:
            client.delete_datasets(**payload)
        assert "Invalid UUID1 format" in str(excinfo.value), str(excinfo.value)

    @pytest.mark.p2
    @pytest.mark.usefixtures("add_dataset_func")
    def test_id_wrong_uuid(self, client):
        payload = {"ids": ["d94a8dc02c9711f0930f7fbc369eab6d"]}
        with pytest.raises(Exception) as excinfo:
            client.delete_datasets(**payload)
        assert "lacks permission for dataset" in str(excinfo.value), str(excinfo.value)

        datasets = client.list_datasets()
        assert len(datasets) == 1, str(datasets)

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "func",
        [
            lambda r: {"ids": ["d94a8dc02c9711f0930f7fbc369eab6d"] + r},
            lambda r: {"ids": r[:1] + ["d94a8dc02c9711f0930f7fbc369eab6d"] + r[1:3]},
            lambda r: {"ids": r + ["d94a8dc02c9711f0930f7fbc369eab6d"]},
        ],
    )
    def test_ids_partial_invalid(self, client, add_datasets_func, func):
        if callable(func):
            payload = func([dataset.id for dataset in add_datasets_func])
        with pytest.raises(Exception) as excinfo:
            client.delete_datasets(**payload)
        assert "lacks permission for dataset" in str(excinfo.value), str(excinfo.value)

        datasets = client.list_datasets()
        assert len(datasets) == 3, str(datasets)

    @pytest.mark.p2
    def test_ids_duplicate(self, client, add_datasets_func):
        dataset_ids = [dataset.id for dataset in add_datasets_func]
        payload = {"ids": dataset_ids + dataset_ids}
        with pytest.raises(Exception) as excinfo:
            client.delete_datasets(**payload)
        assert "Duplicate ids:" in str(excinfo.value), str(excinfo.value)

        datasets = client.list_datasets()
        assert len(datasets) == 3, str(datasets)

    @pytest.mark.p2
    def test_repeated_delete(self, client, add_datasets_func):
        dataset_ids = [dataset.id for dataset in add_datasets_func]
        payload = {"ids": dataset_ids}
        client.delete_datasets(**payload)

        with pytest.raises(Exception) as excinfo:
            client.delete_datasets(**payload)
        assert "lacks permission for dataset" in str(excinfo.value), str(excinfo.value)

    @pytest.mark.p2
    @pytest.mark.usefixtures("add_dataset_func")
    def test_field_unsupported(self, client):
        payload = {"unknown_field": "unknown_field"}
        with pytest.raises(Exception) as excinfo:
            client.delete_datasets(**payload)
        assert "got an unexpected keyword argument 'unknown_field'" in str(excinfo.value), str(excinfo.value)

        datasets = client.list_datasets()
        assert len(datasets) == 1, str(datasets)

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_sdk_api/test_dataset_mangement/test_delete_datasets.py` is located in the `test/testcases/test_sdk_api/test_dataset_mangement` directory.

This file is part of the **Testing** infrastructure.

### Architecture Context

Files in this location typically handle concerns related to test_dataset_mangement.

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

- [conftest.py](conftest.py_docs.md)
- [test_create_dataset.py](test_create_dataset.py_docs.md)
- [test_list_datasets.py](test_list_datasets.py_docs.md)
- [test_update_dataset.py](test_update_dataset.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
