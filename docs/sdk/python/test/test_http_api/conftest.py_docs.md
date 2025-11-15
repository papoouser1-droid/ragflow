# Documentation: sdk/python/test/test_http_api/conftest.py

## File Metadata

- **Path**: `sdk/python/test/test_http_api/conftest.py`
- **Size**: 6196 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/test/test_http_api/conftest.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `os`
- `pytest`
- `common`
- `libs.auth`
- `libs.utils`
- `libs.utils.file_utils`
- `time`

### Functions Defined

This file defines 20 function(s):

#### Function: `pytest_addoption` (line 53)

**Parameters**: parser

#### Function: `pytest_configure` (line 63)

**Parameters**: config

#### Function: `condition` (line 71)

**Parameters**: _auth, _dataset_id

#### Function: `get_http_api_auth` (line 80)

**Parameters**: get_api_key_fixture

#### Function: `clear_datasets` (line 85)

**Parameters**: request, get_http_api_auth

#### Function: `clear_chat_assistants` (line 93)

**Parameters**: request, get_http_api_auth

#### Function: `clear_session_with_chat_assistants` (line 101)

**Parameters**: request, get_http_api_auth, add_chat_assistants

#### Function: `generate_test_files` (line 112)

**Parameters**: request, tmp_path

#### Function: `ragflow_tmp_dir` (line 135)

**Parameters**: request, tmp_path_factory

#### Function: `add_dataset` (line 141)

**Parameters**: request, get_http_api_auth

#### Function: `add_dataset_func` (line 152)

**Parameters**: request, get_http_api_auth

#### Function: `add_document` (line 162)

**Parameters**: get_http_api_auth, add_dataset, ragflow_tmp_dir

#### Function: `add_chunks` (line 169)

**Parameters**: get_http_api_auth, add_document

#### Function: `add_chat_assistants` (line 187)

**Parameters**: request, get_http_api_auth, add_document

#### Function: `cleanup` (line 86)

**Parameters**: None

#### Function: `cleanup` (line 94)

**Parameters**: None

#### Function: `cleanup` (line 104)

**Parameters**: None

#### Function: `cleanup` (line 142)

**Parameters**: None

#### Function: `cleanup` (line 153)

**Parameters**: None

#### Function: `cleanup` (line 188)

**Parameters**: None

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

import pytest
from common import (
    add_chunk,
    batch_create_datasets,
    bulk_upload_documents,
    create_chat_assistant,
    delete_chat_assistants,
    delete_datasets,
    delete_session_with_chat_assistants,
    list_documnets,
    parse_documnets,
)
from libs.auth import RAGFlowHttpApiAuth
from libs.utils import wait_for
from libs.utils.file_utils import (
    create_docx_file,
    create_eml_file,
    create_excel_file,
    create_html_file,
    create_image_file,
    create_json_file,
    create_md_file,
    create_pdf_file,
    create_ppt_file,
    create_txt_file,
)

MARKER_EXPRESSIONS = {
    "p1": "p1",
    "p2": "p1 or p2",
    "p3": "p1 or p2 or p3",
}
HOST_ADDRESS = os.getenv("HOST_ADDRESS", "http://127.0.0.1:9380")


def pytest_addoption(parser: pytest.Parser) -> None:
    parser.addoption(
        "--level",
        action="store",
        default="p2",
        choices=list(MARKER_EXPRESSIONS.keys()),
        help=f"Test level ({'/'.join(MARKER_EXPRESSIONS)}): p1=smoke, p2=core, p3=full",
    )


def pytest_configure(config: pytest.Config) -> None:
    level = config.getoption("--level")
    config.option.markexpr = MARKER_EXPRESSIONS[level]
    if config.option.verbose > 0:
        print(f"\n[CONFIG] Active test level: {level}")


@wait_for(30, 1, "Document parsing timeout")
def condition(_auth, _dataset_id):
    res = list_documnets(_auth, _dataset_id)
    for doc in res["data"]["docs"]:
        if doc["run"] != "DONE":
            return False
    return True


@pytest.fixture(scope="session")
def get_http_api_auth(get_api_key_fixture):
    return RAGFlowHttpApiAuth(get_api_key_fixture)


@pytest.fixture(scope="function")
def clear_datasets(request, get_http_api_auth):
    def cleanup():
        delete_datasets(get_http_api_auth, {"ids": None})

    request.addfinalizer(cleanup)


@pytest.fixture(scope="function")
def clear_chat_assistants(request, get_http_api_auth):
    def cleanup():
        delete_chat_assistants(get_http_api_auth)

    request.addfinalizer(cleanup)


@pytest.fixture(scope="function")
def clear_session_with_chat_assistants(request, get_http_api_auth, add_chat_assistants):
    _, _, chat_assistant_ids = add_chat_assistants

    def cleanup():
        for chat_assistant_id in chat_assistant_ids:
            delete_session_with_chat_assistants(get_http_api_auth, chat_assistant_id)

    request.addfinalizer(cleanup)


@pytest.fixture
def generate_test_files(request, tmp_path):
    file_creators = {
        "docx": (tmp_path / "ragflow_test.docx", create_docx_file),
        "excel": (tmp_path / "ragflow_test.xlsx", create_excel_file),
        "ppt": (tmp_path / "ragflow_test.pptx", create_ppt_file),
        "image": (tmp_path / "ragflow_test.png", create_image_file),
        "pdf": (tmp_path / "ragflow_test.pdf", create_pdf_file),
        "txt": (tmp_path / "ragflow_test.txt", create_txt_file),
        "md": (tmp_path / "ragflow_test.md", create_md_file),
        "json": (tmp_path / "ragflow_test.json", create_json_file),
        "eml": (tmp_path / "ragflow_test.eml", create_eml_file),
        "html": (tmp_path / "ragflow_test.html", create_html_file),
    }

    files = {}
    for file_type, (file_path, creator_func) in file_creators.items():
        if request.param in ["", file_type]:
            creator_func(file_path)
            files[file_type] = file_path
    return files


@pytest.fixture(scope="class")
def ragflow_tmp_dir(request, tmp_path_factory):
    class_name = request.cls.__name__
    return tmp_path_factory.mktemp(class_name)


@pytest.fixture(scope="class")
def add_dataset(request, get_http_api_auth):
    def cleanup():
        delete_datasets(get_http_api_auth, {"ids": None})

    request.addfinalizer(cleanup)

    dataset_ids = batch_create_datasets(get_http_api_auth, 1)
    return dataset_ids[0]


@pytest.fixture(scope="function")
def add_dataset_func(request, get_http_api_auth):
    def cleanup():
        delete_datasets(get_http_api_auth, {"ids": None})

    request.addfinalizer(cleanup)

    return batch_create_datasets(get_http_api_auth, 1)[0]


@pytest.fixture(scope="class")
def add_document(get_http_api_auth, add_dataset, ragflow_tmp_dir):
    dataset_id = add_dataset
    document_ids = bulk_upload_documents(get_http_api_auth, dataset_id, 1, ragflow_tmp_dir)
    return dataset_id, document_ids[0]


@pytest.fixture(scope="class")
def add_chunks(get_http_api_auth, add_document):
    dataset_id, document_id = add_document
    parse_documnets(get_http_api_auth, dataset_id, {"document_ids": [document_id]})
    condition(get_http_api_auth, dataset_id)

    chunk_ids = []
    for i in range(4):
        res = add_chunk(get_http_api_auth, dataset_id, document_id, {"content": f"chunk test {i}"})
        chunk_ids.append(res["data"]["chunk"]["id"])

    # issues/6487
    from time import sleep

    sleep(1)
    return dataset_id, document_id, chunk_ids


@pytest.fixture(scope="class")
def add_chat_assistants(request, get_http_api_auth, add_document):
    def cleanup():
        delete_chat_assistants(get_http_api_auth)

    request.addfinalizer(cleanup)

    dataset_id, document_id = add_document
    parse_documnets(get_http_api_auth, dataset_id, {"document_ids": [document_id]})
    condition(get_http_api_auth, dataset_id)

    chat_assistant_ids = []
    for i in range(5):
        res = create_chat_assistant(get_http_api_auth, {"name": f"test_chat_assistant_{i}", "dataset_ids": [dataset_id]})
        chat_assistant_ids.append(res["data"]["id"])

    return dataset_id, document_id, chat_assistant_ids

```

## Detailed Analysis

### File Role in Repository

The file `sdk/python/test/test_http_api/conftest.py` is located in the `sdk/python/test/test_http_api` directory.

This file is part of the **Testing** infrastructure.

### Architecture Context

Files in this location typically handle concerns related to test_http_api.

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

- [common.py](common.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
