# Documentation: sdk/python/test/test_frontend_api/common.py

## File Metadata

- **Path**: `sdk/python/test/test_frontend_api/common.py`
- **Size**: 2876 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/test/test_frontend_api/common.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `os`
- `requests`

### Functions Defined

This file defines 9 function(s):

#### Function: `create_dataset` (line 26)

**Parameters**: auth, dataset_name

#### Function: `list_dataset` (line 34)

**Parameters**: auth, page_number, page_size

#### Function: `rm_dataset` (line 42)

**Parameters**: auth, dataset_id

#### Function: `update_dataset` (line 50)

**Parameters**: auth, json_req

#### Function: `upload_file` (line 57)

**Parameters**: auth, dataset_id, path

#### Function: `list_document` (line 70)

**Parameters**: auth, dataset_id

#### Function: `get_docs_info` (line 78)

**Parameters**: auth, doc_ids

#### Function: `parse_docs` (line 86)

**Parameters**: auth, doc_ids

#### Function: `parse_file` (line 94)

**Parameters**: auth, document_id

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

import requests

HOST_ADDRESS = os.getenv("HOST_ADDRESS", "http://127.0.0.1:9380")

DATASET_NAME_LIMIT = 128


def create_dataset(auth, dataset_name):
    authorization = {"Authorization": auth}
    url = f"{HOST_ADDRESS}/v1/kb/create"
    json = {"name": dataset_name}
    res = requests.post(url=url, headers=authorization, json=json)
    return res.json()


def list_dataset(auth, page_number, page_size=30):
    authorization = {"Authorization": auth}
    url = f"{HOST_ADDRESS}/v1/kb/list?page={page_number}&page_size={page_size}"
    json = {}
    res = requests.post(url=url, headers=authorization, json=json)
    return res.json()


def rm_dataset(auth, dataset_id):
    authorization = {"Authorization": auth}
    url = f"{HOST_ADDRESS}/v1/kb/rm"
    json = {"kb_id": dataset_id}
    res = requests.post(url=url, headers=authorization, json=json)
    return res.json()


def update_dataset(auth, json_req):
    authorization = {"Authorization": auth}
    url = f"{HOST_ADDRESS}/v1/kb/update"
    res = requests.post(url=url, headers=authorization, json=json_req)
    return res.json()


def upload_file(auth, dataset_id, path):
    authorization = {"Authorization": auth}
    url = f"{HOST_ADDRESS}/v1/document/upload"
    json_req = {
        "kb_id": dataset_id,
    }

    file = {"file": open(f"{path}", "rb")}

    res = requests.post(url=url, headers=authorization, files=file, data=json_req)
    return res.json()


def list_document(auth, dataset_id):
    authorization = {"Authorization": auth}
    url = f"{HOST_ADDRESS}/v1/document/list?kb_id={dataset_id}"
    json = {}
    res = requests.post(url=url, headers=authorization, json=json)
    return res.json()


def get_docs_info(auth, doc_ids):
    authorization = {"Authorization": auth}
    json_req = {"doc_ids": doc_ids}
    url = f"{HOST_ADDRESS}/v1/document/infos"
    res = requests.post(url=url, headers=authorization, json=json_req)
    return res.json()


def parse_docs(auth, doc_ids):
    authorization = {"Authorization": auth}
    json_req = {"doc_ids": doc_ids, "run": 1}
    url = f"{HOST_ADDRESS}/v1/document/run"
    res = requests.post(url=url, headers=authorization, json=json_req)
    return res.json()


def parse_file(auth, document_id):
    pass


```

## Detailed Analysis

### File Role in Repository

The file `sdk/python/test/test_frontend_api/common.py` is located in the `sdk/python/test/test_frontend_api` directory.

This file is part of the **Testing** infrastructure.

### Architecture Context

Files in this location typically handle concerns related to test_frontend_api.

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

- [get_email.py](get_email.py_docs.md)
- [test_chunk.py](test_chunk.py_docs.md)
- [test_dataset.py](test_dataset.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
