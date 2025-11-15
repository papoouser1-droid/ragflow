# File Documentation: common/data_source/google_util/constant.py

## File Metadata

- **Path**: `common/data_source/google_util/constant.py`
- **Extension**: `.py`
- **Lines**: 104
- **Characters**: 2,828
- **Size**: 2,828 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

## Original Source

```python
from enum import Enum

from common.data_source.config import DocumentSource

SLIM_BATCH_SIZE = 500
# NOTE: do not need https://www.googleapis.com/auth/documents.readonly
# this is counted under `/auth/drive.readonly`
GOOGLE_SCOPES = {
    DocumentSource.GOOGLE_DRIVE: [
        "https://www.googleapis.com/auth/drive.readonly",
        "https://www.googleapis.com/auth/drive.metadata.readonly",
        "https://www.googleapis.com/auth/admin.directory.group.readonly",
        "https://www.googleapis.com/auth/admin.directory.user.readonly",
    ],
    DocumentSource.GMAIL: [
        "https://www.googleapis.com/auth/gmail.readonly",
        "https://www.googleapis.com/auth/admin.directory.user.readonly",
        "https://www.googleapis.com/auth/admin.directory.group.readonly",
    ],
}


# This is the Oauth token
DB_CREDENTIALS_DICT_TOKEN_KEY = "google_tokens"
# This is the service account key
DB_CREDENTIALS_DICT_SERVICE_ACCOUNT_KEY = "google_service_account_key"
# The email saved for both auth types
DB_CREDENTIALS_PRIMARY_ADMIN_KEY = "google_primary_admin"


# https://developers.google.com/workspace/guides/create-credentials
# Internally defined authentication method type.
# The value must be one of "oauth_interactive" or "uploaded"
# Used to disambiguate whether credentials have already been created via
# certain methods and what actions we allow users to take
DB_CREDENTIALS_AUTHENTICATION_METHOD = "authentication_method"


class GoogleOAuthAuthenticationMethod(str, Enum):
    OAUTH_INTERACTIVE = "oauth_interactive"
    UPLOADED = "uploaded"


USER_FIELDS = "nextPageToken, users(primaryEmail)"


# Error message substrings
MISSING_SCOPES_ERROR_STR = "client not authorized for any of the scopes requested"
SCOPE_INSTRUCTIONS = ""


GOOGLE_DRIVE_WEB_OAUTH_POPUP_TEMPLATE = """<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <title>Google Drive Authorization</title>
  <style>
    body {{
      font-family: Arial, sans-serif;
      background: #f8fafc;
      color: #0f172a;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
      margin: 0;
    }}
    .card {{
      background: white;
      padding: 32px;
      border-radius: 12px;
      box-shadow: 0 8px 30px rgba(15, 23, 42, 0.1);
      max-width: 420px;
      text-align: center;
    }}
    h1 {{
      font-size: 1.5rem;
      margin-bottom: 12px;
    }}
    p {{
      font-size: 0.95rem;
      line-height: 1.5;
    }}
  </style>
</head>
<body>
  <div class="card">
    <h1>{heading}</h1>
    <p>{message}</p>
    <p>You can close this window.</p>
  </div>
  <script>
    (function(){{
      if (window.opener) {{
        window.opener.postMessage({payload_json}, "*");
      }}
      {auto_close}
    }})();
  </script>
</body>
</html>
"""

```

## High-Level Overview

# NOTE: do not need https://www.googleapis.com/auth/documents.readonly
# this is counted under `/auth/drive.readonly`

## Detailed Walkthrough

### Classes (1)

- `GoogleOAuthAuthenticationMethod`: Class definition

### Imports (2)

- `from enum import Enum`
- `from common.data_source.config import DocumentSource`

## Code Structure Analysis

- Total lines: 104
- Blank lines: 15 (14.4%)
- Comment lines: ~12 (11.5%)
- Code lines: ~77


## Dependencies and Imports

- `from enum import Enum`
- `from common.data_source.config import DocumentSource`

## Design & Architecture

This file is located in the `common` directory, specifically within `common/data_source/google_util`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 2 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `common/data_source/google_util/` directory
- Imports from `enum`
- Imports from `common.data_source.config`
- Potential test file: `test_constant.py`

## Keywords

Arial, Authorization, DB_CREDENTIALS_AUTHENTICATION_METHOD, DB_CREDENTIALS_DICT_SERVICE_ACCOUNT_KEY, DB_CREDENTIALS_DICT_TOKEN_KEY, DB_CREDENTIALS_PRIMARY_ADMIN_KEY, DOCTYPE, DocumentSource, Drive, Enum, Error, GMAIL, GOOGLE_DRIVE, GOOGLE_DRIVE_WEB_OAUTH_POPUP_TEMPLATE, GOOGLE_SCOPES, Google, GoogleOAuthAuthenticationMethod, Internally, MISSING_SCOPES_ERROR_STR, NOTE, OAUTH_INTERACTIVE, Oauth, Python, SCOPE_INSTRUCTIONS, SLIM_BATCH_SIZE, The, This, UPLOADED, USER_FIELDS, Used, You, import

---
*Generated by RAGFlow Repository Documentation Generator*
