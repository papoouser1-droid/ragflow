# File Documentation: api/apps/auth/oauth.py

## File Metadata

- **Path**: `api/apps/auth/oauth.py`
- **Extension**: `.py`
- **Lines**: 111
- **Characters**: 3,900
- **Size**: 3,900 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

## Original Source

```python
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

## High-Level Overview

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

## Detailed Walkthrough

### Classes (2)

- `UserInfo`: Class definition
- `OAuthClient`: Class definition

### Imports (2)

- `import requests`
- `import urllib.parse`

## Code Structure Analysis

- Total lines: 111
- Blank lines: 16 (14.4%)
- Comment lines: ~23 (20.7%)
- Code lines: ~72


## Dependencies and Imports

- `import requests`
- `import urllib.parse`

## Design & Architecture

This file is located in the `api` directory, specifically within `api/apps/auth`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 5 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `api/apps/auth/` directory
- Potential test file: `test_oauth.py`

## Keywords

ANY, Accept, All, Apache, Authorization, Authors, BASIS, Bearer, CONDITIONS, Copyright, Exchange, Failed, Fetch, Generate, InfiniFlow, Initialize, KIND, LICENSE, License, Licensed, None, OAuthClient, Python, RequestException, Reserved, Rights, See, The, URL, Unless, UserInfo, ValueError, Version, WARRANTIES, WITHOUT, You, __init__, exchange_code_for_token, fetch_user_info, get_authorization_url, normalize_user_info, to_dict

---
*Generated by RAGFlow Repository Documentation Generator*
