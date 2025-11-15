# Documentation: test/testcases/test_sdk_api/test_dataset_mangement/test_list_datasets.py

## File Metadata

- **Path**: `test/testcases/test_sdk_api/test_dataset_mangement/test_list_datasets.py`
- **Size**: 11032 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_sdk_api/test_dataset_mangement/test_list_datasets.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `uuid`
- `concurrent.futures`
- `pytest`
- `configs`
- `ragflow_sdk`

### Classes Defined

This file defines 3 class(es):

#### Class: `TestAuthorization` (line 24)

**Methods**: test_auth_invalid

#### Class: `TestCapability` (line 40)

**Methods**: test_concurrent_list

#### Class: `TestDatasetsList` (line 56)

**Methods**: test_params_unset, test_params_empty, test_page, test_page_invalid, test_page_none, test_page_size, test_page_size_invalid, test_page_size_none, test_orderby, test_orderby_invalid, test_orderby_none, test_desc, test_desc_invalid, test_desc_none, test_name, test_name_wrong, test_name_empty, test_name_none, test_id, test_id_not_uuid, test_id_not_uuid1, test_id_wrong_uuid, test_id_empty, test_id_none, test_name_and_id, test_name_and_id_wrong, test_field_unsupported

### Functions Defined

This file defines 29 function(s):

#### Function: `test_auth_invalid` (line 33)

**Parameters**: self, invalid_auth, expected_message

#### Function: `test_concurrent_list` (line 42)

**Parameters**: self, client

#### Function: `test_params_unset` (line 58)

**Parameters**: self, client

#### Function: `test_params_empty` (line 63)

**Parameters**: self, client

#### Function: `test_page` (line 78)

**Parameters**: self, client, params, expected_page_size

#### Function: `test_page_invalid` (line 91)

**Parameters**: self, client, params, expected_message

#### Function: `test_page_none` (line 97)

**Parameters**: self, client

#### Function: `test_page_size` (line 114)

**Parameters**: self, client, params, expected_page_size

#### Function: `test_page_size_invalid` (line 126)

**Parameters**: self, client, params, expected_message

#### Function: `test_page_size_none` (line 132)

**Parameters**: self, client

#### Function: `test_orderby` (line 147)

**Parameters**: self, client, params

#### Function: `test_orderby_invalid` (line 162)

**Parameters**: self, client, params

#### Function: `test_orderby_none` (line 168)

**Parameters**: self, client

#### Function: `test_desc` (line 183)

**Parameters**: self, client, params

#### Function: `test_desc_invalid` (line 195)

**Parameters**: self, client, params

#### Function: `test_desc_none` (line 201)

**Parameters**: self, client

#### Function: `test_name` (line 208)

**Parameters**: self, client

#### Function: `test_name_wrong` (line 215)

**Parameters**: self, client

#### Function: `test_name_empty` (line 222)

**Parameters**: self, client

#### Function: `test_name_none` (line 228)

**Parameters**: self, client

#### Function: `test_id` (line 234)

**Parameters**: self, client, add_datasets

#### Function: `test_id_not_uuid` (line 242)

**Parameters**: self, client

#### Function: `test_id_not_uuid1` (line 249)

**Parameters**: self, client

#### Function: `test_id_wrong_uuid` (line 256)

**Parameters**: self, client

#### Function: `test_id_empty` (line 263)

**Parameters**: self, client

#### Function: `test_id_none` (line 270)

**Parameters**: self, client

#### Function: `test_name_and_id` (line 284)

**Parameters**: self, client, add_datasets, func, name, expected_num

#### Function: `test_name_and_id_wrong` (line 299)

**Parameters**: self, client, add_datasets, dataset_id, name

#### Function: `test_field_unsupported` (line 309)

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
            client.list_datasets()
        assert expected_message in str(excinfo.value)


class TestCapability:
    @pytest.mark.p3
    def test_concurrent_list(self, client):
        count = 100
        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [
                executor.submit(
                    client.list_datasets,
                )
                for i in range(count)
            ]
        responses = list(as_completed(futures))
        assert len(responses) == count, responses


@pytest.mark.usefixtures("add_datasets")
class TestDatasetsList:
    @pytest.mark.p1
    def test_params_unset(self, client):
        datasets = client.list_datasets()
        assert len(datasets) == 5, str(datasets)

    @pytest.mark.p2
    def test_params_empty(self, client):
        datasets = client.list_datasets(**{})
        assert len(datasets) == 5, str(datasets)

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "params, expected_page_size",
        [
            ({"page": 2, "page_size": 2}, 2),
            ({"page": 3, "page_size": 2}, 1),
            ({"page": 4, "page_size": 2}, 0),
            ({"page": 1, "page_size": 10}, 5),
        ],
        ids=["normal_middle_page", "normal_last_partial_page", "beyond_max_page", "full_data_single_page"],
    )
    def test_page(self, client, params, expected_page_size):
        datasets = client.list_datasets(**params)
        assert len(datasets) == expected_page_size, str(datasets)

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "params, expected_message",
        [
            ({"page": 0}, "Input should be greater than or equal to 1"),
            ({"page": "a"}, "not instance of"),
        ],
        ids=["page_0", "page_a"],
    )
    def test_page_invalid(self, client, params, expected_message):
        with pytest.raises(Exception) as excinfo:
            client.list_datasets(**params)
        assert expected_message in str(excinfo.value), str(excinfo.value)

    @pytest.mark.p2
    def test_page_none(self, client):
        params = {"page": None}
        with pytest.raises(Exception) as excinfo:
            client.list_datasets(**params)
        assert "not instance of" in str(excinfo.value), str(excinfo.value)

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "params, expected_page_size",
        [
            ({"page_size": 1}, 1),
            ({"page_size": 3}, 3),
            ({"page_size": 5}, 5),
            ({"page_size": 6}, 5),
        ],
        ids=["min_valid_page_size", "medium_page_size", "page_size_equals_total", "page_size_exceeds_total"],
    )
    def test_page_size(self, client, params, expected_page_size):
        datasets = client.list_datasets(**params)
        assert len(datasets) == expected_page_size, str(datasets)

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "params, expected_message",
        [
            ({"page_size": 0}, "Input should be greater than or equal to 1"),
            ({"page_size": "a"}, "not instance of"),
        ],
    )
    def test_page_size_invalid(self, client, params, expected_message):
        with pytest.raises(Exception) as excinfo:
            client.list_datasets(**params)
        assert expected_message in str(excinfo.value), str(excinfo.value)

    @pytest.mark.p2
    def test_page_size_none(self, client):
        params = {"page_size": None}
        with pytest.raises(Exception) as excinfo:
            client.list_datasets(**params)
        assert "not instance of" in str(excinfo.value), str(excinfo.value)

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "params",
        [
            {"orderby": "create_time"},
            {"orderby": "update_time"},
        ],
        ids=["orderby_create_time", "orderby_update_time"],
    )
    def test_orderby(self, client, params):
        client.list_datasets(**params)

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "params",
        [
            {"orderby": ""},
            {"orderby": "unknown"},
            {"orderby": "CREATE_TIME"},
            {"orderby": "UPDATE_TIME"},
            {"orderby": " create_time "},
        ],
        ids=["empty", "unknown", "orderby_create_time_upper", "orderby_update_time_upper", "whitespace"],
    )
    def test_orderby_invalid(self, client, params):
        with pytest.raises(Exception) as excinfo:
            client.list_datasets(**params)
        assert "Input should be 'create_time' or 'update_time'" in str(excinfo.value), str(excinfo.value)

    @pytest.mark.p3
    def test_orderby_none(self, client):
        params = {"orderby": None}
        with pytest.raises(Exception) as excinfo:
            client.list_datasets(**params)
        assert "not instance of" in str(excinfo.value), str(excinfo.value)

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "params",
        [
            {"desc": True},
            {"desc": False},
        ],
        ids=["desc=True", "desc=False"],
    )
    def test_desc(self, client, params):
        client.list_datasets(**params)

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "params",
        [
            {"desc": 3.14},
            {"desc": "unknown"},
        ],
        ids=["float_value", "invalid_string"],
    )
    def test_desc_invalid(self, client, params):
        with pytest.raises(Exception) as excinfo:
            client.list_datasets(**params)
        assert "not instance of" in str(excinfo.value), str(excinfo.value)

    @pytest.mark.p3
    def test_desc_none(self, client):
        params = {"desc": None}
        with pytest.raises(Exception) as excinfo:
            client.list_datasets(**params)
        assert "not instance of" in str(excinfo.value), str(excinfo.value)

    @pytest.mark.p1
    def test_name(self, client):
        params = {"name": "dataset_1"}
        datasets = client.list_datasets(**params)
        assert len(datasets) == 1, str(datasets)
        assert datasets[0].name == "dataset_1", str(datasets)

    @pytest.mark.p2
    def test_name_wrong(self, client):
        params = {"name": "wrong name"}
        with pytest.raises(Exception) as excinfo:
            client.list_datasets(**params)
        assert "lacks permission for dataset" in str(excinfo.value), str(excinfo.value)

    @pytest.mark.p2
    def test_name_empty(self, client):
        params = {"name": ""}
        datasets = client.list_datasets(**params)
        assert len(datasets) == 5, str(datasets)

    @pytest.mark.p2
    def test_name_none(self, client):
        params = {"name": None}
        datasets = client.list_datasets(**params)
        assert len(datasets) == 5, str(datasets)

    @pytest.mark.p1
    def test_id(self, client, add_datasets):
        dataset_ids = [dataset.id for dataset in add_datasets]
        params = {"id": dataset_ids[0]}
        datasets = client.list_datasets(**params)
        assert len(datasets) == 1, str(datasets)
        assert datasets[0].id == dataset_ids[0], str(datasets)

    @pytest.mark.p2
    def test_id_not_uuid(self, client):
        params = {"id": "not_uuid"}
        with pytest.raises(Exception) as excinfo:
            client.list_datasets(**params)
        assert "Invalid UUID1 format" in str(excinfo.value), str(excinfo.value)

    @pytest.mark.p2
    def test_id_not_uuid1(self, client):
        params = {"id": uuid.uuid4().hex}
        with pytest.raises(Exception) as excinfo:
            client.list_datasets(**params)
        assert "Invalid UUID1 format" in str(excinfo.value), str(excinfo.value)

    @pytest.mark.p2
    def test_id_wrong_uuid(self, client):
        params = {"id": "d94a8dc02c9711f0930f7fbc369eab6d"}
        with pytest.raises(Exception) as excinfo:
            client.list_datasets(**params)
        assert "lacks permission for dataset" in str(excinfo.value), str(excinfo.value)

    @pytest.mark.p2
    def test_id_empty(self, client):
        params = {"id": ""}
        with pytest.raises(Exception) as excinfo:
            client.list_datasets(**params)
        assert "Invalid UUID1 format" in str(excinfo.value), str(excinfo.value)

    @pytest.mark.p2
    def test_id_none(self, client):
        params = {"id": None}
        datasets = client.list_datasets(**params)
        assert len(datasets) == 5, str(datasets)

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "func, name, expected_num",
        [
            (lambda r: r[0].id, "dataset_0", 1),
            (lambda r: r[0].id, "dataset_1", 0),
        ],
        ids=["name_and_id_match", "name_and_id_mismatch"],
    )
    def test_name_and_id(self, client, add_datasets, func, name, expected_num):
        if callable(func):
            params = {"id": func(add_datasets), "name": name}
        datasets = client.list_datasets(**params)
        assert len(datasets) == expected_num, str(datasets)

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "dataset_id, name",
        [
            (lambda r: r[0].id, "wrong_name"),
            (uuid.uuid1().hex, "dataset_0"),
        ],
        ids=["name", "id"],
    )
    def test_name_and_id_wrong(self, client, add_datasets, dataset_id, name):
        if callable(dataset_id):
            params = {"id": dataset_id(add_datasets), "name": name}
        else:
            params = {"id": dataset_id, "name": name}
        with pytest.raises(Exception) as excinfo:
            client.list_datasets(**params)
        assert "lacks permission for dataset" in str(excinfo.value), str(excinfo.value)

    @pytest.mark.p2
    def test_field_unsupported(self, client):
        params = {"unknown_field": "unknown_field"}
        with pytest.raises(Exception) as excinfo:
            client.list_datasets(**params)
        assert "got an unexpected keyword argument" in str(excinfo.value), str(excinfo.value)

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_sdk_api/test_dataset_mangement/test_list_datasets.py` is located in the `test/testcases/test_sdk_api/test_dataset_mangement` directory.

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
- [test_delete_datasets.py](test_delete_datasets.py_docs.md)
- [test_update_dataset.py](test_update_dataset.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
