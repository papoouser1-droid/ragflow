# Documentation: api/db/services/langfuse_service.py

## File Metadata

- **Path**: `api/db/services/langfuse_service.py`
- **Size**: 2799 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `api/db/services/langfuse_service.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `datetime`
- `peewee`
- `api.db.db_models`
- `api.db.services.common_service`
- `common.time_utils`

### Classes Defined

This file defines 1 class(es):

#### Class: `TenantLangfuseService` (line 26)

**Docstring**: All methods that modify the status should be enclosed within a DB.atomic() context to ensure atomicity
and maintain data integrity in case of errors during execution....

**Methods**: filter_by_tenant, filter_by_tenant_with_info, delete_ty_tenant_id, update_by_tenant, save, delete_model

### Functions Defined

This file defines 6 function(s):

#### Function: `filter_by_tenant` (line 36)

**Parameters**: cls, tenant_id

#### Function: `filter_by_tenant_with_info` (line 46)

**Parameters**: cls, tenant_id

#### Function: `delete_ty_tenant_id` (line 56)

**Parameters**: cls, tenant_id

#### Function: `update_by_tenant` (line 60)

**Parameters**: cls, tenant_id, langfuse_keys

#### Function: `save` (line 66)

**Parameters**: cls

#### Function: `delete_model` (line 75)

**Parameters**: cls, langfuse_model

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

from datetime import datetime

import peewee

from api.db.db_models import DB, TenantLangfuse
from api.db.services.common_service import CommonService
from common.time_utils import current_timestamp, datetime_format


class TenantLangfuseService(CommonService):
    """
    All methods that modify the status should be enclosed within a DB.atomic() context to ensure atomicity
    and maintain data integrity in case of errors during execution.
    """

    model = TenantLangfuse

    @classmethod
    @DB.connection_context()
    def filter_by_tenant(cls, tenant_id):
        fields = [cls.model.tenant_id, cls.model.host, cls.model.secret_key, cls.model.public_key]
        try:
            keys = cls.model.select(*fields).where(cls.model.tenant_id == tenant_id).first()
            return keys
        except peewee.DoesNotExist:
            return None

    @classmethod
    @DB.connection_context()
    def filter_by_tenant_with_info(cls, tenant_id):
        fields = [cls.model.tenant_id, cls.model.host, cls.model.secret_key, cls.model.public_key]
        try:
            keys = cls.model.select(*fields).where(cls.model.tenant_id == tenant_id).dicts().first()
            return keys
        except peewee.DoesNotExist:
            return None

    @classmethod
    @DB.connection_context()
    def delete_ty_tenant_id(cls, tenant_id):
        return cls.model.delete().where(cls.model.tenant_id == tenant_id).execute()

    @classmethod
    def update_by_tenant(cls, tenant_id, langfuse_keys):
        langfuse_keys["update_time"] = current_timestamp()
        langfuse_keys["update_date"] = datetime_format(datetime.now())
        return cls.model.update(**langfuse_keys).where(cls.model.tenant_id == tenant_id).execute()

    @classmethod
    def save(cls, **kwargs):
        kwargs["create_time"] = current_timestamp()
        kwargs["create_date"] = datetime_format(datetime.now())
        kwargs["update_time"] = current_timestamp()
        kwargs["update_date"] = datetime_format(datetime.now())
        obj = cls.model.create(**kwargs)
        return obj

    @classmethod
    def delete_model(cls, langfuse_model):
        langfuse_model.delete_instance()

```

## Detailed Analysis

### File Role in Repository

The file `api/db/services/langfuse_service.py` is located in the `api/db/services` directory.

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
- [file2document_service.py](file2document_service.py_docs.md)
- [file_service.py](file_service.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
