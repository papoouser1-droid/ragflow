# File Documentation: api/common/check_team_permission.py

## File Metadata

- **Path**: `api/common/check_team_permission.py`
- **Extension**: `.py`
- **Lines**: 60
- **Characters**: 1,897
- **Size**: 1,897 bytes
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


from api.db import TenantPermission
from api.db.db_models import File, Knowledgebase
from api.db.services.file_service import FileService
from api.db.services.knowledgebase_service import KnowledgebaseService
from api.db.services.user_service import TenantService


def check_kb_team_permission(kb: dict | Knowledgebase, other: str) -> bool:
    kb = kb.to_dict() if isinstance(kb, Knowledgebase) else kb

    kb_tenant_id = kb["tenant_id"]

    if kb_tenant_id == other:
        return True

    if kb["permission"] != TenantPermission.TEAM:
        return False

    joined_tenants = TenantService.get_joined_tenants_by_user_id(other)
    return any(tenant["tenant_id"] == kb_tenant_id for tenant in joined_tenants)


def check_file_team_permission(file: dict | File, other: str) -> bool:
    file = file.to_dict() if isinstance(file, File) else file

    file_tenant_id = file["tenant_id"]
    if file_tenant_id == other:
        return True

    file_id = file["id"]

    kb_ids = [kb_info["kb_id"] for kb_info in FileService.get_kb_id_by_file_id(file_id)]

    for kb_id in kb_ids:
        ok, kb = KnowledgebaseService.get_by_id(kb_id)
        if not ok:
            continue

        if check_kb_team_permission(kb, other):
            return True

    return False

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


### Functions (2)

- `check_kb_team_permission()`: Function definition
- `check_file_team_permission()`: Function definition

### Imports (5)

- `from api.db import TenantPermission`
- `from api.db.db_models import File, Knowledgebase`
- `from api.db.services.file_service import FileService`
- `from api.db.services.knowledgebase_service import KnowledgebaseService`
- `from api.db.services.user_service import TenantService`

## Code Structure Analysis

- Total lines: 60
- Blank lines: 17 (28.3%)
- Comment lines: ~15 (25.0%)
- Code lines: ~28


## Dependencies and Imports

- `from api.db import TenantPermission`
- `from api.db.db_models import File, Knowledgebase`
- `from api.db.services.file_service import FileService`
- `from api.db.services.knowledgebase_service import KnowledgebaseService`
- `from api.db.services.user_service import TenantService`

## Design & Architecture

This file is located in the `api` directory, specifically within `api/common`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 4 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `api/common/` directory
- Imports from `api.db`
- Imports from `api.db.db_models`
- Imports from `api.db.services.file_service`
- Imports from `api.db.services.knowledgebase_service`
- Imports from `api.db.services.user_service`
- Potential test file: `test_check_team_permission.py`

## Keywords

ANY, All, Apache, Authors, BASIS, CONDITIONS, Copyright, False, File, FileService, InfiniFlow, KIND, Knowledgebase, KnowledgebaseService, LICENSE, License, Licensed, Python, Reserved, Rights, See, TEAM, TenantPermission, TenantService, The, True, Unless, Version, WARRANTIES, WITHOUT, You, check_file_team_permission, check_kb_team_permission

---
*Generated by RAGFlow Repository Documentation Generator*
