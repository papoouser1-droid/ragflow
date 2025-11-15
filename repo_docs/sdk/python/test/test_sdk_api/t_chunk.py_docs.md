# File Documentation: sdk/python/test/test_sdk_api/t_chunk.py

## File Metadata

- **Path**: `sdk/python/test/test_sdk_api/t_chunk.py`
- **Extension**: `.py`
- **Lines**: 217
- **Characters**: 7,942
- **Size**: 7,942 bytes
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


### Functions (9)

- `test_parse_document_with_txt()`: Function definition
- `test_parse_and_cancel_document()`: Function definition
- `test_bulk_parse_documents()`: Function definition
- `test_list_chunks_with_success()`: Function definition
- `test_add_chunk_with_success()`: Function definition
- `test_delete_chunk_with_success()`: Function definition
- `test_update_chunk_content()`: Function definition
- `test_update_chunk_available()`: Function definition
- `test_retrieve_chunks()`: Function definition

### Imports (3)

- `from ragflow_sdk import RAGFlow`
- `from common import HOST_ADDRESS`
- `from time import sleep`

## Code Structure Analysis

- Total lines: 217
- Blank lines: 21 (9.7%)
- Comment lines: ~48 (22.1%)
- Code lines: ~148


## Dependencies and Imports

- `from ragflow_sdk import RAGFlow`
- `from common import HOST_ADDRESS`
- `from time import sleep`

## Design & Architecture

This file is located in the `sdk` directory, specifically within `sdk/python/test/test_sdk_api`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 21 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `sdk/python/test/test_sdk_api/` directory
- Imports from `ragflow_sdk`
- Imports from `common`
- Imports from `time`

## Keywords

ANY, API_KEY, All, Apache, Authors, BASIS, Bulk, CONDITIONS, Chunk, Copyright, Document, ERROR, Elasticsearch, Exception, For, HOST_ADDRESS, InfiniFlow, KIND, LICENSE, License, Licensed, Python, RAGFlow, Reserved, Rights, Run, See, The, This, Unless, Version, WARRANTIES, WITHOUT, You, test_add_chunk_with_success, test_bulk_parse_documents, test_delete_chunk_with_success, test_list_chunks_with_success, test_parse_and_cancel_document, test_parse_document_with_txt, test_retrieve_chunks, test_update_chunk_available, test_update_chunk_content

---
*Generated by RAGFlow Repository Documentation Generator*
