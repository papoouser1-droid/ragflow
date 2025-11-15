# Documentation: api/db/db_models.py

## File Metadata

- **Path**: `api/db/db_models.py`
- **Size**: 54352 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `api/db/db_models.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `hashlib`
- `inspect`
- `logging`
- `operator`
- `os`
- `sys`
- `time`
- `typing`
- `datetime`
- `enum`
- `functools`
- `flask_login`
- `itsdangerous.url_safe`
- `peewee`
- `playhouse.migrate`
- `playhouse.pool`
- `api`
- `api.db`
- `api.utils.json_encode`
- `api.utils.configs`

### Classes Defined

This file defines 69 class(es):

#### Class: `TextFieldType` (line 49)

#### Class: `LongTextField` (line 54)

#### Class: `JSONField` (line 58)

**Methods**: __init__, db_value, python_value

#### Class: `ListField` (line 77)

#### Class: `SerializedField` (line 81)

**Methods**: __init__, db_value, python_value

#### Class: `BaseModel` (line 134)

**Methods**: to_json, to_dict, to_human_model_dict, meta, get_primary_keys_name, getter_by, query, insert, _normalize_data

#### Class: `JsonSerializedField` (line 237)

**Methods**: __init__

#### Class: `RetryingPooledMySQLDatabase` (line 242)

**Methods**: __init__, execute_sql, _handle_connection_loss, begin

#### Class: `RetryingPooledPostgresqlDatabase` (line 310)

**Methods**: __init__, execute_sql, _handle_connection_loss, begin

#### Class: `PooledDatabase` (line 377)

#### Class: `DatabaseMigrator` (line 382)

#### Class: `BaseDataBase` (line 388)

**Methods**: __init__

#### Class: `PostgresDatabaseLock` (line 446)

**Methods**: __init__, lock, unlock, __enter__, __exit__, __call__

#### Class: `MysqlDatabaseLock` (line 493)

**Methods**: __init__, lock, unlock, __enter__, __exit__, __call__

#### Class: `DatabaseLock` (line 540)

#### Class: `DataBaseModel` (line 557)

#### Class: `User` (line 597)

**Methods**: __str__, get_id

#### Class: `Tenant` (line 626)

#### Class: `UserTenant` (line 644)

#### Class: `InvitationCode` (line 656)

#### Class: `LLMFactories` (line 668)

**Methods**: __str__

#### Class: `LLM` (line 682)

**Methods**: __str__

#### Class: `TenantLLM` (line 701)

**Methods**: __str__

#### Class: `TenantLangfuse` (line 720)

**Methods**: __str__

#### Class: `Knowledgebase` (line 733)

**Methods**: __str__

#### Class: `Document` (line 770)

#### Class: `File` (line 799)

#### Class: `File2Document` (line 814)

#### Class: `Task` (line 823)

#### Class: `Dialog` (line 841)

#### Class: `Conversation` (line 876)

#### Class: `APIToken` (line 888)

#### Class: `API4Conversation` (line 900)

#### Class: `UserCanvas` (line 918)

#### Class: `CanvasTemplate` (line 934)

#### Class: `UserCanvasVersion` (line 947)

#### Class: `MCPServer` (line 959)

#### Class: `Search` (line 973)

**Methods**: __str__

#### Class: `PipelineOperationLog` (line 1018)

#### Class: `Connector` (line 1044)

**Methods**: __str__

#### Class: `Connector2Kb` (line 1064)

#### Class: `DateTimeTzField` (line 1074)

**Methods**: db_value, python_value

#### Class: `SyncLogs` (line 1095)

#### Class: `Meta` (line 558)

#### Class: `Meta` (line 622)

#### Class: `Meta` (line 640)

#### Class: `Meta` (line 652)

#### Class: `Meta` (line 664)

#### Class: `Meta` (line 678)

#### Class: `Meta` (line 696)

#### Class: `Meta` (line 715)

#### Class: `Meta` (line 729)

#### Class: `Meta` (line 766)

#### Class: `Meta` (line 795)

#### Class: `Meta` (line 810)

#### Class: `Meta` (line 819)

#### Class: `Meta` (line 872)

#### Class: `Meta` (line 884)

#### Class: `Meta` (line 895)

#### Class: `Meta` (line 914)

#### Class: `Meta` (line 930)

#### Class: `Meta` (line 943)

#### Class: `Meta` (line 955)

#### Class: `Meta` (line 969)

#### Class: `Meta` (line 1014)

#### Class: `Meta` (line 1040)

#### Class: `Meta` (line 1060)

#### Class: `Meta` (line 1070)

#### Class: `Meta` (line 1111)

### Functions Defined

This file defines 61 function(s):

#### Function: `is_continuous_field` (line 109)

**Parameters**: cls

#### Function: `auto_date_timestamp_field` (line 122)

**Parameters**: None

#### Function: `auto_date_timestamp_db_field` (line 126)

**Parameters**: None

#### Function: `remove_field_name_prefix` (line 130)

**Parameters**: field_name

#### Function: `with_retry` (line 405)

**Parameters**: max_retries, retry_delay

**Docstring**: Decorator: Add retry mechanism to database operations

Args:
    max_retries (int): maximum number of retries
    retry_delay (float): initial retry delay (seconds), will increase exponentially

Retur...

#### Function: `close_connection` (line 549)

**Parameters**: None

#### Function: `init_database_tables` (line 564)

**Parameters**: alter_fields

#### Function: `fill_db_model_object` (line 589)

**Parameters**: model_object, human_model_dict

#### Function: `migrate_db` (line 1115)

**Parameters**: None

#### Function: `__init__` (line 61)

**Parameters**: self, object_hook, object_pairs_hook

#### Function: `db_value` (line 66)

**Parameters**: self, value

#### Function: `python_value` (line 71)

**Parameters**: self, value

#### Function: `__init__` (line 82)

**Parameters**: self, serialized_type, object_hook, object_pairs_hook

#### Function: `db_value` (line 88)

**Parameters**: self, value

#### Function: `python_value` (line 98)

**Parameters**: self, value

#### Function: `to_json` (line 140)

**Parameters**: self

#### Function: `to_dict` (line 144)

**Parameters**: self

#### Function: `to_human_model_dict` (line 147)

**Parameters**: self, only_primary_with

#### Function: `meta` (line 161)

**Parameters**: self

#### Function: `get_primary_keys_name` (line 165)

**Parameters**: cls

#### Function: `getter_by` (line 169)

**Parameters**: cls, attr

#### Function: `query` (line 173)

**Parameters**: cls, reverse, order_by

#### Function: `insert` (line 213)

**Parameters**: cls, __data

#### Function: `_normalize_data` (line 223)

**Parameters**: cls, data, kwargs

#### Function: `__init__` (line 238)

**Parameters**: self, object_hook, object_pairs_hook

#### Function: `__init__` (line 243)

**Parameters**: self

#### Function: `execute_sql` (line 248)

**Parameters**: self, sql, params, commit

#### Function: `_handle_connection_loss` (line 272)

**Parameters**: self

#### Function: `begin` (line 286)

**Parameters**: self

#### Function: `__init__` (line 311)

**Parameters**: self

#### Function: `execute_sql` (line 316)

**Parameters**: self, sql, params, commit

#### Function: `_handle_connection_loss` (line 344)

**Parameters**: self

#### Function: `begin` (line 356)

**Parameters**: self

#### Function: `__init__` (line 389)

**Parameters**: self

#### Function: `decorator` (line 416)

**Parameters**: func

#### Function: `__init__` (line 447)

**Parameters**: self, lock_name, timeout, db

#### Function: `lock` (line 454)

**Parameters**: self

#### Function: `unlock` (line 465)

**Parameters**: self

#### Function: `__enter__` (line 475)

**Parameters**: self

#### Function: `__exit__` (line 480)

**Parameters**: self, exc_type, exc_val, exc_tb

#### Function: `__call__` (line 484)

**Parameters**: self, func

#### Function: `__init__` (line 494)

**Parameters**: self, lock_name, timeout, db

#### Function: `lock` (line 500)

**Parameters**: self

#### Function: `unlock` (line 512)

**Parameters**: self

#### Function: `__enter__` (line 522)

**Parameters**: self

#### Function: `__exit__` (line 527)

**Parameters**: self, exc_type, exc_val, exc_tb

#### Function: `__call__` (line 531)

**Parameters**: self, func

#### Function: `__str__` (line 615)

**Parameters**: self

#### Function: `get_id` (line 618)

**Parameters**: self

#### Function: `__str__` (line 675)

**Parameters**: self

#### Function: `__str__` (line 693)

**Parameters**: self

#### Function: `__str__` (line 712)

**Parameters**: self

#### Function: `__str__` (line 726)

**Parameters**: self

#### Function: `__str__` (line 763)

**Parameters**: self

#### Function: `__str__` (line 1011)

**Parameters**: self

#### Function: `__str__` (line 1057)

**Parameters**: self

#### Function: `db_value` (line 1077)

**Parameters**: self, value

#### Function: `python_value` (line 1085)

**Parameters**: self, value

#### Function: `wrapper` (line 418)

**Parameters**: None

#### Function: `magic` (line 486)

**Parameters**: None

#### Function: `magic` (line 533)

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
import hashlib
import inspect
import logging
import operator
import os
import sys
import time
import typing
from datetime import datetime, timezone
from enum import Enum
from functools import wraps

from flask_login import UserMixin
from itsdangerous.url_safe import URLSafeTimedSerializer as Serializer
from peewee import InterfaceError, OperationalError, BigIntegerField, BooleanField, CharField, CompositeKey, DateTimeField, Field, FloatField, IntegerField, Metadata, Model, TextField
from playhouse.migrate import MySQLMigrator, PostgresqlMigrator, migrate
from playhouse.pool import PooledMySQLDatabase, PooledPostgresqlDatabase

from api import utils
from api.db import SerializedType
from api.utils.json_encode import json_dumps, json_loads
from api.utils.configs import deserialize_b64, serialize_b64

from common.time_utils import current_timestamp, timestamp_to_date, date_string_to_timestamp
from common.decorator import singleton
from common.constants import ParserType
from common import settings


CONTINUOUS_FIELD_TYPE = {IntegerField, FloatField, DateTimeField}
AUTO_DATE_TIMESTAMP_FIELD_PREFIX = {"create", "start", "end", "update", "read_access", "write_access"}


class TextFieldType(Enum):
    MYSQL = "LONGTEXT"
    POSTGRES = "TEXT"


class LongTextField(TextField):
    field_type = TextFieldType[settings.DATABASE_TYPE.upper()].value


class JSONField(LongTextField):
    default_value = {}

    def __init__(self, object_hook=None, object_pairs_hook=None, **kwargs):
        self._object_hook = object_hook
        self._object_pairs_hook = object_pairs_hook
        super().__init__(**kwargs)

    def db_value(self, value):
        if value is None:
            value = self.default_value
        return json_dumps(value)

    def python_value(self, value):
        if not value:
            return self.default_value
        return json_loads(value, object_hook=self._object_hook, object_pairs_hook=self._object_pairs_hook)


class ListField(JSONField):
    default_value = []


class SerializedField(LongTextField):
    def __init__(self, serialized_type=SerializedType.PICKLE, object_hook=None, object_pairs_hook=None, **kwargs):
        self._serialized_type = serialized_type
        self._object_hook = object_hook
        self._object_pairs_hook = object_pairs_hook
        super().__init__(**kwargs)

    def db_value(self, value):
        if self._serialized_type == SerializedType.PICKLE:
            return serialize_b64(value, to_str=True)
        elif self._serialized_type == SerializedType.JSON:
            if value is None:
                return None
            return json_dumps(value, with_type=True)
        else:
            raise ValueError(f"the serialized type {self._serialized_type} is not supported")

    def python_value(self, value):
        if self._serialized_type == SerializedType.PICKLE:
            return deserialize_b64(value)
        elif self._serialized_type == SerializedType.JSON:
            if value is None:
                return {}
            return json_loads(value, object_hook=self._object_hook, object_pairs_hook=self._object_pairs_hook)
        else:
            raise ValueError(f"the serialized type {self._serialized_type} is not supported")


def is_continuous_field(cls: typing.Type) -> bool:
    if cls in CONTINUOUS_FIELD_TYPE:
        return True
    for p in cls.__bases__:
        if p in CONTINUOUS_FIELD_TYPE:
            return True
        elif p is not Field and p is not object:
            if is_continuous_field(p):
                return True
    else:
        return False


def auto_date_timestamp_field():
    return {f"{f}_time" for f in AUTO_DATE_TIMESTAMP_FIELD_PREFIX}


def auto_date_timestamp_db_field():
    return {f"f_{f}_time" for f in AUTO_DATE_TIMESTAMP_FIELD_PREFIX}


def remove_field_name_prefix(field_name):
    return field_name[2:] if field_name.startswith("f_") else field_name


class BaseModel(Model):
    create_time = BigIntegerField(null=True, index=True)
    create_date = DateTimeField(null=True, index=True)
    update_time = BigIntegerField(null=True, index=True)
    update_date = DateTimeField(null=True, index=True)

    def to_json(self):
        # This function is obsolete
        return self.to_dict()

    def to_dict(self):
        return self.__dict__["__data__"]

    def to_human_model_dict(self, only_primary_with: list = None):
        model_dict = self.__dict__["__data__"]

        if not only_primary_with:
            return {remove_field_name_prefix(k): v for k, v in model_dict.items()}

        human_model_dict = {}
        for k in self._meta.primary_key.field_names:
            human_model_dict[remove_field_name_prefix(k)] = model_dict[k]
        for k in only_primary_with:
            human_model_dict[k] = model_dict[f"f_{k}"]
        return human_model_dict

    @property
    def meta(self) -> Metadata:
        return self._meta

    @classmethod
    def get_primary_keys_name(cls):
        return cls._meta.primary_key.field_names if isinstance(cls._meta.primary_key, CompositeKey) else [cls._meta.primary_key.name]

    @classmethod
    def getter_by(cls, attr):
        return operator.attrgetter(attr)(cls)

    @classmethod
    def query(cls, reverse=None, order_by=None, **kwargs):
        filters = []
        for f_n, f_v in kwargs.items():
            attr_name = "%s" % f_n
            if not hasattr(cls, attr_name) or f_v is None:
                continue
            if type(f_v) in {list, set}:
                f_v = list(f_v)
                if is_continuous_field(type(getattr(cls, attr_name))):
                    if len(f_v) == 2:
                        for i, v in enumerate(f_v):
                            if isinstance(v, str) and f_n in auto_date_timestamp_field():
                                # time type: %Y-%m-%d %H:%M:%S
                                f_v[i] = date_string_to_timestamp(v)
                        lt_value = f_v[0]
                        gt_value = f_v[1]
                        if lt_value is not None and gt_value is not None:
                            filters.append(cls.getter_by(attr_name).between(lt_value, gt_value))
                        elif lt_value is not None:
                            filters.append(operator.attrgetter(attr_name)(cls) >= lt_value)
                        elif gt_value is not None:
                            filters.append(operator.attrgetter(attr_name)(cls) <= gt_value)
                else:
                    filters.append(operator.attrgetter(attr_name)(cls) << f_v)
            else:
                filters.append(operator.attrgetter(attr_name)(cls) == f_v)
        if filters:
            query_records = cls.select().where(*filters)
            if reverse is not None:
                if not order_by or not hasattr(cls, f"{order_by}"):
                    order_by = "create_time"
                if reverse is True:
                    query_records = query_records.order_by(cls.getter_by(f"{order_by}").desc())
                elif reverse is False:
                    query_records = query_records.order_by(cls.getter_by(f"{order_by}").asc())
            return [query_record for query_record in query_records]
        else:
            return []

    @classmethod
    def insert(cls, __data=None, **insert):
        if isinstance(__data, dict) and __data:
            __data[cls._meta.combined["create_time"]] = current_timestamp()
        if insert:
            insert["create_time"] = current_timestamp()

        return super().insert(__data, **insert)

    # update and insert will call this method
    @classmethod
    def _normalize_data(cls, data, kwargs):
        normalized = super()._normalize_data(data, kwargs)
        if not normalized:
            return {}

        normalized[cls._meta.combined["update_time"]] = current_timestamp()

        for f_n in AUTO_DATE_TIMESTAMP_FIELD_PREFIX:
            if {f"{f_n}_time", f"{f_n}_date"}.issubset(cls._meta.combined.keys()) and cls._meta.combined[f"{f_n}_time"] in normalized and normalized[cls._meta.combined[f"{f_n}_time"]] is not None:
                normalized[cls._meta.combined[f"{f_n}_date"]] = timestamp_to_date(normalized[cls._meta.combined[f"{f_n}_time"]])

        return normalized


class JsonSerializedField(SerializedField):
    def __init__(self, object_hook=utils.from_dict_hook, object_pairs_hook=None, **kwargs):
        super(JsonSerializedField, self).__init__(serialized_type=SerializedType.JSON, object_hook=object_hook, object_pairs_hook=object_pairs_hook, **kwargs)


class RetryingPooledMySQLDatabase(PooledMySQLDatabase):
    def __init__(self, *args, **kwargs):
        self.max_retries = kwargs.pop("max_retries", 5)
        self.retry_delay = kwargs.pop("retry_delay", 1)
        super().__init__(*args, **kwargs)

    def execute_sql(self, sql, params=None, commit=True):
        for attempt in range(self.max_retries + 1):
            try:
                return super().execute_sql(sql, params, commit)
            except (OperationalError, InterfaceError) as e:
                error_codes = [2013, 2006]
                error_messages = ['', 'Lost connection']
                should_retry = (
                    (hasattr(e, 'args') and e.args and e.args[0] in error_codes) or
                    (str(e) in error_messages) or
                    (hasattr(e, '__class__') and e.__class__.__name__ == 'Inter

... [Content truncated - file is 54352 bytes] ...

art = DateTimeField(null=True, index=True)
    status = CharField(max_length=16, null=True, help_text="schedule", default="schedule", index=True)

    def __str__(self):
        return self.name

    class Meta:
        db_table = "connector"


class Connector2Kb(DataBaseModel):
    id = CharField(max_length=32, primary_key=True)
    connector_id = CharField(max_length=32, null=False, index=True)
    kb_id = CharField(max_length=32, null=False, index=True)
    auto_parse = CharField(max_length=1, null=False, default="1", index=False)

    class Meta:
        db_table = "connector2kb"


class DateTimeTzField(CharField):
    field_type = 'VARCHAR'

    def db_value(self, value: datetime|None) -> str|None:
        if value is not None:
            if value.tzinfo is not None:
                return value.isoformat()
            else:
                return value.replace(tzinfo=timezone.utc).isoformat()
        return value

    def python_value(self, value: str|None) -> datetime|None:
        if value is not None:
            dt = datetime.fromisoformat(value)
            if dt.tzinfo is None:
                import pytz
                return dt.replace(tzinfo=pytz.UTC)
            return dt
        return value


class SyncLogs(DataBaseModel):
    id = CharField(max_length=32, primary_key=True)
    connector_id = CharField(max_length=32, index=True)
    status = CharField(max_length=128, null=False, help_text="Processing status", index=True)
    from_beginning = CharField(max_length=1, null=True, help_text="", default="0", index=False)
    new_docs_indexed = IntegerField(default=0, index=False)
    total_docs_indexed = IntegerField(default=0, index=False)
    docs_removed_from_index = IntegerField(default=0, index=False)
    error_msg = TextField(null=False, help_text="process message", default="")
    error_count = IntegerField(default=0, index=False)
    full_exception_trace = TextField(null=True, help_text="process message", default="")
    time_started = DateTimeField(null=True, index=True)
    poll_range_start = DateTimeTzField(max_length=255, null=True, index=True)
    poll_range_end = DateTimeTzField(max_length=255, null=True, index=True)
    kb_id = CharField(max_length=32, null=False, index=True)

    class Meta:
        db_table = "sync_logs"


def migrate_db():
    logging.disable(logging.ERROR)
    migrator = DatabaseMigrator[settings.DATABASE_TYPE.upper()].value(DB)
    try:
        migrate(migrator.add_column("file", "source_type", CharField(max_length=128, null=False, default="", help_text="where dose this document come from", index=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("tenant", "rerank_id", CharField(max_length=128, null=False, default="BAAI/bge-reranker-v2-m3", help_text="default rerank model ID")))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("dialog", "rerank_id", CharField(max_length=128, null=False, default="", help_text="default rerank model ID")))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("dialog", "top_k", IntegerField(default=1024)))
    except Exception:
        pass
    try:
        migrate(migrator.alter_column_type("tenant_llm", "api_key", CharField(max_length=2048, null=True, help_text="API KEY", index=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("api_token", "source", CharField(max_length=16, null=True, help_text="none|agent|dialog", index=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("tenant", "tts_id", CharField(max_length=256, null=True, help_text="default tts model ID", index=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("api_4_conversation", "source", CharField(max_length=16, null=True, help_text="none|agent|dialog", index=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("task", "retry_count", IntegerField(default=0)))
    except Exception:
        pass
    try:
        migrate(migrator.alter_column_type("api_token", "dialog_id", CharField(max_length=32, null=True, index=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("tenant_llm", "max_tokens", IntegerField(default=8192, index=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("api_4_conversation", "dsl", JSONField(null=True, default={})))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("knowledgebase", "pagerank", IntegerField(default=0, index=False)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("api_token", "beta", CharField(max_length=255, null=True, index=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("task", "digest", TextField(null=True, help_text="task digest", default="")))
    except Exception:
        pass

    try:
        migrate(migrator.add_column("task", "chunk_ids", LongTextField(null=True, help_text="chunk ids", default="")))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("conversation", "user_id", CharField(max_length=255, null=True, help_text="user_id", index=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("document", "meta_fields", JSONField(null=True, default={})))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("task", "task_type", CharField(max_length=32, null=False, default="")))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("task", "priority", IntegerField(default=0)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("user_canvas", "permission", CharField(max_length=16, null=False, help_text="me|team", default="me", index=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("llm", "is_tools", BooleanField(null=False, help_text="support tools", default=False)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("mcp_server", "variables", JSONField(null=True, help_text="MCP Server variables", default=dict)))
    except Exception:
        pass
    try:
        migrate(migrator.rename_column("task", "process_duation", "process_duration"))
    except Exception:
        pass
    try:
        migrate(migrator.rename_column("document", "process_duation", "process_duration"))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("document", "suffix", CharField(max_length=32, null=False, default="", help_text="The real file extension suffix", index=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("api_4_conversation", "errors", TextField(null=True, help_text="errors")))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("dialog", "meta_data_filter", JSONField(null=True, default={})))
    except Exception:
        pass
    try:
        migrate(migrator.alter_column_type("canvas_template", "title", JSONField(null=True, default=dict, help_text="Canvas title")))
    except Exception:
        pass
    try:
        migrate(migrator.alter_column_type("canvas_template", "description", JSONField(null=True, default=dict, help_text="Canvas description")))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("user_canvas", "canvas_category", CharField(max_length=32, null=False, default="agent_canvas", help_text="agent_canvas|dataflow_canvas", index=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("canvas_template", "canvas_category", CharField(max_length=32, null=False, default="agent_canvas", help_text="agent_canvas|dataflow_canvas", index=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("knowledgebase", "pipeline_id", CharField(max_length=32, null=True, help_text="Pipeline ID", index=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("document", "pipeline_id", CharField(max_length=32, null=True, help_text="Pipeline ID", index=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("knowledgebase", "graphrag_task_id", CharField(max_length=32, null=True, help_text="Gragh RAG task ID", index=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("knowledgebase", "raptor_task_id", CharField(max_length=32, null=True, help_text="RAPTOR task ID", index=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("knowledgebase", "graphrag_task_finish_at", DateTimeField(null=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("knowledgebase", "raptor_task_finish_at", CharField(null=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("knowledgebase", "mindmap_task_id", CharField(max_length=32, null=True, help_text="Mindmap task ID", index=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("knowledgebase", "mindmap_task_finish_at", CharField(null=True)))
    except Exception:
        pass
    try:
        migrate(migrator.alter_column_type("tenant_llm", "api_key", TextField(null=True, help_text="API KEY")))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("tenant_llm", "status", CharField(max_length=1, null=False, help_text="is it validate(0: wasted, 1: validate)", default="1", index=True)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("connector2kb", "auto_parse", CharField(max_length=1, null=False, default="1", index=False)))
    except Exception:
        pass
    try:
        migrate(migrator.add_column("llm_factories", "rank", IntegerField(default=0, index=False)))
    except Exception:
        pass
    logging.disable(logging.NOTSET)

```

## Detailed Analysis

### File Role in Repository

The file `api/db/db_models.py` is located in the `api/db` directory.

This file is part of the **API/Backend** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to db.

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
- [db_utils.py](db_utils.py_docs.md)
- [init_data.py](init_data.py_docs.md)
- [reload_config_base.py](reload_config_base.py_docs.md)
- [runtime_config.py](runtime_config.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
