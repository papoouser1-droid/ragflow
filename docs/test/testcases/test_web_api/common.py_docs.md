# Documentation: test/testcases/test_web_api/common.py

## File Metadata

- **Path**: `test/testcases/test_web_api/common.py`
- **Size**: 9406 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_web_api/common.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `pathlib`
- `requests`
- `configs`
- `requests_toolbelt`
- `utils.file_utils`

### Functions Defined

This file defines 32 function(s):

#### Function: `create_kb` (line 34)

**Parameters**: auth, payload

#### Function: `list_kbs` (line 39)

**Parameters**: auth, params, payload

#### Function: `update_kb` (line 46)

**Parameters**: auth, payload

#### Function: `rm_kb` (line 51)

**Parameters**: auth, payload

#### Function: `detail_kb` (line 56)

**Parameters**: auth, params

#### Function: `list_tags_from_kbs` (line 61)

**Parameters**: auth, params

#### Function: `list_tags` (line 66)

**Parameters**: auth, dataset_id, params

#### Function: `rm_tags` (line 71)

**Parameters**: auth, dataset_id, payload

#### Function: `rename_tags` (line 76)

**Parameters**: auth, dataset_id, payload

#### Function: `knowledge_graph` (line 81)

**Parameters**: auth, dataset_id, params

#### Function: `delete_knowledge_graph` (line 86)

**Parameters**: auth, dataset_id, payload

#### Function: `batch_create_datasets` (line 91)

**Parameters**: auth, num

#### Function: `upload_documents` (line 100)

**Parameters**: auth, payload, files_path

#### Function: `create_document` (line 132)

**Parameters**: auth, payload

#### Function: `list_documents` (line 137)

**Parameters**: auth, params, payload

#### Function: `delete_document` (line 144)

**Parameters**: auth, payload

#### Function: `parse_documents` (line 149)

**Parameters**: auth, payload

#### Function: `bulk_upload_documents` (line 154)

**Parameters**: auth, kb_id, num, tmp_path

#### Function: `add_chunk` (line 168)

**Parameters**: auth, payload

#### Function: `list_chunks` (line 173)

**Parameters**: auth, payload

#### Function: `get_chunk` (line 178)

**Parameters**: auth, params

#### Function: `update_chunk` (line 183)

**Parameters**: auth, payload

#### Function: `delete_chunks` (line 188)

**Parameters**: auth, payload

#### Function: `retrieval_chunks` (line 193)

**Parameters**: auth, payload

#### Function: `batch_add_chunks` (line 198)

**Parameters**: auth, doc_id, num

#### Function: `create_dialog` (line 207)

**Parameters**: auth, payload

#### Function: `update_dialog` (line 212)

**Parameters**: auth, payload

#### Function: `get_dialog` (line 217)

**Parameters**: auth, params

#### Function: `list_dialogs` (line 222)

**Parameters**: auth

#### Function: `delete_dialog` (line 227)

**Parameters**: auth, payload

#### Function: `batch_create_dialogs` (line 232)

**Parameters**: auth, num, kb_ids

#### Function: `delete_dialogs` (line 255)

**Parameters**: auth

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

KB_APP_URL = f"/{VERSION}/kb"
DOCUMENT_APP_URL = f"/{VERSION}/document"
CHUNK_API_URL = f"/{VERSION}/chunk"
DIALOG_APP_URL = f"/{VERSION}/dialog"
# SESSION_WITH_CHAT_ASSISTANT_API_URL = "/api/v1/chats/{chat_id}/sessions"
# SESSION_WITH_AGENT_API_URL = "/api/v1/agents/{agent_id}/sessions"


# KB APP
def create_kb(auth, payload=None, *, headers=HEADERS, data=None):
    res = requests.post(url=f"{HOST_ADDRESS}{KB_APP_URL}/create", headers=headers, auth=auth, json=payload, data=data)
    return res.json()


def list_kbs(auth, params=None, payload=None, *, headers=HEADERS, data=None):
    if payload is None:
        payload = {}
    res = requests.post(url=f"{HOST_ADDRESS}{KB_APP_URL}/list", headers=headers, auth=auth, params=params, json=payload, data=data)
    return res.json()


def update_kb(auth, payload=None, *, headers=HEADERS, data=None):
    res = requests.post(url=f"{HOST_ADDRESS}{KB_APP_URL}/update", headers=headers, auth=auth, json=payload, data=data)
    return res.json()


def rm_kb(auth, payload=None, *, headers=HEADERS, data=None):
    res = requests.post(url=f"{HOST_ADDRESS}{KB_APP_URL}/rm", headers=headers, auth=auth, json=payload, data=data)
    return res.json()


def detail_kb(auth, params=None, *, headers=HEADERS):
    res = requests.get(url=f"{HOST_ADDRESS}{KB_APP_URL}/detail", headers=headers, auth=auth, params=params)
    return res.json()


def list_tags_from_kbs(auth, params=None, *, headers=HEADERS):
    res = requests.get(url=f"{HOST_ADDRESS}{KB_APP_URL}/tags", headers=headers, auth=auth, params=params)
    return res.json()


def list_tags(auth, dataset_id, params=None, *, headers=HEADERS):
    res = requests.get(url=f"{HOST_ADDRESS}{KB_APP_URL}/{dataset_id}/tags", headers=headers, auth=auth, params=params)
    return res.json()


def rm_tags(auth, dataset_id, payload=None, *, headers=HEADERS, data=None):
    res = requests.post(url=f"{HOST_ADDRESS}{KB_APP_URL}/{dataset_id}/rm_tags", headers=headers, auth=auth, json=payload, data=data)
    return res.json()


def rename_tags(auth, dataset_id, payload=None, *, headers=HEADERS, data=None):
    res = requests.post(url=f"{HOST_ADDRESS}{KB_APP_URL}/{dataset_id}/rename_tags", headers=headers, auth=auth, json=payload, data=data)
    return res.json()


def knowledge_graph(auth, dataset_id, params=None, *, headers=HEADERS):
    res = requests.get(url=f"{HOST_ADDRESS}{KB_APP_URL}/{dataset_id}/knowledge_graph", headers=headers, auth=auth, params=params)
    return res.json()


def delete_knowledge_graph(auth, dataset_id, payload=None, *, headers=HEADERS, data=None):
    res = requests.delete(url=f"{HOST_ADDRESS}{KB_APP_URL}/{dataset_id}/delete_knowledge_graph", headers=headers, auth=auth, json=payload, data=data)
    return res.json()


def batch_create_datasets(auth, num):
    ids = []
    for i in range(num):
        res = create_kb(auth, {"name": f"kb_{i}"})
        ids.append(res["data"]["kb_id"])
    return ids


# DOCUMENT APP
def upload_documents(auth, payload=None, files_path=None):
    url = f"{HOST_ADDRESS}{DOCUMENT_APP_URL}/upload"

    if files_path is None:
        files_path = []

    fields = []
    file_objects = []
    try:
        if payload:
            for k, v in payload.items():
                fields.append((k, str(v)))

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


def create_document(auth, payload=None, *, headers=HEADERS, data=None):
    res = requests.post(url=f"{HOST_ADDRESS}{DOCUMENT_APP_URL}/create", headers=headers, auth=auth, json=payload, data=data)
    return res.json()


def list_documents(auth, params=None, payload=None, *, headers=HEADERS, data=None):
    if payload is None:
        payload = {}
    res = requests.post(url=f"{HOST_ADDRESS}{DOCUMENT_APP_URL}/list", headers=headers, auth=auth, params=params, json=payload, data=data)
    return res.json()


def delete_document(auth, payload=None, *, headers=HEADERS, data=None):
    res = requests.post(url=f"{HOST_ADDRESS}{DOCUMENT_APP_URL}/rm", headers=headers, auth=auth, json=payload, data=data)
    return res.json()


def parse_documents(auth, payload=None, *, headers=HEADERS, data=None):
    res = requests.post(url=f"{HOST_ADDRESS}{DOCUMENT_APP_URL}/run", headers=headers, auth=auth, json=payload, data=data)
    return res.json()


def bulk_upload_documents(auth, kb_id, num, tmp_path):
    fps = []
    for i in range(num):
        fp = create_txt_file(tmp_path / f"ragflow_test_upload_{i}.txt")
        fps.append(fp)

    res = upload_documents(auth, {"kb_id": kb_id}, fps)
    document_ids = []
    for document in res["data"]:
        document_ids.append(document["id"])
    return document_ids


# CHUNK APP
def add_chunk(auth, payload=None, *, headers=HEADERS, data=None):
    res = requests.post(url=f"{HOST_ADDRESS}{CHUNK_API_URL}/create", headers=headers, auth=auth, json=payload, data=data)
    return res.json()


def list_chunks(auth, payload=None, *, headers=HEADERS):
    res = requests.post(url=f"{HOST_ADDRESS}{CHUNK_API_URL}/list", headers=headers, auth=auth, json=payload)
    return res.json()


def get_chunk(auth, params=None, *, headers=HEADERS):
    res = requests.get(url=f"{HOST_ADDRESS}{CHUNK_API_URL}/get", headers=headers, auth=auth, params=params)
    return res.json()


def update_chunk(auth, payload=None, *, headers=HEADERS):
    res = requests.post(url=f"{HOST_ADDRESS}{CHUNK_API_URL}/set", headers=headers, auth=auth, json=payload)
    return res.json()


def delete_chunks(auth, payload=None, *, headers=HEADERS):
    res = requests.post(url=f"{HOST_ADDRESS}{CHUNK_API_URL}/rm", headers=headers, auth=auth, json=payload)
    return res.json()


def retrieval_chunks(auth, payload=None, *, headers=HEADERS):
    res = requests.post(url=f"{HOST_ADDRESS}{CHUNK_API_URL}/retrieval_test", headers=headers, auth=auth, json=payload)
    return res.json()


def batch_add_chunks(auth, doc_id, num):
    chunk_ids = []
    for i in range(num):
        res = add_chunk(auth, {"doc_id": doc_id, "content_with_weight": f"chunk test {i}"})
        chunk_ids.append(res["data"]["chunk_id"])
    return chunk_ids


# DIALOG APP
def create_dialog(auth, payload=None, *, headers=HEADERS, data=None):
    res = requests.post(url=f"{HOST_ADDRESS}{DIALOG_APP_URL}/set", headers=headers, auth=auth, json=payload, data=data)
    return res.json()


def update_dialog(auth, payload=None, *, headers=HEADERS, data=None):
    res = requests.post(url=f"{HOST_ADDRESS}{DIALOG_APP_URL}/set", headers=headers, auth=auth, json=payload, data=data)
    return res.json()


def get_dialog(auth, params=None, *, headers=HEADERS):
    res = requests.get(url=f"{HOST_ADDRESS}{DIALOG_APP_URL}/get", headers=headers, auth=auth, params=params)
    return res.json()


def list_dialogs(auth, *, headers=HEADERS):
    res = requests.get(url=f"{HOST_ADDRESS}{DIALOG_APP_URL}/list", headers=headers, auth=auth)
    return res.json()


def delete_dialog(auth, payload=None, *, headers=HEADERS, data=None):
    res = requests.post(url=f"{HOST_ADDRESS}{DIALOG_APP_URL}/rm", headers=headers, auth=auth, json=payload, data=data)
    return res.json()


def batch_create_dialogs(auth, num, kb_ids=None):
    if kb_ids is None:
        kb_ids = []

    dialog_ids = []
    for i in range(num):
        payload = {
            "name": f"dialog_{i}",
            "description": f"Test dialog {i}",
            "kb_ids": kb_ids,
            "prompt_config": {"system": "You are a helpful assistant. Use the following knowledge to answer questions: {knowledge}", "parameters": [{"key": "knowledge", "optional": False}]},
            "top_n": 6,
            "top_k": 1024,
            "similarity_threshold": 0.1,
            "vector_similarity_weight": 0.3,
            "llm_setting": {"model": "gpt-3.5-turbo", "temperature": 0.7},
        }
        res = create_dialog(auth, payload)
        if res["code"] == 0:
            dialog_ids.append(res["data"]["id"])
    return dialog_ids


def delete_dialogs(auth):
    res = list_dialogs(auth)
    if res["code"] == 0 and res["data"]:
        dialog_ids = [dialog["id"] for dialog in res["data"]]
        if dialog_ids:
            delete_dialog(auth, {"dialog_ids": dialog_ids})

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_web_api/common.py` is located in the `test/testcases/test_web_api` directory.

This file is part of the **Testing** infrastructure.

### Architecture Context

Files in this location typically handle concerns related to test_web_api.

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
