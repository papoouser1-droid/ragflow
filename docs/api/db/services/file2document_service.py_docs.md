# Documentation: api/db/services/file2document_service.py

## File Metadata

- **Path**: `api/db/services/file2document_service.py`
- **Size**: 3547 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `api/db/services/file2document_service.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `datetime`
- `common.constants`
- `api.db.db_models`
- `api.db.db_models`
- `api.db.services.common_service`
- `api.db.services.document_service`
- `common.time_utils`

### Classes Defined

This file defines 1 class(es):

#### Class: `File2DocumentService` (line 26)

**Methods**: get_by_file_id, get_by_document_id, get_by_document_ids, insert, delete_by_file_id, delete_by_document_ids_or_file_ids, delete_by_document_id, update_by_file_id, get_storage_address

### Functions Defined

This file defines 9 function(s):

#### Function: `get_by_file_id` (line 31)

**Parameters**: cls, file_id

#### Function: `get_by_document_id` (line 37)

**Parameters**: cls, document_id

#### Function: `get_by_document_ids` (line 43)

**Parameters**: cls, document_ids

#### Function: `insert` (line 49)

**Parameters**: cls, obj

#### Function: `delete_by_file_id` (line 56)

**Parameters**: cls, file_id

#### Function: `delete_by_document_ids_or_file_ids` (line 61)

**Parameters**: cls, document_ids, file_ids

#### Function: `delete_by_document_id` (line 70)

**Parameters**: cls, doc_id

#### Function: `update_by_file_id` (line 75)

**Parameters**: cls, file_id, obj

#### Function: `get_storage_address` (line 83)

**Parameters**: cls, doc_id, file_id

## Original Source Code

```py
#
#  Copyright 2024 The InfiniFlow Authors. All Rights Reserved.
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
from datetime import datetime

from common.constants import FileSource
from api.db.db_models import DB
from api.db.db_models import File, File2Document
from api.db.services.common_service import CommonService
from api.db.services.document_service import DocumentService
from common.time_utils import current_timestamp, datetime_format


class File2DocumentService(CommonService):
    model = File2Document

    @classmethod
    @DB.connection_context()
    def get_by_file_id(cls, file_id):
        objs = cls.model.select().where(cls.model.file_id == file_id)
        return objs

    @classmethod
    @DB.connection_context()
    def get_by_document_id(cls, document_id):
        objs = cls.model.select().where(cls.model.document_id == document_id)
        return objs

    @classmethod
    @DB.connection_context()
    def get_by_document_ids(cls, document_ids):
        objs = cls.model.select().where(cls.model.document_id.in_(document_ids))
        return list(objs.dicts())

    @classmethod
    @DB.connection_context()
    def insert(cls, obj):
        if not cls.save(**obj):
            raise RuntimeError("Database error (File)!")
        return File2Document(**obj)

    @classmethod
    @DB.connection_context()
    def delete_by_file_id(cls, file_id):
        return cls.model.delete().where(cls.model.file_id == file_id).execute()

    @classmethod
    @DB.connection_context()
    def delete_by_document_ids_or_file_ids(cls, document_ids, file_ids):
        if not document_ids:
            return cls.model.delete().where(cls.model.file_id.in_(file_ids)).execute()
        elif not file_ids:
            return cls.model.delete().where(cls.model.document_id.in_(document_ids)).execute()
        return cls.model.delete().where(cls.model.document_id.in_(document_ids) | cls.model.file_id.in_(file_ids)).execute()

    @classmethod
    @DB.connection_context()
    def delete_by_document_id(cls, doc_id):
        return cls.model.delete().where(cls.model.document_id == doc_id).execute()

    @classmethod
    @DB.connection_context()
    def update_by_file_id(cls, file_id, obj):
        obj["update_time"] = current_timestamp()
        obj["update_date"] = datetime_format(datetime.now())
        cls.model.update(obj).where(cls.model.id == file_id).execute()
        return File2Document(**obj)

    @classmethod
    @DB.connection_context()
    def get_storage_address(cls, doc_id=None, file_id=None):
        if doc_id:
            f2d = cls.get_by_document_id(doc_id)
        else:
            f2d = cls.get_by_file_id(file_id)
        if f2d:
            file = File.get_by_id(f2d[0].file_id)
            if not file.source_type or file.source_type == FileSource.LOCAL:
                return file.parent_id, file.location
            doc_id = f2d[0].document_id

        assert doc_id, "please specify doc_id"
        e, doc = DocumentService.get_by_id(doc_id)
        return doc.kb_id, doc.location

```

## Detailed Analysis

### File Role in Repository

The file `api/db/services/file2document_service.py` is located in the `api/db/services` directory.

This file is part of the **API/Backend** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to services.

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
- [api_service.py](api_service.py_docs.md)
- [canvas_service.py](canvas_service.py_docs.md)
- [common_service.py](common_service.py_docs.md)
- [connector_service.py](connector_service.py_docs.md)
- [conversation_service.py](conversation_service.py_docs.md)
- [dialog_service.py](dialog_service.py_docs.md)
- [document_service.py](document_service.py_docs.md)
- [file_service.py](file_service.py_docs.md)
- [knowledgebase_service.py](knowledgebase_service.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
