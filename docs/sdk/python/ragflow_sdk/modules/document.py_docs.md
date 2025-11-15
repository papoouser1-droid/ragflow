# Documentation: sdk/python/ragflow_sdk/modules/document.py

## File Metadata

- **Path**: `sdk/python/ragflow_sdk/modules/document.py`
- **Size**: 3780 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/ragflow_sdk/modules/document.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `json`
- `base`
- `chunk`

### Classes Defined

This file defines 2 class(es):

#### Class: `Document` (line 23)

**Methods**: __init__, update, download, list_chunks, add_chunk, delete_chunks

#### Class: `ParserConfig` (line 24)

**Methods**: __init__

### Functions Defined

This file defines 7 function(s):

#### Function: `__init__` (line 28)

**Parameters**: self, rag, res_dict

#### Function: `update` (line 53)

**Parameters**: self, update_message

#### Function: `download` (line 65)

**Parameters**: self

#### Function: `list_chunks` (line 78)

**Parameters**: self, page, page_size, keywords, id

#### Function: `add_chunk` (line 90)

**Parameters**: self, content, important_keywords, questions

#### Function: `delete_chunks` (line 97)

**Parameters**: self, ids

#### Function: `__init__` (line 25)

**Parameters**: self, rag, res_dict

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

import json

from .base import Base
from .chunk import Chunk


class Document(Base):
    class ParserConfig(Base):
        def __init__(self, rag, res_dict):
            super().__init__(rag, res_dict)

    def __init__(self, rag, res_dict):
        self.id = ""
        self.name = ""
        self.thumbnail = None
        self.dataset_id = None
        self.chunk_method = "naive"
        self.parser_config = {"pages": [[1, 1000000]]}
        self.source_type = "local"
        self.type = ""
        self.created_by = ""
        self.size = 0
        self.token_count = 0
        self.chunk_count = 0
        self.progress = 0.0
        self.progress_msg = ""
        self.process_begin_at = None
        self.process_duration = 0.0
        self.run = "0"
        self.status = "1"
        self.meta_fields = {}
        for k in list(res_dict.keys()):
            if k not in self.__dict__:
                res_dict.pop(k)
        super().__init__(rag, res_dict)

    def update(self, update_message: dict):
        if "meta_fields" in update_message:
            if not isinstance(update_message["meta_fields"], dict):
                raise Exception("meta_fields must be a dictionary")
        res = self.put(f"/datasets/{self.dataset_id}/documents/{self.id}", update_message)
        res = res.json()
        if res.get("code") != 0:
            raise Exception(res["message"])

        self._update_from_dict(self.rag, res.get("data", {}))
        return self

    def download(self):
        res = self.get(f"/datasets/{self.dataset_id}/documents/{self.id}")
        error_keys = set(["code", "message"])
        try:
            response = res.json()
            actual_keys = set(response.keys())
            if actual_keys == error_keys:
                raise Exception(res.get("message"))
            else:
                return res.content
        except json.JSONDecodeError:
            return res.content

    def list_chunks(self, page=1, page_size=30, keywords="", id=""):
        data = {"keywords": keywords, "page": page, "page_size": page_size, "id": id}
        res = self.get(f"/datasets/{self.dataset_id}/documents/{self.id}/chunks", data)
        res = res.json()
        if res.get("code") == 0:
            chunks = []
            for data in res["data"].get("chunks"):
                chunk = Chunk(self.rag, data)
                chunks.append(chunk)
            return chunks
        raise Exception(res.get("message"))

    def add_chunk(self, content: str, important_keywords: list[str] = [], questions: list[str] = []):
        res = self.post(f"/datasets/{self.dataset_id}/documents/{self.id}/chunks", {"content": content, "important_keywords": important_keywords, "questions": questions})
        res = res.json()
        if res.get("code") == 0:
            return Chunk(self.rag, res["data"].get("chunk"))
        raise Exception(res.get("message"))

    def delete_chunks(self, ids: list[str] | None = None):
        res = self.rm(f"/datasets/{self.dataset_id}/documents/{self.id}/chunks", {"chunk_ids": ids})
        res = res.json()
        if res.get("code") != 0:
            raise Exception(res.get("message"))

```

## Detailed Analysis

### File Role in Repository

The file `sdk/python/ragflow_sdk/modules/document.py` is located in the `sdk/python/ragflow_sdk/modules` directory.

### Architecture Context

Files in this location typically handle concerns related to modules.

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

- [__init__.py](__init__.py_docs.md)
- [agent.py](agent.py_docs.md)
- [base.py](base.py_docs.md)
- [chat.py](chat.py_docs.md)
- [chunk.py](chunk.py_docs.md)
- [dataset.py](dataset.py_docs.md)
- [session.py](session.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
