# Documentation: api/db/services/user_service.py

## File Metadata

- **Path**: `api/db/services/user_service.py`
- **Size**: 11755 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `api/db/services/user_service.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `hashlib`
- `datetime`
- `logging`
- `peewee`
- `werkzeug.security`
- `api.db`
- `api.db.db_models`
- `api.db.db_models`
- `api.db.services.common_service`
- `common.misc_utils`
- `common.time_utils`
- `common.constants`
- `common`

### Classes Defined

This file defines 3 class(es):

#### Class: `UserService` (line 33)

**Docstring**: Service class for managing user-related database operations.

This class extends CommonService to provide specialized functionality for user management,
including authentication, user creation, update...

**Methods**: query, filter_by_id, query_user, query_user_by_email, save, delete_user, update_user, update_user_password, is_admin, get_all_users

#### Class: `TenantService` (line 168)

**Docstring**: Service class for managing tenant-related database operations.

This class extends CommonService to provide functionality for tenant management,
including tenant information retrieval and credit manag...

**Methods**: get_info_by, get_joined_tenants_by_user_id, decrease, user_gateway

#### Class: `UserTenantService` (line 227)

**Docstring**: Service class for managing user-tenant relationship operations.

This class extends CommonService to handle the many-to-many relationship
between users and tenants, managing user roles and tenant memb...

**Methods**: filter_by_id, save, get_by_tenant_id, get_tenants_by_user_id, get_user_tenant_relation_by_user_id, get_num_members, filter_by_tenant_and_user_id

### Functions Defined

This file defines 21 function(s):

#### Function: `query` (line 46)

**Parameters**: cls, cols, reverse, order_by

#### Function: `filter_by_id` (line 70)

**Parameters**: cls, user_id

**Docstring**: Retrieve a user by their ID.

Args:
    user_id: The unique identifier of the user.

Returns:
    User object if found, None otherwise....

#### Function: `query_user` (line 87)

**Parameters**: cls, email, password

**Docstring**: Authenticate a user with email and password.

Args:
    email: User's email address.
    password: User's password in plain text.

Returns:
    User object if authentication successful, None otherwise...

#### Function: `query_user_by_email` (line 106)

**Parameters**: cls, email

#### Function: `save` (line 112)

**Parameters**: cls

#### Function: `delete_user` (line 128)

**Parameters**: cls, user_ids, update_user_dict

#### Function: `update_user` (line 135)

**Parameters**: cls, user_id, user_dict

#### Function: `update_user_password` (line 145)

**Parameters**: cls, user_id, new_password

#### Function: `is_admin` (line 156)

**Parameters**: cls, user_id

#### Function: `get_all_users` (line 163)

**Parameters**: cls

#### Function: `get_info_by` (line 181)

**Parameters**: cls, user_id

#### Function: `get_joined_tenants_by_user_id` (line 199)

**Parameters**: cls, user_id

#### Function: `decrease` (line 214)

**Parameters**: cls, user_id, num

#### Function: `user_gateway` (line 222)

**Parameters**: cls, tenant_id

#### Function: `filter_by_id` (line 240)

**Parameters**: cls, user_tenant_id

#### Function: `save` (line 249)

**Parameters**: cls

#### Function: `get_by_tenant_id` (line 257)

**Parameters**: cls, tenant_id

#### Function: `get_tenants_by_user_id` (line 279)

**Parameters**: cls, user_id

#### Function: `get_user_tenant_relation_by_user_id` (line 294)

**Parameters**: cls, user_id

#### Function: `get_num_members` (line 305)

**Parameters**: cls, user_id

#### Function: `filter_by_tenant_and_user_id` (line 311)

**Parameters**: cls, tenant_id, user_id

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
from datetime import datetime
import logging

import peewee
from werkzeug.security import generate_password_hash, check_password_hash

from api.db import UserTenantRole
from api.db.db_models import DB, UserTenant
from api.db.db_models import User, Tenant
from api.db.services.common_service import CommonService
from common.misc_utils import get_uuid
from common.time_utils import current_timestamp, datetime_format
from common.constants import StatusEnum
from common import settings


class UserService(CommonService):
    """Service class for managing user-related database operations.

    This class extends CommonService to provide specialized functionality for user management,
    including authentication, user creation, updates, and deletions.

    Attributes:
        model: The User model class for database operations.
    """
    model = User

    @classmethod
    @DB.connection_context()
    def query(cls, cols=None, reverse=None, order_by=None, **kwargs):
        if 'access_token' in kwargs:
            access_token = kwargs['access_token']

            # Reject empty, None, or whitespace-only access tokens
            if not access_token or not str(access_token).strip():
                logging.warning("UserService.query: Rejecting empty access_token query")
                return cls.model.select().where(cls.model.id == "INVALID_EMPTY_TOKEN")  # Returns empty result

            # Reject tokens that are too short (should be UUID, 32+ chars)
            if len(str(access_token).strip()) < 32:
                logging.warning(f"UserService.query: Rejecting short access_token query: {len(str(access_token))} chars")
                return cls.model.select().where(cls.model.id == "INVALID_SHORT_TOKEN")  # Returns empty result

            # Reject tokens that start with "INVALID_" (from logout)
            if str(access_token).startswith("INVALID_"):
                logging.warning("UserService.query: Rejecting invalidated access_token")
                return cls.model.select().where(cls.model.id == "INVALID_LOGOUT_TOKEN")  # Returns empty result

        # Call parent query method for valid requests
        return super().query(cols=cols, reverse=reverse, order_by=order_by, **kwargs)

    @classmethod
    @DB.connection_context()
    def filter_by_id(cls, user_id):
        """Retrieve a user by their ID.

        Args:
            user_id: The unique identifier of the user.

        Returns:
            User object if found, None otherwise.
        """
        try:
            user = cls.model.select().where(cls.model.id == user_id).get()
            return user
        except peewee.DoesNotExist:
            return None

    @classmethod
    @DB.connection_context()
    def query_user(cls, email, password):
        """Authenticate a user with email and password.

        Args:
            email: User's email address.
            password: User's password in plain text.

        Returns:
            User object if authentication successful, None otherwise.
        """
        user = cls.model.select().where((cls.model.email == email),
                                        (cls.model.status == StatusEnum.VALID.value)).first()
        if user and check_password_hash(str(user.password), password):
            return user
        else:
            return None

    @classmethod
    @DB.connection_context()
    def query_user_by_email(cls, email):
        users = cls.model.select().where((cls.model.email == email))
        return list(users)

    @classmethod
    @DB.connection_context()
    def save(cls, **kwargs):
        if "id" not in kwargs:
            kwargs["id"] = get_uuid()
        if "password" in kwargs:
            kwargs["password"] = generate_password_hash(
                str(kwargs["password"]))

        kwargs["create_time"] = current_timestamp()
        kwargs["create_date"] = datetime_format(datetime.now())
        kwargs["update_time"] = current_timestamp()
        kwargs["update_date"] = datetime_format(datetime.now())
        obj = cls.model(**kwargs).save(force_insert=True)
        return obj

    @classmethod
    @DB.connection_context()
    def delete_user(cls, user_ids, update_user_dict):
        with DB.atomic():
            cls.model.update({"status": 0}).where(
                cls.model.id.in_(user_ids)).execute()

    @classmethod
    @DB.connection_context()
    def update_user(cls, user_id, user_dict):
        with DB.atomic():
            if user_dict:
                user_dict["update_time"] = current_timestamp()
                user_dict["update_date"] = datetime_format(datetime.now())
                cls.model.update(user_dict).where(
                    cls.model.id == user_id).execute()

    @classmethod
    @DB.connection_context()
    def update_user_password(cls, user_id, new_password):
        with DB.atomic():
            update_dict = {
                "password": generate_password_hash(str(new_password)),
                "update_time": current_timestamp(),
                "update_date": datetime_format(datetime.now())
            }
            cls.model.update(update_dict).where(cls.model.id == user_id).execute()

    @classmethod
    @DB.connection_context()
    def is_admin(cls, user_id):
        return cls.model.select().where(
            cls.model.id == user_id,
            cls.model.is_superuser == 1).count() > 0

    @classmethod
    @DB.connection_context()
    def get_all_users(cls):
        users = cls.model.select()
        return list(users)


class TenantService(CommonService):
    """Service class for managing tenant-related database operations.

    This class extends CommonService to provide functionality for tenant management,
    including tenant information retrieval and credit management.

    Attributes:
        model: The Tenant model class for database operations.
    """
    model = Tenant

    @classmethod
    @DB.connection_context()
    def get_info_by(cls, user_id):
        fields = [
            cls.model.id.alias("tenant_id"),
            cls.model.name,
            cls.model.llm_id,
            cls.model.embd_id,
            cls.model.rerank_id,
            cls.model.asr_id,
            cls.model.img2txt_id,
            cls.model.tts_id,
            cls.model.parser_ids,
            UserTenant.role]
        return list(cls.model.select(*fields)
                    .join(UserTenant, on=((cls.model.id == UserTenant.tenant_id) & (UserTenant.user_id == user_id) & (UserTenant.status == StatusEnum.VALID.value) & (UserTenant.role == UserTenantRole.OWNER)))
                    .where(cls.model.status == StatusEnum.VALID.value).dicts())

    @classmethod
    @DB.connection_context()
    def get_joined_tenants_by_user_id(cls, user_id):
        fields = [
            cls.model.id.alias("tenant_id"),
            cls.model.name,
            cls.model.llm_id,
            cls.model.embd_id,
            cls.model.asr_id,
            cls.model.img2txt_id,
            UserTenant.role]
        return list(cls.model.select(*fields)
                    .join(UserTenant, on=((cls.model.id == UserTenant.tenant_id) & (UserTenant.user_id == user_id) & (UserTenant.status == StatusEnum.VALID.value) & (UserTenant.role == UserTenantRole.NORMAL)))
                    .where(cls.model.status == StatusEnum.VALID.value).dicts())

    @classmethod
    @DB.connection_context()
    def decrease(cls, user_id, num):
        num = cls.model.update(credit=cls.model.credit - num).where(
            cls.model.id == user_id).execute()
        if num == 0:
            raise LookupError("Tenant not found which is supposed to be there")

    @classmethod
    @DB.connection_context()
    def user_gateway(cls, tenant_id):
        hash_obj = hashlib.sha256(tenant_id.encode("utf-8"))
        return int(hash_obj.hexdigest(), 16)%len(settings.MINIO)


class UserTenantService(CommonService):
    """Service class for managing user-tenant relationship operations.

    This class extends CommonService to handle the many-to-many relationship
    between users and tenants, managing user roles and tenant memberships.

    Attributes:
        model: The UserTenant model class for database operations.
    """
    model = UserTenant

    @classmethod
    @DB.connection_context()
    def filter_by_id(cls, user_tenant_id):
        try:
            user_tenant = cls.model.select().where((cls.model.id == user_tenant_id) & (cls.model.status == StatusEnum.VALID.value)).get()
            return user_tenant
        except peewee.DoesNotExist:
            return None

    @classmethod
    @DB.connection_context()
    def save(cls, **kwargs):
        if "id" not in kwargs:
            kwargs["id"] = get_uuid()
        obj = cls.model(**kwargs).save(force_insert=True)
        return obj

    @classmethod
    @DB.connection_context()
    def get_by_tenant_id(cls, tenant_id):
        fields = [
            cls.model.id,
            cls.model.user_id,
            cls.model.status,
            cls.model.role,
            User.nickname,
            User.email,
            User.avatar,
            User.is_authenticated,
            User.is_active,
            User.is_anonymous,
            User.status,
            User.update_date,
            User.is_superuser]
        return list(cls.model.select(*fields)
                    .join(User, on=((cls.model.user_id == User.id) & (cls.model.status == StatusEnum.VALID.value) & (cls.model.role != UserTenantRole.OWNER)))
                    .where(cls.model.tenant_id == tenant_id)
                    .dicts())

    @classmethod
    @DB.connection_context()
    def get_tenants_by_user_id(cls, user_id):
        fields = [
            cls.model.tenant_id,
            cls.model.role,
            User.nickname,
            User.email,
            User.avatar,
            User.update_date
        ]
        return list(cls.model.select(*fields)
                    .join(User, on=((cls.model.tenant_id == User.id) & (UserTenant.user_id == user_id) & (UserTenant.status == StatusEnum.VALID.value)))
                    .where(cls.model.status == StatusEnum.VALID.value).dicts())

    @classmethod
    @DB.connection_context()
    def get_user_tenant_relation_by_user_id(cls, user_id):
        fields = [
            cls.model.id,
            cls.model.user_id,
            cls.model.tenant_id,
            cls.model.role
        ]
        return list(cls.model.select(*fields).where(cls.model.user_id == user_id).dicts().dicts())

    @classmethod
    @DB.connection_context()
    def get_num_members(cls, user_id: str):
        cnt_members = cls.model.select(peewee.fn.COUNT(cls.model.id)).where(cls.model.tenant_id == user_id).scalar()
        return cnt_members

    @classmethod
    @DB.connection_context()
    def filter_by_tenant_and_user_id(cls, tenant_id, user_id):
        try:
            user_tenant = cls.model.select().where(
                (cls.model.tenant_id == tenant_id) & (cls.model.status == StatusEnum.VALID.value) &
                (cls.model.user_id == user_id)
            ).first()
            return user_tenant
        except peewee.DoesNotExist:
            return None

```

## Detailed Analysis

### File Role in Repository

The file `api/db/services/user_service.py` is located in the `api/db/services` directory.

This file is part of the **API/Backend** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to services.

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
- [api_service.py](api_service.py_docs.md)
- [canvas_service.py](canvas_service.py_docs.md)
- [common_service.py](common_service.py_docs.md)
- [connector_service.py](connector_service.py_docs.md)
- [conversation_service.py](conversation_service.py_docs.md)
- [dialog_service.py](dialog_service.py_docs.md)
- [document_service.py](document_service.py_docs.md)
- [file2document_service.py](file2document_service.py_docs.md)
- [file_service.py](file_service.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
