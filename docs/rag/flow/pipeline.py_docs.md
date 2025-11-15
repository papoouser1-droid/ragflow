# Documentation: rag/flow/pipeline.py

## File Metadata

- **Path**: `rag/flow/pipeline.py`
- **Size**: 7047 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `rag/flow/pipeline.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `datetime`
- `json`
- `logging`
- `random`
- `timeit`
- `trio`
- `agent.canvas`
- `api.db.services.document_service`
- `api.db.services.task_service`
- `rag.utils.redis_conn`
- `common.exceptions`

### Classes Defined

This file defines 1 class(es):

#### Class: `Pipeline` (line 28)

**Methods**: __init__, callback, fetch_logs

### Functions Defined

This file defines 3 function(s):

#### Function: `__init__` (line 29)

**Parameters**: self, dsl, tenant_id, doc_id, task_id, flow_id

#### Function: `callback` (line 43)

**Parameters**: self, component_name, progress, message

#### Function: `fetch_logs` (line 106)

**Parameters**: self

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
import datetime
import json
import logging
import random
from timeit import default_timer as timer
import trio
from agent.canvas import Graph
from api.db.services.document_service import DocumentService
from api.db.services.task_service import has_canceled, TaskService, CANVAS_DEBUG_DOC_ID
from rag.utils.redis_conn import REDIS_CONN


class Pipeline(Graph):
    def __init__(self, dsl: str|dict, tenant_id=None, doc_id=None, task_id=None, flow_id=None):
        if isinstance(dsl, dict):
            dsl = json.dumps(dsl, ensure_ascii=False)
        super().__init__(dsl, tenant_id, task_id)
        if doc_id == CANVAS_DEBUG_DOC_ID:
            doc_id = None
        self._doc_id = doc_id
        self._flow_id = flow_id
        self._kb_id = None
        if self._doc_id:
            self._kb_id = DocumentService.get_knowledgebase_id(doc_id)
            if not self._kb_id:
                self._doc_id = None

    def callback(self, component_name: str, progress: float | int | None = None, message: str = "") -> None:
        from common.exceptions import TaskCanceledException
        log_key = f"{self._flow_id}-{self.task_id}-logs"
        timestamp = timer()
        if has_canceled(self.task_id):
            progress = -1
            message += "[CANCEL]"
        try:
            bin = REDIS_CONN.get(log_key)
            obj = json.loads(bin.encode("utf-8"))
            if obj:
                if obj[-1]["component_id"] == component_name:
                    obj[-1]["trace"].append(
                        {
                            "progress": progress,
                            "message": message,
                            "datetime": datetime.datetime.now().strftime("%H:%M:%S"),
                            "timestamp": timestamp,
                            "elapsed_time": timestamp - obj[-1]["trace"][-1]["timestamp"],
                        }
                    )
                else:
                    obj.append(
                        {
                            "component_id": component_name,
                            "trace": [{"progress": progress, "message": message, "datetime": datetime.datetime.now().strftime("%H:%M:%S"), "timestamp": timestamp, "elapsed_time": 0}],
                        }
                    )
            else:
                obj = [
                    {
                        "component_id": component_name,
                        "trace": [{"progress": progress, "message": message, "datetime": datetime.datetime.now().strftime("%H:%M:%S"), "timestamp": timestamp, "elapsed_time": 0}],
                    }
                ]
            if component_name != "END" and self._doc_id and self.task_id:
                percentage = 1.0 / len(self.components.items())
                finished = 0.0
                for o in obj:
                    for t in o["trace"]:
                        if t["progress"] < 0:
                            finished = -1
                            break
                    if finished < 0:
                        break
                    finished += o["trace"][-1]["progress"] * percentage

                msg = ""
                if len(obj[-1]["trace"]) == 1:
                    msg += f"\n-------------------------------------\n[{self.get_component_name(o['component_id'])}]:\n"
                t = obj[-1]["trace"][-1]
                msg += "%s: %s\n" % (t["datetime"], t["message"])
                TaskService.update_progress(self.task_id, {"progress": finished, "progress_msg": msg})
            elif component_name == "END" and not self._doc_id:
                obj[-1]["trace"][-1]["dsl"] = json.loads(str(self))
            REDIS_CONN.set_obj(log_key, obj, 60 * 30)

        except Exception as e:
            logging.exception(e)

        if has_canceled(self.task_id):
            raise TaskCanceledException(message)

    def fetch_logs(self):
        log_key = f"{self._flow_id}-{self.task_id}-logs"
        try:
            bin = REDIS_CONN.get(log_key)
            if bin:
                return json.loads(bin.encode("utf-8"))
        except Exception as e:
            logging.exception(e)
        return []


    async def run(self, **kwargs):
        log_key = f"{self._flow_id}-{self.task_id}-logs"
        try:
            REDIS_CONN.set_obj(log_key, [], 60 * 10)
        except Exception as e:
            logging.exception(e)
        self.error = ""
        if not self.path:
            self.path.append("File")
            cpn_obj = self.get_component_obj(self.path[0])
            await cpn_obj.invoke(**kwargs)
            if cpn_obj.error():
                self.error = "[ERROR]" + cpn_obj.error()
                self.callback(cpn_obj.component_name, -1, self.error)

        if self._doc_id:
            TaskService.update_progress(self.task_id, {
                "progress": random.randint(0, 5) / 100.0,
                "progress_msg": "Start the pipeline...",
                "begin_at": datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")})

        idx = len(self.path) - 1
        cpn_obj = self.get_component_obj(self.path[idx])
        idx += 1
        self.path.extend(cpn_obj.get_downstream())

        while idx < len(self.path) and not self.error:
            last_cpn = self.get_component_obj(self.path[idx - 1])
            cpn_obj = self.get_component_obj(self.path[idx])

            async def invoke():
                nonlocal last_cpn, cpn_obj
                await cpn_obj.invoke(**last_cpn.output())
                #if inspect.iscoroutinefunction(cpn_obj.invoke):
                #    await cpn_obj.invoke(**last_cpn.output())
                #else:
                #    cpn_obj.invoke(**last_cpn.output())

            async with trio.open_nursery() as nursery:
                nursery.start_soon(invoke)

            if cpn_obj.error():
                self.error = "[ERROR]" + cpn_obj.error()
                self.callback(cpn_obj._id, -1, self.error)
                break
            idx += 1
            self.path.extend(cpn_obj.get_downstream())

        self.callback("END", 1 if not self.error else -1, json.dumps(self.get_component_obj(self.path[-1]).output(), ensure_ascii=False))

        if not self.error:
            return self.get_component_obj(self.path[-1]).output()

        TaskService.update_progress(self.task_id, {
            "progress": -1,
            "progress_msg": f"[ERROR]: {self.error}"})

        return {}

```

## Detailed Analysis

### File Role in Repository

The file `rag/flow/pipeline.py` is located in the `rag/flow` directory.

This file is part of the **RAG (Retrieval-Augmented Generation)** core engine.

### Architecture Context

Files in this location typically handle concerns related to flow.

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
- [base.py](base.py_docs.md)
- [file.py](file.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
