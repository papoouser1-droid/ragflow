# Documentation: api/apps/dialog_app.py

## File Metadata

- **Path**: `api/apps/dialog_app.py`
- **Size**: 9222 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `api/apps/dialog_app.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `flask`
- `flask_login`
- `api.db.services`
- `api.db.services.dialog_service`
- `common.constants`
- `api.db.services.tenant_llm_service`
- `api.db.services.knowledgebase_service`
- `api.db.services.user_service`
- `api.utils.api_utils`
- `common.misc_utils`
- `common.constants`
- `api.utils.api_utils`

### Functions Defined

This file defines 6 function(s):

#### Function: `set_dialog` (line 34)

**Parameters**: None

#### Function: `get` (line 129)

**Parameters**: None

#### Function: `get_kb_names` (line 142)

**Parameters**: kb_ids

#### Function: `list_dialogs` (line 155)

**Parameters**: None

#### Function: `list_dialogs_next` (line 172)

**Parameters**: None

#### Function: `rm` (line 210)

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

from flask import request
from flask_login import login_required, current_user
from api.db.services import duplicate_name
from api.db.services.dialog_service import DialogService
from common.constants import StatusEnum
from api.db.services.tenant_llm_service import TenantLLMService
from api.db.services.knowledgebase_service import KnowledgebaseService
from api.db.services.user_service import TenantService, UserTenantService
from api.utils.api_utils import server_error_response, get_data_error_result, validate_request
from common.misc_utils import get_uuid
from common.constants import RetCode
from api.utils.api_utils import get_json_result


@manager.route('/set', methods=['POST'])  # noqa: F821
@validate_request("prompt_config")
@login_required
def set_dialog():
    req = request.json
    dialog_id = req.get("dialog_id", "")
    is_create = not dialog_id
    name = req.get("name", "New Dialog")
    if not isinstance(name, str):
        return get_data_error_result(message="Dialog name must be string.")
    if name.strip() == "":
        return get_data_error_result(message="Dialog name can't be empty.")
    if len(name.encode("utf-8")) > 255:
        return get_data_error_result(message=f"Dialog name length is {len(name)} which is larger than 255")

    if is_create and DialogService.query(tenant_id=current_user.id, name=name.strip()):
        name = name.strip()
        name = duplicate_name(
            DialogService.query,
            name=name,
            tenant_id=current_user.id,
            status=StatusEnum.VALID.value)

    description = req.get("description", "A helpful dialog")
    icon = req.get("icon", "")
    top_n = req.get("top_n", 6)
    top_k = req.get("top_k", 1024)
    rerank_id = req.get("rerank_id", "")
    if not rerank_id:
        req["rerank_id"] = ""
    similarity_threshold = req.get("similarity_threshold", 0.1)
    vector_similarity_weight = req.get("vector_similarity_weight", 0.3)
    llm_setting = req.get("llm_setting", {})
    meta_data_filter = req.get("meta_data_filter", {})
    prompt_config = req["prompt_config"]

    if not is_create:
        if not req.get("kb_ids", []) and not prompt_config.get("tavily_api_key") and "{knowledge}" in prompt_config['system']:
            return get_data_error_result(message="Please remove `{knowledge}` in system prompt since no knowledge base / Tavily used here.")

        for p in prompt_config["parameters"]:
            if p["optional"]:
                continue
            if prompt_config["system"].find("{%s}" % p["key"]) < 0:
                return get_data_error_result(
                    message="Parameter '{}' is not used".format(p["key"]))

    try:
        e, tenant = TenantService.get_by_id(current_user.id)
        if not e:
            return get_data_error_result(message="Tenant not found!")
        kbs = KnowledgebaseService.get_by_ids(req.get("kb_ids", []))
        embd_ids = [TenantLLMService.split_model_name_and_factory(kb.embd_id)[0] for kb in kbs]  # remove vendor suffix for comparison
        embd_count = len(set(embd_ids))
        if embd_count > 1:
            return get_data_error_result(message=f'Datasets use different embedding models: {[kb.embd_id for kb in kbs]}"')

        llm_id = req.get("llm_id", tenant.llm_id)
        if not dialog_id:
            dia = {
                "id": get_uuid(),
                "tenant_id": current_user.id,
                "name": name,
                "kb_ids": req.get("kb_ids", []),
                "description": description,
                "llm_id": llm_id,
                "llm_setting": llm_setting,
                "prompt_config": prompt_config,
                "meta_data_filter": meta_data_filter,
                "top_n": top_n,
                "top_k": top_k,
                "rerank_id": rerank_id,
                "similarity_threshold": similarity_threshold,
                "vector_similarity_weight": vector_similarity_weight,
                "icon": icon
            }
            if not DialogService.save(**dia):
                return get_data_error_result(message="Fail to new a dialog!")
            return get_json_result(data=dia)
        else:
            del req["dialog_id"]
            if "kb_names" in req:
                del req["kb_names"]
            if not DialogService.update_by_id(dialog_id, req):
                return get_data_error_result(message="Dialog not found!")
            e, dia = DialogService.get_by_id(dialog_id)
            if not e:
                return get_data_error_result(message="Fail to update a dialog!")
            dia = dia.to_dict()
            dia.update(req)
            dia["kb_ids"], dia["kb_names"] = get_kb_names(dia["kb_ids"])
            return get_json_result(data=dia)
    except Exception as e:
        return server_error_response(e)


@manager.route('/get', methods=['GET'])  # noqa: F821
@login_required
def get():
    dialog_id = request.args["dialog_id"]
    try:
        e, dia = DialogService.get_by_id(dialog_id)
        if not e:
            return get_data_error_result(message="Dialog not found!")
        dia = dia.to_dict()
        dia["kb_ids"], dia["kb_names"] = get_kb_names(dia["kb_ids"])
        return get_json_result(data=dia)
    except Exception as e:
        return server_error_response(e)


def get_kb_names(kb_ids):
    ids, nms = [], []
    for kid in kb_ids:
        e, kb = KnowledgebaseService.get_by_id(kid)
        if not e or kb.status != StatusEnum.VALID.value:
            continue
        ids.append(kid)
        nms.append(kb.name)
    return ids, nms


@manager.route('/list', methods=['GET'])  # noqa: F821
@login_required
def list_dialogs():
    try:
        conversations = DialogService.query(
            tenant_id=current_user.id,
            status=StatusEnum.VALID.value,
            reverse=True,
            order_by=DialogService.model.create_time)
        conversations = [d.to_dict() for d in conversations]
        for conversation in conversations:
            conversation["kb_ids"], conversation["kb_names"] = get_kb_names(conversation["kb_ids"])
        return get_json_result(data=conversations)
    except Exception as e:
        return server_error_response(e)


@manager.route('/next', methods=['POST'])  # noqa: F821
@login_required
def list_dialogs_next():
    keywords = request.args.get("keywords", "")
    page_number = int(request.args.get("page", 0))
    items_per_page = int(request.args.get("page_size", 0))
    parser_id = request.args.get("parser_id")
    orderby = request.args.get("orderby", "create_time")
    if request.args.get("desc", "true").lower() == "false":
        desc = False
    else:
        desc = True

    req = request.get_json()
    owner_ids = req.get("owner_ids", [])
    try:
        if not owner_ids:
            # tenants = TenantService.get_joined_tenants_by_user_id(current_user.id)
            # tenants = [tenant["tenant_id"] for tenant in tenants]
            tenants = [] # keep it here
            dialogs, total = DialogService.get_by_tenant_ids(
                tenants, current_user.id, page_number,
                items_per_page, orderby, desc, keywords, parser_id)
        else:
            tenants = owner_ids
            dialogs, total = DialogService.get_by_tenant_ids(
                tenants, current_user.id, 0,
                0, orderby, desc, keywords, parser_id)
            dialogs = [dialog for dialog in dialogs if dialog["tenant_id"] in tenants]
            total = len(dialogs)
            if page_number and items_per_page:
                dialogs = dialogs[(page_number-1)*items_per_page:page_number*items_per_page]
        return get_json_result(data={"dialogs": dialogs, "total": total})
    except Exception as e:
        return server_error_response(e)


@manager.route('/rm', methods=['POST'])  # noqa: F821
@login_required
@validate_request("dialog_ids")
def rm():
    req = request.json
    dialog_list=[]
    tenants = UserTenantService.query(user_id=current_user.id)
    try:
        for id in req["dialog_ids"]:
            for tenant in tenants:
                if DialogService.query(tenant_id=tenant.tenant_id, id=id):
                    break
            else:
                return get_json_result(
                    data=False, message='Only owner of dialog authorized for this operation.',
                    code=RetCode.OPERATING_ERROR)
            dialog_list.append({"id": id,"status":StatusEnum.INVALID.value})
        DialogService.update_many_by_id(dialog_list)
        return get_json_result(data=True)
    except Exception as e:
        return server_error_response(e)

```

## Detailed Analysis

### File Role in Repository

The file `api/apps/dialog_app.py` is located in the `api/apps` directory.

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
