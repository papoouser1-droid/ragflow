# File Documentation: common/data_source/jira_connector.py

## File Metadata

- **Path**: `common/data_source/jira_connector.py`
- **Extension**: `.py`
- **Lines**: 112
- **Characters**: 3,971
- **Size**: 3,971 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

## Original Source

```python
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

## High-Level Overview

"""Jira connector"""

## Detailed Walkthrough

### Classes (1)

- `JiraConnector`: Class definition

### Imports (6)

- `from typing import Any`
- `from jira import JIRA`
- `from common.data_source.config import INDEX_BATCH_SIZE`
- `from common.data_source.exceptions import (`
- `from common.data_source.interfaces import (`
- `from common.data_source.models import (`

## Code Structure Analysis

- Total lines: 112
- Blank lines: 18 (16.1%)
- Comment lines: ~18 (16.1%)
- Code lines: ~76


## Dependencies and Imports

- `from typing import Any`
- `from jira import JIRA`
- `from common.data_source.config import INDEX_BATCH_SIZE`
- `from common.data_source.exceptions import (`
- `from common.data_source.interfaces import (`
- `from common.data_source.models import (`

## Design & Architecture

This file is located in the `common` directory, specifically within `common/data_source`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 2 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `common/data_source/` directory
- Imports from `jira`
- Imports from `common.data_source.config`
- Imports from `common.data_source.exceptions`
- Imports from `common.data_source.interfaces`
- Potential test file: `test_jira_connector.py`

## Keywords

API, Any, Basic, Build, CheckpointedConnectorWithPermSync, ConnectorCheckpoint, ConnectorMissingCredentialError, ConnectorValidationError, Exception, INDEX_BATCH_SIZE, InsufficientPermissionsError, Invalid, JIRA, JSON, Jira, JiraConnector, Load, None, Poll, Python, Retrieve, SecondsSinceUnixEpoch, Simplified, SlimConnectorWithPermSync, Test, URL, UnexpectedValidationError, Validate, __init__, build_dummy_checkpoint, load_credentials, load_from_checkpoint, load_from_checkpoint_with_perm_sync, poll_source, retrieve_all_slim_docs_perm_sync, validate_checkpoint_json, validate_connector_settings

---
*Generated by RAGFlow Repository Documentation Generator*
