# File Documentation: rag/utils/azure_spn_conn.py

## File Metadata

- **Path**: `rag/utils/azure_spn_conn.py`
- **Extension**: `.py`
- **Lines**: 107
- **Characters**: 3,822
- **Size**: 3,822 bytes
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

import logging
import os
import time
from common.decorator import singleton
from azure.identity import ClientSecretCredential, AzureAuthorityHosts
from azure.storage.filedatalake import FileSystemClient
from common import settings


@singleton
class RAGFlowAzureSpnBlob:
    def __init__(self):
        self.conn = None
        self.account_url = os.getenv('ACCOUNT_URL', settings.AZURE["account_url"])
        self.client_id = os.getenv('CLIENT_ID', settings.AZURE["client_id"])
        self.secret = os.getenv('SECRET', settings.AZURE["secret"])
        self.tenant_id = os.getenv('TENANT_ID', settings.AZURE["tenant_id"])
        self.container_name = os.getenv('CONTAINER_NAME', settings.AZURE["container_name"])
        self.__open__()

    def __open__(self):
        try:
            if self.conn:
                self.__close__()
        except Exception:
            pass

        try:
            credentials = ClientSecretCredential(tenant_id=self.tenant_id, client_id=self.client_id, client_secret=self.secret, authority=AzureAuthorityHosts.AZURE_CHINA)
            self.conn = FileSystemClient(account_url=self.account_url, file_system_name=self.container_name, credential=credentials)
        except Exception:
            logging.exception("Fail to connect %s" % self.account_url)

    def __close__(self):
        del self.conn
        self.conn = None

    def health(self):
        _bucket, fnm, binary = "txtxtxtxt1", "txtxtxtxt1", b"_t@@@1"
        f = self.conn.create_file(fnm)
        f.append_data(binary, offset=0, length=len(binary))
        return f.flush_data(len(binary))

    def put(self, bucket, fnm, binary):
        for _ in range(3):
            try:
                f = self.conn.create_file(fnm)
                f.append_data(binary, offset=0, length=len(binary))
                return f.flush_data(len(binary))
            except Exception:
                logging.exception(f"Fail put {bucket}/{fnm}")
                self.__open__()
                time.sleep(1)
                return None
        return None

    def rm(self, bucket, fnm):
        try:
            self.conn.delete_file(fnm)
        except Exception:
            logging.exception(f"Fail rm {bucket}/{fnm}")

    def get(self, bucket, fnm):
        for _ in range(1):
            try:
                client = self.conn.get_file_client(fnm)
                r = client.download_file()
                return r.read()
            except Exception:
                logging.exception(f"fail get {bucket}/{fnm}")
                self.__open__()
                time.sleep(1)
        return None

    def obj_exist(self, bucket, fnm):
        try:
            client = self.conn.get_file_client(fnm)
            return client.exists()
        except Exception:
            logging.exception(f"Fail put {bucket}/{fnm}")
        return False

    def get_presigned_url(self, bucket, fnm, expires):
        for _ in range(10):
            try:
                return self.conn.get_presigned_url("GET", bucket, fnm, expires)
            except Exception:
                logging.exception(f"fail get {bucket}/{fnm}")
                self.__open__()
                time.sleep(1)
        return None
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

- `RAGFlowAzureSpnBlob`: Class definition

### Imports (7)

- `import logging`
- `import os`
- `import time`
- `from common.decorator import singleton`
- `from azure.identity import ClientSecretCredential, AzureAuthorityHosts`
- `from azure.storage.filedatalake import FileSystemClient`
- `from common import settings`

## Code Structure Analysis

- Total lines: 107
- Blank lines: 12 (11.2%)
- Comment lines: ~15 (14.0%)
- Code lines: ~80


## Dependencies and Imports

- `import logging`
- `import os`
- `import time`
- `from common.decorator import singleton`
- `from azure.identity import ClientSecretCredential, AzureAuthorityHosts`
- `from azure.storage.filedatalake import FileSystemClient`
- `from common import settings`

## Design & Architecture

This file is located in the `rag` directory, specifically within `rag/utils`.

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

- Other files in `rag/utils/` directory
- Imports from `common.decorator`
- Imports from `azure.identity`
- Imports from `azure.storage.filedatalake`
- Imports from `common`
- Potential test file: `test_azure_spn_conn.py`

## Keywords

1, ACCOUNT_URL, ANY, AZURE, AZURE_CHINA, All, Apache, Authors, AzureAuthorityHosts, BASIS, CLIENT_ID, CONDITIONS, CONTAINER_NAME, ClientSecretCredential, Copyright, Exception, Fail, False, FileSystemClient, GET, InfiniFlow, KIND, LICENSE, License, Licensed, None, Python, RAGFlowAzureSpnBlob, Reserved, Rights, SECRET, See, TENANT_ID, The, Unless, Version, WARRANTIES, WITHOUT, You, __close__, __init__, __open__, get, get_presigned_url, health, obj_exist, put, rm, singleton

---
*Generated by RAGFlow Repository Documentation Generator*
