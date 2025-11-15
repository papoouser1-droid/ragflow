# Documentation: sdk/python/test/test_http_api/test_file_management_within_dataset/test_update_document.py

## File Metadata

- **Path**: `sdk/python/test/test_http_api/test_file_management_within_dataset/test_update_document.py`
- **Size**: 19625 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/test/test_http_api/test_file_management_within_dataset/test_update_document.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `pytest`
- `common`
- `libs.auth`

### Classes Defined

This file defines 3 class(es):

#### Class: `TestAuthorization` (line 24)

**Methods**: test_invalid_auth

#### Class: `TestDocumentsUpdated` (line 42)

**Methods**: test_name, test_invalid_document_id, test_invalid_dataset_id, test_meta_fields, test_chunk_method, test_invalid_field

#### Class: `TestUpdateDocumentParserConfig` (line 302)

**Methods**: test_parser_config

### Functions Defined

This file defines 8 function(s):

#### Function: `test_invalid_auth` (line 36)

**Parameters**: self, auth, expected_code, expected_message

#### Function: `test_name` (line 85)

**Parameters**: self, get_http_api_auth, add_documents, name, expected_code, expected_message

#### Function: `test_invalid_document_id` (line 107)

**Parameters**: self, get_http_api_auth, add_documents, document_id, expected_code, expected_message

#### Function: `test_invalid_dataset_id` (line 125)

**Parameters**: self, get_http_api_auth, add_documents, dataset_id, expected_code, expected_message

#### Function: `test_meta_fields` (line 136)

**Parameters**: self, get_http_api_auth, add_documents, meta_fields, expected_code, expected_message

#### Function: `test_chunk_method` (line 170)

**Parameters**: self, get_http_api_auth, add_documents, chunk_method, expected_code, expected_message

#### Function: `test_invalid_field` (line 288)

**Parameters**: self, get_http_api_auth, add_documents, payload, expected_code, expected_message

#### Function: `test_parser_config` (line 516)

**Parameters**: self, get_http_api_auth, add_documents, chunk_method, parser_config, expected_code, expected_message

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
from common import DOCUMENT_NAME_LIMIT, INVALID_API_TOKEN, list_documnets, update_documnet
from libs.auth import RAGFlowHttpApiAuth


@pytest.mark.p1
class TestAuthorization:
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
    def test_invalid_auth(self, auth, expected_code, expected_message):
        res = update_documnet(auth, "dataset_id", "document_id")
        assert res["code"] == expected_code
        assert res["message"] == expected_message


class TestDocumentsUpdated:
    @pytest.mark.p1
    @pytest.mark.parametrize(
        "name, expected_code, expected_message",
        [
            ("new_name.txt", 0, ""),
            (
                f"{'a' * (DOCUMENT_NAME_LIMIT - 3)}.txt",
                101,
                "The name should be less than 128 bytes.",
            ),
            (
                0,
                100,
                """AttributeError("\'int\' object has no attribute \'encode\'")""",
            ),
            (
                None,
                100,
                """AttributeError("\'NoneType\' object has no attribute \'encode\'")""",
            ),
            (
                "",
                101,
                "The extension of file can't be changed",
            ),
            (
                "ragflow_test_upload_0",
                101,
                "The extension of file can't be changed",
            ),
            (
                "ragflow_test_upload_1.txt",
                102,
                "Duplicated document name in the same dataset.",
            ),
            (
                "RAGFLOW_TEST_UPLOAD_1.TXT",
                0,
                "",
            ),
        ],
    )
    def test_name(self, get_http_api_auth, add_documents, name, expected_code, expected_message):
        dataset_id, document_ids = add_documents
        res = update_documnet(get_http_api_auth, dataset_id, document_ids[0], {"name": name})
        assert res["code"] == expected_code
        if expected_code == 0:
            res = list_documnets(get_http_api_auth, dataset_id, {"id": document_ids[0]})
            assert res["data"]["docs"][0]["name"] == name
        else:
            assert res["message"] == expected_message

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "document_id, expected_code, expected_message",
        [
            ("", 100, "<MethodNotAllowed '405: Method Not Allowed'>"),
            (
                "invalid_document_id",
                102,
                "The dataset doesn't own the document.",
            ),
        ],
    )
    def test_invalid_document_id(self, get_http_api_auth, add_documents, document_id, expected_code, expected_message):
        dataset_id, _ = add_documents
        res = update_documnet(get_http_api_auth, dataset_id, document_id, {"name": "new_name.txt"})
        assert res["code"] == expected_code
        assert res["message"] == expected_message

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "dataset_id, expected_code, expected_message",
        [
            ("", 100, "<NotFound '404: Not Found'>"),
            (
                "invalid_dataset_id",
                102,
                "You don't own the dataset.",
            ),
        ],
    )
    def test_invalid_dataset_id(self, get_http_api_auth, add_documents, dataset_id, expected_code, expected_message):
        _, document_ids = add_documents
        res = update_documnet(get_http_api_auth, dataset_id, document_ids[0], {"name": "new_name.txt"})
        assert res["code"] == expected_code
        assert res["message"] == expected_message

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "meta_fields, expected_code, expected_message",
        [({"test": "test"}, 0, ""), ("test", 102, "meta_fields must be a dictionary")],
    )
    def test_meta_fields(self, get_http_api_auth, add_documents, meta_fields, expected_code, expected_message):
        dataset_id, document_ids = add_documents
        res = update_documnet(get_http_api_auth, dataset_id, document_ids[0], {"meta_fields": meta_fields})
        if expected_code == 0:
            res = list_documnets(get_http_api_auth, dataset_id, {"id": document_ids[0]})
            assert res["data"]["docs"][0]["meta_fields"] == meta_fields
        else:
            assert res["message"] == expected_message

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "chunk_method, expected_code, expected_message",
        [
            ("naive", 0, ""),
            ("manual", 0, ""),
            ("qa", 0, ""),
            ("table", 0, ""),
            ("paper", 0, ""),
            ("book", 0, ""),
            ("laws", 0, ""),
            ("presentation", 0, ""),
            ("picture", 0, ""),
            ("one", 0, ""),
            ("knowledge_graph", 0, ""),
            ("email", 0, ""),
            ("tag", 0, ""),
            ("", 102, "`chunk_method`  doesn't exist"),
            (
                "other_chunk_method",
                102,
                "`chunk_method` other_chunk_method doesn't exist",
            ),
        ],
    )
    def test_chunk_method(self, get_http_api_auth, add_documents, chunk_method, expected_code, expected_message):
        dataset_id, document_ids = add_documents
        res = update_documnet(get_http_api_auth, dataset_id, document_ids[0], {"chunk_method": chunk_method})
        assert res["code"] == expected_code
        if expected_code == 0:
            res = list_documnets(get_http_api_auth, dataset_id, {"id": document_ids[0]})
            if chunk_method == "":
                assert res["data"]["docs"][0]["chunk_method"] == "naive"
            else:
                assert res["data"]["docs"][0]["chunk_method"] == chunk_method
        else:
            assert res["message"] == expected_message

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "payload, expected_code, expected_message",
        [
            ({"chunk_count": 1}, 102, "Can't change `chunk_count`."),
            pytest.param(
                {"create_date": "Fri, 14 Mar 2025 16:53:42 GMT"},
                102,
                "The input parameters are invalid.",
                marks=pytest.mark.skip(reason="issues/6104"),
            ),
            pytest.param(
                {"create_time": 1},
                102,
                "The input parameters are invalid.",
                marks=pytest.mark.skip(reason="issues/6104"),
            ),
            pytest.param(
                {"created_by": "ragflow_test"},
                102,
                "The input parameters are invalid.",
                marks=pytest.mark.skip(reason="issues/6104"),
            ),
            pytest.param(
                {"dataset_id": "ragflow_test"},
                102,
                "The input parameters are invalid.",
                marks=pytest.mark.skip(reason="issues/6104"),
            ),
            pytest.param(
                {"id": "ragflow_test"},
                102,
                "The input parameters are invalid.",
                marks=pytest.mark.skip(reason="issues/6104"),
            ),
            pytest.param(
                {"location": "ragflow_test.txt"},
                102,
                "The input parameters are invalid.",
                marks=pytest.mark.skip(reason="issues/6104"),
            ),
            pytest.param(
                {"process_begin_at": 1},
                102,
                "The input parameters are invalid.",
                marks=pytest.mark.skip(reason="issues/6104"),
            ),
            pytest.param(
                {"process_duration": 1.0},
                102,
                "The input parameters are invalid.",
                marks=pytest.mark.skip(reason="issues/6104"),
            ),
            pytest.param({"progress": 1.0}, 102, "Can't change `progress`."),
            pytest.param(
                {"progress_msg": "ragflow_test"},
                102,
                "The input parameters are invalid.",
                marks=pytest.mark.skip(reason="issues/6104"),
            ),
            pytest.param(
                {"run": "ragflow_test"},
                102,
                "The input parameters are invalid.",
                marks=pytest.mark.skip(reason="issues/6104"),
            ),
            pytest.param(
                {"size": 1},
                102,
                "The input parameters are invalid.",
                marks=pytest.mark.skip(reason="issues/6104"),
            ),
            pytest.param(
                {"source_type": "ragflow_test"},
                102,
                "The input parameters are invalid.",
                marks=pytest.mark.skip(reason="issues/6104"),
            ),
            pytest.param(
                {"thumbnail": "ragflow_test"},
                102,
                "The input parameters are invalid.",
                marks=pytest.mark.skip(reason="issues/6104"),
            ),
            ({"token_count": 1}, 102, "Can't change `token_count`."),
            pytest.param(
                {"type": "ragflow_test"},
                102,
                "The input parameters are invalid.",
                marks=pytest.mark.skip(reason="issues/6104"),
            ),
            pytest.param(
                {"update_date": "Fri, 14 Mar 2025 16:33:17 GMT"},
                102,
                "The input parameters are invalid.",
                marks=pytest.mark.skip(reason="issues/6104"),
            ),
            pytest.param(
                {"update_time": 1},
                102,
                "The input parameters are invalid.",
                marks=pytest.mark.skip(reason="issues/6104"),
            ),
        ],
    )
    def test_invalid_field(
        self,
        get_http_api_auth,
        add_documents,
        payload,
        expected_code,
        expected_message,
    ):
        dataset_id, document_ids = add_documents
        res = update_documnet(get_http_api_auth, dataset_id, document_ids[0], payload)
        assert res["code"] == expected_code
        assert res["message"] == expected_message


class TestUpdateDocumentParserConfig:
    @pytest.mark.p2
    @pytest.mark.parametrize(
        "chunk_method, parser_config, expected_code, expected_message",
        [
            ("naive", {}, 0, ""),
            (
                "naive",
                {
                    "chunk_token_num": 128,
                    "layout_recognize": "DeepDOC",
                    "html4excel": False,
                    "delimiter": r"\n",
                    "task_page_size": 12,
                    "raptor": {"use_raptor": False},
                },
                0,
                "",
            ),
            pytest.param(
                "naive",
                {"chunk_token_num": -1},
                100,
                "AssertionError('chunk_token_num should be in range from 1 to 100000000')",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"chunk_token_num": 0},
                100,
                "AssertionError('chunk_token_num should be in range from 1 to 100000000')",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"chunk_token_num": 100000000},
                100,
                "AssertionError('chunk_token_num should be in range from 1 to 100000000')",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"chunk_token_num": 3.14},
                102,
                "",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"chunk_token_num": "1024"},
                100,
                "",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            (
                "naive",
                {"layout_recognize": "DeepDOC"},
                0,
                "",
            ),
            (
                "naive",
                {"layout_recognize": "Naive"},
                0,
                "",
            ),
            ("naive", {"html4excel": True}, 0, ""),
            ("naive", {"html4excel": False}, 0, ""),
            pytest.param(
                "naive",
                {"html4excel": 1},
                100,
                "AssertionError('html4excel should be True or False')",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            ("naive", {"delimiter": ""}, 0, ""),
            ("naive", {"delimiter": "`##`"}, 0, ""),
            pytest.param(
                "naive",
                {"delimiter": 1},
                100,
                "",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"task_page_size": -1},
                100,
                "AssertionError('task_page_size should be in range from 1 to 100000000')",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"task_page_size": 0},
                100,
                "AssertionError('task_page_size should be in range from 1 to 100000000')",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"task_page_size": 100000000},
                100,
                "AssertionError('task_page_size should be in range from 1 to 100000000')",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"task_page_size": 3.14},
                100,
                "",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"task_page_size": "1024"},
                100,
                "",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            ("naive", {"raptor": {"use_raptor": True}}, 0, ""),
            ("naive", {"raptor": {"use_raptor": False}}, 0, ""),
            pytest.param(
                "naive",
                {"invalid_key": "invalid_value"},
                100,
                """AssertionError("Abnormal \'parser_config\'. Invalid key: invalid_key")""",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"auto_keywords": -1},
                100,
                "AssertionError('auto_keywords should be in range from 0 to 32')",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"auto_keywords": 32},
                100,
                "AssertionError('auto_keywords should be in range from 0 to 32')",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"auto_questions": 3.14},
                100,
                "",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"auto_keywords": "1024"},
                100,
                "",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"auto_questions": -1},
                100,
                "AssertionError('auto_questions should be in range from 0 to 10')",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"auto_questions": 10},
                100,
                "AssertionError('auto_questions should be in range from 0 to 10')",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"auto_questions": 3.14},
                100,
                "",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"auto_questions": "1024"},
                100,
                "",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"topn_tags": -1},
                100,
                "AssertionError('topn_tags should be in range from 0 to 10')",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"topn_tags": 10},
                100,
                "AssertionError('topn_tags should be in range from 0 to 10')",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"topn_tags": 3.14},
                100,
                "",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
            pytest.param(
                "naive",
                {"topn_tags": "1024"},
                100,
                "",
                marks=pytest.mark.skip(reason="issues/6098"),
            ),
        ],
    )
    def test_parser_config(
        self,
        get_http_api_auth,
        add_documents,
        chunk_method,
        parser_config,
        expected_code,
        expected_message,
    ):
        dataset_id, document_ids = add_documents
        res = update_documnet(
            get_http_api_auth,
            dataset_id,
            document_ids[0],
            {"chunk_method": chunk_method, "parser_config": parser_config},
        )
        assert res["code"] == expected_code
        if expected_code == 0:
            res = list_documnets(get_http_api_auth, dataset_id, {"id": document_ids[0]})
            if parser_config == {}:
                assert res["data"]["docs"][0]["parser_config"] == {
                    "chunk_token_num": 128,
                    "delimiter": r"\n",
                    "html4excel": False,
                    "layout_recognize": "DeepDOC",
                    "raptor": {"use_raptor": False},
                }
            else:
                for k, v in parser_config.items():
                    assert res["data"]["docs"][0]["parser_config"][k] == v
        if expected_code != 0 or expected_message:
            assert res["message"] == expected_message

```

## Detailed Analysis

### File Role in Repository

The file `sdk/python/test/test_http_api/test_file_management_within_dataset/test_update_document.py` is located in the `sdk/python/test/test_http_api/test_file_management_within_dataset` directory.

This file is part of the **Testing** infrastructure.

### Architecture Context

Files in this location typically handle concerns related to test_file_management_within_dataset.

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
- [test_delete_documents.py](test_delete_documents.py_docs.md)
- [test_download_document.py](test_download_document.py_docs.md)
- [test_list_documents.py](test_list_documents.py_docs.md)
- [test_parse_documents.py](test_parse_documents.py_docs.md)
- [test_stop_parse_documents.py](test_stop_parse_documents.py_docs.md)
- [test_upload_documents.py](test_upload_documents.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
