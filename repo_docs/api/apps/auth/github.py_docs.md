# File Documentation: api/apps/auth/github.py

## File Metadata

- **Path**: `api/apps/auth/github.py`
- **Extension**: `.py`
- **Lines**: 64
- **Characters**: 2,494
- **Size**: 2,494 bytes
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
from .oauth import OAuthClient, UserInfo


class GithubOAuthClient(OAuthClient):
    def __init__(self, config):
        """
        Initialize the GithubOAuthClient with the provider's configuration.
        """
        config.update({
            "authorization_url": "https://github.com/login/oauth/authorize",
            "token_url": "https://github.com/login/oauth/access_token",
            "userinfo_url": "https://api.github.com/user",
            "scope": "user:email"
        })
        super().__init__(config)


    def fetch_user_info(self, access_token, **kwargs):
        """
        Fetch GitHub user info.
        """
        user_info = {}
        try:
            headers = {"Authorization": f"Bearer {access_token}"}
            # user info
            response = requests.get(self.userinfo_url, headers=headers, timeout=self.http_request_timeout)
            response.raise_for_status()
            user_info.update(response.json())
            # email info
            response = requests.get(self.userinfo_url+"/emails", headers=headers, timeout=self.http_request_timeout)
            response.raise_for_status()
            email_info = response.json()
            user_info["email"] = next(
                (email for email in email_info if email["primary"]), None
            )["email"]
            return self.normalize_user_info(user_info)
        except requests.exceptions.RequestException as e:
            raise ValueError(f"Failed to fetch github user info: {e}")


    def normalize_user_info(self, user_info):
        email = user_info.get("email")
        username = user_info.get("login", str(email).split("@")[0])
        nickname = user_info.get("name", username)
        avatar_url = user_info.get("avatar_url", "")
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

### Classes (1)

- `GithubOAuthClient`: Class definition

### Imports (2)

- `import requests`
- `from .oauth import OAuthClient, UserInfo`

## Code Structure Analysis

- Total lines: 64
- Blank lines: 8 (12.5%)
- Comment lines: ~21 (32.8%)
- Code lines: ~35


## Dependencies and Imports

- `import requests`
- `from .oauth import OAuthClient, UserInfo`

## Design & Architecture

This file is located in the `api` directory, specifically within `api/apps/auth`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 2 loop(s) - consider algorithmic complexity

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
- Imports from `.oauth`
- Potential test file: `test_github.py`

## Keywords

ANY, All, Apache, Authorization, Authors, BASIS, Bearer, CONDITIONS, Copyright, Failed, Fetch, GitHub, GithubOAuthClient, InfiniFlow, Initialize, KIND, LICENSE, License, Licensed, None, OAuthClient, Python, RequestException, Reserved, Rights, See, The, Unless, UserInfo, ValueError, Version, WARRANTIES, WITHOUT, You, __init__, fetch_user_info, normalize_user_info

---
*Generated by RAGFlow Repository Documentation Generator*
