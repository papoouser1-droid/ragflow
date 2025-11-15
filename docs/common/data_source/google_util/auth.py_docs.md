# Documentation: common/data_source/google_util/auth.py

## File Metadata

- **Path**: `common/data_source/google_util/auth.py`
- **Size**: 6994 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `common/data_source/google_util/auth.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `json`
- `logging`
- `typing`
- `google.auth.transport.requests`
- `google.oauth2.credentials`
- `google.oauth2.service_account`
- `common.data_source.config`
- `common.data_source.google_util.constant`
- `common.data_source.google_util.oauth_flow`

### Functions Defined

This file defines 3 function(s):

#### Function: `sanitize_oauth_credentials` (line 21)

**Parameters**: oauth_creds

**Docstring**: we really don't want to be persisting the client id and secret anywhere but the
environment.

Returns a string of serialized json....

#### Function: `get_google_creds` (line 37)

**Parameters**: credentials, source

**Docstring**: Checks for two different types of credentials.
(1) A credential which holds a token acquired via a user going through
the Google OAuth flow.
(2) A credential which holds a service account key JSON fil...

#### Function: `get_google_oauth_creds` (line 130)

**Parameters**: token_json_str, source

**Docstring**: creds_json only needs to contain client_id, client_secret and refresh_token to
refresh the creds.

expiry and token are optional ... however, if passing in expiry, token
should also be passed in or el...

## Original Source Code

```py
import json
import logging
from typing import Any

from google.auth.transport.requests import Request  # type: ignore
from google.oauth2.credentials import Credentials as OAuthCredentials  # type: ignore  # type: ignore
from google.oauth2.service_account import Credentials as ServiceAccountCredentials  # type: ignore  # type: ignore

from common.data_source.config import OAUTH_GOOGLE_DRIVE_CLIENT_ID, OAUTH_GOOGLE_DRIVE_CLIENT_SECRET, DocumentSource
from common.data_source.google_util.constant import (
    DB_CREDENTIALS_AUTHENTICATION_METHOD,
    DB_CREDENTIALS_DICT_SERVICE_ACCOUNT_KEY,
    DB_CREDENTIALS_DICT_TOKEN_KEY,
    DB_CREDENTIALS_PRIMARY_ADMIN_KEY,
    GOOGLE_SCOPES,
    GoogleOAuthAuthenticationMethod,
)
from common.data_source.google_util.oauth_flow import ensure_oauth_token_dict


def sanitize_oauth_credentials(oauth_creds: OAuthCredentials) -> str:
    """we really don't want to be persisting the client id and secret anywhere but the
    environment.

    Returns a string of serialized json.
    """

    # strip the client id and secret
    oauth_creds_json_str = oauth_creds.to_json()
    oauth_creds_sanitized_json: dict[str, Any] = json.loads(oauth_creds_json_str)
    oauth_creds_sanitized_json.pop("client_id", None)
    oauth_creds_sanitized_json.pop("client_secret", None)
    oauth_creds_sanitized_json_str = json.dumps(oauth_creds_sanitized_json)
    return oauth_creds_sanitized_json_str


def get_google_creds(
    credentials: dict[str, str],
    source: DocumentSource,
) -> tuple[ServiceAccountCredentials | OAuthCredentials, dict[str, str] | None]:
    """Checks for two different types of credentials.
    (1) A credential which holds a token acquired via a user going through
    the Google OAuth flow.
    (2) A credential which holds a service account key JSON file, which
    can then be used to impersonate any user in the workspace.

    Return a tuple where:
        The first element is the requested credentials
        The second element is a new credentials dict that the caller should write back
        to the db. This happens if token rotation occurs while loading credentials.
    """
    oauth_creds = None
    service_creds = None
    new_creds_dict = None
    if DB_CREDENTIALS_DICT_TOKEN_KEY in credentials:
        # OAUTH
        authentication_method: str = credentials.get(
            DB_CREDENTIALS_AUTHENTICATION_METHOD,
            GoogleOAuthAuthenticationMethod.UPLOADED,
        )

        credentials_dict_str = credentials[DB_CREDENTIALS_DICT_TOKEN_KEY]
        credentials_dict = json.loads(credentials_dict_str)

        regenerated_from_client_secret = False
        if "client_id" not in credentials_dict or "client_secret" not in credentials_dict or "refresh_token" not in credentials_dict:
            try:
                credentials_dict = ensure_oauth_token_dict(credentials_dict, source)
            except Exception as exc:
                raise PermissionError(
                    "Google Drive OAuth credentials are incomplete. Please finish the OAuth flow to generate access tokens."
                ) from exc
            credentials_dict_str = json.dumps(credentials_dict)
            regenerated_from_client_secret = True

        # only send what get_google_oauth_creds needs
        authorized_user_info = {}

        # oauth_interactive is sanitized and needs credentials from the environment
        if authentication_method == GoogleOAuthAuthenticationMethod.OAUTH_INTERACTIVE:
            authorized_user_info["client_id"] = OAUTH_GOOGLE_DRIVE_CLIENT_ID
            authorized_user_info["client_secret"] = OAUTH_GOOGLE_DRIVE_CLIENT_SECRET
        else:
            authorized_user_info["client_id"] = credentials_dict["client_id"]
            authorized_user_info["client_secret"] = credentials_dict["client_secret"]

        authorized_user_info["refresh_token"] = credentials_dict["refresh_token"]

        authorized_user_info["token"] = credentials_dict["token"]
        authorized_user_info["expiry"] = credentials_dict["expiry"]

        token_json_str = json.dumps(authorized_user_info)
        oauth_creds = get_google_oauth_creds(token_json_str=token_json_str, source=source)

        # tell caller to update token stored in DB if the refresh token changed
        if oauth_creds:
            should_persist = regenerated_from_client_secret or oauth_creds.refresh_token != authorized_user_info["refresh_token"]
            if should_persist:
                # if oauth_interactive, sanitize the credentials so they don't get stored in the db
                if authentication_method == GoogleOAuthAuthenticationMethod.OAUTH_INTERACTIVE:
                    oauth_creds_json_str = sanitize_oauth_credentials(oauth_creds)
                else:
                    oauth_creds_json_str = oauth_creds.to_json()

                new_creds_dict = {
                    DB_CREDENTIALS_DICT_TOKEN_KEY: oauth_creds_json_str,
                    DB_CREDENTIALS_PRIMARY_ADMIN_KEY: credentials[DB_CREDENTIALS_PRIMARY_ADMIN_KEY],
                    DB_CREDENTIALS_AUTHENTICATION_METHOD: authentication_method,
                }
    elif DB_CREDENTIALS_DICT_SERVICE_ACCOUNT_KEY in credentials:
        # SERVICE ACCOUNT
        service_account_key_json_str = credentials[DB_CREDENTIALS_DICT_SERVICE_ACCOUNT_KEY]
        service_account_key = json.loads(service_account_key_json_str)

        service_creds = ServiceAccountCredentials.from_service_account_info(service_account_key, scopes=GOOGLE_SCOPES[source])

        if not service_creds.valid or not service_creds.expired:
            service_creds.refresh(Request())

        if not service_creds.valid:
            raise PermissionError(f"Unable to access {source} - service account credentials are invalid.")

    creds: ServiceAccountCredentials | OAuthCredentials | None = oauth_creds or service_creds
    if creds is None:
        raise PermissionError(f"Unable to access {source} - unknown credential structure.")

    return creds, new_creds_dict


def get_google_oauth_creds(token_json_str: str, source: DocumentSource) -> OAuthCredentials | None:
    """creds_json only needs to contain client_id, client_secret and refresh_token to
    refresh the creds.

    expiry and token are optional ... however, if passing in expiry, token
    should also be passed in or else we may not return any creds.
    (probably a sign we should refactor the function)
    """

    creds_json = json.loads(token_json_str)
    creds = OAuthCredentials.from_authorized_user_info(
        info=creds_json,
        scopes=GOOGLE_SCOPES[source],
    )
    if creds.valid:
        return creds

    if creds.expired and creds.refresh_token:
        try:
            creds.refresh(Request())
            if creds.valid:
                logging.info("Refreshed Google Drive tokens.")
                return creds
        except Exception:
            logging.exception("Failed to refresh google drive access token")
            return None

    return None

```

## Detailed Analysis

### File Role in Repository

The file `common/data_source/google_util/auth.py` is located in the `common/data_source/google_util` directory.

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
- [constant.py](constant.py_docs.md)
- [oauth_flow.py](oauth_flow.py_docs.md)
- [resource.py](resource.py_docs.md)
- [util.py](util.py_docs.md)
- [util_threadpool_concurrency.py](util_threadpool_concurrency.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
