# Documentation: common/data_source/sharepoint_connector.py

## File Metadata

- **Path**: `common/data_source/sharepoint_connector.py`
- **Size**: 4759 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `common/data_source/sharepoint_connector.py`.

## Python Module Overview

### Module Docstring

```
SharePoint connector
```

### Imports and Dependencies

This module imports the following dependencies:

- `typing`
- `msal`
- `office365.graph_client`
- `office365.runtime.client_request`
- `office365.sharepoint.client_context`
- `common.data_source.config`
- `common.data_source.exceptions`
- `common.data_source.interfaces`
- `common.data_source.models`

### Classes Defined

This file defines 1 class(es):

#### Class: `SharePointConnector` (line 21)

**Docstring**: SharePoint connector for accessing SharePoint sites and documents...

**Methods**: __init__, load_credentials, validate_connector_settings, poll_source, load_from_checkpoint, load_from_checkpoint_with_perm_sync, build_dummy_checkpoint, validate_checkpoint_json, retrieve_all_slim_docs_perm_sync

### Functions Defined

This file defines 9 function(s):

#### Function: `__init__` (line 24)

**Parameters**: self, batch_size

#### Function: `load_credentials` (line 29)

**Parameters**: self, credentials

**Docstring**: Load SharePoint credentials...

#### Function: `validate_connector_settings` (line 63)

**Parameters**: self

**Docstring**: Validate SharePoint connector settings...

#### Function: `poll_source` (line 79)

**Parameters**: self, start, end

**Docstring**: Poll SharePoint for recent documents...

#### Function: `load_from_checkpoint` (line 84)

**Parameters**: self, start, end, checkpoint

**Docstring**: Load documents from checkpoint...

#### Function: `load_from_checkpoint_with_perm_sync` (line 94)

**Parameters**: self, start, end, checkpoint

**Docstring**: Load documents from checkpoint with permission sync...

#### Function: `build_dummy_checkpoint` (line 104)

**Parameters**: self

**Docstring**: Build dummy checkpoint...

#### Function: `validate_checkpoint_json` (line 108)

**Parameters**: self, checkpoint_json

**Docstring**: Validate checkpoint JSON...

#### Function: `retrieve_all_slim_docs_perm_sync` (line 113)

**Parameters**: self, start, end, callback

**Docstring**: Retrieve all simplified documents with permission sync...

## Original Source Code

```py
"""SharePoint connector"""

from typing import Any
import msal
from office365.graph_client import GraphClient
from office365.runtime.client_request import ClientRequestException
from office365.sharepoint.client_context import ClientContext

from common.data_source.config import INDEX_BATCH_SIZE
from common.data_source.exceptions import ConnectorValidationError, ConnectorMissingCredentialError
from common.data_source.interfaces import (
    CheckpointedConnectorWithPermSync,
    SecondsSinceUnixEpoch,
    SlimConnectorWithPermSync
)
from common.data_source.models import (
    ConnectorCheckpoint
)


class SharePointConnector(CheckpointedConnectorWithPermSync, SlimConnectorWithPermSync):
    """SharePoint connector for accessing SharePoint sites and documents"""

    def __init__(self, batch_size: int = INDEX_BATCH_SIZE) -> None:
        self.batch_size = batch_size
        self.sharepoint_client = None
        self.graph_client = None

    def load_credentials(self, credentials: dict[str, Any]) -> dict[str, Any] | None:
        """Load SharePoint credentials"""
        try:
            tenant_id = credentials.get("tenant_id")
            client_id = credentials.get("client_id")
            client_secret = credentials.get("client_secret")
            site_url = credentials.get("site_url")
            
            if not all([tenant_id, client_id, client_secret, site_url]):
                raise ConnectorMissingCredentialError("SharePoint credentials are incomplete")
            
            # Create MSAL confidential client
            app = msal.ConfidentialClientApplication(
                client_id=client_id,
                client_credential=client_secret,
                authority=f"https://login.microsoftonline.com/{tenant_id}"
            )
            
            # Get access token
            result = app.acquire_token_for_client(scopes=["https://graph.microsoft.com/.default"])
            
            if "access_token" not in result:
                raise ConnectorMissingCredentialError("Failed to acquire SharePoint access token")
            
            # Create Graph client
            self.graph_client = GraphClient(result["access_token"])
            
            # Create SharePoint client context
            self.sharepoint_client = ClientContext(site_url).with_access_token(result["access_token"])
            
            return None
        except Exception as e:
            raise ConnectorMissingCredentialError(f"SharePoint: {e}")

    def validate_connector_settings(self) -> None:
        """Validate SharePoint connector settings"""
        if not self.sharepoint_client or not self.graph_client:
            raise ConnectorMissingCredentialError("SharePoint")
        
        try:
            # Test connection by getting site info
            site = self.sharepoint_client.site.get().execute_query()
            if not site:
                raise ConnectorValidationError("Failed to access SharePoint site")
        except ClientRequestException as e:
            if "401" in str(e) or "403" in str(e):
                raise ConnectorValidationError("Invalid credentials or insufficient permissions")
            else:
                raise ConnectorValidationError(f"SharePoint validation error: {e}")

    def poll_source(self, start: SecondsSinceUnixEpoch, end: SecondsSinceUnixEpoch) -> Any:
        """Poll SharePoint for recent documents"""
        # Simplified implementation - in production this would handle actual polling
        return []

    def load_from_checkpoint(
        self,
        start: SecondsSinceUnixEpoch,
        end: SecondsSinceUnixEpoch,
        checkpoint: ConnectorCheckpoint,
    ) -> Any:
        """Load documents from checkpoint"""
        # Simplified implementation
        return []

    def load_from_checkpoint_with_perm_sync(
        self,
        start: SecondsSinceUnixEpoch,
        end: SecondsSinceUnixEpoch,
        checkpoint: ConnectorCheckpoint,
    ) -> Any:
        """Load documents from checkpoint with permission sync"""
        # Simplified implementation
        return []

    def build_dummy_checkpoint(self) -> ConnectorCheckpoint:
        """Build dummy checkpoint"""
        return ConnectorCheckpoint()

    def validate_checkpoint_json(self, checkpoint_json: str) -> ConnectorCheckpoint:
        """Validate checkpoint JSON"""
        # Simplified implementation
        return ConnectorCheckpoint()

    def retrieve_all_slim_docs_perm_sync(
        self,
        start: SecondsSinceUnixEpoch | None = None,
        end: SecondsSinceUnixEpoch | None = None,
        callback: Any = None,
    ) -> Any:
        """Retrieve all simplified documents with permission sync"""
        # Simplified implementation
        return []
```

## Detailed Analysis

### File Role in Repository

The file `common/data_source/sharepoint_connector.py` is located in the `common/data_source` directory.

### Architecture Context

Files in this location typically handle concerns related to data_source.

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
- [blob_connector.py](blob_connector.py_docs.md)
- [config.py](config.py_docs.md)
- [confluence_connector.py](confluence_connector.py_docs.md)
- [discord_connector.py](discord_connector.py_docs.md)
- [dropbox_connector.py](dropbox_connector.py_docs.md)
- [exceptions.py](exceptions.py_docs.md)
- [file_types.py](file_types.py_docs.md)
- [gmail_connector.py](gmail_connector.py_docs.md)
- [html_utils.py](html_utils.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
