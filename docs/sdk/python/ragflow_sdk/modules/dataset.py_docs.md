# Documentation: sdk/python/ragflow_sdk/modules/dataset.py

## File Metadata

- **Path**: `sdk/python/ragflow_sdk/modules/dataset.py`
- **Size**: 5420 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/ragflow_sdk/modules/dataset.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `base`
- `document`
- `time`

### Classes Defined

This file defines 2 class(es):

#### Class: `DataSet` (line 21)

**Methods**: __init__, update, upload_documents, list_documents, delete_documents, _get_documents_status, async_parse_documents, parse_documents, async_cancel_parse_documents

#### Class: `ParserConfig` (line 22)

**Methods**: __init__

### Functions Defined

This file defines 11 function(s):

#### Function: `__init__` (line 26)

**Parameters**: self, rag, res_dict

#### Function: `update` (line 44)

**Parameters**: self, update_message

#### Function: `upload_documents` (line 53)

**Parameters**: self, document_list

#### Function: `list_documents` (line 66)

**Parameters**: self, id, name, keywords, page, page_size, orderby, desc, create_time_from, create_time_to

#### Function: `delete_documents` (line 98)

**Parameters**: self, ids

#### Function: `_get_documents_status` (line 105)

**Parameters**: self, document_ids

#### Function: `async_parse_documents` (line 132)

**Parameters**: self, document_ids

#### Function: `parse_documents` (line 139)

**Parameters**: self, document_ids

#### Function: `async_cancel_parse_documents` (line 149)

**Parameters**: self, document_ids

#### Function: `__init__` (line 23)

**Parameters**: self, rag, res_dict

#### Function: `fetch_doc` (line 113)

**Parameters**: doc_id

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

from .base import Base
from .document import Document


class DataSet(Base):
    class ParserConfig(Base):
        def __init__(self, rag, res_dict):
            super().__init__(rag, res_dict)

    def __init__(self, rag, res_dict):
        self.id = ""
        self.name = ""
        self.avatar = ""
        self.tenant_id = None
        self.description = ""
        self.embedding_model = ""
        self.permission = "me"
        self.document_count = 0
        self.chunk_count = 0
        self.chunk_method = "naive"
        self.parser_config = None
        self.pagerank = 0
        for k in list(res_dict.keys()):
            if k not in self.__dict__:
                res_dict.pop(k)
        super().__init__(rag, res_dict)

    def update(self, update_message: dict):
        res = self.put(f"/datasets/{self.id}", update_message)
        res = res.json()
        if res.get("code") != 0:
            raise Exception(res["message"])

        self._update_from_dict(self.rag, res.get("data", {}))
        return self

    def upload_documents(self, document_list: list[dict]):
        url = f"/datasets/{self.id}/documents"
        files = [("file", (ele["display_name"], ele["blob"])) for ele in document_list]
        res = self.post(path=url, json=None, files=files)
        res = res.json()
        if res.get("code") == 0:
            doc_list = []
            for doc in res["data"]:
                document = Document(self.rag, doc)
                doc_list.append(document)
            return doc_list
        raise Exception(res.get("message"))

    def list_documents(
        self,
        id: str | None = None,
        name: str | None = None,
        keywords: str | None = None,
        page: int = 1,
        page_size: int = 30,
        orderby: str = "create_time",
        desc: bool = True,
        create_time_from: int = 0,
        create_time_to: int = 0,
    ):
        params = {
            "id": id,
            "name": name,
            "keywords": keywords,
            "page": page,
            "page_size": page_size,
            "orderby": orderby,
            "desc": desc,
            "create_time_from": create_time_from,
            "create_time_to": create_time_to,
        }
        res = self.get(f"/datasets/{self.id}/documents", params=params)
        res = res.json()
        documents = []
        if res.get("code") == 0:
            for document in res["data"].get("docs"):
                documents.append(Document(self.rag, document))
            return documents
        raise Exception(res["message"])

    def delete_documents(self, ids: list[str] | None = None):
        res = self.rm(f"/datasets/{self.id}/documents", {"ids": ids})
        res = res.json()
        if res.get("code") != 0:
            raise Exception(res["message"])
        
    
    def _get_documents_status(self, document_ids):
        import time
        terminal_states = {"DONE", "FAIL", "CANCEL"}
        interval_sec = 1
        pending = set(document_ids)
        finished = []
        while pending:
            for doc_id in list(pending):
                def fetch_doc(doc_id: str) -> Document | None:
                    try:
                        docs = self.list_documents(id=doc_id)
                        return docs[0] if docs else None
                    except Exception:
                        return None
                doc = fetch_doc(doc_id)
                if doc is None:
                    continue
                if isinstance(doc.run, str) and doc.run.upper() in terminal_states:
                    finished.append((doc_id, doc.run, doc.chunk_count, doc.token_count))
                    pending.discard(doc_id)
                elif float(doc.progress or 0.0) >= 1.0:
                    finished.append((doc_id, "DONE", doc.chunk_count, doc.token_count))
                    pending.discard(doc_id)
            if pending:
                time.sleep(interval_sec)
        return finished
    
    def async_parse_documents(self, document_ids):
        res = self.post(f"/datasets/{self.id}/chunks", {"document_ids": document_ids})
        res = res.json()
        if res.get("code") != 0:
            raise Exception(res.get("message"))
        

    def parse_documents(self, document_ids):
        try:
            self.async_parse_documents(document_ids)
            self._get_documents_status(document_ids)
        except KeyboardInterrupt:
            self.async_cancel_parse_documents(document_ids)
            
        return self._get_documents_status(document_ids)


    def async_cancel_parse_documents(self, document_ids):
        res = self.rm(f"/datasets/{self.id}/chunks", {"document_ids": document_ids})
        res = res.json()
        if res.get("code") != 0:
            raise Exception(res.get("message"))

```

## Detailed Analysis

### File Role in Repository

The file `sdk/python/ragflow_sdk/modules/dataset.py` is located in the `sdk/python/ragflow_sdk/modules` directory.

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
- [document.py](document.py_docs.md)
- [session.py](session.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
