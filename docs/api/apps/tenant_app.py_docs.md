# Documentation: api/apps/tenant_app.py

## File Metadata

- **Path**: `api/apps/tenant_app.py`
- **Size**: 5139 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `api/apps/tenant_app.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `flask`
- `flask_login`
- `api.apps`
- `api.db`
- `api.db.db_models`
- `api.db.services.user_service`
- `common.constants`
- `common.misc_utils`
- `common.time_utils`
- `api.utils.api_utils`
- `api.utils.web_utils`
- `common`
- `threading`

### Functions Defined

This file defines 5 function(s):

#### Function: `user_list` (line 35)

**Parameters**: tenant_id

#### Function: `create` (line 54)

**Parameters**: tenant_id

#### Function: `rm` (line 108)

**Parameters**: tenant_id, user_id

#### Function: `tenant_list` (line 124)

**Parameters**: None

#### Function: `agree` (line 136)

**Parameters**: tenant_id

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

from flask import request
from flask_login import login_required, current_user

from api.apps import smtp_mail_server
from api.db import UserTenantRole
from api.db.db_models import UserTenant
from api.db.services.user_service import UserTenantService, UserService

from common.constants import RetCode, StatusEnum
from common.misc_utils import get_uuid
from common.time_utils import delta_seconds
from api.utils.api_utils import get_json_result, validate_request, server_error_response, get_data_error_result
from api.utils.web_utils import send_invite_email
from common import settings


@manager.route("/<tenant_id>/user/list", methods=["GET"])  # noqa: F821
@login_required
def user_list(tenant_id):
    if current_user.id != tenant_id:
        return get_json_result(
            data=False,
            message='No authorization.',
            code=RetCode.AUTHENTICATION_ERROR)

    try:
        users = UserTenantService.get_by_tenant_id(tenant_id)
        for u in users:
            u["delta_seconds"] = delta_seconds(str(u["update_date"]))
        return get_json_result(data=users)
    except Exception as e:
        return server_error_response(e)


@manager.route('/<tenant_id>/user', methods=['POST'])  # noqa: F821
@login_required
@validate_request("email")
def create(tenant_id):
    if current_user.id != tenant_id:
        return get_json_result(
            data=False,
            message='No authorization.',
            code=RetCode.AUTHENTICATION_ERROR)

    req = request.json
    invite_user_email = req["email"]
    invite_users = UserService.query(email=invite_user_email)
    if not invite_users:
        return get_data_error_result(message="User not found.")

    user_id_to_invite = invite_users[0].id
    user_tenants = UserTenantService.query(user_id=user_id_to_invite, tenant_id=tenant_id)
    if user_tenants:
        user_tenant_role = user_tenants[0].role
        if user_tenant_role == UserTenantRole.NORMAL:
            return get_data_error_result(message=f"{invite_user_email} is already in the team.")
        if user_tenant_role == UserTenantRole.OWNER:
            return get_data_error_result(message=f"{invite_user_email} is the owner of the team.")
        return get_data_error_result(
            message=f"{invite_user_email} is in the team, but the role: {user_tenant_role} is invalid.")

    UserTenantService.save(
        id=get_uuid(),
        user_id=user_id_to_invite,
        tenant_id=tenant_id,
        invited_by=current_user.id,
        role=UserTenantRole.INVITE,
        status=StatusEnum.VALID.value)

    if smtp_mail_server and settings.SMTP_CONF:
        from threading import Thread

        user_name = ""
        _, user = UserService.get_by_id(current_user.id)
        if user:
            user_name = user.nickname

        Thread(
            target=send_invite_email,
            args=(invite_user_email, settings.MAIL_FRONTEND_URL, tenant_id, user_name or current_user.email),
            daemon=True
        ).start()

    usr = invite_users[0].to_dict()
    usr = {k: v for k, v in usr.items() if k in ["id", "avatar", "email", "nickname"]}

    return get_json_result(data=usr)


@manager.route('/<tenant_id>/user/<user_id>', methods=['DELETE'])  # noqa: F821
@login_required
def rm(tenant_id, user_id):
    if current_user.id != tenant_id and current_user.id != user_id:
        return get_json_result(
            data=False,
            message='No authorization.',
            code=RetCode.AUTHENTICATION_ERROR)

    try:
        UserTenantService.filter_delete([UserTenant.tenant_id == tenant_id, UserTenant.user_id == user_id])
        return get_json_result(data=True)
    except Exception as e:
        return server_error_response(e)


@manager.route("/list", methods=["GET"])  # noqa: F821
@login_required
def tenant_list():
    try:
        users = UserTenantService.get_tenants_by_user_id(current_user.id)
        for u in users:
            u["delta_seconds"] = delta_seconds(str(u["update_date"]))
        return get_json_result(data=users)
    except Exception as e:
        return server_error_response(e)


@manager.route("/agree/<tenant_id>", methods=["PUT"])  # noqa: F821
@login_required
def agree(tenant_id):
    try:
        UserTenantService.filter_update([UserTenant.tenant_id == tenant_id, UserTenant.user_id == current_user.id],
                                        {"role": UserTenantRole.NORMAL})
        return get_json_result(data=True)
    except Exception as e:
        return server_error_response(e)

```

## Detailed Analysis

### File Role in Repository

The file `api/apps/tenant_app.py` is located in the `api/apps` directory.

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
- [api_app.py](api_app.py_docs.md)
- [canvas_app.py](canvas_app.py_docs.md)
- [chunk_app.py](chunk_app.py_docs.md)
- [connector_app.py](connector_app.py_docs.md)
- [conversation_app.py](conversation_app.py_docs.md)
- [dialog_app.py](dialog_app.py_docs.md)
- [document_app.py](document_app.py_docs.md)
- [file2document_app.py](file2document_app.py_docs.md)
- [file_app.py](file_app.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
