# File Documentation: api/apps/auth/oidc.py

## File Metadata

- **Path**: `api/apps/auth/oidc.py`
- **Extension**: `.py`
- **Lines**: 101
- **Characters**: 3,498
- **Size**: 3,498 bytes
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

import jwt
import requests
from .oauth import OAuthClient


class OIDCClient(OAuthClient):
    def __init__(self, config):
        """
        Initialize the OIDCClient with the provider's configuration.
        Use `issuer` as the single source of truth for configuration discovery.
        """
        self.issuer = config.get("issuer")
        if not self.issuer:
            raise ValueError("Missing issuer in configuration.")

        oidc_metadata = self._load_oidc_metadata(self.issuer)
        config.update({
            'issuer': oidc_metadata['issuer'],
            'jwks_uri': oidc_metadata['jwks_uri'], 
            'authorization_url': oidc_metadata['authorization_endpoint'],
            'token_url': oidc_metadata['token_endpoint'],
            'userinfo_url': oidc_metadata['userinfo_endpoint']
        })

        super().__init__(config)
        self.issuer = config['issuer']
        self.jwks_uri = config['jwks_uri']


    @staticmethod
    def _load_oidc_metadata(issuer):
        """
        Load OIDC metadata from `/.well-known/openid-configuration`.
        """
        try:
            metadata_url = f"{issuer}/.well-known/openid-configuration"
            response = requests.get(metadata_url, timeout=7)
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            raise ValueError(f"Failed to fetch OIDC metadata: {e}")


    def parse_id_token(self, id_token):
        """
        Parse and validate OIDC ID Token (JWT format) with signature verification.
        """
        try:
            # Decode JWT header without verifying signature
            headers = jwt.get_unverified_header(id_token)
            
            # OIDC usually uses `RS256` for signing
            alg = headers.get("alg", "RS256")

            # Use PyJWT's PyJWKClient to fetch JWKS and find signing key
            jwks_cli = jwt.PyJWKClient(self.jwks_uri)
            signing_key = jwks_cli.get_signing_key_from_jwt(id_token).key

            # Decode and verify signature
            decoded_token = jwt.decode(
                id_token,
                key=signing_key,
                algorithms=[alg],  
                audience=str(self.client_id),
                issuer=self.issuer,
            )
            return decoded_token
        except Exception as e:
            raise ValueError(f"Error parsing ID Token: {e}")


    def fetch_user_info(self, access_token, id_token=None, **kwargs):
        """
        Fetch user info.
        """
        user_info = {}
        if id_token:
            user_info = self.parse_id_token(id_token)
        user_info.update(super().fetch_user_info(access_token).to_dict())
        return self.normalize_user_info(user_info)


    def normalize_user_info(self, user_info):
        return super().normalize_user_info(user_info)

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

- `OIDCClient`: Class definition

### Imports (3)

- `import jwt`
- `import requests`
- `from .oauth import OAuthClient`

## Code Structure Analysis

- Total lines: 101
- Blank lines: 17 (16.8%)
- Comment lines: ~27 (26.7%)
- Code lines: ~57


## Dependencies and Imports

- `import jwt`
- `import requests`
- `from .oauth import OAuthClient`

## Design & Architecture

This file is located in the `api` directory, specifically within `api/apps/auth`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 3 loop(s) - consider algorithmic complexity

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
- Potential test file: `test_oidc.py`

## Keywords

ANY, All, Apache, Authors, BASIS, CONDITIONS, Copyright, Decode, Error, Exception, Failed, Fetch, InfiniFlow, Initialize, JWKS, JWT, KIND, LICENSE, License, Licensed, Load, Missing, None, OAuthClient, OIDC, OIDCClient, Parse, PyJWKClient, PyJWT, Python, RS256, RequestException, Reserved, Rights, See, The, Token, Unless, Use, ValueError, Version, WARRANTIES, WITHOUT, You, __init__, _load_oidc_metadata, fetch_user_info, normalize_user_info, parse_id_token, staticmethod

---
*Generated by RAGFlow Repository Documentation Generator*
