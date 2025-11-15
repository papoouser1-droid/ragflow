# File Documentation: api/apps/sdk/agents.py

## File Metadata

- **Path**: `api/apps/sdk/agents.py`
- **Extension**: `.py`
- **Lines**: 180
- **Characters**: 6,705
- **Size**: 6,705 bytes
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

import json
import logging
import time
from typing import Any, cast

from agent.canvas import Canvas
from api.db import CanvasCategory
from api.db.services.canvas_service import UserCanvasService
from api.db.services.user_canvas_version import UserCanvasVersionService
from common.constants import RetCode
from common.misc_utils import get_uuid
from api.utils.api_utils import get_data_error_result, get_error_data_result, get_json_result, token_required
from api.utils.api_utils import get_result
from flask import request, Response


@manager.route('/agents', methods=['GET'])  # noqa: F821
@token_required
def list_agents(tenant_id):
    id = request.args.get("id")
    title = request.args.get("title")
    if id or title:
        canvas = UserCanvasService.query(id=id, title=title, user_id=tenant_id)
        if not canvas:
            return get_error_data_result("The agent doesn't exist.")
    page_number = int(request.args.get("page", 1))
    items_per_page = int(request.args.get("page_size", 30))
    orderby = request.args.get("orderby", "update_time")
    if request.args.get("desc") == "False" or request.args.get("desc") == "false":
        desc = False
    else:
        desc = True
    canvas = UserCanvasService.get_list(tenant_id, page_number, items_per_page, orderby, desc, id, title)
    return get_result(data=canvas)


@manager.route("/agents", methods=["POST"])  # noqa: F821
@token_required
def create_agent(tenant_id: str):
    req: dict[str, Any] = cast(dict[str, Any], request.json)
    req["user_id"] = tenant_id

    if req.get("dsl") is not None:
        if not isinstance(req["dsl"], str):
            req["dsl"] = json.dumps(req["dsl"], ensure_ascii=False)

        req["dsl"] = json.loads(req["dsl"])
    else:
        return get_json_result(data=False, message="No DSL data in request.", code=RetCode.ARGUMENT_ERROR)

    if req.get("title") is not None:
        req["title"] = req["title"].strip()
    else:
        return get_json_result(data=False, message="No title in request.", code=RetCode.ARGUMENT_ERROR)

    if UserCanvasService.query(user_id=tenant_id, title=req["title"]):
        return get_data_error_result(message=f"Agent with title {req['title']} already exists.")

    agent_id = get_uuid()
    req["id"] = agent_id

    if not UserCanvasService.save(**req):
        return get_data_error_result(message="Fail to create agent.")

    UserCanvasVersionService.insert(
        user_canvas_id=agent_id,
        title="{0}_{1}".format(req["title"], time.strftime("%Y_%m_%d_%H_%M_%S")),
        dsl=req["dsl"]
    )

    return get_json_result(data=True)


@manager.route("/agents/<agent_id>", methods=["PUT"])  # noqa: F821
@token_required
def update_agent(tenant_id: str, agent_id: str):
    req: dict[str, Any] = {k: v for k, v in cast(dict[str, Any], request.json).items() if v is not None}
    req["user_id"] = tenant_id

    if req.get("dsl") is not None:
        if not isinstance(req["dsl"], str):
            req["dsl"] = json.dumps(req["dsl"], ensure_ascii=False)

        req["dsl"] = json.loads(req["dsl"])

    if req.get("title") is not None:
        req["title"] = req["title"].strip()

    if not UserCanvasService.query(user_id=tenant_id, id=agent_id):
        return get_json_result(
            data=False, message="Only owner of canvas authorized for this operation.",
            code=RetCode.OPERATING_ERROR)

    UserCanvasService.update_by_id(agent_id, req)

    if req.get("dsl") is not None:
        UserCanvasVersionService.insert(
            user_canvas_id=agent_id,
            title="{0}_{1}".format(req["title"], time.strftime("%Y_%m_%d_%H_%M_%S")),
            dsl=req["dsl"]
        )

        UserCanvasVersionService.delete_all_versions(agent_id)

    return get_json_result(data=True)


@manager.route("/agents/<agent_id>", methods=["DELETE"])  # noqa: F821
@token_required
def delete_agent(tenant_id: str, agent_id: str):
    if not UserCanvasService.query(user_id=tenant_id, id=agent_id):
        return get_json_result(
            data=False, message="Only owner of canvas authorized for this operation.",
            code=RetCode.OPERATING_ERROR)

    UserCanvasService.delete_by_id(agent_id)
    return get_json_result(data=True)


@manager.route('/webhook/<agent_id>', methods=['POST'])  # noqa: F821
@token_required
def webhook(tenant_id: str, agent_id: str):
    req = request.json
    if not UserCanvasService.accessible(req["id"], tenant_id):
        return get_json_result(
            data=False, message='Only owner of canvas authorized for this operation.',
            code=RetCode.OPERATING_ERROR)

    e, cvs = UserCanvasService.get_by_id(req["id"])
    if not e:
        return get_data_error_result(message="canvas not found.")

    if not isinstance(cvs.dsl, str):
        cvs.dsl = json.dumps(cvs.dsl, ensure_ascii=False)

    if cvs.canvas_category == CanvasCategory.DataFlow:
        return get_data_error_result(message="Dataflow can not be triggered by webhook.")

    try:
        canvas = Canvas(cvs.dsl, tenant_id, agent_id)
    except Exception as e:
        return get_json_result(
            data=False, message=str(e),
            code=RetCode.EXCEPTION_ERROR)

    def sse():
        nonlocal canvas
        try:
            for ans in canvas.run(query=req.get("query", ""), files=req.get("files", []), user_id=req.get("user_id", tenant_id), webhook_payload=req):
                yield "data:" + json.dumps(ans, ensure_ascii=False) + "\n\n"

            cvs.dsl = json.loads(str(canvas))
            UserCanvasService.update_by_id(req["id"], cvs.to_dict())
        except Exception as e:
            logging.exception(e)
            yield "data:" + json.dumps({"code": 500, "message": str(e), "data": False}, ensure_ascii=False) + "\n\n"

    resp = Response(sse(), mimetype="text/event-stream")
    resp.headers.add_header("Cache-control", "no-cache")
    resp.headers.add_header("Connection", "keep-alive")
    resp.headers.add_header("X-Accel-Buffering", "no")
    resp.headers.add_header("Content-Type", "text/event-stream; charset=utf-8")
    return resp

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


### Functions (5)

- `list_agents()`: Function definition
- `create_agent()`: Function definition
- `update_agent()`: Function definition
- `delete_agent()`: Function definition
- `webhook()`: Function definition

### Imports (13)

- `import json`
- `import logging`
- `import time`
- `from typing import Any, cast`
- `from agent.canvas import Canvas`
- `from api.db import CanvasCategory`
- `from api.db.services.canvas_service import UserCanvasService`
- `from api.db.services.user_canvas_version import UserCanvasVersionService`
- `from common.constants import RetCode`
- `from common.misc_utils import get_uuid`

## Code Structure Analysis

- Total lines: 180
- Blank lines: 37 (20.6%)
- Comment lines: ~15 (8.3%)
- Code lines: ~128


## Dependencies and Imports

- `import json`
- `import logging`
- `import time`
- `from typing import Any, cast`
- `from agent.canvas import Canvas`
- `from api.db import CanvasCategory`
- `from api.db.services.canvas_service import UserCanvasService`
- `from api.db.services.user_canvas_version import UserCanvasVersionService`
- `from common.constants import RetCode`
- `from common.misc_utils import get_uuid`
- `from api.utils.api_utils import get_data_error_result, get_error_data_result, get_json_result, token_required`
- `from api.utils.api_utils import get_result`
- `from flask import request, Response`

## Design & Architecture

This file is located in the `api` directory, specifically within `api/apps/sdk`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 6 loop(s) - consider algorithmic complexity
- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `api/apps/sdk/` directory
- Imports from `agent.canvas`
- Imports from `api.db`
- Imports from `api.db.services.canvas_service`
- Imports from `api.db.services.user_canvas_version`
- Potential test file: `test_agents.py`

## Keywords

ANY, ARGUMENT_ERROR, Accel, Agent, All, Any, Apache, Authors, BASIS, Buffering, CONDITIONS, Cache, Canvas, CanvasCategory, Connection, Content, Copyright, DELETE, DSL, DataFlow, Dataflow, EXCEPTION_ERROR, Exception, F821, Fail, False, GET, InfiniFlow, KIND, LICENSE, License, Licensed, None, OPERATING_ERROR, Only, POST, PUT, Python, Reserved, Response, RetCode, Rights, See, The, True, Type, Unless, UserCanvasService, UserCanvasVersionService, Version...

---
*Generated by RAGFlow Repository Documentation Generator*
