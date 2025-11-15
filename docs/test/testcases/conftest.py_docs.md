# Documentation: test/testcases/conftest.py

## File Metadata

- **Path**: `test/testcases/conftest.py`
- **Size**: 4729 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/conftest.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `pytest`
- `requests`
- `configs`

### Functions Defined

This file defines 10 function(s):

#### Function: `pytest_addoption` (line 28)

**Parameters**: parser

#### Function: `pytest_configure` (line 46)

**Parameters**: config

#### Function: `register` (line 53)

**Parameters**: None

#### Function: `login` (line 63)

**Parameters**: None

#### Function: `auth` (line 75)

**Parameters**: None

#### Function: `token` (line 85)

**Parameters**: auth

#### Function: `get_my_llms` (line 95)

**Parameters**: auth, name

#### Function: `add_models` (line 107)

**Parameters**: auth

#### Function: `get_tenant_info` (line 122)

**Parameters**: auth

#### Function: `set_tenant_info` (line 133)

**Parameters**: auth

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

import pytest
import requests
from configs import EMAIL, HOST_ADDRESS, PASSWORD, VERSION, ZHIPU_AI_API_KEY

MARKER_EXPRESSIONS = {
    "p1": "p1",
    "p2": "p1 or p2",
    "p3": "p1 or p2 or p3",
}


def pytest_addoption(parser: pytest.Parser) -> None:
    parser.addoption(
        "--level",
        action="store",
        default="p2",
        choices=list(MARKER_EXPRESSIONS.keys()),
        help=f"Test level ({'/'.join(MARKER_EXPRESSIONS)}): p1=smoke, p2=core, p3=full",
    )

    parser.addoption(
        "--client-type",
        action="store",
        default="http",
        choices=["python_sdk", "http", "web"],
        help="Test client type: 'python_sdk', 'http', 'web'",
    )


def pytest_configure(config: pytest.Config) -> None:
    level = config.getoption("--level")
    config.option.markexpr = MARKER_EXPRESSIONS[level]
    if config.option.verbose > 0:
        print(f"\n[CONFIG] Active test level: {level}")


def register():
    url = HOST_ADDRESS + f"/{VERSION}/user/register"
    name = "qa"
    register_data = {"email": EMAIL, "nickname": name, "password": PASSWORD}
    res = requests.post(url=url, json=register_data)
    res = res.json()
    if res.get("code") != 0 and "has already registered" not in res.get("message"):
        raise Exception(res.get("message"))


def login():
    url = HOST_ADDRESS + f"/{VERSION}/user/login"
    login_data = {"email": EMAIL, "password": PASSWORD}
    response = requests.post(url=url, json=login_data)
    res = response.json()
    if res.get("code") != 0:
        raise Exception(res.get("message"))
    auth = response.headers["Authorization"]
    return auth


@pytest.fixture(scope="session")
def auth():
    try:
        register()
    except Exception as e:
        print(e)
    auth = login()
    return auth


@pytest.fixture(scope="session")
def token(auth):
    url = HOST_ADDRESS + f"/{VERSION}/system/new_token"
    auth = {"Authorization": auth}
    response = requests.post(url=url, headers=auth)
    res = response.json()
    if res.get("code") != 0:
        raise Exception(res.get("message"))
    return res["data"].get("token")


def get_my_llms(auth, name):
    url = HOST_ADDRESS + f"/{VERSION}/llm/my_llms"
    authorization = {"Authorization": auth}
    response = requests.get(url=url, headers=authorization)
    res = response.json()
    if res.get("code") != 0:
        raise Exception(res.get("message"))
    if name in res.get("data"):
        return True
    return False


def add_models(auth):
    url = HOST_ADDRESS + f"/{VERSION}/llm/set_api_key"
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
    url = HOST_ADDRESS + f"/{VERSION}/user/tenant_info"
    authorization = {"Authorization": auth}
    response = requests.get(url=url, headers=authorization)
    res = response.json()
    if res.get("code") != 0:
        raise Exception(res.get("message"))
    return res["data"].get("tenant_id")


@pytest.fixture(scope="session", autouse=True)
def set_tenant_info(auth):
    try:
        add_models(auth)
        tenant_id = get_tenant_info(auth)
    except Exception as e:
        pytest.exit(f"Error in set_tenant_info: {str(e)}")
    url = HOST_ADDRESS + f"/{VERSION}/user/set_tenant_info"
    authorization = {"Authorization": auth}
    tenant_info = {
        "tenant_id": tenant_id,
        "llm_id": "glm-4-flash@ZHIPU-AI",
        "embd_id": "BAAI/bge-small-en-v1.5@Builtin",
        "img2txt_id": "",
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

The file `test/testcases/conftest.py` is located in the `test/testcases` directory.

This file is part of the **Testing** infrastructure.

### Architecture Context

Files in this location typically handle concerns related to testcases.

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

- [configs.py](configs.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
