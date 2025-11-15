# Documentation: sdk/python/test/test_sdk_api/t_document.py

## File Metadata

- **Path**: `sdk/python/test/test_sdk_api/t_document.py`
- **Size**: 7787 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/test/test_sdk_api/t_document.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `ragflow_sdk`
- `common`
- `pytest`

### Functions Defined

This file defines 15 function(s):

#### Function: `test_upload_document_with_success` (line 22)

**Parameters**: get_api_key_fixture

#### Function: `test_update_document_with_success` (line 35)

**Parameters**: get_api_key_fixture

#### Function: `test_download_document_with_success` (line 46)

**Parameters**: get_api_key_fixture

#### Function: `test_list_documents_in_dataset_with_success` (line 58)

**Parameters**: get_api_key_fixture

#### Function: `test_delete_documents_in_dataset_with_success` (line 68)

**Parameters**: get_api_key_fixture

#### Function: `test_upload_and_parse_pdf_documents_with_general_parse_method` (line 80)

**Parameters**: get_api_key_fixture

#### Function: `test_upload_and_parse_docx_documents_with_general_parse_method` (line 92)

**Parameters**: get_api_key_fixture

#### Function: `test_upload_and_parse_excel_documents_with_general_parse_method` (line 104)

**Parameters**: get_api_key_fixture

#### Function: `test_upload_and_parse_ppt_documents_with_general_parse_method` (line 116)

**Parameters**: get_api_key_fixture

#### Function: `test_upload_and_parse_image_documents_with_general_parse_method` (line 128)

**Parameters**: get_api_key_fixture

#### Function: `test_upload_and_parse_txt_documents_with_general_parse_method` (line 140)

**Parameters**: get_api_key_fixture

#### Function: `test_upload_and_parse_md_documents_with_general_parse_method` (line 152)

**Parameters**: get_api_key_fixture

#### Function: `test_upload_and_parse_json_documents_with_general_parse_method` (line 164)

**Parameters**: get_api_key_fixture

#### Function: `test_upload_and_parse_eml_documents_with_general_parse_method` (line 177)

**Parameters**: get_api_key_fixture

#### Function: `test_upload_and_parse_html_documents_with_general_parse_method` (line 189)

**Parameters**: get_api_key_fixture

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

from ragflow_sdk import RAGFlow
from common import HOST_ADDRESS
import pytest


def test_upload_document_with_success(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_upload_document")
    blob = b"Sample document content for test."
    with open("test_data/ragflow.txt", "rb") as file:
        blob_2 = file.read()
    document_infos = []
    document_infos.append({"display_name": "test_1.txt", "blob": blob})
    document_infos.append({"display_name": "test_2.txt", "blob": blob_2})
    ds.upload_documents(document_infos)


def test_update_document_with_success(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_update_document")
    blob = b"Sample document content for test."
    document_infos = [{"display_name": "test.txt", "blob": blob}]
    docs = ds.upload_documents(document_infos)
    doc = docs[0]
    doc.update({"chunk_method": "manual", "name": "manual.txt"})


def test_download_document_with_success(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_download_document")
    blob = b"Sample document content for test."
    document_infos = [{"display_name": "test_1.txt", "blob": blob}]
    docs = ds.upload_documents(document_infos)
    doc = docs[0]
    with open("test_download.txt", "wb+") as file:
        file.write(doc.download())


def test_list_documents_in_dataset_with_success(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_list_documents")
    blob = b"Sample document content for test."
    document_infos = [{"display_name": "test.txt", "blob": blob}]
    ds.upload_documents(document_infos)
    ds.list_documents(keywords="test", page=1, page_size=12)


def test_delete_documents_in_dataset_with_success(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_delete_documents")
    name = "test_delete_documents.txt"
    blob = b"Sample document content for test."
    document_infos = [{"display_name": name, "blob": blob}]
    docs = ds.upload_documents(document_infos)
    ds.delete_documents([docs[0].id])


# upload and parse the document with different in different parse method.
def test_upload_and_parse_pdf_documents_with_general_parse_method(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_pdf_document")
    with open("test_data/test.pdf", "rb") as file:
        blob = file.read()
    document_infos = [{"display_name": "test.pdf", "blob": blob}]
    docs = ds.upload_documents(document_infos)
    doc = docs[0]
    ds.async_parse_documents([doc.id])


def test_upload_and_parse_docx_documents_with_general_parse_method(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_docx_document")
    with open("test_data/test.docx", "rb") as file:
        blob = file.read()
    document_infos = [{"display_name": "test.docx", "blob": blob}]
    docs = ds.upload_documents(document_infos)
    doc = docs[0]
    ds.async_parse_documents([doc.id])


def test_upload_and_parse_excel_documents_with_general_parse_method(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_excel_document")
    with open("test_data/test.xlsx", "rb") as file:
        blob = file.read()
    document_infos = [{"display_name": "test.xlsx", "blob": blob}]
    docs = ds.upload_documents(document_infos)
    doc = docs[0]
    ds.async_parse_documents([doc.id])


def test_upload_and_parse_ppt_documents_with_general_parse_method(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_ppt_document")
    with open("test_data/test.ppt", "rb") as file:
        blob = file.read()
    document_infos = [{"display_name": "test.ppt", "blob": blob}]
    docs = ds.upload_documents(document_infos)
    doc = docs[0]
    ds.async_parse_documents([doc.id])


def test_upload_and_parse_image_documents_with_general_parse_method(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_image_document")
    with open("test_data/test.jpg", "rb") as file:
        blob = file.read()
    document_infos = [{"display_name": "test.jpg", "blob": blob}]
    docs = ds.upload_documents(document_infos)
    doc = docs[0]
    ds.async_parse_documents([doc.id])


def test_upload_and_parse_txt_documents_with_general_parse_method(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_txt_document")
    with open("test_data/test.txt", "rb") as file:
        blob = file.read()
    document_infos = [{"display_name": "test.txt", "blob": blob}]
    docs = ds.upload_documents(document_infos)
    doc = docs[0]
    ds.async_parse_documents([doc.id])


def test_upload_and_parse_md_documents_with_general_parse_method(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_md_document")
    with open("test_data/test.md", "rb") as file:
        blob = file.read()
    document_infos = [{"display_name": "test.md", "blob": blob}]
    docs = ds.upload_documents(document_infos)
    doc = docs[0]
    ds.async_parse_documents([doc.id])


def test_upload_and_parse_json_documents_with_general_parse_method(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_json_document")
    with open("test_data/test.json", "rb") as file:
        blob = file.read()
    document_infos = [{"display_name": "test.json", "blob": blob}]
    docs = ds.upload_documents(document_infos)
    doc = docs[0]
    ds.async_parse_documents([doc.id])


@pytest.mark.skip(reason="")
def test_upload_and_parse_eml_documents_with_general_parse_method(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_eml_document")
    with open("test_data/test.eml", "rb") as file:
        blob = file.read()
    document_infos = [{"display_name": "test.eml", "blob": blob}]
    docs = ds.upload_documents(document_infos)
    doc = docs[0]
    ds.async_parse_documents([doc.id])


def test_upload_and_parse_html_documents_with_general_parse_method(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_html_document")
    with open("test_data/test.html", "rb") as file:
        blob = file.read()
    document_infos = [{"display_name": "test.html", "blob": blob}]
    docs = ds.upload_documents(document_infos)
    doc = docs[0]
    ds.async_parse_documents([doc.id])

```

## Detailed Analysis

### File Role in Repository

The file `sdk/python/test/test_sdk_api/t_document.py` is located in the `sdk/python/test/test_sdk_api` directory.

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
- [get_email.py](get_email.py_docs.md)
- [t_agent.py](t_agent.py_docs.md)
- [t_chat.py](t_chat.py_docs.md)
- [t_chunk.py](t_chunk.py_docs.md)
- [t_dataset.py](t_dataset.py_docs.md)
- [t_session.py](t_session.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
