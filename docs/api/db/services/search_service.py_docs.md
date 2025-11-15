# Documentation: api/db/services/search_service.py

## File Metadata

- **Path**: `api/db/services/search_service.py`
- **Size**: 4016 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `api/db/services/search_service.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `datetime`
- `peewee`
- `common.constants`
- `api.db.db_models`
- `api.db.services.common_service`
- `common.time_utils`

### Classes Defined

This file defines 1 class(es):

#### Class: `SearchService` (line 26)

**Methods**: save, accessible4deletion, get_detail, get_by_tenant_ids, delete_by_tenant_id

### Functions Defined

This file defines 5 function(s):

#### Function: `save` (line 30)

**Parameters**: cls

#### Function: `accessible4deletion` (line 40)

**Parameters**: cls, search_id, user_id

#### Function: `get_detail` (line 54)

**Parameters**: cls, search_id

#### Function: `get_by_tenant_ids` (line 80)

**Parameters**: cls, joined_tenant_ids, user_id, page_number, items_per_page, orderby, desc, keywords

#### Function: `delete_by_tenant_id` (line 117)

**Parameters**: cls, tenant_id

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

from peewee import fn

from common.constants import StatusEnum
from api.db.db_models import DB, Search, User
from api.db.services.common_service import CommonService
from common.time_utils import current_timestamp, datetime_format


class SearchService(CommonService):
    model = Search

    @classmethod
    def save(cls, **kwargs):
        kwargs["create_time"] = current_timestamp()
        kwargs["create_date"] = datetime_format(datetime.now())
        kwargs["update_time"] = current_timestamp()
        kwargs["update_date"] = datetime_format(datetime.now())
        obj = cls.model.create(**kwargs)
        return obj

    @classmethod
    @DB.connection_context()
    def accessible4deletion(cls, search_id, user_id) -> bool:
        search = (
            cls.model.select(cls.model.id)
            .where(
                cls.model.id == search_id,
                cls.model.created_by == user_id,
                cls.model.status == StatusEnum.VALID.value,
            )
            .first()
        )
        return search is not None

    @classmethod
    @DB.connection_context()
    def get_detail(cls, search_id):
        fields = [
            cls.model.id,
            cls.model.avatar,
            cls.model.tenant_id,
            cls.model.name,
            cls.model.description,
            cls.model.created_by,
            cls.model.search_config,
            cls.model.update_time,
            User.nickname,
            User.avatar.alias("tenant_avatar"),
        ]
        search = (
            cls.model.select(*fields)
            .join(User, on=((User.id == cls.model.tenant_id) & (User.status == StatusEnum.VALID.value)))
            .where((cls.model.id == search_id) & (cls.model.status == StatusEnum.VALID.value))
            .first()
            .to_dict()
        )
        if not search:
            return {}
        return search

    @classmethod
    @DB.connection_context()
    def get_by_tenant_ids(cls, joined_tenant_ids, user_id, page_number, items_per_page, orderby, desc, keywords):
        fields = [
            cls.model.id,
            cls.model.avatar,
            cls.model.tenant_id,
            cls.model.name,
            cls.model.description,
            cls.model.created_by,
            cls.model.status,
            cls.model.update_time,
            cls.model.create_time,
            User.nickname,
            User.avatar.alias("tenant_avatar"),
        ]
        query = (
            cls.model.select(*fields)
            .join(User, on=(cls.model.tenant_id == User.id))
            .where(((cls.model.tenant_id.in_(joined_tenant_ids)) | (cls.model.tenant_id == user_id)) & (
                        cls.model.status == StatusEnum.VALID.value))
        )

        if keywords:
            query = query.where(fn.LOWER(cls.model.name).contains(keywords.lower()))
        if desc:
            query = query.order_by(cls.model.getter_by(orderby).desc())
        else:
            query = query.order_by(cls.model.getter_by(orderby).asc())

        count = query.count()

        if page_number and items_per_page:
            query = query.paginate(page_number, items_per_page)

        return list(query.dicts()), count

    @classmethod
    @DB.connection_context()
    def delete_by_tenant_id(cls, tenant_id):
        return cls.model.delete().where(cls.model.tenant_id == tenant_id).execute()

```

## Detailed Analysis

### File Role in Repository

The file `api/db/services/search_service.py` is located in the `api/db/services` directory.

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
