# Documentation: api/apps/auth/oauth.py

## File Metadata

- **Path**: `api/apps/auth/oauth.py`
- **Size**: 3900 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `api/apps/auth/oauth.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `requests`
- `urllib.parse`

### Classes Defined

This file defines 2 class(es):

#### Class: `UserInfo` (line 21)

**Methods**: __init__, to_dict

#### Class: `OAuthClient` (line 32)

**Methods**: __init__, get_authorization_url, exchange_code_for_token, fetch_user_info, normalize_user_info

### Functions Defined

This file defines 7 function(s):

#### Function: `__init__` (line 22)

**Parameters**: self, email, username, nickname, avatar_url

#### Function: `to_dict` (line 28)

**Parameters**: self

#### Function: `__init__` (line 33)

**Parameters**: self, config

**Docstring**: Initialize the OAuthClient with the provider's configuration....

#### Function: `get_authorization_url` (line 48)

**Parameters**: self, state

**Docstring**: Generate the authorization URL for user login....

#### Function: `exchange_code_for_token` (line 65)

**Parameters**: self, code

**Docstring**: Exchange authorization code for access token....

#### Function: `fetch_user_info` (line 89)

**Parameters**: self, access_token

**Docstring**: Fetch user information using access token....

#### Function: `normalize_user_info` (line 103)

**Parameters**: self, user_info

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

import requests
import urllib.parse


class UserInfo:
    def __init__(self, email, username, nickname, avatar_url):
        self.email = email
        self.username = username
        self.nickname = nickname
        self.avatar_url = avatar_url
    
    def to_dict(self):
        return {key: value for key, value in self.__dict__.items()}


class OAuthClient:
    def __init__(self, config):
        """
        Initialize the OAuthClient with the provider's configuration.
        """
        self.client_id = config["client_id"]
        self.client_secret = config["client_secret"]
        self.authorization_url = config["authorization_url"]
        self.token_url = config["token_url"]
        self.userinfo_url = config["userinfo_url"]
        self.redirect_uri = config["redirect_uri"]
        self.scope = config.get("scope", None)

        self.http_request_timeout = 7


    def get_authorization_url(self, state=None):
        """
        Generate the authorization URL for user login.
        """
        params = {
            "client_id": self.client_id,
            "redirect_uri": self.redirect_uri,
            "response_type": "code",
        }
        if self.scope:
            params["scope"] = self.scope
        if state:
            params["state"] = state
        authorization_url = f"{self.authorization_url}?{urllib.parse.urlencode(params)}"
        return authorization_url


    def exchange_code_for_token(self, code):
        """
        Exchange authorization code for access token.
        """
        try:
            payload = {
                "client_id": self.client_id,
                "client_secret": self.client_secret,
                "code": code,
                "redirect_uri": self.redirect_uri,
                "grant_type": "authorization_code"
            }
            response = requests.post(
                self.token_url,
                data=payload,
                headers={"Accept": "application/json"},
                timeout=self.http_request_timeout
            )
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            raise ValueError(f"Failed to exchange authorization code for token: {e}")


    def fetch_user_info(self, access_token, **kwargs):
        """
        Fetch user information using access token.
        """
        try:
            headers = {"Authorization": f"Bearer {access_token}"}
            response = requests.get(self.userinfo_url, headers=headers, timeout=self.http_request_timeout)
            response.raise_for_status()
            user_info = response.json()
            return self.normalize_user_info(user_info)
        except requests.exceptions.RequestException as e:
            raise ValueError(f"Failed to fetch user info: {e}")


    def normalize_user_info(self, user_info):
        email = user_info.get("email")
        username = user_info.get("username", str(email).split("@")[0])
        nickname = user_info.get("nickname", username)
        avatar_url = user_info.get("avatar_url", None)
        if avatar_url is None:
            avatar_url = user_info.get("picture", "")
        return UserInfo(email=email, username=username, nickname=nickname, avatar_url=avatar_url)

```

## Detailed Analysis

### File Role in Repository

The file `api/apps/auth/oauth.py` is located in the `api/apps/auth` directory.

This file is part of the **API/Backend** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to auth.

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

- [README.md](README.md_docs.md)
- [__init__.py](__init__.py_docs.md)
- [github.py](github.py_docs.md)
- [oidc.py](oidc.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
