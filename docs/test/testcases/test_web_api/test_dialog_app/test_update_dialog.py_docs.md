# Documentation: test/testcases/test_web_api/test_dialog_app/test_update_dialog.py

## File Metadata

- **Path**: `test/testcases/test_web_api/test_dialog_app/test_update_dialog.py`
- **Size**: 8144 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_web_api/test_dialog_app/test_update_dialog.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `pytest`
- `common`
- `configs`
- `libs.auth`

### Classes Defined

This file defines 2 class(es):

#### Class: `TestAuthorization` (line 23)

**Methods**: test_auth_invalid

#### Class: `TestDialogUpdate` (line 41)

**Methods**: test_update_name, test_update_description, test_update_prompt_config, test_update_kb_ids, test_update_llm_settings, test_update_retrieval_settings, test_update_nonexistent_dialog, test_update_with_invalid_prompt_config, test_update_with_knowledge_but_no_kb, test_update_icon, test_update_rerank_id, test_update_multiple_fields

### Functions Defined

This file defines 13 function(s):

#### Function: `test_auth_invalid` (line 33)

**Parameters**: self, invalid_auth, expected_code, expected_message, add_dialog_func

#### Function: `test_update_name` (line 43)

**Parameters**: self, WebApiAuth, add_dialog_func

#### Function: `test_update_description` (line 52)

**Parameters**: self, WebApiAuth, add_dialog_func

#### Function: `test_update_prompt_config` (line 61)

**Parameters**: self, WebApiAuth, add_dialog_func

#### Function: `test_update_kb_ids` (line 70)

**Parameters**: self, WebApiAuth, add_dialog_func, add_dataset_func

#### Function: `test_update_llm_settings` (line 83)

**Parameters**: self, WebApiAuth, add_dialog_func

#### Function: `test_update_retrieval_settings` (line 93)

**Parameters**: self, WebApiAuth, add_dialog_func

#### Function: `test_update_nonexistent_dialog` (line 111)

**Parameters**: self, WebApiAuth

#### Function: `test_update_with_invalid_prompt_config` (line 119)

**Parameters**: self, WebApiAuth, add_dialog_func

#### Function: `test_update_with_knowledge_but_no_kb` (line 127)

**Parameters**: self, WebApiAuth, add_dialog_func

#### Function: `test_update_icon` (line 135)

**Parameters**: self, WebApiAuth, add_dialog_func

#### Function: `test_update_rerank_id` (line 144)

**Parameters**: self, WebApiAuth, add_dialog_func

#### Function: `test_update_multiple_fields` (line 152)

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
import pytest
from common import update_dialog
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
        payload = {"dialog_id": dialog_id, "name": "updated_name", "prompt_config": {"system": "You are a helpful assistant.", "parameters": []}}
        res = update_dialog(invalid_auth, payload)
        assert res["code"] == expected_code, res
        assert res["message"] == expected_message, res


class TestDialogUpdate:
    @pytest.mark.p1
    def test_update_name(self, WebApiAuth, add_dialog_func):
        _, dialog_id = add_dialog_func
        new_name = "updated_dialog_name"
        payload = {"dialog_id": dialog_id, "name": new_name, "prompt_config": {"system": "You are a helpful assistant.", "parameters": []}}
        res = update_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res
        assert res["data"]["name"] == new_name, res

    @pytest.mark.p1
    def test_update_description(self, WebApiAuth, add_dialog_func):
        _, dialog_id = add_dialog_func
        new_description = "Updated description"
        payload = {"dialog_id": dialog_id, "description": new_description, "prompt_config": {"system": "You are a helpful assistant.", "parameters": []}}
        res = update_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res
        assert res["data"]["description"] == new_description, res

    @pytest.mark.p1
    def test_update_prompt_config(self, WebApiAuth, add_dialog_func):
        _, dialog_id = add_dialog_func
        new_prompt_config = {"system": "You are an updated helpful assistant with {param1}.", "parameters": [{"key": "param1", "optional": False}]}
        payload = {"dialog_id": dialog_id, "prompt_config": new_prompt_config}
        res = update_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res
        assert res["data"]["prompt_config"]["system"] == new_prompt_config["system"], res

    @pytest.mark.p1
    def test_update_kb_ids(self, WebApiAuth, add_dialog_func, add_dataset_func):
        _, dialog_id = add_dialog_func
        new_dataset_id = add_dataset_func
        payload = {
            "dialog_id": dialog_id,
            "kb_ids": [new_dataset_id],
            "prompt_config": {"system": "You are a helpful assistant with knowledge: {knowledge}", "parameters": [{"key": "knowledge", "optional": True}]},
        }
        res = update_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res
        assert new_dataset_id in res["data"]["kb_ids"], res

    @pytest.mark.p1
    def test_update_llm_settings(self, WebApiAuth, add_dialog_func):
        _, dialog_id = add_dialog_func
        new_llm_setting = {"model": "gpt-4", "temperature": 0.9, "max_tokens": 2000}
        payload = {"dialog_id": dialog_id, "llm_setting": new_llm_setting, "prompt_config": {"system": "You are a helpful assistant.", "parameters": []}}
        res = update_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res
        assert res["data"]["llm_setting"]["model"] == "gpt-4", res
        assert res["data"]["llm_setting"]["temperature"] == 0.9, res

    @pytest.mark.p1
    def test_update_retrieval_settings(self, WebApiAuth, add_dialog_func):
        _, dialog_id = add_dialog_func
        payload = {
            "dialog_id": dialog_id,
            "top_n": 15,
            "top_k": 4096,
            "similarity_threshold": 0.3,
            "vector_similarity_weight": 0.7,
            "prompt_config": {"system": "You are a helpful assistant.", "parameters": []},
        }
        res = update_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res
        assert res["data"]["top_n"] == 15, res
        assert res["data"]["top_k"] == 4096, res
        assert res["data"]["similarity_threshold"] == 0.3, res
        assert res["data"]["vector_similarity_weight"] == 0.7, res

    @pytest.mark.p2
    def test_update_nonexistent_dialog(self, WebApiAuth):
        fake_dialog_id = "nonexistent_dialog_id"
        payload = {"dialog_id": fake_dialog_id, "name": "updated_name", "prompt_config": {"system": "You are a helpful assistant.", "parameters": []}}
        res = update_dialog(WebApiAuth, payload)
        assert res["code"] == 102, res
        assert "Dialog not found" in res["message"], res

    @pytest.mark.p2
    def test_update_with_invalid_prompt_config(self, WebApiAuth, add_dialog_func):
        _, dialog_id = add_dialog_func
        payload = {"dialog_id": dialog_id, "prompt_config": {"system": "You are a helpful assistant.", "parameters": [{"key": "unused_param", "optional": False}]}}
        res = update_dialog(WebApiAuth, payload)
        assert res["code"] == 102, res
        assert "Parameter 'unused_param' is not used" in res["message"], res

    @pytest.mark.p2
    def test_update_with_knowledge_but_no_kb(self, WebApiAuth, add_dialog_func):
        _, dialog_id = add_dialog_func
        payload = {"dialog_id": dialog_id, "kb_ids": [], "prompt_config": {"system": "You are a helpful assistant with knowledge: {knowledge}", "parameters": [{"key": "knowledge", "optional": True}]}}
        res = update_dialog(WebApiAuth, payload)
        assert res["code"] == 102, res
        assert "Please remove `{knowledge}` in system prompt" in res["message"], res

    @pytest.mark.p2
    def test_update_icon(self, WebApiAuth, add_dialog_func):
        _, dialog_id = add_dialog_func
        new_icon = "🚀"
        payload = {"dialog_id": dialog_id, "icon": new_icon, "prompt_config": {"system": "You are a helpful assistant.", "parameters": []}}
        res = update_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res
        assert res["data"]["icon"] == new_icon, res

    @pytest.mark.p2
    def test_update_rerank_id(self, WebApiAuth, add_dialog_func):
        _, dialog_id = add_dialog_func
        payload = {"dialog_id": dialog_id, "rerank_id": "test_rerank_model", "prompt_config": {"system": "You are a helpful assistant.", "parameters": []}}
        res = update_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res
        assert res["data"]["rerank_id"] == "test_rerank_model", res

    @pytest.mark.p3
    def test_update_multiple_fields(self, WebApiAuth, add_dialog_func):
        _, dialog_id = add_dialog_func
        payload = {
            "dialog_id": dialog_id,
            "name": "multi_update_dialog",
            "description": "Updated with multiple fields",
            "icon": "🔄",
            "top_n": 20,
            "similarity_threshold": 0.4,
            "prompt_config": {"system": "You are a multi-updated assistant.", "parameters": []},
        }
        res = update_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res
        data = res["data"]
        assert data["name"] == "multi_update_dialog", res
        assert data["description"] == "Updated with multiple fields", res
        assert data["icon"] == "🔄", res
        assert data["top_n"] == 20, res
        assert data["similarity_threshold"] == 0.4, res

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_web_api/test_dialog_app/test_update_dialog.py` is located in the `test/testcases/test_web_api/test_dialog_app` directory.

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
- [test_delete_dialogs.py](test_delete_dialogs.py_docs.md)
- [test_dialog_edge_cases.py](test_dialog_edge_cases.py_docs.md)
- [test_get_dialog.py](test_get_dialog.py_docs.md)
- [test_list_dialogs.py](test_list_dialogs.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
