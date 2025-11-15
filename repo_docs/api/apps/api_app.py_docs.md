# File Documentation: api/apps/api_app.py

## File Metadata

- **Path**: `api/apps/api_app.py`
- **Extension**: `.py`
- **Lines**: 116
- **Characters**: 4,407
- **Size**: 4,407 bytes
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
from datetime import datetime, timedelta
from flask import request
from flask_login import login_required, current_user
from api.db.db_models import APIToken
from api.db.services.api_service import APITokenService, API4ConversationService
from api.db.services.user_service import UserTenantService
from api.utils.api_utils import server_error_response, get_data_error_result, get_json_result, validate_request, \
    generate_confirmation_token
from common.time_utils import current_timestamp, datetime_format


@manager.route('/new_token', methods=['POST'])  # noqa: F821
@login_required
def new_token():
    req = request.json
    try:
        tenants = UserTenantService.query(user_id=current_user.id)
        if not tenants:
            return get_data_error_result(message="Tenant not found!")

        tenant_id = tenants[0].tenant_id
        obj = {"tenant_id": tenant_id, "token": generate_confirmation_token(),
               "create_time": current_timestamp(),
               "create_date": datetime_format(datetime.now()),
               "update_time": None,
               "update_date": None
               }
        if req.get("canvas_id"):
            obj["dialog_id"] = req["canvas_id"]
            obj["source"] = "agent"
        else:
            obj["dialog_id"] = req["dialog_id"]

        if not APITokenService.save(**obj):
            return get_data_error_result(message="Fail to new a dialog!")

        return get_json_result(data=obj)
    except Exception as e:
        return server_error_response(e)


@manager.route('/token_list', methods=['GET'])  # noqa: F821
@login_required
def token_list():
    try:
        tenants = UserTenantService.query(user_id=current_user.id)
        if not tenants:
            return get_data_error_result(message="Tenant not found!")

        id = request.args["dialog_id"] if "dialog_id" in request.args else request.args["canvas_id"]
        objs = APITokenService.query(tenant_id=tenants[0].tenant_id, dialog_id=id)
        return get_json_result(data=[o.to_dict() for o in objs])
    except Exception as e:
        return server_error_response(e)


@manager.route('/rm', methods=['POST'])  # noqa: F821
@validate_request("tokens", "tenant_id")
@login_required
def rm():
    req = request.json
    try:
        for token in req["tokens"]:
            APITokenService.filter_delete(
                [APIToken.tenant_id == req["tenant_id"], APIToken.token == token])
        return get_json_result(data=True)
    except Exception as e:
        return server_error_response(e)


@manager.route('/stats', methods=['GET'])  # noqa: F821
@login_required
def stats():
    try:
        tenants = UserTenantService.query(user_id=current_user.id)
        if not tenants:
            return get_data_error_result(message="Tenant not found!")
        objs = API4ConversationService.stats(
            tenants[0].tenant_id,
            request.args.get(
                "from_date",
                (datetime.now() -
                 timedelta(
                     days=7)).strftime("%Y-%m-%d 00:00:00")),
            request.args.get(
                "to_date",
                datetime.now().strftime("%Y-%m-%d %H:%M:%S")),
            "agent" if "canvas_id" in request.args else None)
        res = {
            "pv": [(o["dt"], o["pv"]) for o in objs],
            "uv": [(o["dt"], o["uv"]) for o in objs],
            "speed": [(o["dt"], float(o["tokens"]) / (float(o["duration"] + 0.1))) for o in objs],
            "tokens": [(o["dt"], float(o["tokens"]) / 1000.) for o in objs],
            "round": [(o["dt"], o["round"]) for o in objs],
            "thumb_up": [(o["dt"], o["thumb_up"]) for o in objs]
        }
        return get_json_result(data=res)
    except Exception as e:
        return server_error_response(e)


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


### Functions (4)

- `new_token()`: Function definition
- `token_list()`: Function definition
- `rm()`: Function definition
- `stats()`: Function definition

### Imports (8)

- `from datetime import datetime, timedelta`
- `from flask import request`
- `from flask_login import login_required, current_user`
- `from api.db.db_models import APIToken`
- `from api.db.services.api_service import APITokenService, API4ConversationService`
- `from api.db.services.user_service import UserTenantService`
- `from api.utils.api_utils import server_error_response, get_data_error_result, get_json_result, validate_request, \`
- `from common.time_utils import current_timestamp, datetime_format`

## Code Structure Analysis

- Total lines: 116
- Blank lines: 14 (12.1%)
- Comment lines: ~15 (12.9%)
- Code lines: ~87


## Dependencies and Imports

- `from datetime import datetime, timedelta`
- `from flask import request`
- `from flask_login import login_required, current_user`
- `from api.db.db_models import APIToken`
- `from api.db.services.api_service import APITokenService, API4ConversationService`
- `from api.db.services.user_service import UserTenantService`
- `from api.utils.api_utils import server_error_response, get_data_error_result, get_json_result, validate_request, \`
- `from common.time_utils import current_timestamp, datetime_format`

## Design & Architecture

This file is located in the `api` directory, specifically within `api/apps`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 9 loop(s) - consider algorithmic complexity
- Contains database queries - ensure proper indexing and query optimization

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
- Imports from `datetime`
- Imports from `flask`
- Imports from `flask_login`
- Imports from `api.db.db_models`
- Imports from `api.db.services.api_service`
- Potential test file: `test_api_app.py`

## Keywords

ANY, API4ConversationService, APIToken, APITokenService, All, Apache, Authors, BASIS, CONDITIONS, Copyright, Exception, F821, Fail, GET, InfiniFlow, KIND, LICENSE, License, Licensed, None, POST, Python, Reserved, Rights, See, Tenant, The, True, Unless, UserTenantService, Version, WARRANTIES, WITHOUT, You, login_required, manager, new_token, rm, stats, token_list, validate_request

---
*Generated by RAGFlow Repository Documentation Generator*
