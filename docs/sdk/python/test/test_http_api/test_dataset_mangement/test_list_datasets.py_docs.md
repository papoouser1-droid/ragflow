# Documentation: sdk/python/test/test_http_api/test_dataset_mangement/test_list_datasets.py

## File Metadata

- **Path**: `sdk/python/test/test_http_api/test_dataset_mangement/test_list_datasets.py`
- **Size**: 13250 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/test/test_http_api/test_dataset_mangement/test_list_datasets.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `uuid`
- `concurrent.futures`
- `pytest`
- `common`
- `libs.auth`
- `libs.utils`

### Classes Defined

This file defines 3 class(es):

#### Class: `TestAuthorization` (line 25)

**Methods**: test_auth_invalid

#### Class: `TestCapability` (line 44)

**Methods**: test_concurrent_list

#### Class: `TestDatasetsList` (line 54)

**Methods**: test_params_unset, test_params_empty, test_page, test_page_invalid, test_page_none, test_page_size, test_page_size_invalid, test_page_size_none, test_orderby, test_orderby_invalid, test_orderby_none, test_desc, test_desc_invalid, test_desc_none, test_name, test_name_wrong, test_name_empty, test_name_none, test_id, test_id_not_uuid, test_id_not_uuid1, test_id_wrong_uuid, test_id_empty, test_id_none, test_name_and_id, test_name_and_id_wrong, test_field_unsupported

### Functions Defined

This file defines 29 function(s):

#### Function: `test_auth_invalid` (line 38)

**Parameters**: self, auth, expected_code, expected_message

#### Function: `test_concurrent_list` (line 46)

**Parameters**: self, get_http_api_auth

#### Function: `test_params_unset` (line 56)

**Parameters**: self, get_http_api_auth

#### Function: `test_params_empty` (line 62)

**Parameters**: self, get_http_api_auth

#### Function: `test_page` (line 79)

**Parameters**: self, get_http_api_auth, params, expected_page_size

#### Function: `test_page_invalid` (line 93)

**Parameters**: self, get_http_api_auth, params, expected_code, expected_message

#### Function: `test_page_none` (line 99)

**Parameters**: self, get_http_api_auth

#### Function: `test_page_size` (line 117)

**Parameters**: self, get_http_api_auth, params, expected_page_size

#### Function: `test_page_size_invalid` (line 130)

**Parameters**: self, get_http_api_auth, params, expected_code, expected_message

#### Function: `test_page_size_none` (line 136)

**Parameters**: self, get_http_api_auth

#### Function: `test_orderby` (line 154)

**Parameters**: self, get_http_api_auth, params, assertions

#### Function: `test_orderby_invalid` (line 169)

**Parameters**: self, get_http_api_auth, params

#### Function: `test_orderby_none` (line 175)

**Parameters**: self, get_http_api_auth

#### Function: `test_desc` (line 198)

**Parameters**: self, get_http_api_auth, params, assertions

#### Function: `test_desc_invalid` (line 213)

**Parameters**: self, get_http_api_auth, params

#### Function: `test_desc_none` (line 219)

**Parameters**: self, get_http_api_auth

#### Function: `test_name` (line 226)

**Parameters**: self, get_http_api_auth

#### Function: `test_name_wrong` (line 234)

**Parameters**: self, get_http_api_auth

#### Function: `test_name_empty` (line 241)

**Parameters**: self, get_http_api_auth

#### Function: `test_name_none` (line 248)

**Parameters**: self, get_http_api_auth

#### Function: `test_id` (line 255)

**Parameters**: self, get_http_api_auth, add_datasets

#### Function: `test_id_not_uuid` (line 264)

**Parameters**: self, get_http_api_auth

#### Function: `test_id_not_uuid1` (line 271)

**Parameters**: self, get_http_api_auth

#### Function: `test_id_wrong_uuid` (line 278)

**Parameters**: self, get_http_api_auth

#### Function: `test_id_empty` (line 285)

**Parameters**: self, get_http_api_auth

#### Function: `test_id_none` (line 292)

**Parameters**: self, get_http_api_auth

#### Function: `test_name_and_id` (line 307)

**Parameters**: self, get_http_api_auth, add_datasets, func, name, expected_num

#### Function: `test_name_and_id_wrong` (line 324)

**Parameters**: self, get_http_api_auth, add_datasets, dataset_id, name

#### Function: `test_field_unsupported` (line 335)

**Parameters**: self, get_http_api_auth

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
from concurrent.futures import ThreadPoolExecutor

import pytest
from common import INVALID_API_TOKEN, list_datasets
from libs.auth import RAGFlowHttpApiAuth
from libs.utils import is_sorted


class TestAuthorization:
    @pytest.mark.p1
    @pytest.mark.parametrize(
        "auth, expected_code, expected_message",
        [
            (None, 0, "`Authorization` can't be empty"),
            (
                RAGFlowHttpApiAuth(INVALID_API_TOKEN),
                109,
                "Authentication error: API key is invalid!",
            ),
        ],
    )
    def test_auth_invalid(self, auth, expected_code, expected_message):
        res = list_datasets(auth)
        assert res["code"] == expected_code, res
        assert res["message"] == expected_message, res


class TestCapability:
    @pytest.mark.p3
    def test_concurrent_list(self, get_http_api_auth):
        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [executor.submit(list_datasets, get_http_api_auth) for i in range(100)]
        responses = [f.result() for f in futures]
        assert all(r["code"] == 0 for r in responses), responses


@pytest.mark.usefixtures("add_datasets")
class TestDatasetsList:
    @pytest.mark.p1
    def test_params_unset(self, get_http_api_auth):
        res = list_datasets(get_http_api_auth, None)
        assert res["code"] == 0, res
        assert len(res["data"]) == 5, res

    @pytest.mark.p2
    def test_params_empty(self, get_http_api_auth):
        res = list_datasets(get_http_api_auth, {})
        assert res["code"] == 0, res
        assert len(res["data"]) == 5, res

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "params, expected_page_size",
        [
            ({"page": 2, "page_size": 2}, 2),
            ({"page": 3, "page_size": 2}, 1),
            ({"page": 4, "page_size": 2}, 0),
            ({"page": "2", "page_size": 2}, 2),
            ({"page": 1, "page_size": 10}, 5),
        ],
        ids=["normal_middle_page", "normal_last_partial_page", "beyond_max_page", "string_page_number", "full_data_single_page"],
    )
    def test_page(self, get_http_api_auth, params, expected_page_size):
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 0, res
        assert len(res["data"]) == expected_page_size, res

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "params, expected_code, expected_message",
        [
            ({"page": 0}, 101, "Input should be greater than or equal to 1"),
            ({"page": "a"}, 101, "Input should be a valid integer, unable to parse string as an integer"),
        ],
        ids=["page_0", "page_a"],
    )
    def test_page_invalid(self, get_http_api_auth, params, expected_code, expected_message):
        res = list_datasets(get_http_api_auth, params=params)
        assert res["code"] == expected_code, res
        assert expected_message in res["message"], res

    @pytest.mark.p2
    def test_page_none(self, get_http_api_auth):
        params = {"page": None}
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 0, res
        assert len(res["data"]) == 5, res

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "params, expected_page_size",
        [
            ({"page_size": 1}, 1),
            ({"page_size": 3}, 3),
            ({"page_size": 5}, 5),
            ({"page_size": 6}, 5),
            ({"page_size": "1"}, 1),
        ],
        ids=["min_valid_page_size", "medium_page_size", "page_size_equals_total", "page_size_exceeds_total", "string_type_page_size"],
    )
    def test_page_size(self, get_http_api_auth, params, expected_page_size):
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 0, res
        assert len(res["data"]) == expected_page_size, res

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "params, expected_code, expected_message",
        [
            ({"page_size": 0}, 101, "Input should be greater than or equal to 1"),
            ({"page_size": "a"}, 101, "Input should be a valid integer, unable to parse string as an integer"),
        ],
    )
    def test_page_size_invalid(self, get_http_api_auth, params, expected_code, expected_message):
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == expected_code, res
        assert expected_message in res["message"], res

    @pytest.mark.p2
    def test_page_size_none(self, get_http_api_auth):
        params = {"page_size": None}
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 0, res
        assert len(res["data"]) == 5, res

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "params, assertions",
        [
            ({"orderby": "create_time"}, lambda r: (is_sorted(r["data"], "create_time", True))),
            ({"orderby": "update_time"}, lambda r: (is_sorted(r["data"], "update_time", True))),
            ({"orderby": "CREATE_TIME"}, lambda r: (is_sorted(r["data"], "create_time", True))),
            ({"orderby": "UPDATE_TIME"}, lambda r: (is_sorted(r["data"], "update_time", True))),
            ({"orderby": " create_time "}, lambda r: (is_sorted(r["data"], "update_time", True))),
        ],
        ids=["orderby_create_time", "orderby_update_time", "orderby_create_time_upper", "orderby_update_time_upper", "whitespace"],
    )
    def test_orderby(self, get_http_api_auth, params, assertions):
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 0, res
        if callable(assertions):
            assert assertions(res), res

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "params",
        [
            {"orderby": ""},
            {"orderby": "unknown"},
        ],
        ids=["empty", "unknown"],
    )
    def test_orderby_invalid(self, get_http_api_auth, params):
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 101, res
        assert "Input should be 'create_time' or 'update_time'" in res["message"], res

    @pytest.mark.p3
    def test_orderby_none(self, get_http_api_auth):
        params = {"order_by": None}
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 0, res
        assert is_sorted(res["data"], "create_time", True), res

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "params, assertions",
        [
            ({"desc": True}, lambda r: (is_sorted(r["data"], "create_time", True))),
            ({"desc": False}, lambda r: (is_sorted(r["data"], "create_time", False))),
            ({"desc": "true"}, lambda r: (is_sorted(r["data"], "create_time", True))),
            ({"desc": "false"}, lambda r: (is_sorted(r["data"], "create_time", False))),
            ({"desc": 1}, lambda r: (is_sorted(r["data"], "create_time", True))),
            ({"desc": 0}, lambda r: (is_sorted(r["data"], "create_time", False))),
            ({"desc": "yes"}, lambda r: (is_sorted(r["data"], "create_time", True))),
            ({"desc": "no"}, lambda r: (is_sorted(r["data"], "create_time", False))),
            ({"desc": "y"}, lambda r: (is_sorted(r["data"], "create_time", True))),
            ({"desc": "n"}, lambda r: (is_sorted(r["data"], "create_time", False))),
        ],
        ids=["desc=True", "desc=False", "desc=true", "desc=false", "desc=1", "desc=0", "desc=yes", "desc=no", "desc=y", "desc=n"],
    )
    def test_desc(self, get_http_api_auth, params, assertions):
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 0, res
        if callable(assertions):
            assert assertions(res), res

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "params",
        [
            {"desc": 3.14},
            {"desc": "unknown"},
        ],
        ids=["empty", "unknown"],
    )
    def test_desc_invalid(self, get_http_api_auth, params):
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 101, res
        assert "Input should be a valid boolean, unable to interpret input" in res["message"], res

    @pytest.mark.p3
    def test_desc_none(self, get_http_api_auth):
        params = {"desc": None}
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 0, res
        assert is_sorted(res["data"], "create_time", True), res

    @pytest.mark.p1
    def test_name(self, get_http_api_auth):
        params = {"name": "dataset_1"}
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 0, res
        assert len(res["data"]) == 1, res
        assert res["data"][0]["name"] == "dataset_1", res

    @pytest.mark.p2
    def test_name_wrong(self, get_http_api_auth):
        params = {"name": "wrong name"}
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 108, res
        assert "lacks permission for dataset" in res["message"], res

    @pytest.mark.p2
    def test_name_empty(self, get_http_api_auth):
        params = {"name": ""}
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 0, res
        assert len(res["data"]) == 5, res

    @pytest.mark.p2
    def test_name_none(self, get_http_api_auth):
        params = {"name": None}
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 0, res
        assert len(res["data"]) == 5, res

    @pytest.mark.p1
    def test_id(self, get_http_api_auth, add_datasets):
        dataset_ids = add_datasets
        params = {"id": dataset_ids[0]}
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 0
        assert len(res["data"]) == 1
        assert res["data"][0]["id"] == dataset_ids[0]

    @pytest.mark.p2
    def test_id_not_uuid(self, get_http_api_auth):
        params = {"id": "not_uuid"}
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 101, res
        assert "Invalid UUID1 format" in res["message"], res

    @pytest.mark.p2
    def test_id_not_uuid1(self, get_http_api_auth):
        params = {"id": uuid.uuid4().hex}
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 101, res
        assert "Invalid UUID1 format" in res["message"], res

    @pytest.mark.p2
    def test_id_wrong_uuid(self, get_http_api_auth):
        params = {"id": "d94a8dc02c9711f0930f7fbc369eab6d"}
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 108, res
        assert "lacks permission for dataset" in res["message"], res

    @pytest.mark.p2
    def test_id_empty(self, get_http_api_auth):
        params = {"id": ""}
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 101, res
        assert "Invalid UUID1 format" in res["message"], res

    @pytest.mark.p2
    def test_id_none(self, get_http_api_auth):
        params = {"id": None}
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 0, res
        assert len(res["data"]) == 5, res

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "func, name, expected_num",
        [
            (lambda r: r[0], "dataset_0", 1),
            (lambda r: r[0], "dataset_1", 0),
        ],
        ids=["name_and_id_match", "name_and_id_mismatch"],
    )
    def test_name_and_id(self, get_http_api_auth, add_datasets, func, name, expected_num):
        dataset_ids = add_datasets
        if callable(func):
            params = {"id": func(dataset_ids), "name": name}
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 0, res
        assert len(res["data"]) == expected_num, res

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "dataset_id, name",
        [
            (lambda r: r[0], "wrong_name"),
            (uuid.uuid1().hex, "dataset_0"),
        ],
        ids=["name", "id"],
    )
    def test_name_and_id_wrong(self, get_http_api_auth, add_datasets, dataset_id, name):
        dataset_ids = add_datasets
        if callable(dataset_id):
            params = {"id": dataset_id(dataset_ids), "name": name}
        else:
            params = {"id": dataset_id, "name": name}
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 108, res
        assert "lacks permission for dataset" in res["message"], res

    @pytest.mark.p2
    def test_field_unsupported(self, get_http_api_auth):
        params = {"unknown_field": "unknown_field"}
        res = list_datasets(get_http_api_auth, params)
        assert res["code"] == 101, res
        assert "Extra inputs are not permitted" in res["message"], res

```

## Detailed Analysis

### File Role in Repository

The file `sdk/python/test/test_http_api/test_dataset_mangement/test_list_datasets.py` is located in the `sdk/python/test/test_http_api/test_dataset_mangement` directory.

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
