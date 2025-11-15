# File Documentation: test/testcases/test_sdk_api/test_dataset_mangement/test_delete_datasets.py

## File Metadata

- **Path**: `test/testcases/test_sdk_api/test_dataset_mangement/test_delete_datasets.py`
- **Extension**: `.py`
- **Lines**: 179
- **Characters**: 6,690
- **Size**: 6,690 bytes
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

### Classes (3)

- `TestAuthorization`: Class definition
- `TestCapability`: Class definition
- `TestDatasetsDelete`: Class definition

### Imports (6)

- `import uuid`
- `from concurrent.futures import ThreadPoolExecutor, as_completed`
- `import pytest`
- `from common import batch_create_datasets`
- `from configs import HOST_ADDRESS, INVALID_API_TOKEN`
- `from ragflow_sdk import RAGFlow`

## Code Structure Analysis

- Total lines: 179
- Blank lines: 29 (16.2%)
- Comment lines: ~15 (8.4%)
- Code lines: ~135


## Dependencies and Imports

- `import uuid`
- `from concurrent.futures import ThreadPoolExecutor, as_completed`
- `import pytest`
- `from common import batch_create_datasets`
- `from configs import HOST_ADDRESS, INVALID_API_TOKEN`
- `from ragflow_sdk import RAGFlow`

## Design & Architecture

This file is located in the `test` directory, specifically within `test/testcases/test_sdk_api/test_dataset_mangement`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 11 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `test/testcases/test_sdk_api/test_dataset_mangement/` directory
- Imports from `concurrent.futures`
- Imports from `common`
- Imports from `configs`
- Imports from `ragflow_sdk`

## Keywords

ANY, API, All, Apache, Authentication, Authors, BASIS, CONDITIONS, Copyright, Duplicate, Exception, HOST_ADDRESS, INVALID_API_TOKEN, InfiniFlow, Invalid, KIND, LICENSE, License, Licensed, None, Python, RAGFlow, Reserved, Rights, See, TestAuthorization, TestCapability, TestDatasetsDelete, The, ThreadPoolExecutor, UUID1, Unless, Version, WARRANTIES, WITHOUT, You, pytest, test_auth_invalid, test_concurrent_deletion, test_delete_dataset_1k, test_field_unsupported, test_id_not_uuid, test_id_not_uuid1, test_id_wrong_uuid, test_ids, test_ids_duplicate, test_ids_empty, test_ids_none, test_ids_partial_invalid, test_repeated_delete

---
*Generated by RAGFlow Repository Documentation Generator*
