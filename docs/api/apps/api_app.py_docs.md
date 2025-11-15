# Documentation: api/apps/api_app.py

## File Metadata

- **Path**: `api/apps/api_app.py`
- **Size**: 4407 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `api/apps/api_app.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `datetime`
- `flask`
- `flask_login`
- `api.db.db_models`
- `api.db.services.api_service`
- `api.db.services.user_service`
- `api.utils.api_utils`
- `common.time_utils`

### Functions Defined

This file defines 4 function(s):

#### Function: `new_token` (line 29)

**Parameters**: None

#### Function: `token_list` (line 59)

**Parameters**: None

#### Function: `rm` (line 75)

**Parameters**: None

#### Function: `stats` (line 88)

**Parameters**: None

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

## Detailed Analysis

### File Role in Repository

The file `api/apps/api_app.py` is located in the `api/apps` directory.

This file is part of the **API/Backend** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to apps.

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
- [canvas_app.py](canvas_app.py_docs.md)
- [chunk_app.py](chunk_app.py_docs.md)
- [connector_app.py](connector_app.py_docs.md)
- [conversation_app.py](conversation_app.py_docs.md)
- [dialog_app.py](dialog_app.py_docs.md)
- [document_app.py](document_app.py_docs.md)
- [file2document_app.py](file2document_app.py_docs.md)
- [file_app.py](file_app.py_docs.md)
- [kb_app.py](kb_app.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
