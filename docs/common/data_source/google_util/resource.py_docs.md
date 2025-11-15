# Documentation: common/data_source/google_util/resource.py

## File Metadata

- **Path**: `common/data_source/google_util/resource.py`
- **Size**: 3963 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `common/data_source/google_util/resource.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `collections.abc`
- `typing`
- `google.auth.exceptions`
- `google.oauth2.credentials`
- `google.oauth2.service_account`
- `googleapiclient.discovery`

### Classes Defined

This file defines 5 class(es):

#### Class: `GoogleDriveService` (line 14)

#### Class: `GoogleDocsService` (line 18)

#### Class: `AdminService` (line 22)

#### Class: `GmailService` (line 26)

#### Class: `RefreshableDriveObject` (line 30)

**Docstring**: Running Google drive service retrieval functions
involves accessing methods of the service object (ie. files().list())
which can raise a RefreshError if the access token is expired.
This class is a wr...

**Methods**: __init__, __getattr__, __call__, make_refreshable_execute

### Functions Defined

This file defines 10 function(s):

#### Function: `_get_google_service` (line 78)

**Parameters**: service_name, service_version, creds, user_email

#### Function: `get_google_docs_service` (line 95)

**Parameters**: creds, user_email

#### Function: `get_drive_service` (line 102)

**Parameters**: creds, user_email

#### Function: `get_admin_service` (line 109)

**Parameters**: creds, user_email

#### Function: `get_gmail_service` (line 116)

**Parameters**: creds, user_email

#### Function: `__init__` (line 39)

**Parameters**: self, call_stack, creds, creds_getter

#### Function: `__getattr__` (line 49)

**Parameters**: self, name

#### Function: `__call__` (line 58)

**Parameters**: self

#### Function: `make_refreshable_execute` (line 65)

**Parameters**: self

#### Function: `execute` (line 66)

**Parameters**: None

## Original Source Code

```py
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

## Detailed Analysis

### File Role in Repository

The file `common/data_source/google_util/resource.py` is located in the `common/data_source/google_util` directory.

### Architecture Context

Files in this location typically handle concerns related to google_util.

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
- [auth.py](auth.py_docs.md)
- [constant.py](constant.py_docs.md)
- [oauth_flow.py](oauth_flow.py_docs.md)
- [util.py](util.py_docs.md)
- [util_threadpool_concurrency.py](util_threadpool_concurrency.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
