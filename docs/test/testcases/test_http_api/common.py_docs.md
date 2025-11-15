# Documentation: test/testcases/test_http_api/common.py

## File Metadata

- **Path**: `test/testcases/test_http_api/common.py`
- **Size**: 9251 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_http_api/common.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `pathlib`
- `requests`
- `configs`
- `requests_toolbelt`
- `utils.file_utils`

### Functions Defined

This file defines 29 function(s):

#### Function: `create_dataset` (line 34)

**Parameters**: auth, payload

#### Function: `list_datasets` (line 39)

**Parameters**: auth, params

#### Function: `update_dataset` (line 44)

**Parameters**: auth, dataset_id, payload

#### Function: `delete_datasets` (line 49)

**Parameters**: auth, payload

#### Function: `batch_create_datasets` (line 54)

**Parameters**: auth, num

#### Function: `upload_documents` (line 63)

**Parameters**: auth, dataset_id, files_path

#### Function: `download_document` (line 91)

**Parameters**: auth, dataset_id, document_id, save_path

#### Function: `list_documents` (line 105)

**Parameters**: auth, dataset_id, params

#### Function: `update_document` (line 111)

**Parameters**: auth, dataset_id, document_id, payload

#### Function: `delete_documents` (line 117)

**Parameters**: auth, dataset_id, payload

#### Function: `parse_documents` (line 123)

**Parameters**: auth, dataset_id, payload

#### Function: `stop_parse_documents` (line 129)

**Parameters**: auth, dataset_id, payload

#### Function: `bulk_upload_documents` (line 135)

**Parameters**: auth, dataset_id, num, tmp_path

#### Function: `add_chunk` (line 148)

**Parameters**: auth, dataset_id, document_id, payload

#### Function: `list_chunks` (line 154)

**Parameters**: auth, dataset_id, document_id, params

#### Function: `update_chunk` (line 160)

**Parameters**: auth, dataset_id, document_id, chunk_id, payload

#### Function: `delete_chunks` (line 166)

**Parameters**: auth, dataset_id, document_id, payload

#### Function: `retrieval_chunks` (line 172)

**Parameters**: auth, payload

#### Function: `batch_add_chunks` (line 178)

**Parameters**: auth, dataset_id, document_id, num

#### Function: `create_chat_assistant` (line 187)

**Parameters**: auth, payload

#### Function: `list_chat_assistants` (line 193)

**Parameters**: auth, params

#### Function: `update_chat_assistant` (line 199)

**Parameters**: auth, chat_assistant_id, payload

#### Function: `delete_chat_assistants` (line 205)

**Parameters**: auth, payload

#### Function: `batch_create_chat_assistants` (line 211)

**Parameters**: auth, num

#### Function: `create_session_with_chat_assistant` (line 220)

**Parameters**: auth, chat_assistant_id, payload

#### Function: `list_session_with_chat_assistants` (line 226)

**Parameters**: auth, chat_assistant_id, params

#### Function: `update_session_with_chat_assistant` (line 232)

**Parameters**: auth, chat_assistant_id, session_id, payload

#### Function: `delete_session_with_chat_assistants` (line 238)

**Parameters**: auth, chat_assistant_id, payload

#### Function: `batch_add_sessions_with_chat_assistant` (line 244)

**Parameters**: auth, chat_assistant_id, num

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

import requests
from configs import HOST_ADDRESS, VERSION
from requests_toolbelt import MultipartEncoder
from utils.file_utils import create_txt_file

HEADERS = {"Content-Type": "application/json"}
DATASETS_API_URL = f"/api/{VERSION}/datasets"
FILE_API_URL = f"/api/{VERSION}/datasets/{{dataset_id}}/documents"
FILE_CHUNK_API_URL = f"/api/{VERSION}/datasets/{{dataset_id}}/chunks"
CHUNK_API_URL = f"/api/{VERSION}/datasets/{{dataset_id}}/documents/{{document_id}}/chunks"
CHAT_ASSISTANT_API_URL = f"/api/{VERSION}/chats"
SESSION_WITH_CHAT_ASSISTANT_API_URL = f"/api/{VERSION}/chats/{{chat_id}}/sessions"
SESSION_WITH_AGENT_API_URL = f"/api/{VERSION}/agents/{{agent_id}}/sessions"


# DATASET MANAGEMENT
def create_dataset(auth, payload=None, *, headers=HEADERS, data=None):
    res = requests.post(url=f"{HOST_ADDRESS}{DATASETS_API_URL}", headers=headers, auth=auth, json=payload, data=data)
    return res.json()


def list_datasets(auth, params=None, *, headers=HEADERS):
    res = requests.get(url=f"{HOST_ADDRESS}{DATASETS_API_URL}", headers=headers, auth=auth, params=params)
    return res.json()


def update_dataset(auth, dataset_id, payload=None, *, headers=HEADERS, data=None):
    res = requests.put(url=f"{HOST_ADDRESS}{DATASETS_API_URL}/{dataset_id}", headers=headers, auth=auth, json=payload, data=data)
    return res.json()


def delete_datasets(auth, payload=None, *, headers=HEADERS, data=None):
    res = requests.delete(url=f"{HOST_ADDRESS}{DATASETS_API_URL}", headers=headers, auth=auth, json=payload, data=data)
    return res.json()


def batch_create_datasets(auth, num):
    ids = []
    for i in range(num):
        res = create_dataset(auth, {"name": f"dataset_{i}"})
        ids.append(res["data"]["id"])
    return ids


# FILE MANAGEMENT WITHIN DATASET
def upload_documents(auth, dataset_id, files_path=None):
    url = f"{HOST_ADDRESS}{FILE_API_URL}".format(dataset_id=dataset_id)

    if files_path is None:
        files_path = []

    fields = []
    file_objects = []
    try:
        for fp in files_path:
            p = Path(fp)
            f = p.open("rb")
            fields.append(("file", (p.name, f)))
            file_objects.append(f)
        m = MultipartEncoder(fields=fields)

        res = requests.post(
            url=url,
            headers={"Content-Type": m.content_type},
            auth=auth,
            data=m,
        )
        return res.json()
    finally:
        for f in file_objects:
            f.close()


def download_document(auth, dataset_id, document_id, save_path):
    url = f"{HOST_ADDRESS}{FILE_API_URL}/{document_id}".format(dataset_id=dataset_id)
    res = requests.get(url=url, auth=auth, stream=True)
    try:
        if res.status_code == 200:
            with open(save_path, "wb") as f:
                for chunk in res.iter_content(chunk_size=8192):
                    f.write(chunk)
    finally:
        res.close()

    return res


def list_documents(auth, dataset_id, params=None):
    url = f"{HOST_ADDRESS}{FILE_API_URL}".format(dataset_id=dataset_id)
    res = requests.get(url=url, headers=HEADERS, auth=auth, params=params)
    return res.json()


def update_document(auth, dataset_id, document_id, payload=None):
    url = f"{HOST_ADDRESS}{FILE_API_URL}/{document_id}".format(dataset_id=dataset_id)
    res = requests.put(url=url, headers=HEADERS, auth=auth, json=payload)
    return res.json()


def delete_documents(auth, dataset_id, payload=None):
    url = f"{HOST_ADDRESS}{FILE_API_URL}".format(dataset_id=dataset_id)
    res = requests.delete(url=url, headers=HEADERS, auth=auth, json=payload)
    return res.json()


def parse_documents(auth, dataset_id, payload=None):
    url = f"{HOST_ADDRESS}{FILE_CHUNK_API_URL}".format(dataset_id=dataset_id)
    res = requests.post(url=url, headers=HEADERS, auth=auth, json=payload)
    return res.json()


def stop_parse_documents(auth, dataset_id, payload=None):
    url = f"{HOST_ADDRESS}{FILE_CHUNK_API_URL}".format(dataset_id=dataset_id)
    res = requests.delete(url=url, headers=HEADERS, auth=auth, json=payload)
    return res.json()


def bulk_upload_documents(auth, dataset_id, num, tmp_path):
    fps = []
    for i in range(num):
        fp = create_txt_file(tmp_path / f"ragflow_test_upload_{i}.txt")
        fps.append(fp)
    res = upload_documents(auth, dataset_id, fps)
    document_ids = []
    for document in res["data"]:
        document_ids.append(document["id"])
    return document_ids


# CHUNK MANAGEMENT WITHIN DATASET
def add_chunk(auth, dataset_id, document_id, payload=None):
    url = f"{HOST_ADDRESS}{CHUNK_API_URL}".format(dataset_id=dataset_id, document_id=document_id)
    res = requests.post(url=url, headers=HEADERS, auth=auth, json=payload)
    return res.json()


def list_chunks(auth, dataset_id, document_id, params=None):
    url = f"{HOST_ADDRESS}{CHUNK_API_URL}".format(dataset_id=dataset_id, document_id=document_id)
    res = requests.get(url=url, headers=HEADERS, auth=auth, params=params)
    return res.json()


def update_chunk(auth, dataset_id, document_id, chunk_id, payload=None):
    url = f"{HOST_ADDRESS}{CHUNK_API_URL}/{chunk_id}".format(dataset_id=dataset_id, document_id=document_id)
    res = requests.put(url=url, headers=HEADERS, auth=auth, json=payload)
    return res.json()


def delete_chunks(auth, dataset_id, document_id, payload=None):
    url = f"{HOST_ADDRESS}{CHUNK_API_URL}".format(dataset_id=dataset_id, document_id=document_id)
    res = requests.delete(url=url, headers=HEADERS, auth=auth, json=payload)
    return res.json()


def retrieval_chunks(auth, payload=None):
    url = f"{HOST_ADDRESS}/api/v1/retrieval"
    res = requests.post(url=url, headers=HEADERS, auth=auth, json=payload)
    return res.json()


def batch_add_chunks(auth, dataset_id, document_id, num):
    chunk_ids = []
    for i in range(num):
        res = add_chunk(auth, dataset_id, document_id, {"content": f"chunk test {i}"})
        chunk_ids.append(res["data"]["chunk"]["id"])
    return chunk_ids


# CHAT ASSISTANT MANAGEMENT
def create_chat_assistant(auth, payload=None):
    url = f"{HOST_ADDRESS}{CHAT_ASSISTANT_API_URL}"
    res = requests.post(url=url, headers=HEADERS, auth=auth, json=payload)
    return res.json()


def list_chat_assistants(auth, params=None):
    url = f"{HOST_ADDRESS}{CHAT_ASSISTANT_API_URL}"
    res = requests.get(url=url, headers=HEADERS, auth=auth, params=params)
    return res.json()


def update_chat_assistant(auth, chat_assistant_id, payload=None):
    url = f"{HOST_ADDRESS}{CHAT_ASSISTANT_API_URL}/{chat_assistant_id}"
    res = requests.put(url=url, headers=HEADERS, auth=auth, json=payload)
    return res.json()


def delete_chat_assistants(auth, payload=None):
    url = f"{HOST_ADDRESS}{CHAT_ASSISTANT_API_URL}"
    res = requests.delete(url=url, headers=HEADERS, auth=auth, json=payload)
    return res.json()


def batch_create_chat_assistants(auth, num):
    chat_assistant_ids = []
    for i in range(num):
        res = create_chat_assistant(auth, {"name": f"test_chat_assistant_{i}", "dataset_ids": []})
        chat_assistant_ids.append(res["data"]["id"])
    return chat_assistant_ids


# SESSION MANAGEMENT
def create_session_with_chat_assistant(auth, chat_assistant_id, payload=None):
    url = f"{HOST_ADDRESS}{SESSION_WITH_CHAT_ASSISTANT_API_URL}".format(chat_id=chat_assistant_id)
    res = requests.post(url=url, headers=HEADERS, auth=auth, json=payload)
    return res.json()


def list_session_with_chat_assistants(auth, chat_assistant_id, params=None):
    url = f"{HOST_ADDRESS}{SESSION_WITH_CHAT_ASSISTANT_API_URL}".format(chat_id=chat_assistant_id)
    res = requests.get(url=url, headers=HEADERS, auth=auth, params=params)
    return res.json()


def update_session_with_chat_assistant(auth, chat_assistant_id, session_id, payload=None):
    url = f"{HOST_ADDRESS}{SESSION_WITH_CHAT_ASSISTANT_API_URL}/{session_id}".format(chat_id=chat_assistant_id)
    res = requests.put(url=url, headers=HEADERS, auth=auth, json=payload)
    return res.json()


def delete_session_with_chat_assistants(auth, chat_assistant_id, payload=None):
    url = f"{HOST_ADDRESS}{SESSION_WITH_CHAT_ASSISTANT_API_URL}".format(chat_id=chat_assistant_id)
    res = requests.delete(url=url, headers=HEADERS, auth=auth, json=payload)
    return res.json()


def batch_add_sessions_with_chat_assistant(auth, chat_assistant_id, num):
    session_ids = []
    for i in range(num):
        res = create_session_with_chat_assistant(auth, chat_assistant_id, {"name": f"session_with_chat_assistant_{i}"})
        session_ids.append(res["data"]["id"])
    return session_ids

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_http_api/common.py` is located in the `test/testcases/test_http_api` directory.

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

- [conftest.py](conftest.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
