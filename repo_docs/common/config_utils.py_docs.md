# File Documentation: common/config_utils.py

## File Metadata

- **Path**: `common/config_utils.py`
- **Extension**: `.py`
- **Lines**: 156
- **Characters**: 5,054
- **Size**: 5,054 bytes
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

import os
import copy
import logging
import importlib
from filelock import FileLock

from common.file_utils import get_project_base_directory
from common.constants import SERVICE_CONF
from ruamel.yaml import YAML


def load_yaml_conf(conf_path):
    if not os.path.isabs(conf_path):
        conf_path = os.path.join(get_project_base_directory(), conf_path)
    try:
        with open(conf_path) as f:
            yaml = YAML(typ="safe", pure=True)
            return yaml.load(f)
    except Exception as e:
        raise EnvironmentError("loading yaml file config from {} failed:".format(conf_path), e)


def rewrite_yaml_conf(conf_path, config):
    if not os.path.isabs(conf_path):
        conf_path = os.path.join(get_project_base_directory(), conf_path)
    try:
        with open(conf_path, "w") as f:
            yaml = YAML(typ="safe")
            yaml.dump(config, f)
    except Exception as e:
        raise EnvironmentError("rewrite yaml file config {} failed:".format(conf_path), e)


def conf_realpath(conf_name):
    conf_path = f"conf/{conf_name}"
    return os.path.join(get_project_base_directory(), conf_path)


def read_config(conf_name=SERVICE_CONF):
    local_config = {}
    local_path = conf_realpath(f'local.{conf_name}')

    # load local config file
    if os.path.exists(local_path):
        local_config = load_yaml_conf(local_path)
        if not isinstance(local_config, dict):
            raise ValueError(f'Invalid config file: "{local_path}".')

    global_config_path = conf_realpath(conf_name)
    global_config = load_yaml_conf(global_config_path)

    if not isinstance(global_config, dict):
        raise ValueError(f'Invalid config file: "{global_config_path}".')

    global_config.update(local_config)
    return global_config


CONFIGS = read_config()


def show_configs():
    msg = f"Current configs, from {conf_realpath(SERVICE_CONF)}:"
    for k, v in CONFIGS.items():
        if isinstance(v, dict):
            if "password" in v:
                v = copy.deepcopy(v)
                v["password"] = "*" * 8
            if "access_key" in v:
                v = copy.deepcopy(v)
                v["access_key"] = "*" * 8
            if "secret_key" in v:
                v = copy.deepcopy(v)
                v["secret_key"] = "*" * 8
            if "secret" in v:
                v = copy.deepcopy(v)
                v["secret"] = "*" * 8
            if "sas_token" in v:
                v = copy.deepcopy(v)
                v["sas_token"] = "*" * 8
            if "oauth" in k:
                v = copy.deepcopy(v)
                for key, val in v.items():
                    if "client_secret" in val:
                        val["client_secret"] = "*" * 8
            if "authentication" in k:
                v = copy.deepcopy(v)
                for key, val in v.items():
                    if "http_secret_key" in val:
                        val["http_secret_key"] = "*" * 8
        msg += f"\n\t{k}: {v}"
    logging.info(msg)


def get_base_config(key, default=None):
    if key is None:
        return None
    if default is None:
        default = os.environ.get(key.upper())
    return CONFIGS.get(key, default)


def decrypt_database_password(password):
    encrypt_password = get_base_config("encrypt_password", False)
    encrypt_module = get_base_config("encrypt_module", False)
    private_key = get_base_config("private_key", None)

    if not password or not encrypt_password:
        return password

    if not private_key:
        raise ValueError("No private key")

    module_fun = encrypt_module.split("#")
    pwdecrypt_fun = getattr(
        importlib.import_module(
            module_fun[0]),
        module_fun[1])

    return pwdecrypt_fun(private_key, password)


def decrypt_database_config(database=None, passwd_key="password", name="database"):
    if not database:
        database = get_base_config(name, {})

    database[passwd_key] = decrypt_database_password(database[passwd_key])
    return database


def update_config(key, value, conf_name=SERVICE_CONF):
    conf_path = conf_realpath(conf_name=conf_name)
    if not os.path.isabs(conf_path):
        conf_path = os.path.join(get_project_base_directory(), conf_path)

    with FileLock(os.path.join(os.path.dirname(conf_path), ".lock")):
        config = load_yaml_conf(conf_path=conf_path) or {}
        config[key] = value
        rewrite_yaml_conf(conf_path=conf_path, config=config)

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


### Functions (9)

- `load_yaml_conf()`: Function definition
- `rewrite_yaml_conf()`: Function definition
- `conf_realpath()`: Function definition
- `read_config()`: Function definition
- `show_configs()`: Function definition
- `get_base_config()`: Function definition
- `decrypt_database_password()`: Function definition
- `decrypt_database_config()`: Function definition
- `update_config()`: Function definition

### Imports (8)

- `import os`
- `import copy`
- `import logging`
- `import importlib`
- `from filelock import FileLock`
- `from common.file_utils import get_project_base_directory`
- `from common.constants import SERVICE_CONF`
- `from ruamel.yaml import YAML`

## Code Structure Analysis

- Total lines: 156
- Blank lines: 33 (21.2%)
- Comment lines: ~16 (10.3%)
- Code lines: ~107


## Dependencies and Imports

- `import os`
- `import copy`
- `import logging`
- `import importlib`
- `from filelock import FileLock`
- `from common.file_utils import get_project_base_directory`
- `from common.constants import SERVICE_CONF`
- `from ruamel.yaml import YAML`

## Design & Architecture

This file is located in the `common` directory, specifically within `common`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 4 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `common/` directory
- Imports from `filelock`
- Imports from `common.file_utils`
- Imports from `common.constants`
- Imports from `ruamel.yaml`
- Potential test file: `test_config_utils.py`

## Keywords

ANY, All, Apache, Authors, BASIS, CONDITIONS, CONFIGS, Copyright, Current, EnvironmentError, Exception, False, FileLock, InfiniFlow, Invalid, KIND, LICENSE, License, Licensed, None, Python, Reserved, Rights, SERVICE_CONF, See, The, True, Unless, ValueError, Version, WARRANTIES, WITHOUT, YAML, You, conf_realpath, decrypt_database_config, decrypt_database_password, get_base_config, load_yaml_conf, read_config, rewrite_yaml_conf, show_configs, update_config

---
*Generated by RAGFlow Repository Documentation Generator*
