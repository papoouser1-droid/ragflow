# Documentation: common/data_source/jira_connector.py

## File Metadata

- **Path**: `common/data_source/jira_connector.py`
- **Size**: 3971 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `common/data_source/jira_connector.py`.

## Python Module Overview

### Module Docstring

```
Jira connector
```

### Imports and Dependencies

This module imports the following dependencies:

- `typing`
- `jira`
- `common.data_source.config`
- `common.data_source.exceptions`
- `common.data_source.interfaces`
- `common.data_source.models`

### Classes Defined

This file defines 1 class(es):

#### Class: `JiraConnector` (line 23)

**Docstring**: Jira connector for accessing Jira issues and projects...

**Methods**: __init__, load_credentials, validate_connector_settings, poll_source, load_from_checkpoint, load_from_checkpoint_with_perm_sync, build_dummy_checkpoint, validate_checkpoint_json, retrieve_all_slim_docs_perm_sync

### Functions Defined

This file defines 9 function(s):

#### Function: `__init__` (line 26)

**Parameters**: self, batch_size

#### Function: `load_credentials` (line 30)

**Parameters**: self, credentials

**Docstring**: Load Jira credentials...

#### Function: `validate_connector_settings` (line 54)

**Parameters**: self

**Docstring**: Validate Jira connector settings...

#### Function: `poll_source` (line 70)

**Parameters**: self, start, end

**Docstring**: Poll Jira for recent issues...

#### Function: `load_from_checkpoint` (line 75)

**Parameters**: self, start, end, checkpoint

**Docstring**: Load documents from checkpoint...

#### Function: `load_from_checkpoint_with_perm_sync` (line 85)

**Parameters**: self, start, end, checkpoint

**Docstring**: Load documents from checkpoint with permission sync...

#### Function: `build_dummy_checkpoint` (line 95)

**Parameters**: self

**Docstring**: Build dummy checkpoint...

#### Function: `validate_checkpoint_json` (line 99)

**Parameters**: self, checkpoint_json

**Docstring**: Validate checkpoint JSON...

#### Function: `retrieve_all_slim_docs_perm_sync` (line 104)

**Parameters**: self, start, end, callback

**Docstring**: Retrieve all simplified documents with permission sync...

## Original Source Code

```py
"""Jira connector"""

from typing import Any

from jira import JIRA

from common.data_source.config import INDEX_BATCH_SIZE
from common.data_source.exceptions import (
    ConnectorValidationError,
    InsufficientPermissionsError,
    UnexpectedValidationError, ConnectorMissingCredentialError
)
from common.data_source.interfaces import (
    CheckpointedConnectorWithPermSync,
    SecondsSinceUnixEpoch,
    SlimConnectorWithPermSync
)
from common.data_source.models import (
    ConnectorCheckpoint
)


class JiraConnector(CheckpointedConnectorWithPermSync, SlimConnectorWithPermSync):
    """Jira connector for accessing Jira issues and projects"""

    def __init__(self, batch_size: int = INDEX_BATCH_SIZE) -> None:
        self.batch_size = batch_size
        self.jira_client: JIRA | None = None

    def load_credentials(self, credentials: dict[str, Any]) -> dict[str, Any] | None:
        """Load Jira credentials"""
        try:
            url = credentials.get("url")
            username = credentials.get("username")
            password = credentials.get("password")
            token = credentials.get("token")
            
            if not url:
                raise ConnectorMissingCredentialError("Jira URL is required")
            
            if token:
                # API token authentication
                self.jira_client = JIRA(server=url, token_auth=token)
            elif username and password:
                # Basic authentication
                self.jira_client = JIRA(server=url, basic_auth=(username, password))
            else:
                raise ConnectorMissingCredentialError("Jira credentials are incomplete")
            
            return None
        except Exception as e:
            raise ConnectorMissingCredentialError(f"Jira: {e}")

    def validate_connector_settings(self) -> None:
        """Validate Jira connector settings"""
        if not self.jira_client:
            raise ConnectorMissingCredentialError("Jira")
        
        try:
            # Test connection by getting server info
            self.jira_client.server_info()
        except Exception as e:
            if "401" in str(e) or "403" in str(e):
                raise InsufficientPermissionsError("Invalid credentials or insufficient permissions")
            elif "404" in str(e):
                raise ConnectorValidationError("Jira instance not found")
            else:
                raise UnexpectedValidationError(f"Jira validation error: {e}")

    def poll_source(self, start: SecondsSinceUnixEpoch, end: SecondsSinceUnixEpoch) -> Any:
        """Poll Jira for recent issues"""
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

The file `common/data_source/jira_connector.py` is located in the `common/data_source` directory.

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
