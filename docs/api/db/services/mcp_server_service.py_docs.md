# Documentation: api/db/services/mcp_server_service.py

## File Metadata

- **Path**: `api/db/services/mcp_server_service.py`
- **Size**: 3349 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `api/db/services/mcp_server_service.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `peewee`
- `api.db.db_models`
- `api.db.services.common_service`

### Classes Defined

This file defines 1 class(es):

#### Class: `MCPServerService` (line 22)

**Docstring**: Service class for managing MCP server related database operations.

This class extends CommonService to provide specialized functionality for MCP server management,
including MCP server creation, upda...

**Methods**: get_servers, get_by_name_and_tenant, delete_by_tenant_id

### Functions Defined

This file defines 3 function(s):

#### Function: `get_servers` (line 36)

**Parameters**: cls, tenant_id, id_list, page_number, items_per_page, orderby, desc, keywords

**Docstring**: Retrieve all MCP servers associated with a tenant.

This method fetches all MCP servers for a given tenant, ordered by creation time.
It only includes fields for list display.

Args:
    tenant_id (st...

#### Function: `get_by_name_and_tenant` (line 82)

**Parameters**: cls, name, tenant_id

#### Function: `delete_by_tenant_id` (line 91)

**Parameters**: cls, tenant_id

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
from peewee import fn

from api.db.db_models import DB, MCPServer
from api.db.services.common_service import CommonService


class MCPServerService(CommonService):
    """Service class for managing MCP server related database operations.

    This class extends CommonService to provide specialized functionality for MCP server management,
    including MCP server creation, updates, and deletions.

    Attributes:
        model: The MCPServer model class for database operations.
    """

    model = MCPServer

    @classmethod
    @DB.connection_context()
    def get_servers(cls, tenant_id: str, id_list: list[str] | None, page_number, items_per_page, orderby, desc,
                    keywords):
        """Retrieve all MCP servers associated with a tenant.

        This method fetches all MCP servers for a given tenant, ordered by creation time.
        It only includes fields for list display.

        Args:
            tenant_id (str): The unique identifier of the tenant.
            id_list (list[str]): Get servers by ID list. Will ignore this condition if None.

        Returns:
            list[dict]: List of MCP server dictionaries containing MCP server details.
                       Returns None if no MCP servers are found.
        """
        fields = [
            cls.model.id,
            cls.model.name,
            cls.model.server_type,
            cls.model.url,
            cls.model.description,
            cls.model.variables,
            cls.model.create_date,
            cls.model.update_date,
        ]

        query = cls.model.select(*fields).order_by(cls.model.create_time.desc()).where(cls.model.tenant_id == tenant_id)

        if id_list:
            query = query.where(cls.model.id.in_(id_list))
        if keywords:
            query = query.where(fn.LOWER(cls.model.name).contains(keywords.lower()))
        if desc:
            query = query.order_by(cls.model.getter_by(orderby).desc())
        else:
            query = query.order_by(cls.model.getter_by(orderby).asc())
        if page_number and items_per_page:
            query = query.paginate(page_number, items_per_page)

        servers = list(query.dicts())
        if not servers:
            return None
        return servers

    @classmethod
    @DB.connection_context()
    def get_by_name_and_tenant(cls, name: str, tenant_id: str):
        try:
            mcp_server = cls.model.query(name=name, tenant_id=tenant_id)
            return bool(mcp_server), mcp_server
        except Exception:
            return False, None

    @classmethod
    @DB.connection_context()
    def delete_by_tenant_id(cls, tenant_id: str):
        return cls.model.delete().where(cls.model.tenant_id == tenant_id).execute()

```

## Detailed Analysis

### File Role in Repository

The file `api/db/services/mcp_server_service.py` is located in the `api/db/services` directory.

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
