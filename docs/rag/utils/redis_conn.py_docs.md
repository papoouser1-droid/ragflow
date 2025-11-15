# Documentation: rag/utils/redis_conn.py

## File Metadata

- **Path**: `rag/utils/redis_conn.py`
- **Size**: 14292 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `rag/utils/redis_conn.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `json`
- `uuid`
- `valkey`
- `common.decorator`
- `common`
- `valkey.lock`
- `trio`

### Classes Defined

This file defines 3 class(es):

#### Class: `RedisMsg` (line 36)

**Methods**: __init__, ack, get_message, get_msg_id

#### Class: `RedisDB` (line 60)

**Methods**: __init__, register_scripts, __open__, health, info, is_alive, exist, get, set_obj, set, sadd, srem, smembers, zadd, zcount, zpopmin, zrangebyscore, transaction, queue_product, queue_consumer, get_unacked_iterator, get_pending_msg, requeue_msg, queue_info, delete_if_equal, delete

#### Class: `RedisDistributedLock` (line 386)

**Methods**: __init__, acquire, release

### Functions Defined

This file defines 33 function(s):

#### Function: `__init__` (line 37)

**Parameters**: self, consumer, queue_name, group_name, msg_id, message

#### Function: `ack` (line 44)

**Parameters**: self

#### Function: `get_message` (line 52)

**Parameters**: self

#### Function: `get_msg_id` (line 55)

**Parameters**: self

#### Function: `__init__` (line 71)

**Parameters**: self

#### Function: `register_scripts` (line 76)

**Parameters**: self

#### Function: `__open__` (line 81)

**Parameters**: self

#### Function: `health` (line 100)

**Parameters**: self

#### Function: `info` (line 109)

**Parameters**: self

#### Function: `is_alive` (line 123)

**Parameters**: self

#### Function: `exist` (line 126)

**Parameters**: self, k

#### Function: `get` (line 135)

**Parameters**: self, k

#### Function: `set_obj` (line 144)

**Parameters**: self, k, obj, exp

#### Function: `set` (line 153)

**Parameters**: self, k, v, exp

#### Function: `sadd` (line 162)

**Parameters**: self, key, member

#### Function: `srem` (line 171)

**Parameters**: self, key, member

#### Function: `smembers` (line 180)

**Parameters**: self, key

#### Function: `zadd` (line 191)

**Parameters**: self, key, member, score

#### Function: `zcount` (line 200)

**Parameters**: self, key, min, max

#### Function: `zpopmin` (line 209)

**Parameters**: self, key, count

#### Function: `zrangebyscore` (line 218)

**Parameters**: self, key, min, max

#### Function: `transaction` (line 229)

**Parameters**: self, key, value, exp

#### Function: `queue_product` (line 242)

**Parameters**: self, queue, message

#### Function: `queue_consumer` (line 255)

**Parameters**: self, queue_name, group_name, consumer_name, msg_id

**Docstring**: https://redis.io/docs/latest/commands/xreadgroup/...

#### Function: `get_unacked_iterator` (line 302)

**Parameters**: self, queue_names, group_name, consumer_name

#### Function: `get_pending_msg` (line 328)

**Parameters**: self, queue, group_name

#### Function: `requeue_msg` (line 339)

**Parameters**: self, queue, group_name, msg_id

#### Function: `queue_info` (line 352)

**Parameters**: self, queue, group_name

#### Function: `delete_if_equal` (line 366)

**Parameters**: self, key, expected_value

**Docstring**: Do following atomically:
Delete a key if its value is equals to the given one, do nothing otherwise....

#### Function: `delete` (line 373)

**Parameters**: self, key

#### Function: `__init__` (line 387)

**Parameters**: self, lock_key, lock_value, timeout, blocking_timeout

#### Function: `acquire` (line 396)

**Parameters**: self

#### Function: `release` (line 407)

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

import logging
import json
import uuid

import valkey as redis
from common.decorator import singleton
from common import settings
from valkey.lock import Lock
import trio

REDIS = {}
try:
    REDIS = settings.decrypt_database_config(name="redis")
except Exception:
    try:
        REDIS = settings.get_base_config("redis", {})
    except Exception:
        REDIS = {}

class RedisMsg:
    def __init__(self, consumer, queue_name, group_name, msg_id, message):
        self.__consumer = consumer
        self.__queue_name = queue_name
        self.__group_name = group_name
        self.__msg_id = msg_id
        self.__message = json.loads(message["message"])

    def ack(self):
        try:
            self.__consumer.xack(self.__queue_name, self.__group_name, self.__msg_id)
            return True
        except Exception as e:
            logging.warning("[EXCEPTION]ack" + str(self.__queue_name) + "||" + str(e))
        return False

    def get_message(self):
        return self.__message

    def get_msg_id(self):
        return self.__msg_id


@singleton
class RedisDB:
    lua_delete_if_equal = None
    LUA_DELETE_IF_EQUAL_SCRIPT = """
        local current_value = redis.call('get', KEYS[1])
        if current_value and current_value == ARGV[1] then
            redis.call('del', KEYS[1])
            return 1
        end
        return 0
    """

    def __init__(self):
        self.REDIS = None
        self.config = REDIS
        self.__open__()

    def register_scripts(self) -> None:
        cls = self.__class__
        client = self.REDIS
        cls.lua_delete_if_equal = client.register_script(cls.LUA_DELETE_IF_EQUAL_SCRIPT)

    def __open__(self):
        try:
            conn_params = {
                "host": self.config["host"].split(":")[0],
                "port": int(self.config.get("host", ":6379").split(":")[1]),
                "db": int(self.config.get("db", 1)),
                "decode_responses": True,
            }
            password = self.config.get("password")
            if password:
                conn_params["password"] = password

            self.REDIS = redis.StrictRedis(**conn_params)

            self.register_scripts()
        except Exception as e:
            logging.warning(f"Redis can't be connected. Error: {str(e)}")
        return self.REDIS

    def health(self):
        self.REDIS.ping()
        a, b = "xx", "yy"
        self.REDIS.set(a, b, 3)

        if self.REDIS.get(a) == b:
            return True
        return False

    def info(self):
        info = self.REDIS.info()
        return {
            'redis_version': info["redis_version"],
            'server_mode': info["server_mode"] if "server_mode" in info else info.get("redis_mode", ""),
            'used_memory': info["used_memory_human"],
            'total_system_memory': info["total_system_memory_human"],
            'mem_fragmentation_ratio': info["mem_fragmentation_ratio"],
            'connected_clients': info["connected_clients"],
            'blocked_clients': info["blocked_clients"],
            'instantaneous_ops_per_sec': info["instantaneous_ops_per_sec"],
            'total_commands_processed': info["total_commands_processed"]
        }

    def is_alive(self):
        return self.REDIS is not None

    def exist(self, k):
        if not self.REDIS:
            return None
        try:
            return self.REDIS.exists(k)
        except Exception as e:
            logging.warning("RedisDB.exist " + str(k) + " got exception: " + str(e))
            self.__open__()

    def get(self, k):
        if not self.REDIS:
            return None
        try:
            return self.REDIS.get(k)
        except Exception as e:
            logging.warning("RedisDB.get " + str(k) + " got exception: " + str(e))
            self.__open__()

    def set_obj(self, k, obj, exp=3600):
        try:
            self.REDIS.set(k, json.dumps(obj, ensure_ascii=False), exp)
            return True
        except Exception as e:
            logging.warning("RedisDB.set_obj " + str(k) + " got exception: " + str(e))
            self.__open__()
        return False

    def set(self, k, v, exp=3600):
        try:
            self.REDIS.set(k, v, exp)
            return True
        except Exception as e:
            logging.warning("RedisDB.set " + str(k) + " got exception: " + str(e))
            self.__open__()
        return False

    def sadd(self, key: str, member: str):
        try:
            self.REDIS.sadd(key, member)
            return True
        except Exception as e:
            logging.warning("RedisDB.sadd " + str(key) + " got exception: " + str(e))
            self.__open__()
        return False

    def srem(self, key: str, member: str):
        try:
            self.REDIS.srem(key, member)
            return True
        except Exception as e:
            logging.warning("RedisDB.srem " + str(key) + " got exception: " + str(e))
            self.__open__()
        return False

    def smembers(self, key: str):
        try:
            res = self.REDIS.smembers(key)
            return res
        except Exception as e:
            logging.warning(
                "RedisDB.smembers " + str(key) + " got exception: " + str(e)
            )
            self.__open__()
        return None

    def zadd(self, key: str, member: str, score: float):
        try:
            self.REDIS.zadd(key, {member: score})
            return True
        except Exception as e:
            logging.warning("RedisDB.zadd " + str(key) + " got exception: " + str(e))
            self.__open__()
        return False

    def zcount(self, key: str, min: float, max: float):
        try:
            res = self.REDIS.zcount(key, min, max)
            return res
        except Exception as e:
            logging.warning("RedisDB.zcount " + str(key) + " got exception: " + str(e))
            self.__open__()
        return 0

    def zpopmin(self, key: str, count: int):
        try:
            res = self.REDIS.zpopmin(key, count)
            return res
        except Exception as e:
            logging.warning("RedisDB.zpopmin " + str(key) + " got exception: " + str(e))
            self.__open__()
        return None

    def zrangebyscore(self, key: str, min: float, max: float):
        try:
            res = self.REDIS.zrangebyscore(key, min, max)
            return res
        except Exception as e:
            logging.warning(
                "RedisDB.zrangebyscore " + str(key) + " got exception: " + str(e)
            )
            self.__open__()
        return None

    def transaction(self, key, value, exp=3600):
        try:
            pipeline = self.REDIS.pipeline(transaction=True)
            pipeline.set(key, value, exp, nx=True)
            pipeline.execute()
            return True
        except Exception as e:
            logging.warning(
                "RedisDB.transaction " + str(key) + " got exception: " + str(e)
            )
            self.__open__()
        return False

    def queue_product(self, queue, message) -> bool:
        for _ in range(3):
            try:
                payload = {"message": json.dumps(message)}
                self.REDIS.xadd(queue, payload)
                return True
            except Exception as e:
                logging.exception(
                    "RedisDB.queue_product " + str(queue) + " got exception: " + str(e)
                )
                self.__open__()
        return False

    def queue_consumer(self, queue_name, group_name, consumer_name, msg_id=b">") -> RedisMsg:
        """https://redis.io/docs/latest/commands/xreadgroup/"""
        for _ in range(3):
            try:

                try:
                    group_info = self.REDIS.xinfo_groups(queue_name)
                    if not any(gi["name"] == group_name for gi in group_info):
                        self.REDIS.xgroup_create(queue_name, group_name, id="0", mkstream=True)
                except redis.exceptions.ResponseError as e:
                    if "no such key" in str(e).lower():
                        self.REDIS.xgroup_create(queue_name, group_name, id="0", mkstream=True)
                    elif "busygroup" in str(e).lower():
                        logging.warning("Group already exists, continue.")
                        pass
                    else:
                        raise

                args = {
                    "groupname": group_name,
                    "consumername": consumer_name,
                    "count": 1,
                    "block": 5,
                    "streams": {queue_name: msg_id},
                }
                messages = self.REDIS.xreadgroup(**args)
                if not messages:
                    return None
                stream, element_list = messages[0]
                if not element_list:
                    return None
                msg_id, payload = element_list[0]
                res = RedisMsg(self.REDIS, queue_name, group_name, msg_id, payload)
                return res
            except Exception as e:
                if str(e) == 'no such key':
                    pass
                else:
                    logging.exception(
                        "RedisDB.queue_consumer "
                        + str(queue_name)
                        + " got exception: "
                        + str(e)
                    )
                    self.__open__()
        return None

    def get_unacked_iterator(self, queue_names: list[str], group_name, consumer_name):
        try:
            for queue_name in queue_names:
                try:
                    group_info = self.REDIS.xinfo_groups(queue_name)
                except Exception as e:
                    if str(e) == 'no such key':
                        logging.warning(f"RedisDB.get_unacked_iterator queue {queue_name} doesn't exist")
                        continue
                if not any(gi["name"] == group_name for gi in group_info):
                    logging.warning(f"RedisDB.get_unacked_iterator queue {queue_name} group {group_name} doesn't exist")
                    continue
                current_min = 0
                while True:
                    payload = self.queue_consumer(queue_name, group_name, consumer_name, current_min)
                    if not payload:
                        break
                    current_min = payload.get_msg_id()
                    logging.info(f"RedisDB.get_unacked_iterator {queue_name} {consumer_name} {current_min}")
                    yield payload
        except Exception:
            logging.exception(
                "RedisDB.get_unacked_iterator got exception: "
            )
            self.__open__()

    def get_pending_msg(self, queue, group_name):
        try:
            messages = self.REDIS.xpending_range(queue, group_name, '-', '+', 10)
            return messages
        except Exception as e:
            if 'No such key' not in (str(e) or ''):
                logging.warning(
                    "RedisDB.get_pending_msg " + str(queue) + " got exception: " + str(e)
                )
        return []

    def requeue_msg(self, queue: str, group_name: str, msg_id: str):
        for _ in range(3):
            try:
                messages = self.REDIS.xrange(queue, msg_id, msg_id)
                if messages:
                    self.REDIS.xadd(queue, messages[0][1])
                    self.REDIS.xack(queue, group_name, msg_id)
            except Exception as e:
                logging.warning(
                    "RedisDB.get_pending_msg " + str(queue) + " got exception: " + str(e)
                )
                self.__open__()

    def queue_info(self, queue, group_name) -> dict | None:
        for _ in range(3):
            try:
                groups = self.REDIS.xinfo_groups(queue)
                for group in groups:
                    if group["name"] == group_name:
                        return group
            except Exception as e:
                logging.warning(
                    "RedisDB.queue_info " + str(queue) + " got exception: " + str(e)
                )
                self.__open__()
        return None

    def delete_if_equal(self, key: str, expected_value: str) -> bool:
        """
        Do following atomically:
        Delete a key if its value is equals to the given one, do nothing otherwise.
        """
        return bool(self.lua_delete_if_equal(keys=[key], args=[expected_value], client=self.REDIS))

    def delete(self, key) -> bool:
        try:
            self.REDIS.delete(key)
            return True
        except Exception as e:
            logging.warning("RedisDB.delete " + str(key) + " got exception: " + str(e))
            self.__open__()
        return False


REDIS_CONN = RedisDB()


class RedisDistributedLock:
    def __init__(self, lock_key, lock_value=None, timeout=10, blocking_timeout=1):
        self.lock_key = lock_key
        if lock_value:
            self.lock_value = lock_value
        else:
            self.lock_value = str(uuid.uuid4())
        self.timeout = timeout
        self.lock = Lock(REDIS_CONN.REDIS, lock_key, timeout=timeout, blocking_timeout=blocking_timeout)

    def acquire(self):
        REDIS_CONN.delete_if_equal(self.lock_key, self.lock_value)
        return self.lock.acquire(token=self.lock_value)

    async def spin_acquire(self):
        REDIS_CONN.delete_if_equal(self.lock_key, self.lock_value)
        while True:
            if self.lock.acquire(token=self.lock_value):
                break
            await trio.sleep(10)

    def release(self):
        REDIS_CONN.delete_if_equal(self.lock_key, self.lock_value)

```

## Detailed Analysis

### File Role in Repository

The file `rag/utils/redis_conn.py` is located in the `rag/utils` directory.

This file is part of the **RAG (Retrieval-Augmented Generation)** core engine.

### Architecture Context

Files in this location typically handle concerns related to utils.

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
- [azure_sas_conn.py](azure_sas_conn.py_docs.md)
- [azure_spn_conn.py](azure_spn_conn.py_docs.md)
- [base64_image.py](base64_image.py_docs.md)
- [doc_store_conn.py](doc_store_conn.py_docs.md)
- [es_conn.py](es_conn.py_docs.md)
- [file_utils.py](file_utils.py_docs.md)
- [infinity_conn.py](infinity_conn.py_docs.md)
- [mcp_tool_call_conn.py](mcp_tool_call_conn.py_docs.md)
- [minio_conn.py](minio_conn.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
