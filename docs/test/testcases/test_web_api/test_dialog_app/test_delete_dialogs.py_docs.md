# Documentation: test/testcases/test_web_api/test_dialog_app/test_delete_dialogs.py

## File Metadata

- **Path**: `test/testcases/test_web_api/test_dialog_app/test_delete_dialogs.py`
- **Size**: 7774 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_web_api/test_dialog_app/test_delete_dialogs.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `concurrent.futures`
- `pytest`
- `common`
- `configs`
- `libs.auth`

### Classes Defined

This file defines 2 class(es):

#### Class: `TestAuthorization` (line 25)

**Methods**: test_auth_invalid

#### Class: `TestDialogDelete` (line 43)

**Methods**: test_delete_single_dialog, test_delete_multiple_dialogs, test_delete_partial_dialogs, test_delete_nonexistent_dialog, test_delete_empty_dialog_ids, test_delete_missing_dialog_ids, test_delete_invalid_dialog_ids_format, test_delete_mixed_valid_invalid_dialogs, test_delete_dialog_concurrent, test_delete_dialog_idempotent, test_delete_large_batch_dialogs, test_delete_dialog_with_special_characters, test_delete_dialog_preserves_other_user_dialogs

### Functions Defined

This file defines 14 function(s):

#### Function: `test_auth_invalid` (line 35)

**Parameters**: self, invalid_auth, expected_code, expected_message, add_dialog_func

#### Function: `test_delete_single_dialog` (line 45)

**Parameters**: self, WebApiAuth, add_dialog_func

#### Function: `test_delete_multiple_dialogs` (line 62)

**Parameters**: self, WebApiAuth, add_dialogs_func

#### Function: `test_delete_partial_dialogs` (line 79)

**Parameters**: self, WebApiAuth, add_dialogs_func

#### Function: `test_delete_nonexistent_dialog` (line 97)

**Parameters**: self, WebApiAuth

#### Function: `test_delete_empty_dialog_ids` (line 105)

**Parameters**: self, WebApiAuth

#### Function: `test_delete_missing_dialog_ids` (line 111)

**Parameters**: self, WebApiAuth

#### Function: `test_delete_invalid_dialog_ids_format` (line 118)

**Parameters**: self, WebApiAuth

#### Function: `test_delete_mixed_valid_invalid_dialogs` (line 125)

**Parameters**: self, WebApiAuth, add_dialog_func

#### Function: `test_delete_dialog_concurrent` (line 139)

**Parameters**: self, WebApiAuth, add_dialogs_func

#### Function: `test_delete_dialog_idempotent` (line 156)

**Parameters**: self, WebApiAuth, add_dialog_func

#### Function: `test_delete_large_batch_dialogs` (line 167)

**Parameters**: self, WebApiAuth, add_document

#### Function: `test_delete_dialog_with_special_characters` (line 183)

**Parameters**: self, WebApiAuth

#### Function: `test_delete_dialog_preserves_other_user_dialogs` (line 199)

**Parameters**: self, WebApiAuth, add_dialog_func

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
from common import batch_create_dialogs, create_dialog, delete_dialog, list_dialogs
from configs import INVALID_API_TOKEN
from libs.auth import RAGFlowWebApiAuth


@pytest.mark.usefixtures("clear_dialogs")
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
    def test_auth_invalid(self, invalid_auth, expected_code, expected_message, add_dialog_func):
        _, dialog_id = add_dialog_func
        payload = {"dialog_ids": [dialog_id]}
        res = delete_dialog(invalid_auth, payload)
        assert res["code"] == expected_code, res
        assert res["message"] == expected_message, res


class TestDialogDelete:
    @pytest.mark.p1
    def test_delete_single_dialog(self, WebApiAuth, add_dialog_func):
        _, dialog_id = add_dialog_func

        res = list_dialogs(WebApiAuth)
        assert res["code"] == 0, res
        assert len(res["data"]) == 1, res

        payload = {"dialog_ids": [dialog_id]}
        res = delete_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res
        assert res["data"] is True, res

        res = list_dialogs(WebApiAuth)
        assert res["code"] == 0, res
        assert len(res["data"]) == 0, res

    @pytest.mark.p1
    def test_delete_multiple_dialogs(self, WebApiAuth, add_dialogs_func):
        _, dialog_ids = add_dialogs_func

        res = list_dialogs(WebApiAuth)
        assert res["code"] == 0, res
        assert len(res["data"]) == 5, res

        payload = {"dialog_ids": dialog_ids}
        res = delete_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res
        assert res["data"] is True, res

        res = list_dialogs(WebApiAuth)
        assert res["code"] == 0, res
        assert len(res["data"]) == 0, res

    @pytest.mark.p1
    def test_delete_partial_dialogs(self, WebApiAuth, add_dialogs_func):
        _, dialog_ids = add_dialogs_func

        dialogs_to_delete = dialog_ids[:3]
        payload = {"dialog_ids": dialogs_to_delete}
        res = delete_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res
        assert res["data"] is True, res

        res = list_dialogs(WebApiAuth)
        assert res["code"] == 0, res
        assert len(res["data"]) == 2, res

        remaining_ids = [dialog["id"] for dialog in res["data"]]
        for dialog_id in dialog_ids[3:]:
            assert dialog_id in remaining_ids, res

    @pytest.mark.p2
    def test_delete_nonexistent_dialog(self, WebApiAuth):
        fake_dialog_id = "nonexistent_dialog_id"
        payload = {"dialog_ids": [fake_dialog_id]}
        res = delete_dialog(WebApiAuth, payload)
        assert res["code"] == 103, res
        assert "Only owner of dialog authorized for this operation." in res["message"], res

    @pytest.mark.p2
    def test_delete_empty_dialog_ids(self, WebApiAuth):
        payload = {"dialog_ids": []}
        res = delete_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res

    @pytest.mark.p2
    def test_delete_missing_dialog_ids(self, WebApiAuth):
        payload = {}
        res = delete_dialog(WebApiAuth, payload)
        assert res["code"] == 101, res
        assert res["message"] == "required argument are missing: dialog_ids; ", res

    @pytest.mark.p2
    def test_delete_invalid_dialog_ids_format(self, WebApiAuth):
        payload = {"dialog_ids": "not_a_list"}
        res = delete_dialog(WebApiAuth, payload)
        assert res["code"] == 103, res
        assert res["message"] == "Only owner of dialog authorized for this operation.", res

    @pytest.mark.p2
    def test_delete_mixed_valid_invalid_dialogs(self, WebApiAuth, add_dialog_func):
        _, valid_dialog_id = add_dialog_func
        invalid_dialog_id = "nonexistent_dialog_id"

        payload = {"dialog_ids": [valid_dialog_id, invalid_dialog_id]}
        res = delete_dialog(WebApiAuth, payload)
        assert res["code"] == 103, res
        assert res["message"] == "Only owner of dialog authorized for this operation.", res

        res = list_dialogs(WebApiAuth)
        assert res["code"] == 0, res
        assert len(res["data"]) == 1, res

    @pytest.mark.p3
    def test_delete_dialog_concurrent(self, WebApiAuth, add_dialogs_func):
        _, dialog_ids = add_dialogs_func

        count = len(dialog_ids)
        with ThreadPoolExecutor(max_workers=3) as executor:
            futures = [executor.submit(delete_dialog, WebApiAuth, {"dialog_ids": [dialog_id]}) for dialog_id in dialog_ids]

        responses = [future.result() for future in as_completed(futures)]

        successful_deletions = sum(1 for response in responses if response["code"] == 0)
        assert successful_deletions > 0, "No dialogs were successfully deleted"

        res = list_dialogs(WebApiAuth)
        assert res["code"] == 0, res
        assert len(res["data"]) == count - successful_deletions, res

    @pytest.mark.p3
    def test_delete_dialog_idempotent(self, WebApiAuth, add_dialog_func):
        _, dialog_id = add_dialog_func

        payload = {"dialog_ids": [dialog_id]}
        res = delete_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res

        res = delete_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res

    @pytest.mark.p3
    def test_delete_large_batch_dialogs(self, WebApiAuth, add_document):
        dataset_id, _ = add_document

        dialog_ids = batch_create_dialogs(WebApiAuth, 50, [dataset_id])
        assert len(dialog_ids) == 50, "Failed to create 50 dialogs"

        payload = {"dialog_ids": dialog_ids}
        res = delete_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res
        assert res["data"] is True, res

        res = list_dialogs(WebApiAuth)
        assert res["code"] == 0, res
        assert len(res["data"]) == 0, res

    @pytest.mark.p3
    def test_delete_dialog_with_special_characters(self, WebApiAuth):
        payload = {"name": "Dialog with 特殊字符 and émojis 🤖", "description": "Test dialog with special characters", "prompt_config": {"system": "You are a helpful assistant.", "parameters": []}}
        create_res = create_dialog(WebApiAuth, payload)
        assert create_res["code"] == 0, create_res
        dialog_id = create_res["data"]["id"]

        delete_payload = {"dialog_ids": [dialog_id]}
        res = delete_dialog(WebApiAuth, delete_payload)
        assert res["code"] == 0, res
        assert res["data"] is True, res

        res = list_dialogs(WebApiAuth)
        assert res["code"] == 0, res
        assert len(res["data"]) == 0, res

    @pytest.mark.p3
    def test_delete_dialog_preserves_other_user_dialogs(self, WebApiAuth, add_dialog_func):
        _, dialog_id = add_dialog_func

        payload = {"dialog_ids": [dialog_id]}
        res = delete_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_web_api/test_dialog_app/test_delete_dialogs.py` is located in the `test/testcases/test_web_api/test_dialog_app` directory.

This file is part of the **Testing** infrastructure.

### Architecture Context

Files in this location typically handle concerns related to test_dialog_app.

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
- [test_create_dialog.py](test_create_dialog.py_docs.md)
- [test_dialog_edge_cases.py](test_dialog_edge_cases.py_docs.md)
- [test_get_dialog.py](test_get_dialog.py_docs.md)
- [test_list_dialogs.py](test_list_dialogs.py_docs.md)
- [test_update_dialog.py](test_update_dialog.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
