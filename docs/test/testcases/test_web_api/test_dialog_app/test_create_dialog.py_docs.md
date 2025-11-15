# Documentation: test/testcases/test_web_api/test_dialog_app/test_create_dialog.py

## File Metadata

- **Path**: `test/testcases/test_web_api/test_dialog_app/test_create_dialog.py`
- **Size**: 7734 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_web_api/test_dialog_app/test_create_dialog.py`.

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

#### Class: `TestCapability` (line 45)

**Methods**: test_create_dialog_100, test_create_dialog_concurrent

#### Class: `TestDialogCreate` (line 64)

**Methods**: test_name, test_name_invalid, test_prompt_config_required, test_prompt_config_with_knowledge_no_kb, test_prompt_config_parameter_not_used, test_create_with_kb_ids, test_create_with_all_parameters, test_name_duplicated, test_optional_parameters

### Functions Defined

This file defines 12 function(s):

#### Function: `test_auth_invalid` (line 37)

**Parameters**: self, invalid_auth, expected_code, expected_message

#### Function: `test_create_dialog_100` (line 47)

**Parameters**: self, WebApiAuth

#### Function: `test_create_dialog_concurrent` (line 54)

**Parameters**: self, WebApiAuth

#### Function: `test_name` (line 69)

**Parameters**: self, WebApiAuth, name

#### Function: `test_name_invalid` (line 86)

**Parameters**: self, WebApiAuth, name, expected_code, expected_message

#### Function: `test_prompt_config_required` (line 93)

**Parameters**: self, WebApiAuth

#### Function: `test_prompt_config_with_knowledge_no_kb` (line 100)

**Parameters**: self, WebApiAuth

#### Function: `test_prompt_config_parameter_not_used` (line 107)

**Parameters**: self, WebApiAuth

#### Function: `test_create_with_kb_ids` (line 114)

**Parameters**: self, WebApiAuth, add_dataset_func

#### Function: `test_create_with_all_parameters` (line 126)

**Parameters**: self, WebApiAuth, add_dataset_func

#### Function: `test_name_duplicated` (line 154)

**Parameters**: self, WebApiAuth

#### Function: `test_optional_parameters` (line 164)

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
from common import create_dialog
from configs import CHAT_ASSISTANT_NAME_LIMIT, INVALID_API_TOKEN
from hypothesis import example, given, settings
from libs.auth import RAGFlowWebApiAuth
from utils.hypothesis_utils import valid_names


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
    def test_auth_invalid(self, invalid_auth, expected_code, expected_message):
        payload = {"name": "auth_test", "prompt_config": {"system": "You are a helpful assistant.", "parameters": []}}
        res = create_dialog(invalid_auth, payload)
        assert res["code"] == expected_code, res
        assert res["message"] == expected_message, res


@pytest.mark.usefixtures("clear_dialogs")
class TestCapability:
    @pytest.mark.p3
    def test_create_dialog_100(self, WebApiAuth):
        for i in range(100):
            payload = {"name": f"dialog_{i}", "prompt_config": {"system": "You are a helpful assistant.", "parameters": []}}
            res = create_dialog(WebApiAuth, payload)
            assert res["code"] == 0, f"Failed to create dialog {i}"

    @pytest.mark.p3
    def test_create_dialog_concurrent(self, WebApiAuth):
        count = 100
        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [executor.submit(create_dialog, WebApiAuth, {"name": f"dialog_{i}", "prompt_config": {"system": "You are a helpful assistant.", "parameters": []}}) for i in range(count)]
        responses = list(as_completed(futures))
        assert len(responses) == count, responses
        assert all(future.result()["code"] == 0 for future in futures)


@pytest.mark.usefixtures("clear_dialogs")
class TestDialogCreate:
    @pytest.mark.p1
    @given(name=valid_names())
    @example("a" * CHAT_ASSISTANT_NAME_LIMIT)
    @settings(max_examples=20)
    def test_name(self, WebApiAuth, name):
        payload = {"name": name, "prompt_config": {"system": "You are a helpful assistant.", "parameters": []}}
        res = create_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "name, expected_code, expected_message",
        [
            ("", 102, "Dialog name can't be empty."),
            (" ", 102, "Dialog name can't be empty."),
            ("a" * (CHAT_ASSISTANT_NAME_LIMIT + 1), 102, "Dialog name length is 256 which is larger than 255"),
            (0, 102, "Dialog name must be string."),
            (None, 102, "Dialog name must be string."),
        ],
        ids=["empty_name", "space_name", "too_long_name", "invalid_name", "None_name"],
    )
    def test_name_invalid(self, WebApiAuth, name, expected_code, expected_message):
        payload = {"name": name, "prompt_config": {"system": "You are a helpful assistant.", "parameters": []}}
        res = create_dialog(WebApiAuth, payload)
        assert res["code"] == expected_code, res
        assert res["message"] == expected_message, res

    @pytest.mark.p1
    def test_prompt_config_required(self, WebApiAuth):
        payload = {"name": "test_dialog"}
        res = create_dialog(WebApiAuth, payload)
        assert res["code"] == 101, res
        assert res["message"] == "required argument are missing: prompt_config; ", res

    @pytest.mark.p1
    def test_prompt_config_with_knowledge_no_kb(self, WebApiAuth):
        payload = {"name": "test_dialog", "prompt_config": {"system": "You are a helpful assistant. Use this knowledge: {knowledge}", "parameters": [{"key": "knowledge", "optional": True}]}}
        res = create_dialog(WebApiAuth, payload)
        assert res["code"] == 102, res
        assert "Please remove `{knowledge}` in system prompt" in res["message"], res

    @pytest.mark.p1
    def test_prompt_config_parameter_not_used(self, WebApiAuth):
        payload = {"name": "test_dialog", "prompt_config": {"system": "You are a helpful assistant.", "parameters": [{"key": "unused_param", "optional": False}]}}
        res = create_dialog(WebApiAuth, payload)
        assert res["code"] == 102, res
        assert "Parameter 'unused_param' is not used" in res["message"], res

    @pytest.mark.p1
    def test_create_with_kb_ids(self, WebApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        payload = {
            "name": "test_dialog_with_kb",
            "kb_ids": [dataset_id],
            "prompt_config": {"system": "You are a helpful assistant. Use this knowledge: {knowledge}", "parameters": [{"key": "knowledge", "optional": True}]},
        }
        res = create_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res
        assert res["data"]["kb_ids"] == [dataset_id], res

    @pytest.mark.p2
    def test_create_with_all_parameters(self, WebApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        payload = {
            "name": "comprehensive_dialog",
            "description": "A comprehensive test dialog",
            "icon": "🤖",
            "kb_ids": [dataset_id],
            "top_n": 10,
            "top_k": 2048,
            "rerank_id": "",
            "similarity_threshold": 0.2,
            "vector_similarity_weight": 0.5,
            "llm_setting": {"model": "gpt-4", "temperature": 0.8, "max_tokens": 1000},
            "prompt_config": {"system": "You are a helpful assistant. Use this knowledge: {knowledge}", "parameters": [{"key": "knowledge", "optional": True}]},
        }
        res = create_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res
        data = res["data"]
        assert data["name"] == "comprehensive_dialog", res
        assert data["description"] == "A comprehensive test dialog", res
        assert data["icon"] == "🤖", res
        assert data["kb_ids"] == [dataset_id], res
        assert data["top_n"] == 10, res
        assert data["top_k"] == 2048, res
        assert data["similarity_threshold"] == 0.2, res
        assert data["vector_similarity_weight"] == 0.5, res

    @pytest.mark.p3
    def test_name_duplicated(self, WebApiAuth):
        name = "duplicated_dialog"
        payload = {"name": name, "prompt_config": {"system": "You are a helpful assistant.", "parameters": []}}
        res = create_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res

        res = create_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res

    @pytest.mark.p2
    def test_optional_parameters(self, WebApiAuth):
        payload = {
            "name": "test_optional_params",
            "prompt_config": {"system": "You are a helpful assistant. Optional param: {optional_param}", "parameters": [{"key": "optional_param", "optional": True}]},
        }
        res = create_dialog(WebApiAuth, payload)
        assert res["code"] == 0, res

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_web_api/test_dialog_app/test_create_dialog.py` is located in the `test/testcases/test_web_api/test_dialog_app` directory.

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
- [test_delete_dialogs.py](test_delete_dialogs.py_docs.md)
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
