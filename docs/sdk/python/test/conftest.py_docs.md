# Documentation: sdk/python/test/conftest.py

## File Metadata

- **Path**: `sdk/python/test/conftest.py`
- **Size**: 4759 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/test/conftest.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `os`
- `pytest`
- `requests`

### Functions Defined

This file defines 10 function(s):

#### Function: `generate_email` (line 31)

**Parameters**: None

#### Function: `register` (line 42)

**Parameters**: None

#### Function: `login` (line 52)

**Parameters**: None

#### Function: `get_api_key_fixture` (line 64)

**Parameters**: None

#### Function: `get_auth` (line 80)

**Parameters**: None

#### Function: `get_email` (line 90)

**Parameters**: None

#### Function: `get_my_llms` (line 94)

**Parameters**: auth, name

#### Function: `add_models` (line 106)

**Parameters**: auth

#### Function: `get_tenant_info` (line 121)

**Parameters**: auth

#### Function: `set_tenant_info` (line 132)

**Parameters**: get_auth

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

import os

import pytest
import requests

HOST_ADDRESS = os.getenv("HOST_ADDRESS", "http://127.0.0.1:9380")
ZHIPU_AI_API_KEY = os.getenv("ZHIPU_AI_API_KEY")
if ZHIPU_AI_API_KEY is None:
    pytest.exit("Error: Environment variable ZHIPU_AI_API_KEY must be set")

# def generate_random_email():
#     return 'user_' + ''.join(random.choices(string.ascii_lowercase + string.digits, k=8))+'@1.com'


def generate_email():
    return "user_123@1.com"


EMAIL = generate_email()
# password is "123"
PASSWORD = """ctAseGvejiaSWWZ88T/m4FQVOpQyUvP+x7sXtdv3feqZACiQleuewkUi35E16wSd5C5QcnkkcV9cYc8TKPTRZlxappDuirxghxoOvFcJxFU4ixLsD
fN33jCHRoDUW81IH9zjij/vaw8IbVyb6vuwg6MX6inOEBRRzVbRYxXOu1wkWY6SsI8X70oF9aeLFp/PzQpjoe/YbSqpTq8qqrmHzn9vO+yvyYyvmDsphXe
X8f7fp9c7vUsfOCkM+gHY3PadG+QHa7KI7mzTKgUTZImK6BZtfRBATDTthEUbbaTewY4H0MnWiCeeDhcbeQao6cFy1To8pE3RpmxnGnS8BsBn8w=="""


def register():
    url = HOST_ADDRESS + "/v1/user/register"
    name = "user"
    register_data = {"email": EMAIL, "nickname": name, "password": PASSWORD}
    res = requests.post(url=url, json=register_data)
    res = res.json()
    if res.get("code") != 0:
        raise Exception(res.get("message"))


def login():
    url = HOST_ADDRESS + "/v1/user/login"
    login_data = {"email": EMAIL, "password": PASSWORD}
    response = requests.post(url=url, json=login_data)
    res = response.json()
    if res.get("code") != 0:
        raise Exception(res.get("message"))
    auth = response.headers["Authorization"]
    return auth


@pytest.fixture(scope="session")
def get_api_key_fixture():
    try:
        register()
    except Exception as e:
        print(e)
    auth = login()
    url = HOST_ADDRESS + "/v1/system/new_token"
    auth = {"Authorization": auth}
    response = requests.post(url=url, headers=auth)
    res = response.json()
    if res.get("code") != 0:
        raise Exception(res.get("message"))
    return res["data"].get("token")


@pytest.fixture(scope="session")
def get_auth():
    try:
        register()
    except Exception as e:
        print(e)
    auth = login()
    return auth


@pytest.fixture(scope="session")
def get_email():
    return EMAIL


def get_my_llms(auth, name):
    url = HOST_ADDRESS + "/v1/llm/my_llms"
    authorization = {"Authorization": auth}
    response = requests.get(url=url, headers=authorization)
    res = response.json()
    if res.get("code") != 0:
        raise Exception(res.get("message"))
    if name in res.get("data"):
        return True
    return False


def add_models(auth):
    url = HOST_ADDRESS + "/v1/llm/set_api_key"
    authorization = {"Authorization": auth}
    models_info = {
        "ZHIPU-AI": {"llm_factory": "ZHIPU-AI", "api_key": ZHIPU_AI_API_KEY},
    }

    for name, model_info in models_info.items():
        if not get_my_llms(auth, name):
            response = requests.post(url=url, headers=authorization, json=model_info)
            res = response.json()
            if res.get("code") != 0:
                pytest.exit(f"Critical error in add_models: {res.get('message')}")


def get_tenant_info(auth):
    url = HOST_ADDRESS + "/v1/user/tenant_info"
    authorization = {"Authorization": auth}
    response = requests.get(url=url, headers=authorization)
    res = response.json()
    if res.get("code") != 0:
        raise Exception(res.get("message"))
    return res["data"].get("tenant_id")


@pytest.fixture(scope="session", autouse=True)
def set_tenant_info(get_auth):
    auth = get_auth
    try:
        add_models(auth)
        tenant_id = get_tenant_info(auth)
    except Exception as e:
        pytest.exit(f"Error in set_tenant_info: {str(e)}")
    url = HOST_ADDRESS + "/v1/user/set_tenant_info"
    authorization = {"Authorization": get_auth}
    tenant_info = {
        "tenant_id": tenant_id,
        "llm_id": "glm-4-flash@ZHIPU-AI",
        "embd_id": "BAAI/bge-small-en-v1.5@Builtin",
        "img2txt_id": "glm-4v@ZHIPU-AI",
        "asr_id": "",
        "tts_id": None,
    }
    response = requests.post(url=url, headers=authorization, json=tenant_info)
    res = response.json()
    if res.get("code") != 0:
        raise Exception(res.get("message"))

```

## Detailed Analysis

### File Role in Repository

The file `sdk/python/test/conftest.py` is located in the `sdk/python/test` directory.

This file is part of the **Testing** infrastructure.

### Architecture Context

Files in this location typically handle concerns related to test.

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



## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
