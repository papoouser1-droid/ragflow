# File Documentation: api/db/services/search_service.py

## File Metadata

- **Path**: `api/db/services/search_service.py`
- **Extension**: `.py`
- **Lines**: 119
- **Characters**: 4,016
- **Size**: 4,016 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

## Original Source

```python
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

## High-Level Overview

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

## Detailed Walkthrough

### Classes (1)

- `SearchService`: Class definition

### Imports (6)

- `from datetime import datetime`
- `from peewee import fn`
- `from common.constants import StatusEnum`
- `from api.db.db_models import DB, Search, User`
- `from api.db.services.common_service import CommonService`
- `from common.time_utils import current_timestamp, datetime_format`

## Code Structure Analysis

- Total lines: 119
- Blank lines: 14 (11.8%)
- Comment lines: ~15 (12.6%)
- Code lines: ~90


## Dependencies and Imports

- `from datetime import datetime`
- `from peewee import fn`
- `from common.constants import StatusEnum`
- `from api.db.db_models import DB, Search, User`
- `from api.db.services.common_service import CommonService`
- `from common.time_utils import current_timestamp, datetime_format`

## Design & Architecture

This file is located in the `api` directory, specifically within `api/db/services`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity
- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `api/db/services/` directory
- Imports from `datetime`
- Imports from `peewee`
- Imports from `common.constants`
- Imports from `api.db.db_models`
- Imports from `api.db.services.common_service`
- Potential test file: `test_search_service.py`

## Keywords

ANY, All, Apache, Authors, BASIS, CONDITIONS, CommonService, Copyright, DB, InfiniFlow, KIND, LICENSE, LOWER, License, Licensed, None, Python, Reserved, Rights, Search, SearchService, See, StatusEnum, The, Unless, User, VALID, Version, WARRANTIES, WITHOUT, You, accessible4deletion, classmethod, delete_by_tenant_id, get_by_tenant_ids, get_detail, save

---
*Generated by RAGFlow Repository Documentation Generator*
