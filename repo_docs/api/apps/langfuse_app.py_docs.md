# File Documentation: api/apps/langfuse_app.py

## File Metadata

- **Path**: `api/apps/langfuse_app.py`
- **Extension**: `.py`
- **Lines**: 98
- **Characters**: 3,751
- **Size**: 3,751 bytes
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


from flask import request
from flask_login import current_user, login_required
from langfuse import Langfuse

from api.db.db_models import DB
from api.db.services.langfuse_service import TenantLangfuseService
from api.utils.api_utils import get_error_data_result, get_json_result, server_error_response, validate_request


@manager.route("/api_key", methods=["POST", "PUT"])  # noqa: F821
@login_required
@validate_request("secret_key", "public_key", "host")
def set_api_key():
    req = request.get_json()
    secret_key = req.get("secret_key", "")
    public_key = req.get("public_key", "")
    host = req.get("host", "")
    if not all([secret_key, public_key, host]):
        return get_error_data_result(message="Missing required fields")

    langfuse_keys = dict(
        tenant_id=current_user.id,
        secret_key=secret_key,
        public_key=public_key,
        host=host,
    )

    langfuse = Langfuse(public_key=langfuse_keys["public_key"], secret_key=langfuse_keys["secret_key"], host=langfuse_keys["host"])
    if not langfuse.auth_check():
        return get_error_data_result(message="Invalid Langfuse keys")

    langfuse_entry = TenantLangfuseService.filter_by_tenant(tenant_id=current_user.id)
    with DB.atomic():
        try:
            if not langfuse_entry:
                TenantLangfuseService.save(**langfuse_keys)
            else:
                TenantLangfuseService.update_by_tenant(tenant_id=current_user.id, langfuse_keys=langfuse_keys)
            return get_json_result(data=langfuse_keys)
        except Exception as e:
            server_error_response(e)


@manager.route("/api_key", methods=["GET"])  # noqa: F821
@login_required
@validate_request()
def get_api_key():
    langfuse_entry = TenantLangfuseService.filter_by_tenant_with_info(tenant_id=current_user.id)
    if not langfuse_entry:
        return get_json_result(message="Have not record any Langfuse keys.")

    langfuse = Langfuse(public_key=langfuse_entry["public_key"], secret_key=langfuse_entry["secret_key"], host=langfuse_entry["host"])
    try:
        if not langfuse.auth_check():
            return get_error_data_result(message="Invalid Langfuse keys loaded")
    except langfuse.api.core.api_error.ApiError as api_err:
        return get_json_result(message=f"Error from Langfuse: {api_err}")
    except Exception as e:
        server_error_response(e)

    langfuse_entry["project_id"] = langfuse.api.projects.get().dict()["data"][0]["id"]
    langfuse_entry["project_name"] = langfuse.api.projects.get().dict()["data"][0]["name"]

    return get_json_result(data=langfuse_entry)


@manager.route("/api_key", methods=["DELETE"])  # noqa: F821
@login_required
@validate_request()
def delete_api_key():
    langfuse_entry = TenantLangfuseService.filter_by_tenant(tenant_id=current_user.id)
    if not langfuse_entry:
        return get_json_result(message="Have not record any Langfuse keys.")

    with DB.atomic():
        try:
            TenantLangfuseService.delete_model(langfuse_entry)
            return get_json_result(data=True)
        except Exception as e:
            server_error_response(e)

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


### Functions (3)

- `set_api_key()`: Function definition
- `get_api_key()`: Function definition
- `delete_api_key()`: Function definition

### Imports (6)

- `from flask import request`
- `from flask_login import current_user, login_required`
- `from langfuse import Langfuse`
- `from api.db.db_models import DB`
- `from api.db.services.langfuse_service import TenantLangfuseService`
- `from api.utils.api_utils import get_error_data_result, get_json_result, server_error_response, validate_request`

## Code Structure Analysis

- Total lines: 98
- Blank lines: 17 (17.3%)
- Comment lines: ~15 (15.3%)
- Code lines: ~66


## Dependencies and Imports

- `from flask import request`
- `from flask_login import current_user, login_required`
- `from langfuse import Langfuse`
- `from api.db.db_models import DB`
- `from api.db.services.langfuse_service import TenantLangfuseService`
- `from api.utils.api_utils import get_error_data_result, get_json_result, server_error_response, validate_request`

## Design & Architecture

This file is located in the `api` directory, specifically within `api/apps`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `api/apps/` directory
- Imports from `flask`
- Imports from `flask_login`
- Imports from `langfuse`
- Imports from `api.db.db_models`
- Imports from `api.db.services.langfuse_service`
- Potential test file: `test_langfuse_app.py`

## Keywords

ANY, All, Apache, ApiError, Authors, BASIS, CONDITIONS, Copyright, DELETE, Error, Exception, F821, GET, Have, InfiniFlow, Invalid, KIND, LICENSE, Langfuse, License, Licensed, Missing, POST, PUT, Python, Reserved, Rights, See, TenantLangfuseService, The, True, Unless, Version, WARRANTIES, WITHOUT, You, delete_api_key, get_api_key, login_required, manager, set_api_key, validate_request

---
*Generated by RAGFlow Repository Documentation Generator*
