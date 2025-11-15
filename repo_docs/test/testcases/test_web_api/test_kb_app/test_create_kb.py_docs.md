# File Documentation: test/testcases/test_web_api/test_kb_app/test_create_kb.py

## File Metadata

- **Path**: `test/testcases/test_web_api/test_kb_app/test_create_kb.py`
- **Extension**: `.py`
- **Lines**: 110
- **Characters**: 4,047
- **Size**: 4,047 bytes
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
from concurrent.futures import ThreadPoolExecutor, as_completed

import pytest
from common import create_kb
from configs import DATASET_NAME_LIMIT, INVALID_API_TOKEN
from hypothesis import example, given, settings
from libs.auth import RAGFlowWebApiAuth
from utils.hypothesis_utils import valid_names


@pytest.mark.usefixtures("clear_datasets")
class TestAuthorization:
    @pytest.mark.p1
    @pytest.mark.parametrize(
        "invalid_auth, expected_code, expected_message",
        [
            (None, 401, "<Unauthorized '401: Unauthorized'>"),
            (RAGFlowWebApiAuth(INVALID_API_TOKEN), 401, "<Unauthorized '401: Unauthorized'>"),
        ],
        ids=["empty_auth", "invalid_api_token"],
    )
    def test_auth_invalid(self, invalid_auth, expected_code, expected_message):
        res = create_kb(invalid_auth, {"name": "auth_test"})
        assert res["code"] == expected_code, res
        assert res["message"] == expected_message, res


@pytest.mark.usefixtures("clear_datasets")
class TestCapability:
    @pytest.mark.p3
    def test_create_kb_1k(self, WebApiAuth):
        for i in range(1_000):
            payload = {"name": f"dataset_{i}"}
            res = create_kb(WebApiAuth, payload)
            assert res["code"] == 0, f"Failed to create dataset {i}"

    @pytest.mark.p3
    def test_create_kb_concurrent(self, WebApiAuth):
        count = 100
        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [executor.submit(create_kb, WebApiAuth, {"name": f"dataset_{i}"}) for i in range(count)]
        responses = list(as_completed(futures))
        assert len(responses) == count, responses
        assert all(future.result()["code"] == 0 for future in futures)


@pytest.mark.usefixtures("clear_datasets")
class TestDatasetCreate:
    @pytest.mark.p1
    @given(name=valid_names())
    @example("a" * 128)
    @settings(max_examples=20)
    def test_name(self, WebApiAuth, name):
        res = create_kb(WebApiAuth, {"name": name})
        assert res["code"] == 0, res

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "name, expected_message",
        [
            ("", "Dataset name can't be empty."),
            (" ", "Dataset name can't be empty."),
            ("a" * (DATASET_NAME_LIMIT + 1), "Dataset name length is 129 which is large than 128"),
            (0, "Dataset name must be string."),
            (None, "Dataset name must be string."),
        ],
        ids=["empty_name", "space_name", "too_long_name", "invalid_name", "None_name"],
    )
    def test_name_invalid(self, WebApiAuth, name, expected_message):
        payload = {"name": name}
        res = create_kb(WebApiAuth, payload)
        assert res["code"] == 102, res
        assert expected_message in res["message"], res

    @pytest.mark.p3
    def test_name_duplicated(self, WebApiAuth):
        name = "duplicated_name"
        payload = {"name": name}
        res = create_kb(WebApiAuth, payload)
        assert res["code"] == 0, res

        res = create_kb(WebApiAuth, payload)
        assert res["code"] == 0, res

    @pytest.mark.p3
    def test_name_case_insensitive(self, WebApiAuth):
        name = "CaseInsensitive"
        payload = {"name": name.upper()}
        res = create_kb(WebApiAuth, payload)
        assert res["code"] == 0, res

        payload = {"name": name.lower()}
        res = create_kb(WebApiAuth, payload)
        assert res["code"] == 0, res

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
- `TestDatasetCreate`: Class definition

### Imports (7)

- `from concurrent.futures import ThreadPoolExecutor, as_completed`
- `import pytest`
- `from common import create_kb`
- `from configs import DATASET_NAME_LIMIT, INVALID_API_TOKEN`
- `from hypothesis import example, given, settings`
- `from libs.auth import RAGFlowWebApiAuth`
- `from utils.hypothesis_utils import valid_names`

## Code Structure Analysis

- Total lines: 110
- Blank lines: 14 (12.7%)
- Comment lines: ~15 (13.6%)
- Code lines: ~81


## Dependencies and Imports

- `from concurrent.futures import ThreadPoolExecutor, as_completed`
- `import pytest`
- `from common import create_kb`
- `from configs import DATASET_NAME_LIMIT, INVALID_API_TOKEN`
- `from hypothesis import example, given, settings`
- `from libs.auth import RAGFlowWebApiAuth`
- `from utils.hypothesis_utils import valid_names`

## Design & Architecture

This file is located in the `test` directory, specifically within `test/testcases/test_web_api/test_kb_app`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 4 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `test/testcases/test_web_api/test_kb_app/` directory
- Imports from `concurrent.futures`
- Imports from `common`
- Imports from `configs`
- Imports from `hypothesis`
- Imports from `libs.auth`

## Keywords

ANY, All, Apache, Authors, BASIS, CONDITIONS, CaseInsensitive, Copyright, DATASET_NAME_LIMIT, Dataset, Failed, INVALID_API_TOKEN, InfiniFlow, KIND, LICENSE, License, Licensed, None, None_name, Python, RAGFlowWebApiAuth, Reserved, Rights, See, TestAuthorization, TestCapability, TestDatasetCreate, The, ThreadPoolExecutor, Unauthorized, Unless, Version, WARRANTIES, WITHOUT, WebApiAuth, You, example, given, pytest, settings, test_auth_invalid, test_create_kb_1k, test_create_kb_concurrent, test_name, test_name_case_insensitive, test_name_duplicated, test_name_invalid

---
*Generated by RAGFlow Repository Documentation Generator*
