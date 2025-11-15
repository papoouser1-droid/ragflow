# File Documentation: sdk/python/test/conftest.py

## File Metadata

- **Path**: `sdk/python/test/conftest.py`
- **Extension**: `.py`
- **Lines**: 153
- **Characters**: 4,759
- **Size**: 4,759 bytes
- **Purpose**: Testing - Contains unit tests, integration tests, or test utilities

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


### Functions (10)

- `generate_email()`: Function definition
- `register()`: Function definition
- `login()`: Function definition
- `get_api_key_fixture()`: Function definition
- `get_auth()`: Function definition
- `get_email()`: Function definition
- `get_my_llms()`: Function definition
- `add_models()`: Function definition
- `get_tenant_info()`: Function definition
- `set_tenant_info()`: Function definition

### Imports (3)

- `import os`
- `import pytest`
- `import requests`

## Code Structure Analysis

- Total lines: 153
- Blank lines: 28 (18.3%)
- Comment lines: ~18 (11.8%)
- Code lines: ~107


## Dependencies and Imports

- `import os`
- `import pytest`
- `import requests`

## Design & Architecture

This file is located in the `sdk` directory, specifically within `sdk/python/test`.

This is a test file, contributing to the quality assurance and validation of the codebase.

## Performance & Complexity

- Contains 2 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `sdk/python/test/` directory

## Keywords

1, ANY, All, Apache, Authorization, Authors, BAAI, BASIS, Builtin, CONDITIONS, Copyright, Critical, EMAIL, Environment, Error, Exception, False, HOST_ADDRESS, InfiniFlow, KIND, LICENSE, License, Licensed, None, PASSWORD, Python, PzQpjoe, QHa7KI7mzTKgUTZImK6BZtfRBATDTthEUbbaTewY4H0MnWiCeeDhcbeQao6cFy1To8pE3RpmxnGnS8BsBn8w, Reserved, Rights, See, The, True, Unless, Version, WARRANTIES, WITHOUT, X8f7fp9c7vUsfOCkM, YbSqpTq8qqrmHzn9vO, You, ZHIPU, ZHIPU_AI_API_KEY, add_models, generate_email, generate_random_email, get_api_key_fixture, get_auth, get_email, get_my_llms, get_tenant_info...

---
*Generated by RAGFlow Repository Documentation Generator*
