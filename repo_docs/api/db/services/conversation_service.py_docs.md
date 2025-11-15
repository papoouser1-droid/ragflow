# File Documentation: api/db/services/conversation_service.py

## File Metadata

- **Path**: `api/db/services/conversation_service.py`
- **Extension**: `.py`
- **Lines**: 243
- **Characters**: 9,379
- **Size**: 9,379 bytes
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
import time
from uuid import uuid4
from common.constants import StatusEnum
from api.db.db_models import Conversation, DB
from api.db.services.api_service import API4ConversationService
from api.db.services.common_service import CommonService
from api.db.services.dialog_service import DialogService, chat
from common.misc_utils import get_uuid
import json

from rag.prompts.generator import chunks_format


class ConversationService(CommonService):
    model = Conversation

    @classmethod
    @DB.connection_context()
    def get_list(cls, dialog_id, page_number, items_per_page, orderby, desc, id, name, user_id=None):
        sessions = cls.model.select().where(cls.model.dialog_id == dialog_id)
        if id:
            sessions = sessions.where(cls.model.id == id)
        if name:
            sessions = sessions.where(cls.model.name == name)
        if user_id:
            sessions = sessions.where(cls.model.user_id == user_id)
        if desc:
            sessions = sessions.order_by(cls.model.getter_by(orderby).desc())
        else:
            sessions = sessions.order_by(cls.model.getter_by(orderby).asc())

        sessions = sessions.paginate(page_number, items_per_page)

        return list(sessions.dicts())

    @classmethod
    @DB.connection_context()
    def get_all_conversation_by_dialog_ids(cls, dialog_ids):
        sessions = cls.model.select().where(cls.model.dialog_id.in_(dialog_ids))
        sessions.order_by(cls.model.create_time.asc())
        offset, limit = 0, 100
        res = []
        while True:
            s_batch = sessions.offset(offset).limit(limit)
            _temp = list(s_batch.dicts())
            if not _temp:
                break
            res.extend(_temp)
            offset += limit
        return res

def structure_answer(conv, ans, message_id, session_id):
    reference = ans["reference"]
    if not isinstance(reference, dict):
        reference = {}
        ans["reference"] = {}

    chunk_list = chunks_format(reference)

    reference["chunks"] = chunk_list
    ans["id"] = message_id
    ans["session_id"] = session_id

    if not conv:
        return ans

    if not conv.message:
        conv.message = []
    if not conv.message or conv.message[-1].get("role", "") != "assistant":
        conv.message.append({"role": "assistant", "content": ans["answer"], "created_at": time.time(), "id": message_id})
    else:
        conv.message[-1] = {"role": "assistant", "content": ans["answer"], "created_at": time.time(), "id": message_id}
    if conv.reference:
        conv.reference[-1] = reference
    return ans


def completion(tenant_id, chat_id, question, name="New session", session_id=None, stream=True, **kwargs):
    assert name, "`name` can not be empty."
    dia = DialogService.query(id=chat_id, tenant_id=tenant_id, status=StatusEnum.VALID.value)
    assert dia, "You do not own the chat."

    if not session_id:
        session_id = get_uuid()
        conv = {
            "id": session_id,
            "dialog_id": chat_id,
            "name": name,
            "message": [{"role": "assistant", "content": dia[0].prompt_config.get("prologue"), "created_at": time.time()}],
            "user_id": kwargs.get("user_id", "")
        }
        ConversationService.save(**conv)
        if stream:
            yield "data:" + json.dumps({"code": 0, "message": "",
                                        "data": {
                                            "answer": conv["message"][0]["content"],
                                            "reference": {},
                                            "audio_binary": None,
                                            "id": None,
                                            "session_id": session_id
                                        }},
                                    ensure_ascii=False) + "\n\n"
            yield "data:" + json.dumps({"code": 0, "message": "", "data": True}, ensure_ascii=False) + "\n\n"
            return

    conv = ConversationService.query(id=session_id, dialog_id=chat_id)
    if not conv:
        raise LookupError("Session does not exist")

    conv = conv[0]
    msg = []
    question = {
        "content": question,
        "role": "user",
        "id": str(uuid4())
    }
    conv.message.append(question)
    for m in conv.message:
        if m["role"] == "system":
            continue
        if m["role"] == "assistant" and not msg:
            continue
        msg.append(m)
    message_id = msg[-1].get("id")
    e, dia = DialogService.get_by_id(conv.dialog_id)

    kb_ids = kwargs.get("kb_ids",[])
    dia.kb_ids = list(set(dia.kb_ids + kb_ids))
    if not conv.reference:
        conv.reference = []
    conv.message.append({"role": "assistant", "content": "", "id": message_id})
    conv.reference.append({"chunks": [], "doc_aggs": []})

    if stream:
        try:
            for ans in chat(dia, msg, True, **kwargs):
                ans = structure_answer(conv, ans, message_id, session_id)
                yield "data:" + json.dumps({"code": 0, "data": ans}, ensure_ascii=False) + "\n\n"
            ConversationService.update_by_id(conv.id, conv.to_dict())
        except Exception as e:
            yield "data:" + json.dumps({"code": 500, "message": str(e),
                                        "data": {"answer": "**ERROR**: " + str(e), "reference": []}},
                                       ensure_ascii=False) + "\n\n"
        yield "data:" + json.dumps({"code": 0, "data": True}, ensure_ascii=False) + "\n\n"

    else:
        answer = None
        for ans in chat(dia, msg, False, **kwargs):
            answer = structure_answer(conv, ans, message_id, session_id)
            ConversationService.update_by_id(conv.id, conv.to_dict())
            break
        yield answer


def iframe_completion(dialog_id, question, session_id=None, stream=True, **kwargs):
    e, dia = DialogService.get_by_id(dialog_id)
    assert e, "Dialog not found"
    if not session_id:
        session_id = get_uuid()
        conv = {
            "id": session_id,
            "dialog_id": dialog_id,
            "user_id": kwargs.get("user_id", ""),
            "message": [{"role": "assistant", "content": dia.prompt_config["prologue"], "created_at": time.time()}]
        }
        API4ConversationService.save(**conv)
        yield "data:" + json.dumps({"code": 0, "message": "",
                                    "data": {
                                        "answer": conv["message"][0]["content"],
                                        "reference": {},
                                        "audio_binary": None,
                                        "id": None,
                                        "session_id": session_id
                                    }},
                                   ensure_ascii=False) + "\n\n"
        yield "data:" + json.dumps({"code": 0, "message": "", "data": True}, ensure_ascii=False) + "\n\n"
        return
    else:
        session_id = session_id
        e, conv = API4ConversationService.get_by_id(session_id)
        assert e, "Session not found!"

    if not conv.message:
        conv.message = []
    messages = conv.message
    question = {
        "role": "user",
        "content": question,
        "id": str(uuid4())
    }
    messages.append(question)

    msg = []
    for m in messages:
        if m["role"] == "system":
            continue
        if m["role"] == "assistant" and not msg:
            continue
        msg.append(m)
    if not msg[-1].get("id"):
        msg[-1]["id"] = get_uuid()
    message_id = msg[-1]["id"]

    if not conv.reference:
        conv.reference = []
    conv.reference.append({"chunks": [], "doc_aggs": []})

    if stream:
        try:
            for ans in chat(dia, msg, True, **kwargs):
                ans = structure_answer(conv, ans, message_id, session_id)
                yield "data:" + json.dumps({"code": 0, "message": "", "data": ans},
                                           ensure_ascii=False) + "\n\n"
            API4ConversationService.append_message(conv.id, conv.to_dict())
        except Exception as e:
            yield "data:" + json.dumps({"code": 500, "message": str(e),
                                        "data": {"answer": "**ERROR**: " + str(e), "reference": []}},
                                       ensure_ascii=False) + "\n\n"
        yield "data:" + json.dumps({"code": 0, "message": "", "data": True}, ensure_ascii=False) + "\n\n"

    else:
        answer = None
        for ans in chat(dia, msg, False, **kwargs):
            answer = structure_answer(conv, ans, message_id, session_id)
            API4ConversationService.append_message(conv.id, conv.to_dict())
            break
        yield answer

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

- `ConversationService`: Class definition

### Functions (3)

- `structure_answer()`: Function definition
- `completion()`: Function definition
- `iframe_completion()`: Function definition

### Imports (10)

- `import time`
- `from uuid import uuid4`
- `from common.constants import StatusEnum`
- `from api.db.db_models import Conversation, DB`
- `from api.db.services.api_service import API4ConversationService`
- `from api.db.services.common_service import CommonService`
- `from api.db.services.dialog_service import DialogService, chat`
- `from common.misc_utils import get_uuid`
- `import json`
- `from rag.prompts.generator import chunks_format`

## Code Structure Analysis

- Total lines: 243
- Blank lines: 28 (11.5%)
- Comment lines: ~15 (6.2%)
- Code lines: ~200


## Dependencies and Imports

- `import time`
- `from uuid import uuid4`
- `from common.constants import StatusEnum`
- `from api.db.db_models import Conversation, DB`
- `from api.db.services.api_service import API4ConversationService`
- `from api.db.services.common_service import CommonService`
- `from api.db.services.dialog_service import DialogService, chat`
- `from common.misc_utils import get_uuid`
- `import json`
- `from rag.prompts.generator import chunks_format`

## Design & Architecture

This file is located in the `api` directory, specifically within `api/db/services`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 8 loop(s) - consider algorithmic complexity
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
- Imports from `uuid`
- Imports from `common.constants`
- Imports from `api.db.db_models`
- Imports from `api.db.services.api_service`
- Imports from `api.db.services.common_service`
- Potential test file: `test_conversation_service.py`

## Keywords

ANY, API4ConversationService, All, Apache, Authors, BASIS, CONDITIONS, CommonService, Conversation, ConversationService, Copyright, DB, Dialog, DialogService, ERROR, Exception, False, InfiniFlow, KIND, LICENSE, License, Licensed, LookupError, New, None, Python, Reserved, Rights, See, Session, StatusEnum, The, True, Unless, VALID, Version, WARRANTIES, WITHOUT, You, classmethod, completion, get_all_conversation_by_dialog_ids, get_list, iframe_completion, structure_answer

---
*Generated by RAGFlow Repository Documentation Generator*
