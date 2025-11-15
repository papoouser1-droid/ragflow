# Documentation: api/apps/auth/oidc.py

## File Metadata

- **Path**: `api/apps/auth/oidc.py`
- **Size**: 3498 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `api/apps/auth/oidc.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `jwt`
- `requests`
- `oauth`

### Classes Defined

This file defines 1 class(es):

#### Class: `OIDCClient` (line 22)

**Methods**: __init__, _load_oidc_metadata, parse_id_token, fetch_user_info, normalize_user_info

### Functions Defined

This file defines 5 function(s):

#### Function: `__init__` (line 23)

**Parameters**: self, config

**Docstring**: Initialize the OIDCClient with the provider's configuration.
Use `issuer` as the single source of truth for configuration discovery....

#### Function: `_load_oidc_metadata` (line 47)

**Parameters**: issuer

**Docstring**: Load OIDC metadata from `/.well-known/openid-configuration`....

#### Function: `parse_id_token` (line 60)

**Parameters**: self, id_token

**Docstring**: Parse and validate OIDC ID Token (JWT format) with signature verification....

#### Function: `fetch_user_info` (line 88)

**Parameters**: self, access_token, id_token

**Docstring**: Fetch user info....

#### Function: `normalize_user_info` (line 99)

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

## Detailed Analysis

### File Role in Repository

The file `api/apps/auth/oidc.py` is located in the `api/apps/auth` directory.

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
- [oauth.py](oauth.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
