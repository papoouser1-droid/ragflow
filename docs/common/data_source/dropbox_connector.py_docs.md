# Documentation: common/data_source/dropbox_connector.py

## File Metadata

- **Path**: `common/data_source/dropbox_connector.py`
- **Size**: 3271 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `common/data_source/dropbox_connector.py`.

## Python Module Overview

### Module Docstring

```
Dropbox connector
```

### Imports and Dependencies

This module imports the following dependencies:

- `typing`
- `dropbox`
- `dropbox.exceptions`
- `common.data_source.config`
- `common.data_source.exceptions`
- `common.data_source.interfaces`

### Classes Defined

This file defines 1 class(es):

#### Class: `DropboxConnector` (line 13)

**Docstring**: Dropbox connector for accessing Dropbox files and folders...

**Methods**: __init__, load_credentials, validate_connector_settings, _download_file, _get_shared_link, poll_source, load_from_state

### Functions Defined

This file defines 7 function(s):

#### Function: `__init__` (line 16)

**Parameters**: self, batch_size

#### Function: `load_credentials` (line 20)

**Parameters**: self, credentials

**Docstring**: Load Dropbox credentials...

#### Function: `validate_connector_settings` (line 32)

**Parameters**: self

**Docstring**: Validate Dropbox connector settings...

#### Function: `_download_file` (line 46)

**Parameters**: self, path

**Docstring**: Download a single file from Dropbox....

#### Function: `_get_shared_link` (line 53)

**Parameters**: self, path

**Docstring**: Create a shared link for a file in Dropbox....

#### Function: `poll_source` (line 71)

**Parameters**: self, start, end

**Docstring**: Poll Dropbox for recent file changes...

#### Function: `load_from_state` (line 76)

**Parameters**: self

**Docstring**: Load files from Dropbox state...

## Original Source Code

```py
"""Dropbox connector"""

from typing import Any

from dropbox import Dropbox
from dropbox.exceptions import ApiError, AuthError

from common.data_source.config import INDEX_BATCH_SIZE
from common.data_source.exceptions import ConnectorValidationError, InsufficientPermissionsError, ConnectorMissingCredentialError
from common.data_source.interfaces import LoadConnector, PollConnector, SecondsSinceUnixEpoch


class DropboxConnector(LoadConnector, PollConnector):
    """Dropbox connector for accessing Dropbox files and folders"""

    def __init__(self, batch_size: int = INDEX_BATCH_SIZE) -> None:
        self.batch_size = batch_size
        self.dropbox_client: Dropbox | None = None

    def load_credentials(self, credentials: dict[str, Any]) -> dict[str, Any] | None:
        """Load Dropbox credentials"""
        try:
            access_token = credentials.get("dropbox_access_token")
            if not access_token:
                raise ConnectorMissingCredentialError("Dropbox access token is required")
            
            self.dropbox_client = Dropbox(access_token)
            return None
        except Exception as e:
            raise ConnectorMissingCredentialError(f"Dropbox: {e}")

    def validate_connector_settings(self) -> None:
        """Validate Dropbox connector settings"""
        if not self.dropbox_client:
            raise ConnectorMissingCredentialError("Dropbox")
        
        try:
            # Test connection by getting current account info
            self.dropbox_client.users_get_current_account()
        except (AuthError, ApiError) as e:
            if "invalid_access_token" in str(e).lower():
                raise InsufficientPermissionsError("Invalid Dropbox access token")
            else:
                raise ConnectorValidationError(f"Dropbox validation error: {e}")

    def _download_file(self, path: str) -> bytes:
        """Download a single file from Dropbox."""
        if self.dropbox_client is None:
            raise ConnectorMissingCredentialError("Dropbox")
        _, resp = self.dropbox_client.files_download(path)
        return resp.content

    def _get_shared_link(self, path: str) -> str:
        """Create a shared link for a file in Dropbox."""
        if self.dropbox_client is None:
            raise ConnectorMissingCredentialError("Dropbox")
        
        try:
            # Try to get existing shared links first
            shared_links = self.dropbox_client.sharing_list_shared_links(path=path)
            if shared_links.links:
                return shared_links.links[0].url
            
            # Create a new shared link
            link_settings = self.dropbox_client.sharing_create_shared_link_with_settings(path)
            return link_settings.url
        except Exception:
            # Fallback to basic link format
            return f"https://www.dropbox.com/home{path}"

    def poll_source(self, start: SecondsSinceUnixEpoch, end: SecondsSinceUnixEpoch) -> Any:
        """Poll Dropbox for recent file changes"""
        # Simplified implementation - in production this would handle actual polling
        return []

    def load_from_state(self) -> Any:
        """Load files from Dropbox state"""
        # Simplified implementation
        return []
```

## Detailed Analysis

### File Role in Repository

The file `common/data_source/dropbox_connector.py` is located in the `common/data_source` directory.

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
- [exceptions.py](exceptions.py_docs.md)
- [file_types.py](file_types.py_docs.md)
- [gmail_connector.py](gmail_connector.py_docs.md)
- [html_utils.py](html_utils.py_docs.md)
- [interfaces.py](interfaces.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
