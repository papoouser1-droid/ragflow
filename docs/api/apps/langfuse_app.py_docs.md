# Documentation: api/apps/langfuse_app.py

## File Metadata

- **Path**: `api/apps/langfuse_app.py`
- **Size**: 3751 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `api/apps/langfuse_app.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `flask`
- `flask_login`
- `langfuse`
- `api.db.db_models`
- `api.db.services.langfuse_service`
- `api.utils.api_utils`

### Functions Defined

This file defines 3 function(s):

#### Function: `set_api_key` (line 30)

**Parameters**: None

#### Function: `get_api_key` (line 64)

**Parameters**: None

#### Function: `delete_api_key` (line 87)

**Parameters**: None

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

## Detailed Analysis

### File Role in Repository

The file `api/apps/langfuse_app.py` is located in the `api/apps` directory.

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
