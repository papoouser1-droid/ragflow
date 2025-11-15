# File Documentation: test/testcases/test_web_api/test_dialog_app/test_get_dialog.py

## File Metadata

- **Path**: `test/testcases/test_web_api/test_dialog_app/test_get_dialog.py`
- **Extension**: `.py`
- **Lines**: 178
- **Characters**: 6,959
- **Size**: 6,959 bytes
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
import pytest
from common import create_dialog, get_dialog
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
        res = get_dialog(invalid_auth, {"dialog_id": dialog_id})
        assert res["code"] == expected_code, res
        assert res["message"] == expected_message, res


class TestDialogGet:
    @pytest.mark.p1
    def test_get_existing_dialog(self, WebApiAuth, add_dialog_func):
        _, dialog_id = add_dialog_func
        res = get_dialog(WebApiAuth, {"dialog_id": dialog_id})
        assert res["code"] == 0, res
        data = res["data"]
        assert data["id"] == dialog_id, res
        assert "name" in data, res
        assert "description" in data, res
        assert "kb_ids" in data, res
        assert "kb_names" in data, res
        assert "prompt_config" in data, res
        assert "llm_setting" in data, res
        assert "top_n" in data, res
        assert "top_k" in data, res
        assert "similarity_threshold" in data, res
        assert "vector_similarity_weight" in data, res

    @pytest.mark.p1
    def test_get_dialog_with_kb_names(self, WebApiAuth, add_dialog_func):
        _, dialog_id = add_dialog_func
        res = get_dialog(WebApiAuth, {"dialog_id": dialog_id})
        assert res["code"] == 0, res
        data = res["data"]
        assert isinstance(data["kb_ids"], list), res
        assert isinstance(data["kb_names"], list), res
        assert len(data["kb_ids"]) == len(data["kb_names"]), res

    @pytest.mark.p2
    def test_get_nonexistent_dialog(self, WebApiAuth):
        fake_dialog_id = "nonexistent_dialog_id"
        res = get_dialog(WebApiAuth, {"dialog_id": fake_dialog_id})
        assert res["code"] == 102, res
        assert "Dialog not found" in res["message"], res

    @pytest.mark.p2
    def test_get_dialog_missing_id(self, WebApiAuth):
        res = get_dialog(WebApiAuth, {})
        assert res["code"] == 100, res
        assert res["message"] == "<BadRequestKeyError '400: Bad Request'>", res

    @pytest.mark.p2
    def test_get_dialog_empty_id(self, WebApiAuth):
        res = get_dialog(WebApiAuth, {"dialog_id": ""})
        assert res["code"] == 102, res

    @pytest.mark.p2
    def test_get_dialog_invalid_id_format(self, WebApiAuth):
        res = get_dialog(WebApiAuth, {"dialog_id": "invalid_format"})
        assert res["code"] == 102, res

    @pytest.mark.p3
    def test_get_dialog_data_structure(self, WebApiAuth, add_dialog_func):
        _, dialog_id = add_dialog_func
        res = get_dialog(WebApiAuth, {"dialog_id": dialog_id})
        assert res["code"] == 0, res
        data = res["data"]

        required_fields = [
            "id",
            "name",
            "description",
            "kb_ids",
            "kb_names",
            "prompt_config",
            "llm_setting",
            "top_n",
            "top_k",
            "similarity_threshold",
            "vector_similarity_weight",
            "create_time",
            "update_time",
        ]
        for field in required_fields:
            assert field in data, f"Missing field: {field}"

        assert isinstance(data["id"], str), res
        assert isinstance(data["name"], str), res
        assert isinstance(data["kb_ids"], list), res
        assert isinstance(data["kb_names"], list), res
        assert isinstance(data["prompt_config"], dict), res
        assert isinstance(data["top_n"], int), res
        assert isinstance(data["top_k"], int), res
        assert isinstance(data["similarity_threshold"], (int, float)), res
        assert isinstance(data["vector_similarity_weight"], (int, float)), res

    @pytest.mark.p3
    def test_get_dialog_prompt_config_structure(self, WebApiAuth, add_dialog_func):
        _, dialog_id = add_dialog_func
        res = get_dialog(WebApiAuth, {"dialog_id": dialog_id})
        assert res["code"] == 0, res

        prompt_config = res["data"]["prompt_config"]
        assert "system" in prompt_config, res
        assert "parameters" in prompt_config, res
        assert isinstance(prompt_config["system"], str), res
        assert isinstance(prompt_config["parameters"], list), res

    @pytest.mark.p3
    def test_get_dialog_with_multiple_kbs(self, WebApiAuth, add_dataset_func):
        dataset_id1 = add_dataset_func
        dataset_id2 = add_dataset_func

        payload = {
            "name": "multi_kb_dialog",
            "kb_ids": [dataset_id1, dataset_id2],
            "prompt_config": {"system": "You are a helpful assistant with knowledge: {knowledge}", "parameters": [{"key": "knowledge", "optional": True}]},
        }
        create_res = create_dialog(WebApiAuth, payload)
        assert create_res["code"] == 0, create_res
        dialog_id = create_res["data"]["id"]

        res = get_dialog(WebApiAuth, {"dialog_id": dialog_id})
        assert res["code"] == 0, res
        data = res["data"]
        assert len(data["kb_ids"]) == 2, res
        assert len(data["kb_names"]) == 2, res
        assert dataset_id1 in data["kb_ids"], res
        assert dataset_id2 in data["kb_ids"], res

    @pytest.mark.p3
    def test_get_dialog_with_invalid_kb(self, WebApiAuth):
        payload = {
            "name": "invalid_kb_dialog",
            "kb_ids": ["invalid_kb_id"],
            "prompt_config": {"system": "You are a helpful assistant with knowledge: {knowledge}", "parameters": [{"key": "knowledge", "optional": True}]},
        }
        create_res = create_dialog(WebApiAuth, payload)
        assert create_res["code"] == 0, create_res
        dialog_id = create_res["data"]["id"]

        res = get_dialog(WebApiAuth, {"dialog_id": dialog_id})
        assert res["code"] == 0, res
        data = res["data"]

        assert len(data["kb_ids"]) == 0, res
        assert len(data["kb_names"]) == 0, res

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

### Classes (2)

- `TestAuthorization`: Class definition
- `TestDialogGet`: Class definition

### Imports (4)

- `import pytest`
- `from common import create_dialog, get_dialog`
- `from configs import INVALID_API_TOKEN`
- `from libs.auth import RAGFlowWebApiAuth`

## Code Structure Analysis

- Total lines: 178
- Blank lines: 21 (11.8%)
- Comment lines: ~15 (8.4%)
- Code lines: ~142


## Dependencies and Imports

- `import pytest`
- `from common import create_dialog, get_dialog`
- `from configs import INVALID_API_TOKEN`
- `from libs.auth import RAGFlowWebApiAuth`

## Design & Architecture

This file is located in the `test` directory, specifically within `test/testcases/test_web_api/test_dialog_app`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 2 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `test/testcases/test_web_api/test_dialog_app/` directory
- Imports from `common`
- Imports from `configs`
- Imports from `libs.auth`

## Keywords

ANY, All, Apache, Authors, BASIS, Bad, BadRequestKeyError, CONDITIONS, Copyright, Dialog, INVALID_API_TOKEN, InfiniFlow, KIND, LICENSE, License, Licensed, Missing, None, Python, RAGFlowWebApiAuth, Request, Reserved, Rights, See, TestAuthorization, TestDialogGet, The, True, Unauthorized, Unless, Version, WARRANTIES, WITHOUT, WebApiAuth, You, pytest, test_auth_invalid, test_get_dialog_data_structure, test_get_dialog_empty_id, test_get_dialog_invalid_id_format, test_get_dialog_missing_id, test_get_dialog_prompt_config_structure, test_get_dialog_with_invalid_kb, test_get_dialog_with_kb_names, test_get_dialog_with_multiple_kbs, test_get_existing_dialog, test_get_nonexistent_dialog

---
*Generated by RAGFlow Repository Documentation Generator*
