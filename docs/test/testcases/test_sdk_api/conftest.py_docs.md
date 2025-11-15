# Documentation: test/testcases/test_sdk_api/conftest.py

## File Metadata

- **Path**: `test/testcases/test_sdk_api/conftest.py`
- **Size**: 5288 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_sdk_api/conftest.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `pathlib`
- `time`
- `pytest`
- `common`
- `configs`
- `pytest`
- `ragflow_sdk`
- `utils`
- `utils.file_utils`

### Functions Defined

This file defines 19 function(s):

#### Function: `condition` (line 46)

**Parameters**: _dataset

#### Function: `generate_test_files` (line 55)

**Parameters**: request, tmp_path

#### Function: `ragflow_tmp_dir` (line 78)

**Parameters**: request, tmp_path_factory

#### Function: `client` (line 84)

**Parameters**: token

#### Function: `clear_datasets` (line 89)

**Parameters**: request, client

#### Function: `clear_chat_assistants` (line 97)

**Parameters**: request, client

#### Function: `clear_session_with_chat_assistants` (line 105)

**Parameters**: request, add_chat_assistants

#### Function: `add_dataset` (line 119)

**Parameters**: request, client

#### Function: `add_dataset_func` (line 128)

**Parameters**: request, client

#### Function: `add_document` (line 137)

**Parameters**: add_dataset, ragflow_tmp_dir

#### Function: `add_chunks` (line 142)

**Parameters**: request, add_document

#### Function: `add_chat_assistants` (line 161)

**Parameters**: request, client, add_document

#### Function: `cleanup` (line 90)

**Parameters**: None

#### Function: `cleanup` (line 98)

**Parameters**: None

#### Function: `cleanup` (line 106)

**Parameters**: None

#### Function: `cleanup` (line 120)

**Parameters**: None

#### Function: `cleanup` (line 129)

**Parameters**: None

#### Function: `cleanup` (line 143)

**Parameters**: None

#### Function: `cleanup` (line 162)

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

from pathlib import Path
from time import sleep

import pytest
from common import (
    batch_add_chunks,
    batch_create_chat_assistants,
    batch_create_datasets,
    bulk_upload_documents,
)
from configs import HOST_ADDRESS, VERSION
from pytest import FixtureRequest
from ragflow_sdk import Chat, Chunk, DataSet, Document, RAGFlow
from utils import wait_for
from utils.file_utils import (
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


@wait_for(30, 1, "Document parsing timeout")
def condition(_dataset: DataSet):
    documents = _dataset.list_documents(page_size=1000)
    for document in documents:
        if document.run != "DONE":
            return False
    return True


@pytest.fixture
def generate_test_files(request: FixtureRequest, tmp_path: Path):
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
def ragflow_tmp_dir(request: FixtureRequest, tmp_path_factory: Path) -> Path:
    class_name = request.cls.__name__
    return tmp_path_factory.mktemp(class_name)


@pytest.fixture(scope="session")
def client(token: str) -> RAGFlow:
    return RAGFlow(api_key=token, base_url=HOST_ADDRESS, version=VERSION)


@pytest.fixture(scope="function")
def clear_datasets(request: FixtureRequest, client: RAGFlow):
    def cleanup():
        client.delete_datasets(ids=None)

    request.addfinalizer(cleanup)


@pytest.fixture(scope="function")
def clear_chat_assistants(request: FixtureRequest, client: RAGFlow):
    def cleanup():
        client.delete_chats(ids=None)

    request.addfinalizer(cleanup)


@pytest.fixture(scope="function")
def clear_session_with_chat_assistants(request, add_chat_assistants):
    def cleanup():
        for chat_assistant in chat_assistants:
            try:
                chat_assistant.delete_sessions(ids=None)
            except Exception:
                pass

    request.addfinalizer(cleanup)

    _, _, chat_assistants = add_chat_assistants


@pytest.fixture(scope="class")
def add_dataset(request: FixtureRequest, client: RAGFlow) -> DataSet:
    def cleanup():
        client.delete_datasets(ids=None)

    request.addfinalizer(cleanup)
    return batch_create_datasets(client, 1)[0]


@pytest.fixture(scope="function")
def add_dataset_func(request: FixtureRequest, client: RAGFlow) -> DataSet:
    def cleanup():
        client.delete_datasets(ids=None)

    request.addfinalizer(cleanup)
    return batch_create_datasets(client, 1)[0]


@pytest.fixture(scope="class")
def add_document(add_dataset: DataSet, ragflow_tmp_dir: Path) -> tuple[DataSet, Document]:
    return add_dataset, bulk_upload_documents(add_dataset, 1, ragflow_tmp_dir)[0]


@pytest.fixture(scope="class")
def add_chunks(request: FixtureRequest, add_document: tuple[DataSet, Document]) -> tuple[DataSet, Document, list[Chunk]]:
    def cleanup():
        try:
            document.delete_chunks(ids=[])
        except Exception:
            pass

    request.addfinalizer(cleanup)

    dataset, document = add_document
    dataset.async_parse_documents([document.id])
    condition(dataset)
    chunks = batch_add_chunks(document, 4)
    # issues/6487
    sleep(1)
    return dataset, document, chunks


@pytest.fixture(scope="class")
def add_chat_assistants(request, client, add_document) -> tuple[DataSet, Document, list[Chat]]:
    def cleanup():
        try:
            client.delete_chats(ids=None)
        except Exception:
            pass

    request.addfinalizer(cleanup)

    dataset, document = add_document
    dataset.async_parse_documents([document.id])
    condition(dataset)
    return dataset, document, batch_create_chat_assistants(client, 5)

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_sdk_api/conftest.py` is located in the `test/testcases/test_sdk_api` directory.

This file is part of the **Testing** infrastructure.

### Architecture Context

Files in this location typically handle concerns related to test_sdk_api.

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
