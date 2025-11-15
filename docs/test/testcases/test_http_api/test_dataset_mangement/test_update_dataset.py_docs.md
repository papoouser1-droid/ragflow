# Documentation: test/testcases/test_http_api/test_dataset_mangement/test_update_dataset.py

## File Metadata

- **Path**: `test/testcases/test_http_api/test_dataset_mangement/test_update_dataset.py`
- **Size**: 36363 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_http_api/test_dataset_mangement/test_update_dataset.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `os`
- `uuid`
- `concurrent.futures`
- `pytest`
- `common`
- `configs`
- `hypothesis`
- `libs.auth`
- `utils`
- `utils.file_utils`
- `utils.hypothesis_utils`
- `configs`

### Classes Defined

This file defines 4 class(es):

#### Class: `TestAuthorization` (line 32)

**Methods**: test_auth_invalid

#### Class: `TestRquest` (line 52)

**Methods**: test_bad_content_type, test_payload_bad, test_payload_empty, test_payload_unset

#### Class: `TestCapability` (line 91)

**Methods**: test_update_dateset_concurrent

#### Class: `TestDatasetUpdate` (line 103)

**Methods**: test_dataset_id_not_uuid, test_dataset_id_not_uuid1, test_dataset_id_wrong_uuid, test_name, test_name_invalid, test_name_duplicated, test_name_case_insensitive, test_avatar, test_avatar_exceeds_limit_length, test_avatar_invalid_prefix, test_avatar_none, test_description, test_description_exceeds_limit_length, test_description_none, test_embedding_model, test_embedding_model_invalid, test_embedding_model_format, test_embedding_model_none, test_permission, test_permission_invalid, test_permission_none, test_chunk_method, test_chunk_method_invalid, test_chunk_method_none, test_pagerank, test_pagerank_set_to_0, test_pagerank_infinity, test_pagerank_invalid, test_pagerank_none, test_parser_config, test_parser_config_invalid, test_parser_config_empty, test_parser_config_none, test_parser_config_empty_with_chunk_method_change, test_parser_config_unset_with_chunk_method_change, test_parser_config_none_with_chunk_method_change, test_field_unsupported, test_field_unset

### Functions Defined

This file defines 44 function(s):

#### Function: `test_auth_invalid` (line 46)

**Parameters**: self, invalid_auth, expected_code, expected_message

#### Function: `test_bad_content_type` (line 54)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_payload_bad` (line 70)

**Parameters**: self, HttpApiAuth, add_dataset_func, payload, expected_message

#### Function: `test_payload_empty` (line 77)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_payload_unset` (line 84)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_update_dateset_concurrent` (line 93)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_dataset_id_not_uuid` (line 105)

**Parameters**: self, HttpApiAuth

#### Function: `test_dataset_id_not_uuid1` (line 112)

**Parameters**: self, HttpApiAuth

#### Function: `test_dataset_id_wrong_uuid` (line 119)

**Parameters**: self, HttpApiAuth

#### Function: `test_name` (line 129)

**Parameters**: self, HttpApiAuth, add_dataset_func, name

#### Function: `test_name_invalid` (line 151)

**Parameters**: self, HttpApiAuth, add_dataset_func, name, expected_message

#### Function: `test_name_duplicated` (line 159)

**Parameters**: self, HttpApiAuth, add_datasets_func

#### Function: `test_name_case_insensitive` (line 168)

**Parameters**: self, HttpApiAuth, add_datasets_func

#### Function: `test_avatar` (line 177)

**Parameters**: self, HttpApiAuth, add_dataset_func, tmp_path

#### Function: `test_avatar_exceeds_limit_length` (line 191)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_avatar_invalid_prefix` (line 209)

**Parameters**: self, HttpApiAuth, add_dataset_func, tmp_path, avatar_prefix, expected_message

#### Function: `test_avatar_none` (line 218)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_description` (line 229)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_description_exceeds_limit_length` (line 240)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_description_none` (line 248)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_embedding_model` (line 267)

**Parameters**: self, HttpApiAuth, add_dataset_func, embedding_model

#### Function: `test_embedding_model_invalid` (line 288)

**Parameters**: self, HttpApiAuth, add_dataset_func, name, embedding_model

#### Function: `test_embedding_model_format` (line 312)

**Parameters**: self, HttpApiAuth, add_dataset_func, name, embedding_model

#### Function: `test_embedding_model_none` (line 323)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_permission` (line 342)

**Parameters**: self, HttpApiAuth, add_dataset_func, permission

#### Function: `test_permission_invalid` (line 365)

**Parameters**: self, HttpApiAuth, add_dataset_func, permission

#### Function: `test_permission_none` (line 373)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_chunk_method` (line 399)

**Parameters**: self, HttpApiAuth, add_dataset_func, chunk_method

#### Function: `test_chunk_method_invalid` (line 419)

**Parameters**: self, HttpApiAuth, add_dataset_func, chunk_method

#### Function: `test_chunk_method_none` (line 427)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_pagerank` (line 437)

**Parameters**: self, HttpApiAuth, add_dataset_func, pagerank

#### Function: `test_pagerank_set_to_0` (line 449)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_pagerank_infinity` (line 469)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_pagerank_invalid` (line 485)

**Parameters**: self, HttpApiAuth, add_dataset_func, pagerank, expected_message

#### Function: `test_pagerank_none` (line 493)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_parser_config` (line 604)

**Parameters**: self, HttpApiAuth, add_dataset_func, parser_config

#### Function: `test_parser_config_invalid` (line 735)

**Parameters**: self, HttpApiAuth, add_dataset_func, parser_config, expected_message

#### Function: `test_parser_config_empty` (line 743)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_parser_config_none` (line 754)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_parser_config_empty_with_chunk_method_change` (line 765)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_parser_config_unset_with_chunk_method_change` (line 776)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_parser_config_none_with_chunk_method_change` (line 787)

**Parameters**: self, HttpApiAuth, add_dataset_func

#### Function: `test_field_unsupported` (line 815)

**Parameters**: self, HttpApiAuth, add_dataset_func, payload

#### Function: `test_field_unset` (line 822)

**Parameters**: self, HttpApiAuth, add_dataset_func

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
import os
import uuid
from concurrent.futures import ThreadPoolExecutor, as_completed

import pytest
from common import list_datasets, update_dataset
from configs import DATASET_NAME_LIMIT, INVALID_API_TOKEN
from hypothesis import HealthCheck, example, given, settings
from libs.auth import RAGFlowHttpApiAuth
from utils import encode_avatar
from utils.file_utils import create_image_file
from utils.hypothesis_utils import valid_names
from configs import DEFAULT_PARSER_CONFIG
# TODO: Missing scenario for updating embedding_model with chunk_count != 0


class TestAuthorization:
    @pytest.mark.p1
    @pytest.mark.parametrize(
        "invalid_auth, expected_code, expected_message",
        [
            (None, 0, "`Authorization` can't be empty"),
            (
                RAGFlowHttpApiAuth(INVALID_API_TOKEN),
                109,
                "Authentication error: API key is invalid!",
            ),
        ],
        ids=["empty_auth", "invalid_api_token"],
    )
    def test_auth_invalid(self, invalid_auth, expected_code, expected_message):
        res = update_dataset(invalid_auth, "dataset_id")
        assert res["code"] == expected_code, res
        assert res["message"] == expected_message, res


class TestRquest:
    @pytest.mark.p3
    def test_bad_content_type(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        BAD_CONTENT_TYPE = "text/xml"
        res = update_dataset(HttpApiAuth, dataset_id, {"name": "bad_content_type"}, headers={"Content-Type": BAD_CONTENT_TYPE})
        assert res["code"] == 101, res
        assert res["message"] == f"Unsupported content type: Expected application/json, got {BAD_CONTENT_TYPE}", res

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "payload, expected_message",
        [
            ("a", "Malformed JSON syntax: Missing commas/brackets or invalid encoding"),
            ('"a"', "Invalid request payload: expected object, got str"),
        ],
        ids=["malformed_json_syntax", "invalid_request_payload_type"],
    )
    def test_payload_bad(self, HttpApiAuth, add_dataset_func, payload, expected_message):
        dataset_id = add_dataset_func
        res = update_dataset(HttpApiAuth, dataset_id, data=payload)
        assert res["code"] == 101, res
        assert res["message"] == expected_message, res

    @pytest.mark.p2
    def test_payload_empty(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        res = update_dataset(HttpApiAuth, dataset_id, {})
        assert res["code"] == 101, res
        assert res["message"] == "No properties were modified", res

    @pytest.mark.p3
    def test_payload_unset(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        res = update_dataset(HttpApiAuth, dataset_id, None)
        assert res["code"] == 101, res
        assert res["message"] == "Malformed JSON syntax: Missing commas/brackets or invalid encoding", res


class TestCapability:
    @pytest.mark.p3
    def test_update_dateset_concurrent(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        count = 100
        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [executor.submit(update_dataset, HttpApiAuth, dataset_id, {"name": f"dataset_{i}"}) for i in range(count)]
        responses = list(as_completed(futures))
        assert len(responses) == count, responses
        assert all(future.result()["code"] == 0 for future in futures)


class TestDatasetUpdate:
    @pytest.mark.p3
    def test_dataset_id_not_uuid(self, HttpApiAuth):
        payload = {"name": "not uuid"}
        res = update_dataset(HttpApiAuth, "not_uuid", payload)
        assert res["code"] == 101, res
        assert "Invalid UUID1 format" in res["message"], res

    @pytest.mark.p3
    def test_dataset_id_not_uuid1(self, HttpApiAuth):
        payload = {"name": "not uuid1"}
        res = update_dataset(HttpApiAuth, uuid.uuid4().hex, payload)
        assert res["code"] == 101, res
        assert "Invalid UUID1 format" in res["message"], res

    @pytest.mark.p3
    def test_dataset_id_wrong_uuid(self, HttpApiAuth):
        payload = {"name": "wrong uuid"}
        res = update_dataset(HttpApiAuth, "d94a8dc02c9711f0930f7fbc369eab6d", payload)
        assert res["code"] == 108, res
        assert "lacks permission for dataset" in res["message"], res

    @pytest.mark.p1
    @given(name=valid_names())
    @example("a" * 128)
    @settings(max_examples=20, suppress_health_check=[HealthCheck.function_scoped_fixture])
    def test_name(self, HttpApiAuth, add_dataset_func, name):
        dataset_id = add_dataset_func
        payload = {"name": name}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 0, res

        res = list_datasets(HttpApiAuth)
        assert res["code"] == 0, res
        assert res["data"][0]["name"] == name, res

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "name, expected_message",
        [
            ("", "String should have at least 1 character"),
            (" ", "String should have at least 1 character"),
            ("a" * (DATASET_NAME_LIMIT + 1), "String should have at most 128 characters"),
            (0, "Input should be a valid string"),
            (None, "Input should be a valid string"),
        ],
        ids=["empty_name", "space_name", "too_long_name", "invalid_name", "None_name"],
    )
    def test_name_invalid(self, HttpApiAuth, add_dataset_func, name, expected_message):
        dataset_id = add_dataset_func
        payload = {"name": name}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 101, res
        assert expected_message in res["message"], res

    @pytest.mark.p3
    def test_name_duplicated(self, HttpApiAuth, add_datasets_func):
        dataset_id = add_datasets_func[0]
        name = "dataset_1"
        payload = {"name": name}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 102, res
        assert res["message"] == f"Dataset name '{name}' already exists", res

    @pytest.mark.p3
    def test_name_case_insensitive(self, HttpApiAuth, add_datasets_func):
        dataset_id = add_datasets_func[0]
        name = "DATASET_1"
        payload = {"name": name}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 102, res
        assert res["message"] == f"Dataset name '{name}' already exists", res

    @pytest.mark.p2
    def test_avatar(self, HttpApiAuth, add_dataset_func, tmp_path):
        dataset_id = add_dataset_func
        fn = create_image_file(tmp_path / "ragflow_test.png")
        payload = {
            "avatar": f"data:image/png;base64,{encode_avatar(fn)}",
        }
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 0, res

        res = list_datasets(HttpApiAuth)
        assert res["code"] == 0, res
        assert res["data"][0]["avatar"] == f"data:image/png;base64,{encode_avatar(fn)}", res

    @pytest.mark.p2
    def test_avatar_exceeds_limit_length(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        payload = {"avatar": "a" * 65536}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 101, res
        assert "String should have at most 65535 characters" in res["message"], res

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "avatar_prefix, expected_message",
        [
            ("", "Missing MIME prefix. Expected format: data:<mime>;base64,<data>"),
            ("data:image/png;base64", "Missing MIME prefix. Expected format: data:<mime>;base64,<data>"),
            ("invalid_mine_prefix:image/png;base64,", "Invalid MIME prefix format. Must start with 'data:'"),
            ("data:unsupported_mine_type;base64,", "Unsupported MIME type. Allowed: ['image/jpeg', 'image/png']"),
        ],
        ids=["empty_prefix", "missing_comma", "unsupported_mine_type", "invalid_mine_type"],
    )
    def test_avatar_invalid_prefix(self, HttpApiAuth, add_dataset_func, tmp_path, avatar_prefix, expected_message):
        dataset_id = add_dataset_func
        fn = create_image_file(tmp_path / "ragflow_test.png")
        payload = {"avatar": f"{avatar_prefix}{encode_avatar(fn)}"}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 101, res
        assert expected_message in res["message"], res

    @pytest.mark.p3
    def test_avatar_none(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        payload = {"avatar": None}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 0, res

        res = list_datasets(HttpApiAuth)
        assert res["code"] == 0, res
        assert res["data"][0]["avatar"] is None, res

    @pytest.mark.p2
    def test_description(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        payload = {"description": "description"}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 0

        res = list_datasets(HttpApiAuth, {"id": dataset_id})
        assert res["code"] == 0, res
        assert res["data"][0]["description"] == "description"

    @pytest.mark.p2
    def test_description_exceeds_limit_length(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        payload = {"description": "a" * 65536}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 101, res
        assert "String should have at most 65535 characters" in res["message"], res

    @pytest.mark.p3
    def test_description_none(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        payload = {"description": None}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 0, res

        res = list_datasets(HttpApiAuth, {"id": dataset_id})
        assert res["code"] == 0, res
        assert res["data"][0]["description"] is None

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "embedding_model",
        [
            "BAAI/bge-small-en-v1.5@Builtin",
            "embedding-3@ZHIPU-AI",
        ],
        ids=["builtin_baai", "tenant_zhipu"],
    )
    def test_embedding_model(self, HttpApiAuth, add_dataset_func, embedding_model):
        dataset_id = add_dataset_func
        payload = {"embedding_model": embedding_model}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 0, res

        res = list_datasets(HttpApiAuth)
        assert res["code"] == 0, res
        assert res["data"][0]["embedding_model"] == embedding_model, res

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "name, embedding_model",
        [
            ("unknown_llm_name", "unknown@ZHIPU-AI"),
            ("unknown_llm_factory", "embedding-3@unknown"),
            ("tenant_no_auth_default_tenant_llm", "text-embedding-v3@Tongyi-Qianwen"),
            ("tenant_no_auth", "text-embedding-3-small@OpenAI"),
        ],
        ids=["unknown_llm_name", "unknown_llm_factory", "tenant_no_auth_default_tenant_llm", "tenant_no_auth"],
    )
    def test_embedding_model_invalid(self, HttpApiAuth, add_dataset_func, name, embedding_model):
        dataset_id = add_dataset_func
        payload = {"name": name, "embedding_model": embedding_model}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 101, res
        if "tenant_no_auth" in name:
            assert res["message"] == f"Unauthorized model: <{embedding_model}>", res
        else:
            assert res["message"] == f"Unsupported model: <{embedding_model}>", res

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "name, embedding_model",
        [
            ("empty", ""),
            ("space", " "),
            ("missing_at", "BAAI/bge-small-en-v1.5Builtin"),
            ("missing_model_name", "@Builtin"),
            ("missing_provider", "BAAI/bge-small-en-v1.5@"),
            ("whitespace_only_model_name", " @Builtin"),
            ("whitespace_only_provider", "BAAI/bge-small-en-v1.5@ "),
        ],
        ids=["empty", "space", "missing_at", "empty_model_name", "empty_provider", "whitespace_only_model_name", "whitespace_only_provider"],
    )
    def test_embedding_model_format(self, HttpApiAuth, add_dataset_func, name, embedding_model):
        dataset_id = add_dataset_func
        payload = {"name": name, "embedding_model": embedding_model}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 101, res
        if name in ["empty", "space", "missing_at"]:
            assert "Embedding model identifier must follow <model_name>@<provider> format" in res["message"], res
        else:
            assert "Both model_name and provider must be non-empty strings" in res["message"], res

    @pytest.mark.p2
    def test_embedding_model_none(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        payload = {"embedding_model": None}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 0, res

        res = list_datasets(HttpApiAuth)
        assert res["code"] == 0, res
        assert res["data"][0]["embedding_model"] == "BAAI/bge-small-en-v1.5@Builtin", res

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "permission",
        [
            "me",
            "team",
        ],
        ids=["me", "team"],
    )
    def test_permission(self, HttpApiAuth, add_dataset_func, permission):
        dataset_id = add_dataset_func
        payload = {"permission": permission}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 0, res

        res = list_datasets(HttpApiAuth)
        assert res["code"] == 0, res
        assert res["data"][0]["permission"] == permission.lower().strip(), res

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "permission",
        [
            "",
            "unknown",
            list(),
            "ME",
            "TEAM",
            " ME ",
        ],
        ids=["empty", "unknown", "type_error", "me_upercase", "team_upercase", "whitespace"],
    )
    def test_permission_invalid(self, HttpApiAuth, add_dataset_func, permission):
        dataset_id = add_dataset_func
        payload = {"permission": permission}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 101
        assert "Input should be 'me' or 'team'" in res["message"]

    @pytest.mark.p3
    def test_permission_none(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        payload = {"permission": None}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 101, res
        assert "Input should be 'me' or 'team'" in res["message"], res

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "chunk_method",
        [
            "naive",
            "book",
            "email",
            "laws",
            "manual",
            "one",
            "paper",
            "picture",
            "presentation",
            "qa",
            "table",
            "tag",
        ],
        ids=["naive", "book", "email", "laws", "manual", "one", "paper", "picture", "presentation", "qa", "table", "tag"],
    )
    def test_chunk_method(self, HttpApiAuth, add_dataset_func, chunk_method):
        dataset_id = add_dataset_func
        payload = {"chunk_method": chunk_method}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 0, res

        res = list_datasets(HttpApiAuth)
        assert res["code"] == 0, res
        assert res["data"][0]["chunk_method"] == chunk_method, res

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "chunk_method",
        [
            "",
            "unknown",
            list(),
        ],
        ids=["empty", "unknown", "type_error"],
    )
    def test_chunk_method_invalid(self, HttpApiAuth, add_dataset_func, chunk_method):
        dataset_id = add_dataset_func
        payload = {"chunk_method": chunk_method}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 101, res
        assert "Input should be 'naive', 'book', 'email', 'laws', 'manual', 'one', 'paper', 'picture', 'presentation', 'qa', 'table' or 'tag'" in res["message"], res

    @pytest.mark.p3
    def test_chunk_method_none(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        payload = {"chunk_method": None}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 101, res
        assert "Input should be 'naive', 'book', 'email', 'laws', 'manual', 'one', 'paper', 'picture', 'presentation', 'qa', 'table' or 'tag'" in res["message"], res

    @pytest.mark.skipif(os.getenv("DOC_ENGINE") == "infinity", reason="#8208")
    @pytest.mark.p2
    @pytest.mark.parametrize("pagerank", [0, 50, 100], ids=["min", "mid", "max"])
    def test_pagerank(self, HttpApiAuth, add_dataset_func, pagerank):
        dataset_id = add_dataset_func
        payload = {"pagerank": pagerank}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 0

        res = list_datasets(HttpApiAuth, {"id": dataset_id})
        assert res["code"] == 0, res
        assert res["data"][0]["pagerank"] == pagerank

    @pytest.mark.skipif(os.getenv("DOC_ENGINE") == "infinity", reason="#8208")
    @pytest.mark.p2
    def test_pagerank_set_to_0(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        payload = {"pagerank": 50}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 0, res

        res = list_datasets(HttpApiAuth, {"id": dataset_id})
        assert res["code"] == 0, res
        assert res["data"][0]["pagerank"] == 50, res

        payload = {"pagerank": 0}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 0

        res = list_datasets(HttpApiAuth, {"id": dataset_id})
        assert res["code"] == 0, res
        assert res["data"][0]["pagerank"] == 0, res

    @pytest.mark.skipif(os.getenv("DOC_ENGINE") != "infinity", reason="#8208")
    @pytest.mark.p2
    def test_pagerank_infinity(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        payload = {"pagerank": 50}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 101, res
        assert res["message"] == "'pagerank' can only be set when doc_engine is elasticsearch", res

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "pagerank, expected_message",
        [
            (-1, "Input should be greater than or equal to 0"),
            (101, "Input should be less than or equal to 100"),
        ],
        ids=["min_limit", "max_limit"],
    )
    def test_pagerank_invalid(self, HttpApiAuth, add_dataset_func, pagerank, expected_message):
        dataset_id = add_dataset_func
        payload = {"pagerank": pagerank}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 101, res
        assert expected_message in res["message"], res

    @pytest.mark.p3
    def test_pagerank_none(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        payload = {"pagerank": None}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 101, res
        assert "Input should be a valid integer" in res["message"], res

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "parser_config",
        [
            {"auto_keywords": 0},
            {"auto_keywords": 16},
            {"auto_keywords": 32},
            {"auto_questions": 0},
            {"auto_questions": 5},
            {"auto_questions": 10},
            {"chunk_token_num": 1},
            {"chunk_token_num": 1024},
            {"chunk_token_num": 2048},
            {"delimiter": "\n"},
            {"delimiter": " "},
            {"html4excel": True},
            {"html4excel": False},
            {"layout_recognize": "DeepDOC"},
            {"layout_recognize": "Plain Text"},
            {"tag_kb_ids": ["1", "2"]},
            {"topn_tags": 1},
            {"topn_tags": 5},
            {"topn_tags": 10},
            {"filename_embd_weight": 0.1},
            {"filename_embd_weight": 0.5},
            {"filename_embd_weight": 1.0},
            {"task_page_size": 1},
            {"task_page_size": None},
            {"pages": [[1, 100]]},
            {"pages": None},
            {"graphrag": {"use_graphrag": True}},
            {"graphrag": {"use_graphrag": False}},
            {"graphrag": {"entity_types": ["age", "sex", "height", "weight"]}},
            {"graphrag": {"method": "general"}},
            {"graphrag": {"method": "light"}},
            {"graphrag": {"community": True}},
            {"graphrag": {"community": False}},
            {"graphrag": {"resolution": True}},
            {"graphrag": {"resolution": False}},
            {"raptor": {"use_raptor": True}},
            {"raptor": {"use_raptor": False}},
            {"raptor": {"prompt": "Who are you?"}},
            {"raptor": {"max_token": 1}},
            {"raptor": {"max_token": 1024}},
            {"raptor": {"max_token": 2048}},
            {"raptor": {"threshold": 0.0}},
            {"raptor": {"threshold": 0.5}},
            {"raptor": {"threshold": 1.0}},
            {"raptor": {"max_cluster": 1}},
            {"raptor": {"max_cluster": 512}},
            {"raptor": {"max_cluster": 1024}},
            {"raptor": {"random_seed": 0}},
        ],
        ids=[
            "auto_keywords_min",
            "auto_keywords_mid",
            "auto_keywords_max",
            "auto_questions_min",
            "auto_questions_mid",
            "auto_questions_max",
            "chunk_token_num_min",
            "chunk_token_num_mid",
            "chunk_token_num_max",
            "delimiter",
            "delimiter_space",
            "html4excel_true",
            "html4excel_false",
            "layout_recognize_DeepDOC",
            "layout_recognize_navie",
            "tag_kb_ids",
            "topn_tags_min",
            "topn_tags_mid",
            "topn_tags_max",
            "filename_embd_weight_min",
            "filename_embd_weight_mid",
            "filename_embd_weight_max",
            "task_page_size_min",
            "task_page_size_None",
            "pages",
            "pages_none",
            "graphrag_true",
            "graphrag_false",
            "graphrag_entity_types",
            "graphrag_method_general",
            "graphrag_method_light",
            "graphrag_community_true",
            "graphrag_community_false",
            "graphrag_resolution_true",
            "graphrag_resolution_false",
            "raptor_true",
            "raptor_false",
            "raptor_prompt",
            "raptor_max_token_min",
            "raptor_max_token_mid",
            "raptor_max_token_max",
            "raptor_threshold_min",
            "raptor_threshold_mid",
            "raptor_threshold_max",
            "raptor_max_cluster_min",
            "raptor_max_cluster_mid",
            "raptor_max_cluster_max",
            "raptor_random_seed_min",
        ],
    )
    def test_parser_config(self, HttpApiAuth, add_dataset_func, parser_config):
        dataset_id = add_dataset_func
        payload = {"parser_config": parser_config}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 0, res

        res = list_datasets(HttpApiAuth)
        assert res["code"] == 0, res
        for k, v in parser_config.items():
            if isinstance(v, dict):
                for kk, vv in v.items():
                    assert res["data"][0]["parser_config"][k][kk] == vv, res
            else:
                assert res["data"][0]["parser_config"][k] == v, res

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "parser_config, expected_message",
        [
            ({"auto_keywords": -1}, "Input should be greater than or equal to 0"),
            ({"auto_keywords": 33}, "Input should be less than or equal to 32"),
            ({"auto_keywords": 3.14}, "Input should be a valid integer"),
            ({"auto_keywords": "string"}, "Input should be a valid integer"),
            ({"auto_questions": -1}, "Input should be greater than or equal to 0"),
            ({"auto_questions": 11}, "Input should be less than or equal to 10"),
            ({"auto_questions": 3.14}, "Input should be a valid integer"),
            ({"auto_questions": "string"}, "Input should be a valid integer"),
            ({"chunk_token_num": 0}, "Input should be greater than or equal to 1"),
            ({"chunk_token_num": 2049}, "Input should be less than or equal to 2048"),
            ({"chunk_token_num": 3.14}, "Input should be a valid integer"),
            ({"chunk_token_num": "string"}, "Input should be a valid integer"),
            ({"delimiter": ""}, "String should have at least 1 character"),
            ({"html4excel": "string"}, "Input should be a valid boolean"),
            ({"tag_kb_ids": "1,2"}, "Input should be a valid list"),
            ({"tag_kb_ids": [1, 2]}, "Input should be a valid string"),
            ({"topn_tags": 0}, "Input should be greater than or equal to 1"),
            ({"topn_tags": 11}, "Input should be less than or equal to 10"),
            ({"topn_tags": 3.14}, "Input should be a valid integer"),
            ({"topn_tags": "string"}, "Input should be a valid integer"),
            ({"filename_embd_weight": -1}, "Input should be greater than or equal to 0"),
            ({"filename_embd_weight": 1.1}, "Input should be less than or equal to 1"),
            ({"filename_embd_weight": "string"}, "Input should be a valid number"),
            ({"task_page_size": 0}, "Input should be greater than or equal to 1"),
            ({"task_page_size": 3.14}, "Input should be a valid integer"),
            ({"task_page_size": "string"}, "Input should be a valid integer"),
            ({"pages": "1,2"}, "Input should be a valid list"),
            ({"pages": ["1,2"]}, "Input should be a valid list"),
            ({"pages": [["string1", "string2"]]}, "Input should be a valid integer"),
            ({"graphrag": {"use_graphrag": "string"}}, "Input should be a valid boolean"),
            ({"graphrag": {"entity_types": "1,2"}}, "Input should be a valid list"),
            ({"graphrag": {"entity_types": [1, 2]}}, "nput should be a valid string"),
            ({"graphrag": {"method": "unknown"}}, "Input should be 'light' or 'general'"),
            ({"graphrag": {"method": None}}, "Input should be 'light' or 'general'"),
            ({"graphrag": {"community": "string"}}, "Input should be a valid boolean"),
            ({"graphrag": {"resolution": "string"}}, "Input should be a valid boolean"),
            ({"raptor": {"use_raptor": "string"}}, "Input should be a valid boolean"),
            ({"raptor": {"prompt": ""}}, "String should have at least 1 character"),
            ({"raptor": {"prompt": " "}}, "String should have at least 1 character"),
            ({"raptor": {"max_token": 0}}, "Input should be greater than or equal to 1"),
            ({"raptor": {"max_token": 2049}}, "Input should be less than or equal to 2048"),
            ({"raptor": {"max_token": 3.14}}, "Input should be a valid integer"),
            ({"raptor": {"max_token": "string"}}, "Input should be a valid integer"),
            ({"raptor": {"threshold": -0.1}}, "Input should be greater than or equal to 0"),
            ({"raptor": {"threshold": 1.1}}, "Input should be less than or equal to 1"),
            ({"raptor": {"threshold": "string"}}, "Input should be a valid number"),
            ({"raptor": {"max_cluster": 0}}, "Input should be greater than or equal to 1"),
            ({"raptor": {"max_cluster": 1025}}, "Input should be less than or equal to 1024"),
            ({"raptor": {"max_cluster": 3.14}}, "Input should be a valid integer"),
            ({"raptor": {"max_cluster": "string"}}, "Input should be a valid integer"),
            ({"raptor": {"random_seed": -1}}, "Input should be greater than or equal to 0"),
            ({"raptor": {"random_seed": 3.14}}, "Input should be a valid integer"),
            ({"raptor": {"random_seed": "string"}}, "Input should be a valid integer"),
            ({"delimiter": "a" * 65536}, "Parser config exceeds size limit (max 65,535 characters)"),
        ],
        ids=[
            "auto_keywords_min_limit",
            "auto_keywords_max_limit",
            "auto_keywords_float_not_allowed",
            "auto_keywords_type_invalid",
            "auto_questions_min_limit",
            "auto_questions_max_limit",
            "auto_questions_float_not_allowed",
            "auto_questions_type_invalid",
            "chunk_token_num_min_limit",
            "chunk_token_num_max_limit",
            "chunk_token_num_float_not_allowed",
            "chunk_token_num_type_invalid",
            "delimiter_empty",
            "html4excel_type_invalid",
            "tag_kb_ids_not_list",
            "tag_kb_ids_int_in_list",
            "topn_tags_min_limit",
            "topn_tags_max_limit",
            "topn_tags_float_not_allowed",
            "topn_tags_type_invalid",
            "filename_embd_weight_min_limit",
            "filename_embd_weight_max_limit",
            "filename_embd_weight_type_invalid",
            "task_page_size_min_limit",
            "task_page_size_float_not_allowed",
            "task_page_size_type_invalid",
            "pages_not_list",
            "pages_not_list_in_list",
            "pages_not_int_list",
            "graphrag_type_invalid",
            "graphrag_entity_types_not_list",
            "graphrag_entity_types_not_str_in_list",
            "graphrag_method_unknown",
            "graphrag_method_none",
            "graphrag_community_type_invalid",
            "graphrag_resolution_type_invalid",
            "raptor_type_invalid",
            "raptor_prompt_empty",
            "raptor_prompt_space",
            "raptor_max_token_min_limit",
            "raptor_max_token_max_limit",
            "raptor_max_token_float_not_allowed",
            "raptor_max_token_type_invalid",
            "raptor_threshold_min_limit",
            "raptor_threshold_max_limit",
            "raptor_threshold_type_invalid",
            "raptor_max_cluster_min_limit",
            "raptor_max_cluster_max_limit",
            "raptor_max_cluster_float_not_allowed",
            "raptor_max_cluster_type_invalid",
            "raptor_random_seed_min_limit",
            "raptor_random_seed_float_not_allowed",
            "raptor_random_seed_type_invalid",
            "parser_config_type_invalid",
        ],
    )
    def test_parser_config_invalid(self, HttpApiAuth, add_dataset_func, parser_config, expected_message):
        dataset_id = add_dataset_func
        payload = {"parser_config": parser_config}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 101, res
        assert expected_message in res["message"], res

    @pytest.mark.p2
    def test_parser_config_empty(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        payload = {"parser_config": {}}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 0, res

        res = list_datasets(HttpApiAuth)
        assert res["code"] == 0, res
        assert res["data"][0]["parser_config"] == DEFAULT_PARSER_CONFIG, res

    @pytest.mark.p3
    def test_parser_config_none(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        payload = {"parser_config": None}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 0, res

        res = list_datasets(HttpApiAuth, {"id": dataset_id})
        assert res["code"] == 0, res
        assert res["data"][0]["parser_config"] == DEFAULT_PARSER_CONFIG, res

    @pytest.mark.p3
    def test_parser_config_empty_with_chunk_method_change(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        payload = {"chunk_method": "qa", "parser_config": {}}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 0, res

        res = list_datasets(HttpApiAuth)
        assert res["code"] == 0, res
        assert res["data"][0]["parser_config"] == {"raptor": {"use_raptor": False}, "graphrag": {"use_graphrag": False}}, res

    @pytest.mark.p3
    def test_parser_config_unset_with_chunk_method_change(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        payload = {"chunk_method": "qa"}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 0, res

        res = list_datasets(HttpApiAuth)
        assert res["code"] == 0, res
        assert res["data"][0]["parser_config"] == {"raptor": {"use_raptor": False}, "graphrag": {"use_graphrag": False}}, res

    @pytest.mark.p3
    def test_parser_config_none_with_chunk_method_change(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        payload = {"chunk_method": "qa", "parser_config": None}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 0, res

        res = list_datasets(HttpApiAuth, {"id": dataset_id})
        assert res["code"] == 0, res
        assert res["data"][0]["parser_config"] == {"raptor": {"use_raptor": False}, "graphrag": {"use_graphrag": False}}, res

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "payload",
        [
            {"id": "id"},
            {"tenant_id": "e57c1966f99211efb41e9e45646e0111"},
            {"created_by": "created_by"},
            {"create_date": "Tue, 11 Mar 2025 13:37:23 GMT"},
            {"create_time": 1741671443322},
            {"update_date": "Tue, 11 Mar 2025 13:37:23 GMT"},
            {"update_time": 1741671443339},
            {"document_count": 1},
            {"chunk_count": 1},
            {"token_num": 1},
            {"status": "1"},
            {"unknown_field": "unknown_field"},
        ],
    )
    def test_field_unsupported(self, HttpApiAuth, add_dataset_func, payload):
        dataset_id = add_dataset_func
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 101, res
        assert "Extra inputs are not permitted" in res["message"], res

    @pytest.mark.p2
    def test_field_unset(self, HttpApiAuth, add_dataset_func):
        dataset_id = add_dataset_func
        res = list_datasets(HttpApiAuth)
        assert res["code"] == 0, res
        original_data = res["data"][0]

        payload = {"name": "default_unset"}
        res = update_dataset(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 0, res

        res = list_datasets(HttpApiAuth)
        assert res["code"] == 0, res
        assert res["data"][0]["avatar"] == original_data["avatar"], res
        assert res["data"][0]["description"] == original_data["description"], res
        assert res["data"][0]["embedding_model"] == original_data["embedding_model"], res
        assert res["data"][0]["permission"] == original_data["permission"], res
        assert res["data"][0]["chunk_method"] == original_data["chunk_method"], res
        assert res["data"][0]["pagerank"] == original_data["pagerank"], res
        assert res["data"][0]["parser_config"] == original_data["parser_config"], res

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_http_api/test_dataset_mangement/test_update_dataset.py` is located in the `test/testcases/test_http_api/test_dataset_mangement` directory.

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
- [test_list_datasets.py](test_list_datasets.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
