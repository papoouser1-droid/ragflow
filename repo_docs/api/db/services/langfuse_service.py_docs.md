# File Documentation: api/db/services/langfuse_service.py

## File Metadata

- **Path**: `api/db/services/langfuse_service.py`
- **Extension**: `.py`
- **Lines**: 77
- **Characters**: 2,799
- **Size**: 2,799 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

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

### Classes (1)

- `TenantLangfuseService`: Class definition

### Imports (5)

- `from datetime import datetime`
- `import peewee`
- `from api.db.db_models import DB, TenantLangfuse`
- `from api.db.services.common_service import CommonService`
- `from common.time_utils import current_timestamp, datetime_format`

## Code Structure Analysis

- Total lines: 77
- Blank lines: 13 (16.9%)
- Comment lines: ~17 (22.1%)
- Code lines: ~47


## Dependencies and Imports

- `from datetime import datetime`
- `import peewee`
- `from api.db.db_models import DB, TenantLangfuse`
- `from api.db.services.common_service import CommonService`
- `from common.time_utils import current_timestamp, datetime_format`

## Design & Architecture

This file is located in the `api` directory, specifically within `api/db/services`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity

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
- Imports from `api.db.db_models`
- Imports from `api.db.services.common_service`
- Imports from `common.time_utils`
- Potential test file: `test_langfuse_service.py`

## Keywords

ANY, All, Apache, Authors, BASIS, CONDITIONS, CommonService, Copyright, DB, DoesNotExist, InfiniFlow, KIND, LICENSE, License, Licensed, None, Python, Reserved, Rights, See, TenantLangfuse, TenantLangfuseService, The, Unless, Version, WARRANTIES, WITHOUT, You, classmethod, delete_model, delete_ty_tenant_id, filter_by_tenant, filter_by_tenant_with_info, save, update_by_tenant

---
*Generated by RAGFlow Repository Documentation Generator*
