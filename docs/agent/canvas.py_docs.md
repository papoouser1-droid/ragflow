# Documentation: agent/canvas.py

## File Metadata

- **Path**: `agent/canvas.py`
- **Size**: 24402 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/canvas.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `base64`
- `json`
- `logging`
- `re`
- `time`
- `concurrent.futures`
- `copy`
- `functools`
- `typing`
- `agent.component`
- `agent.component.base`
- `api.db.services.file_service`
- `api.db.services.task_service`
- `common.misc_utils`
- `common.exceptions`
- `rag.prompts.generator`
- `rag.utils.redis_conn`

### Classes Defined

This file defines 2 class(es):

#### Class: `Graph` (line 35)

**Docstring**: dsl = {
    "components": {
        "begin": {
            "obj":{
                "component_name": "Begin",
                "params": {},
            },
            "downstream": ["answer_0"],
     ...

**Methods**: __init__, load, __str__, reset, get_component_name, run, get_component, get_component_obj, get_component_type, get_component_input_form, get_tenant_id, get_value_with_variable, get_variable_value, get_variable_param_value, is_canceled, cancel_task

#### Class: `Canvas` (line 233)

**Methods**: __init__, load, __str__, reset, run, is_reff, get_history, add_user_input, get_prologue, get_mode, set_global_param, get_preset_param, get_component_input_elements, get_files, tool_use_callback, add_reference, get_reference, add_memory, get_memory, get_component_thoughts

### Functions Defined

This file defines 42 function(s):

#### Function: `__init__` (line 76)

**Parameters**: self, dsl, tenant_id, task_id

#### Function: `load` (line 85)

**Parameters**: self

#### Function: `__str__` (line 104)

**Parameters**: self

#### Function: `reset` (line 125)

**Parameters**: self

#### Function: `get_component_name` (line 135)

**Parameters**: self, cid

#### Function: `run` (line 141)

**Parameters**: self

#### Function: `get_component` (line 144)

**Parameters**: self, cpn_id

#### Function: `get_component_obj` (line 147)

**Parameters**: self, cpn_id

#### Function: `get_component_type` (line 150)

**Parameters**: self, cpn_id

#### Function: `get_component_input_form` (line 153)

**Parameters**: self, cpn_id

#### Function: `get_tenant_id` (line 156)

**Parameters**: self

#### Function: `get_value_with_variable` (line 159)

**Parameters**: self, value

#### Function: `get_variable_value` (line 186)

**Parameters**: self, exp

#### Function: `get_variable_param_value` (line 203)

**Parameters**: self, obj, path

#### Function: `is_canceled` (line 221)

**Parameters**: self

#### Function: `cancel_task` (line 224)

**Parameters**: self

#### Function: `__init__` (line 235)

**Parameters**: self, dsl, tenant_id, task_id

#### Function: `load` (line 244)

**Parameters**: self

#### Function: `__str__` (line 260)

**Parameters**: self

#### Function: `reset` (line 266)

**Parameters**: self, mem

#### Function: `run` (line 287)

**Parameters**: self

#### Function: `is_reff` (line 511)

**Parameters**: self, exp

#### Function: `get_history` (line 522)

**Parameters**: self, window_size

#### Function: `add_user_input` (line 533)

**Parameters**: self, question

#### Function: `get_prologue` (line 536)

**Parameters**: self

#### Function: `get_mode` (line 539)

**Parameters**: self

#### Function: `set_global_param` (line 542)

**Parameters**: self

#### Function: `get_preset_param` (line 545)

**Parameters**: self

#### Function: `get_component_input_elements` (line 548)

**Parameters**: self, cpnnm

#### Function: `get_files` (line 551)

**Parameters**: self, files

#### Function: `tool_use_callback` (line 566)

**Parameters**: self, agent_id, func_name, params, result, elapsed_time

#### Function: `add_reference` (line 590)

**Parameters**: self, chunks, doc_infos

#### Function: `get_reference` (line 605)

**Parameters**: self

#### Function: `add_memory` (line 610)

**Parameters**: self, user, assist, summ

#### Function: `get_memory` (line 613)

**Parameters**: self

#### Function: `get_component_thoughts` (line 616)

**Parameters**: self, cpn_id

#### Function: `decorate` (line 311)

**Parameters**: event, dt

#### Function: `_run_batch` (line 334)

**Parameters**: f, t

#### Function: `_node_finished` (line 360)

**Parameters**: cpn_obj

#### Function: `image_to_base64` (line 554)

**Parameters**: file

#### Function: `_append_path` (line 444)

**Parameters**: cpn_id

#### Function: `_extend_path` (line 452)

**Parameters**: cpn_ids

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
import base64
import json
import logging
import re
import time
from concurrent.futures import ThreadPoolExecutor
from copy import deepcopy
from functools import partial
from typing import Any, Union, Tuple

from agent.component import component_class
from agent.component.base import ComponentBase
from api.db.services.file_service import FileService
from api.db.services.task_service import has_canceled
from common.misc_utils import get_uuid, hash_str2int
from common.exceptions import TaskCanceledException
from rag.prompts.generator import chunks_format
from rag.utils.redis_conn import REDIS_CONN

class Graph:
    """
        dsl = {
            "components": {
                "begin": {
                    "obj":{
                        "component_name": "Begin",
                        "params": {},
                    },
                    "downstream": ["answer_0"],
                    "upstream": [],
                },
                "retrieval_0": {
                    "obj": {
                        "component_name": "Retrieval",
                        "params": {}
                    },
                    "downstream": ["generate_0"],
                    "upstream": ["answer_0"],
                },
                "generate_0": {
                    "obj": {
                        "component_name": "Generate",
                        "params": {}
                    },
                    "downstream": ["answer_0"],
                    "upstream": ["retrieval_0"],
                }
            },
            "history": [],
            "path": ["begin"],
            "retrieval": {"chunks": [], "doc_aggs": []},
            "globals": {
                "sys.query": "",
                "sys.user_id": tenant_id,
                "sys.conversation_turns": 0,
                "sys.files": []
            }
        }
        """

    def __init__(self, dsl: str, tenant_id=None, task_id=None):
        self.path = []
        self.components = {}
        self.error = ""
        self.dsl = json.loads(dsl)
        self._tenant_id = tenant_id
        self.task_id = task_id if task_id else get_uuid()
        self.load()

    def load(self):
        self.components = self.dsl["components"]
        cpn_nms = set([])
        for k, cpn in self.components.items():
            cpn_nms.add(cpn["obj"]["component_name"])

        for k, cpn in self.components.items():
            cpn_nms.add(cpn["obj"]["component_name"])
            param = component_class(cpn["obj"]["component_name"] + "Param")()
            param.update(cpn["obj"]["params"])
            try:
                param.check()
            except Exception as e:
                raise ValueError(self.get_component_name(k) + f": {e}")

            cpn["obj"] = component_class(cpn["obj"]["component_name"])(self, k, param)

        self.path = self.dsl["path"]

    def __str__(self):
        self.dsl["path"] = self.path
        self.dsl["task_id"] = self.task_id
        dsl = {
            "components": {}
        }
        for k in self.dsl.keys():
            if k in ["components"]:
                continue
            dsl[k] = deepcopy(self.dsl[k])

        for k, cpn in self.components.items():
            if k not in dsl["components"]:
                dsl["components"][k] = {}
            for c in cpn.keys():
                if c == "obj":
                    dsl["components"][k][c] = json.loads(str(cpn["obj"]))
                    continue
                dsl["components"][k][c] = deepcopy(cpn[c])
        return json.dumps(dsl, ensure_ascii=False)

    def reset(self):
        self.path = []
        for k, cpn in self.components.items():
            self.components[k]["obj"].reset()
        try:
            REDIS_CONN.delete(f"{self.task_id}-logs")
            REDIS_CONN.delete(f"{self.task_id}-cancel")
        except Exception as e:
            logging.exception(e)

    def get_component_name(self, cid):
        for n in self.dsl.get("graph", {}).get("nodes", []):
            if cid == n["id"]:
                return n["data"]["name"]
        return ""

    def run(self, **kwargs):
        raise NotImplementedError()

    def get_component(self, cpn_id) -> Union[None, dict[str, Any]]:
        return self.components.get(cpn_id)

    def get_component_obj(self, cpn_id) -> ComponentBase:
        return self.components.get(cpn_id)["obj"]

    def get_component_type(self, cpn_id) -> str:
        return self.components.get(cpn_id)["obj"].component_name

    def get_component_input_form(self, cpn_id) -> dict:
        return self.components.get(cpn_id)["obj"].get_input_form()

    def get_tenant_id(self):
        return self._tenant_id

    def get_value_with_variable(self,value: str) -> Any:
        pat = re.compile(r"\{* *\{([a-zA-Z:0-9]+@[A-Za-z0-9_.]+|sys\.[A-Za-z0-9_.]+|env\.[A-Za-z0-9_.]+)\} *\}*")
        out_parts = []
        last = 0

        for m in pat.finditer(value):
            out_parts.append(value[last:m.start()])
            key = m.group(1)
            v = self.get_variable_value(key)
            if v is None:
                rep = ""
            elif isinstance(v, partial):
                buf = []
                for chunk in v():
                    buf.append(chunk)
                rep = "".join(buf)
            elif isinstance(v, str):
                rep = v
            else:
                rep = json.dumps(v, ensure_ascii=False)

            out_parts.append(rep)
            last = m.end()

        out_parts.append(value[last:])
        return("".join(out_parts))

    def get_variable_value(self, exp: str) -> Any:
        exp = exp.strip("{").strip("}").strip(" ").strip("{").strip("}")
        if exp.find("@") < 0:
            return self.globals[exp]
        cpn_id, var_nm = exp.split("@")
        cpn = self.get_component(cpn_id)
        if not cpn:
            raise Exception(f"Can't find variable: '{cpn_id}@{var_nm}'")
        parts = var_nm.split(".", 1)
        root_key = parts[0]
        rest = parts[1] if len(parts) > 1 else ""
        root_val = cpn["obj"].output(root_key)

        if not rest:
            return root_val
        return self.get_variable_param_value(root_val,rest)

    def get_variable_param_value(self, obj: Any, path: str) -> Any:
        cur = obj
        if not path:
            return cur
        for key in path.split('.'):
            if cur is None:
                return None
            if isinstance(cur, str):
                try:
                    cur = json.loads(cur)
                except Exception:
                    return None
            if isinstance(cur, dict):
                cur = cur.get(key)
            else:
                cur = getattr(cur, key, None)
        return cur

    def is_canceled(self) -> bool:
        return has_canceled(self.task_id)

    def cancel_task(self) -> bool:
        try:
            REDIS_CONN.set(f"{self.task_id}-cancel", "x")
        except Exception as e:
            logging.exception(e)
            return False
        return True


class Canvas(Graph):

    def __init__(self, dsl: str, tenant_id=None, task_id=None):
        self.globals = {
            "sys.query": "",
            "sys.user_id": tenant_id,
            "sys.conversation_turns": 0,
            "sys.files": []
        }
        super().__init__(dsl, tenant_id, task_id)

    def load(self):
        super().load()
        self.history = self.dsl["history"]
        if "globals" in self.dsl:
            self.globals = self.dsl["globals"]
        else:
            self.globals = {
            "sys.query": "",
            "sys.user_id": "",
            "sys.conversation_turns": 0,
            "sys.files": []
        }

        self.retrieval = self.dsl["retrieval"]
        self.memory = self.dsl.get("memory", [])

    def __str__(self):
        self.dsl["history"] = self.history
        self.dsl["retrieval"] = self.retrieval
        self.dsl["memory"] = self.memory
        return super().__str__()

    def reset(self, mem=False):
        super().reset()
        if not mem:
            self.history = []
            self.retrieval = []
            self.memory = []
        for k in self.globals.keys():
            if k.startswith("sys."):
                if isinstance(self.globals[k], str):
                    self.globals[k] = ""
                elif isinstance(self.globals[k], int):
                    self.globals[k] = 0
                elif isinstance(self.globals[k], float):
                    self.globals[k] = 0
                elif isinstance(self.globals[k], list):
                    self.globals[k] = []
                elif isinstance(self.globals[k], dict):
                    self.globals[k] = {}
                else:
                    self.globals[k] = None

    def run(self, **kwargs):
        st = time.perf_counter()
        self.message_id = get_uuid()
        created_at = int(time.time())
        self.add_user_input(kwargs.get("query"))
        for k, cpn in self.components.items():
            self.components[k]["obj"].reset(True)

        if kwargs.get("webhook_payload"):
            for k, cpn in self.components.items():
                if self.components[k]["obj"].component_name.lower() == "webhook":
                    for kk, vv in kwargs["webhook_payload"].items():
                        self.components[k]["obj"].set_output(kk, vv)

        for k in kwargs.keys():
            if k in ["query", "user_id", "files"] and kwargs[k]:
                if k == "files":
                    self.globals[f"sys.{k}"] = self.get_files(kwargs[k])
                else:
                    self.globals[f"sys.{k}"] = kwargs[k]
        if not self.globals["sys.conversation_turns"] :
            self.globals["sys.conversation_turns"] = 0
        self.globals["sys.conversation_turns"] += 1

        def decorate(event, dt):
            nonlocal created_at
            return {
                "event": event,
                #"conversation_id": "f3cc152b-24b0-4258-a1a1-7d5e9fc8a115",
                "message_id": self.message_id,
                "created_at": created_at,
                "task_id": self.task_id,
                "data": dt
            }

        if not self.path or self.path[-1].lower().find("userfillup") < 0:
            self.path.append("begin")
            self.retrieval.append({"chunks": [], "doc_aggs": []})

        if self.is_canceled():
            msg = f"Task {self.task_id} has been canceled before starting."
            logging.info(msg)
            raise TaskCanceledException(msg)

        yield decorate("workflow_started", {"inputs": kwargs.get("inputs")})
        self.retrieval.append({"chunks": {}, "doc_aggs": {}})

        def _run_batch(f, t):
            if self.is_canceled():
                msg = f"Task {self.task_id} has been canceled during batch execution."
                logging.info(msg)
                raise TaskCanceledException(msg)

            with ThreadPoolExecutor(max_workers=5) as executor:
                thr = []
                i = f
                while i < t:
                    cpn = self.get_component_obj(self.path[i])
                    if cpn.component_name.lower() in ["begin", "userfillup"]:
                        thr.append(executor.submit(cpn.invoke, inputs=kwargs.get("inputs", {})))
                        i += 1
                    else:
                        for _, ele in cpn.get_input_elements().items():
                            if isinstance(ele, dict) and ele.get("_cpn_id") and ele.get("_cpn_id") not in self.path[:i] and self.path[0].lower().find("userfillup") < 0:
                                self.path.pop(i)
                                t -= 1
                                break
                        else:
                            thr.append(executor.submit(cpn.invoke, **cpn.get_input()))
                            i += 1
                for t in thr:
                    t.result()

        def _node_finished(cpn_obj):
            return decorate("node_finished",{
                           "inputs": cpn_obj.get_input_values(),
                           "outputs": cpn_obj.output(),
                           "component_id": cpn_obj._id,
                           "component_name": self.get_component_name(cpn_obj._id),
                           "component_type": self.get_component_type(cpn_obj._id),
                           "error": cpn_obj.error(),
                           "elapsed_time": time.perf_counter() - cpn_obj.output("_created_time"),
                           "created_at": cpn_obj.output("_created_time"),
                       })

        self.error = ""
        idx = len(self.path) - 1
        partials = []
        while idx < len(self.path):
            to = len(self.path)
            for i in range(idx, to):
                yield decorate("node_started", {
                    "inputs": None, "created_at": int(time.time()),
                    "component_id": self.path[i],
                    "component_name": self.get_component_name(self.path[i]),
                    "component_type": self.get_component_type(self.path[i]),
                    "thoughts": self.get_component_thoughts(self.path[i])
                })
            _run_batch(idx, to)
            to = len(self.path)
            # post processing of components invocation
            for i in range(idx, to):
                cpn = self.get_component(self.path[i])
                cpn_obj = self.get_component_obj(self.path[i])
                if cpn_obj.component_name.lower() == "message":
                    if isinstance(cpn_obj.output("content"), partial):
                        _m = ""
                        for m in cpn_obj.output("content")():
                            if not m:
                                continue
                            if m == "<think>":
                                yield decorate("message", {"content": "", "start_to_think": True})
                            elif m == "</think>":
                                yield decorate("message", {"content": "", "end_to_think": True})
                            else:
                                yield decorate("message", {"content": m})
                                _m += m
                        cpn_obj.set_output("content", _m)
                        cite = re.search(r"\[ID:[ 0-9]+\]", _m)
                    else:
                        yield decorate("message", {"content": cpn_obj.output("content")})
                        cite = re.search(r"\[ID:[ 0-9]+\]",  cpn_obj.output("content"))

                    if isinstance(cpn_obj.output("attachment"), tuple):
                        yield decorate("message", {"attachment": cpn_obj.output("attachment")})
                        
                    yield decorate("message_end", {"reference": self.get_reference() if cite else None})

                    while partials:
                        _cpn_obj = self.get_component_obj(partials[0])
                        if isinstance(_cpn_obj.output("content"), partial):
                            break
                        yield _node_finished(_cpn_obj)
                        partials.pop(0)

                other_branch = False
                if cpn_obj.error():
                    ex = cpn_obj.exception_handler()
                    if ex and ex["goto"]:
                        self.path.extend(ex["goto"])
                        other_branch = True
                    elif ex and ex["default_value"]:
                        yield decorate("message", {"content": ex["default_value"]})
                        yield decorate("message_end", {})
                    else:
                        self.error = cpn_obj.error()

                if cpn_obj.component_name.lower() != "iteration":
                    if isinstance(cpn_obj.output("content"), partial):
                        if self.error:
                            cpn_obj.set_output("content", None)
                            yield _node_finished(cpn_obj)
                        else:
                            partials.append(self.path[i])
                    else:
                        yield _node_finished(cpn_obj)

                def _append_path(cpn_id):
                    nonlocal other_branch
                    if other_branch:
                        return
                    if self.path[-1] == cpn_id:
                        return
                    self.path.append(cpn_id)

                def _extend_path(cpn_ids):
                    nonlocal other_branch
                    if other_branch:
                        return
                    for cpn_id in cpn_ids:
                        _append_path(cpn_id)

                if cpn_obj.component_name.lower() == "iterationitem" and cpn_obj.end():
                    iter = cpn_obj.get_parent()
                    yield _node_finished(iter)
                    _extend_path(self.get_component(cpn["parent_id"])["downstream"])
                elif cpn_obj.component_name.lower() in ["categorize", "switch"]:
                    _extend_path(cpn_obj.output("_next"))
                elif cpn_obj.component_name.lower() == "iteration":
                    _append_path(cpn_obj.get_start())
                elif not cpn["downstream"] and cpn_obj.get_parent():
                    _append_path(cpn_obj.get_parent().get_start())
                else:
                    _extend_path(cpn["downstream"])

            if self.error:
                logging.error(f"Runtime Error: {self.error}")
                break
            idx = to

            if any([self.get_component_obj(c).component_name.lower() == "userfillup" for c in self.path[idx:]]):
                path = [c for c in self.path[idx:] if self.get_component(c)["obj"].component_name.lower() == "userfillup"]
                path.extend([c for c in self.path[idx:] if self.get_component(c)["obj"].component_name.lower() != "userfillup"])
                another_inputs = {}
                tips = ""
                for c in path:
                    o = self.get_component_obj(c)
                    if o.component_name.lower() == "userfillup":
                        o.invoke()
                        another_inputs.update(o.get_input_elements())
                        if o.get_param("enable_tips"):
                            tips = o.output("tips")
                self.path = path
                yield decorate("user_inputs", {"inputs": another_inputs, "tips": tips})
                return
        self.path = self.path[:idx]
        if not self.error:
            yield decorate("workflow_finished",
                       {
                           "inputs": kwargs.get("inputs"),
                           "outputs": self.get_component_obj(self.path[-1]).output(),
                           "elapsed_time": time.perf_counter() - st,
                           "created_at": st,
                       })
            self.history.append(("assistant", self.get_component_obj(self.path[-1]).output()))
        elif "Task has been canceled" in self.error:
            yield decorate("workflow_finished",
                       {
                           "inputs": kwargs.get("inputs"),
                           "outputs": "Task has been canceled",
                           "elapsed_time": time.perf_counter() - st,
                           "created_at": st,
                       })

    def is_reff(self, exp: str) -> bool:
        exp = exp.strip("{").strip("}")
        if exp.find("@") < 0:
            return exp in self.globals
        arr = exp.split("@")
        if len(arr) != 2:
            return False
        if self.get_component(arr[0]) is None:
            return False
        return True

    def get_history(self, window_size):
        convs = []
        if window_size <= 0:
            return convs
        for role, obj in self.history[window_size * -2:]:
            if isinstance(obj, dict):
                convs.append({"role": role, "content": obj.get("content", "")})
            else:
                convs.append({"role": role, "content": str(obj)})
        return convs

    def add_user_input(self, question):
        self.history.append(("user", question))

    def get_prologue(self):
        return self.components["begin"]["obj"]._param.prologue

    def get_mode(self):
        return self.components["begin"]["obj"]._param.mode

    def set_global_param(self, **kwargs):
        self.globals.update(kwargs)

    def get_preset_param(self):
        return self.components["begin"]["obj"]._param.inputs

    def get_component_input_elements(self, cpnnm):
        return self.components[cpnnm]["obj"].get_input_elements()

    def get_files(self, files: Union[None, list[dict]]) -> list[str]:
        if not files:
            return  []
        def image_to_base64(file):
            return "data:{};base64,{}".format(file["mime_type"],
                                        base64.b64encode(FileService.get_blob(file["created_by"], file["id"])).decode("utf-8"))
        exe = ThreadPoolExecutor(max_workers=5)
        threads = []
        for file in files:
            if file["mime_type"].find("image") >=0:
                threads.append(exe.submit(image_to_base64, file))
                continue
            threads.append(exe.submit(FileService.parse, file["name"], FileService.get_blob(file["created_by"], file["id"]), True, file["created_by"]))
        return [th.result() for th in threads]

    def tool_use_callback(self, agent_id: str, func_name: str, params: dict, result: Any, elapsed_time=None):
        agent_ids = agent_id.split("-->")
        agent_name = self.get_component_name(agent_ids[0])
        path = agent_name if len(agent_ids) < 2 else agent_name+"-->"+"-->".join(agent_ids[1:])
        try:
            bin = REDIS_CONN.get(f"{self.task_id}-{self.message_id}-logs")
            if bin:
                obj = json.loads(bin.encode("utf-8"))
                if obj[-1]["component_id"] == agent_ids[0]:
                    obj[-1]["trace"].append({"path": path, "tool_name": func_name, "arguments": params, "result": result, "elapsed_time": elapsed_time})
                else:
                    obj.append({
                    "component_id": agent_ids[0],
                    "trace": [{"path": path, "tool_name": func_name, "arguments": params, "result": result, "elapsed_time": elapsed_time}]
                })
            else:
                obj = [{
                    "component_id": agent_ids[0],
                    "trace": [{"path": path, "tool_name": func_name, "arguments": params, "result": result, "elapsed_time": elapsed_time}]
                }]
            REDIS_CONN.set_obj(f"{self.task_id}-{self.message_id}-logs", obj, 60*10)
        except Exception as e:
            logging.exception(e)

    def add_reference(self, chunks: list[object], doc_infos: list[object]):
        if not self.retrieval:
            self.retrieval = [{"chunks": {}, "doc_aggs": {}}]

        r = self.retrieval[-1]
        for ck in chunks_format({"chunks": chunks}):
            cid = hash_str2int(ck["id"], 500)
            # cid = uuid.uuid5(uuid.NAMESPACE_DNS, ck["id"])
            if cid not in r:
                r["chunks"][cid] = ck

        for doc in doc_infos:
            if doc["doc_name"] not in r:
                r["doc_aggs"][doc["doc_name"]] = doc

    def get_reference(self):
        if not self.retrieval:
            return {"chunks": {}, "doc_aggs": {}}
        return self.retrieval[-1]

    def add_memory(self, user:str, assist:str, summ: str):
        self.memory.append((user, assist, summ))

    def get_memory(self) -> list[Tuple]:
        return self.memory

    def get_component_thoughts(self, cpn_id) -> str:
        return self.components.get(cpn_id)["obj"].thoughts()


```

## Detailed Analysis

### File Role in Repository

The file `agent/canvas.py` is located in the `agent` directory.

This file is part of the **Agent System** for workflow management.

### Architecture Context

Files in this location typically handle concerns related to agent.

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
- [settings.py](settings.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
