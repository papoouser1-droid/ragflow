# Documentation: test/testcases/test_web_api/test_kb_app/test_create_kb.py

## File Metadata

- **Path**: `test/testcases/test_web_api/test_kb_app/test_create_kb.py`
- **Size**: 4047 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_web_api/test_kb_app/test_create_kb.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `concurrent.futures`
- `pytest`
- `common`
- `configs`
- `hypothesis`
- `libs.auth`
- `utils.hypothesis_utils`

### Classes Defined

This file defines 3 class(es):

#### Class: `TestAuthorization` (line 27)

**Methods**: test_auth_invalid

#### Class: `TestCapability` (line 44)

**Methods**: test_create_kb_1k, test_create_kb_concurrent

#### Class: `TestDatasetCreate` (line 63)

**Methods**: test_name, test_name_invalid, test_name_duplicated, test_name_case_insensitive

### Functions Defined

This file defines 7 function(s):

#### Function: `test_auth_invalid` (line 37)

**Parameters**: self, invalid_auth, expected_code, expected_message

#### Function: `test_create_kb_1k` (line 46)

**Parameters**: self, WebApiAuth

#### Function: `test_create_kb_concurrent` (line 53)

**Parameters**: self, WebApiAuth

#### Function: `test_name` (line 68)

**Parameters**: self, WebApiAuth, name

#### Function: `test_name_invalid` (line 84)

**Parameters**: self, WebApiAuth, name, expected_message

#### Function: `test_name_duplicated` (line 91)

**Parameters**: self, WebApiAuth

#### Function: `test_name_case_insensitive` (line 101)

**Parameters**: self, WebApiAuth

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

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_web_api/test_kb_app/test_create_kb.py` is located in the `test/testcases/test_web_api/test_kb_app` directory.

This file is part of the **Testing** infrastructure.

### Architecture Context

Files in this location typically handle concerns related to test_kb_app.

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
- [test_detail_kb.py](test_detail_kb.py_docs.md)
- [test_list_kbs.py](test_list_kbs.py_docs.md)
- [test_rm_kb.py](test_rm_kb.py_docs.md)
- [test_update_kb.py](test_update_kb.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
