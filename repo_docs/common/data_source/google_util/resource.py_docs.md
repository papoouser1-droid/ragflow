# File Documentation: common/data_source/google_util/resource.py

## File Metadata

- **Path**: `common/data_source/google_util/resource.py`
- **Extension**: `.py`
- **Lines**: 121
- **Characters**: 3,963
- **Size**: 3,963 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

## Original Source

```python
import logging
from collections.abc import Callable
from typing import Any

from google.auth.exceptions import RefreshError  # type: ignore
from google.oauth2.credentials import Credentials as OAuthCredentials  # type: ignore  # type: ignore
from google.oauth2.service_account import Credentials as ServiceAccountCredentials  # type: ignore  # type: ignore
from googleapiclient.discovery import (
    Resource,  # type: ignore
    build,  # type: ignore
)


class GoogleDriveService(Resource):
    pass


class GoogleDocsService(Resource):
    pass


class AdminService(Resource):
    pass


class GmailService(Resource):
    pass


class RefreshableDriveObject:
    """
    Running Google drive service retrieval functions
    involves accessing methods of the service object (ie. files().list())
    which can raise a RefreshError if the access token is expired.
    This class is a wrapper that propagates the ability to refresh the access token
    and retry the final retrieval function until execute() is called.
    """

    def __init__(
        self,
        call_stack: Callable[[ServiceAccountCredentials | OAuthCredentials], Any],
        creds: ServiceAccountCredentials | OAuthCredentials,
        creds_getter: Callable[..., ServiceAccountCredentials | OAuthCredentials],
    ):
        self.call_stack = call_stack
        self.creds = creds
        self.creds_getter = creds_getter

    def __getattr__(self, name: str) -> Any:
        if name == "execute":
            return self.make_refreshable_execute()
        return RefreshableDriveObject(
            lambda creds: getattr(self.call_stack(creds), name),
            self.creds,
            self.creds_getter,
        )

    def __call__(self, *args: Any, **kwargs: Any) -> Any:
        return RefreshableDriveObject(
            lambda creds: self.call_stack(creds)(*args, **kwargs),
            self.creds,
            self.creds_getter,
        )

    def make_refreshable_execute(self) -> Callable:
        def execute(*args: Any, **kwargs: Any) -> Any:
            try:
                return self.call_stack(self.creds).execute(*args, **kwargs)
            except RefreshError as e:
                logging.warning(f"RefreshError, going to attempt a creds refresh and retry: {e}")
                # Refresh the access token
                self.creds = self.creds_getter()
                return self.call_stack(self.creds).execute(*args, **kwargs)

        return execute


def _get_google_service(
    service_name: str,
    service_version: str,
    creds: ServiceAccountCredentials | OAuthCredentials,
    user_email: str | None = None,
) -> GoogleDriveService | GoogleDocsService | AdminService | GmailService:
    service: Resource
    if isinstance(creds, ServiceAccountCredentials):
        # NOTE: https://developers.google.com/identity/protocols/oauth2/service-account#error-codes
        creds = creds.with_subject(user_email)
        service = build(service_name, service_version, credentials=creds)
    elif isinstance(creds, OAuthCredentials):
        service = build(service_name, service_version, credentials=creds)

    return service


def get_google_docs_service(
    creds: ServiceAccountCredentials | OAuthCredentials,
    user_email: str | None = None,
) -> GoogleDocsService:
    return _get_google_service("docs", "v1", creds, user_email)


def get_drive_service(
    creds: ServiceAccountCredentials | OAuthCredentials,
    user_email: str | None = None,
) -> GoogleDriveService:
    return _get_google_service("drive", "v3", creds, user_email)


def get_admin_service(
    creds: ServiceAccountCredentials | OAuthCredentials,
    user_email: str | None = None,
) -> AdminService:
    return _get_google_service("admin", "directory_v1", creds, user_email)


def get_gmail_service(
    creds: ServiceAccountCredentials | OAuthCredentials,
    user_email: str | None = None,
) -> GmailService:
    return _get_google_service("gmail", "v1", creds, user_email)

```

## High-Level Overview

    """
    """

## Detailed Walkthrough

### Classes (5)

- `GoogleDriveService`: Class definition
- `GoogleDocsService`: Class definition
- `AdminService`: Class definition
- `GmailService`: Class definition
- `RefreshableDriveObject`: Class definition

### Functions (5)

- `_get_google_service()`: Function definition
- `get_google_docs_service()`: Function definition
- `get_drive_service()`: Function definition
- `get_admin_service()`: Function definition
- `get_gmail_service()`: Function definition

### Imports (7)

- `import logging`
- `from collections.abc import Callable`
- `from typing import Any`
- `from google.auth.exceptions import RefreshError  # type: ignore`
- `from google.oauth2.credentials import Credentials as OAuthCredentials  # type: ignore  # type: ignore`
- `from google.oauth2.service_account import Credentials as ServiceAccountCredentials  # type: ignore  # type: ignore`
- `from googleapiclient.discovery import (`

## Code Structure Analysis

- Total lines: 121
- Blank lines: 28 (23.1%)
- Comment lines: ~4 (3.3%)
- Code lines: ~89


## Dependencies and Imports

- `import logging`
- `from collections.abc import Callable`
- `from typing import Any`
- `from google.auth.exceptions import RefreshError  # type: ignore`
- `from google.oauth2.credentials import Credentials as OAuthCredentials  # type: ignore  # type: ignore`
- `from google.oauth2.service_account import Credentials as ServiceAccountCredentials  # type: ignore  # type: ignore`
- `from googleapiclient.discovery import (`

## Design & Architecture

This file is located in the `common` directory, specifically within `common/data_source/google_util`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `common/data_source/google_util/` directory
- Imports from `collections.abc`
- Imports from `google.auth.exceptions`
- Imports from `google.oauth2.credentials`
- Imports from `google.oauth2.service_account`
- Potential test file: `test_resource.py`

## Keywords

AdminService, Any, Callable, Credentials, GmailService, Google, GoogleDocsService, GoogleDriveService, NOTE, None, OAuthCredentials, Python, Refresh, RefreshError, RefreshableDriveObject, Resource, Running, ServiceAccountCredentials, This, __call__, __getattr__, __init__, _get_google_service, execute, get_admin_service, get_drive_service, get_gmail_service, get_google_docs_service, is, make_refreshable_execute, until

---
*Generated by RAGFlow Repository Documentation Generator*
