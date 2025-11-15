# Documentation: sdk/python/test/test_sdk_api/t_chunk.py

## File Metadata

- **Path**: `sdk/python/test/test_sdk_api/t_chunk.py`
- **Size**: 7942 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/test/test_sdk_api/t_chunk.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `ragflow_sdk`
- `common`
- `time`

### Functions Defined

This file defines 9 function(s):

#### Function: `test_parse_document_with_txt` (line 22)

**Parameters**: get_api_key_fixture

#### Function: `test_parse_and_cancel_document` (line 42)

**Parameters**: get_api_key_fixture

#### Function: `test_bulk_parse_documents` (line 57)

**Parameters**: get_api_key_fixture

#### Function: `test_list_chunks_with_success` (line 82)

**Parameters**: get_api_key_fixture

#### Function: `test_add_chunk_with_success` (line 112)

**Parameters**: get_api_key_fixture

#### Function: `test_delete_chunk_with_success` (line 131)

**Parameters**: get_api_key_fixture

#### Function: `test_update_chunk_content` (line 152)

**Parameters**: get_api_key_fixture

#### Function: `test_update_chunk_available` (line 174)

**Parameters**: get_api_key_fixture

#### Function: `test_retrieve_chunks` (line 196)

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
from time import sleep


def test_parse_document_with_txt(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_parse_document")
    name = 'ragflow_test.txt'
    with open("test_data/ragflow_test.txt", "rb") as file:
        blob = file.read()
    docs = ds.upload_documents([{"display_name": name, "blob": blob}])
    doc = docs[0]
    ds.async_parse_documents(document_ids=[doc.id])
    '''
    for n in range(100):
        if doc.progress == 1:
            break
        sleep(1)
    else:
        raise Exception("Run time ERROR: Document parsing did not complete in time.")
    '''


def test_parse_and_cancel_document(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_parse_and_cancel_document")
    name = 'ragflow_test.txt'
    with open("test_data/ragflow_test.txt", "rb") as file:
        blob = file.read()
    docs = ds.upload_documents([{"display_name": name, "blob": blob}])
    doc = docs[0]
    ds.async_parse_documents(document_ids=[doc.id])
    sleep(1)
    if 0 < doc.progress < 1:
        ds.async_cancel_parse_documents(document_ids=[doc.id])


def test_bulk_parse_documents(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_bulk_parse_and_cancel_documents")
    with open("test_data/ragflow.txt", "rb") as file:
        blob = file.read()
    documents = [
        {'display_name': 'test1.txt', 'blob': blob},
        {'display_name': 'test2.txt', 'blob': blob},
        {'display_name': 'test3.txt', 'blob': blob}
    ]
    docs = ds.upload_documents(documents)
    ids = [doc.id for doc in docs]
    ds.async_parse_documents(ids)
    '''
    for n in range(100):
        all_completed = all(doc.progress == 1 for doc in docs)
        if all_completed:
            break
        sleep(1)
    else:
        raise Exception("Run time ERROR: Bulk document parsing did not complete in time.")
    '''


def test_list_chunks_with_success(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_list_chunks_with_success")
    with open("test_data/ragflow_test.txt", "rb") as file:
        blob = file.read()
    '''
    # chunk_size = 1024 * 1024
    # chunks = [blob[i:i + chunk_size] for i in range(0, len(blob), chunk_size)]
    documents = [
        {'display_name': f'chunk_{i}.txt', 'blob': chunk} for i, chunk in enumerate(chunks)
    ]
    '''
    documents = [{"display_name": "test_list_chunks_with_success.txt", "blob": blob}]
    docs = ds.upload_documents(documents)
    ids = [doc.id for doc in docs]
    ds.async_parse_documents(ids)
    '''
    for n in range(100):
        all_completed = all(doc.progress == 1 for doc in docs)
        if all_completed:
            break
        sleep(1)
    else:
        raise Exception("Run time ERROR: Chunk document parsing did not complete in time.")
    '''
    doc = docs[0]
    doc.list_chunks()


def test_add_chunk_with_success(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_add_chunk_with_success")
    with open("test_data/ragflow_test.txt", "rb") as file:
        blob = file.read()
    '''
    # chunk_size = 1024 * 1024
    # chunks = [blob[i:i + chunk_size] for i in range(0, len(blob), chunk_size)]
    documents = [
        {'display_name': f'chunk_{i}.txt', 'blob': chunk} for i, chunk in enumerate(chunks)
    ]
    '''
    documents = [{"display_name": "test_list_chunks_with_success.txt", "blob": blob}]
    docs = ds.upload_documents(documents)
    doc = docs[0]
    doc.add_chunk(content="This is a chunk addition test")


def test_delete_chunk_with_success(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_delete_chunk_with_success")
    with open("test_data/ragflow_test.txt", "rb") as file:
        blob = file.read()
    '''
    # chunk_size = 1024 * 1024
    # chunks = [blob[i:i + chunk_size] for i in range(0, len(blob), chunk_size)]
    documents = [
        {'display_name': f'chunk_{i}.txt', 'blob': chunk} for i, chunk in enumerate(chunks)
    ]
    '''
    documents = [{"display_name": "test_delete_chunk_with_success.txt", "blob": blob}]
    docs = ds.upload_documents(documents)
    doc = docs[0]
    chunk = doc.add_chunk(content="This is a chunk addition test")
    sleep(5)
    doc.delete_chunks([chunk.id])


def test_update_chunk_content(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_update_chunk_content_with_success")
    with open("test_data/ragflow_test.txt", "rb") as file:
        blob = file.read()
    '''
    # chunk_size = 1024 * 1024
    # chunks = [blob[i:i + chunk_size] for i in range(0, len(blob), chunk_size)]
    documents = [
        {'display_name': f'chunk_{i}.txt', 'blob': chunk} for i, chunk in enumerate(chunks)
    ]
    '''
    documents = [{"display_name": "test_update_chunk_content_with_success.txt", "blob": blob}]
    docs = ds.upload_documents(documents)
    doc = docs[0]
    chunk = doc.add_chunk(content="This is a chunk addition test")
    # For Elasticsearch, the chunk is not searchable in shot time (~2s).
    sleep(3)
    chunk.update({"content": "This is a updated content"})


def test_update_chunk_available(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="test_update_chunk_available_with_success")
    with open("test_data/ragflow_test.txt", "rb") as file:
        blob = file.read()
    '''
    # chunk_size = 1024 * 1024
    # chunks = [blob[i:i + chunk_size] for i in range(0, len(blob), chunk_size)]
    documents = [
        {'display_name': f'chunk_{i}.txt', 'blob': chunk} for i, chunk in enumerate(chunks)
    ]
    '''
    documents = [{"display_name": "test_update_chunk_available_with_success.txt", "blob": blob}]
    docs = ds.upload_documents(documents)
    doc = docs[0]
    chunk = doc.add_chunk(content="This is a chunk addition test")
    # For Elasticsearch, the chunk is not searchable in shot time (~2s).
    sleep(3)
    chunk.update({"available": 0})


def test_retrieve_chunks(get_api_key_fixture):
    API_KEY = get_api_key_fixture
    rag = RAGFlow(API_KEY, HOST_ADDRESS)
    ds = rag.create_dataset(name="retrieval")
    with open("test_data/ragflow_test.txt", "rb") as file:
        blob = file.read()
    '''
    # chunk_size = 1024 * 1024
    # chunks = [blob[i:i + chunk_size] for i in range(0, len(blob), chunk_size)]
    documents = [
        {'display_name': f'chunk_{i}.txt', 'blob': chunk} for i, chunk in enumerate(chunks)
    ]
    '''
    documents = [{"display_name": "test_retrieve_chunks.txt", "blob": blob}]
    docs = ds.upload_documents(documents)
    doc = docs[0]
    doc.add_chunk(content="This is a chunk addition test")
    rag.retrieve(dataset_ids=[ds.id], document_ids=[doc.id])
    rag.delete_datasets(ids=[ds.id])

# test different parameters for the retrieval

```

## Detailed Analysis

### File Role in Repository

The file `sdk/python/test/test_sdk_api/t_chunk.py` is located in the `sdk/python/test/test_sdk_api` directory.

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
- [t_dataset.py](t_dataset.py_docs.md)
- [t_document.py](t_document.py_docs.md)
- [t_session.py](t_session.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
