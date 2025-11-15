# File Documentation: rag/utils/azure_sas_conn.py

## File Metadata

- **Path**: `rag/utils/azure_sas_conn.py`
- **Extension**: `.py`
- **Lines**: 96
- **Characters**: 3,082
- **Size**: 3,082 bytes
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
from io import BytesIO
from common.decorator import singleton
from azure.storage.blob import ContainerClient
from common import settings


@singleton
class RAGFlowAzureSasBlob:
    def __init__(self):
        self.conn = None
        self.container_url = os.getenv('CONTAINER_URL', settings.AZURE["container_url"])
        self.sas_token = os.getenv('SAS_TOKEN', settings.AZURE["sas_token"])
        self.__open__()

    def __open__(self):
        try:
            if self.conn:
                self.__close__()
        except Exception:
            pass

        try:
            self.conn = ContainerClient.from_container_url(self.container_url + "?" + self.sas_token)
        except Exception:
            logging.exception("Fail to connect %s " % self.container_url)

    def __close__(self):
        del self.conn
        self.conn = None

    def health(self):
        _bucket, fnm, binary = "txtxtxtxt1", "txtxtxtxt1", b"_t@@@1"
        return self.conn.upload_blob(name=fnm, data=BytesIO(binary), length=len(binary))

    def put(self, bucket, fnm, binary):
        for _ in range(3):
            try:
                return self.conn.upload_blob(name=fnm, data=BytesIO(binary), length=len(binary))
            except Exception:
                logging.exception(f"Fail put {bucket}/{fnm}")
                self.__open__()
                time.sleep(1)

    def rm(self, bucket, fnm):
        try:
            self.conn.delete_blob(fnm)
        except Exception:
            logging.exception(f"Fail rm {bucket}/{fnm}")

    def get(self, bucket, fnm):
        for _ in range(1):
            try:
                r = self.conn.download_blob(fnm)
                return r.read()
            except Exception:
                logging.exception(f"fail get {bucket}/{fnm}")
                self.__open__()
                time.sleep(1)
        return

    def obj_exist(self, bucket, fnm):
        try:
            return self.conn.get_blob_client(fnm).exists()
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
        return

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

- `RAGFlowAzureSasBlob`: Class definition

### Imports (7)

- `import logging`
- `import os`
- `import time`
- `from io import BytesIO`
- `from common.decorator import singleton`
- `from azure.storage.blob import ContainerClient`
- `from common import settings`

## Code Structure Analysis

- Total lines: 96
- Blank lines: 13 (13.5%)
- Comment lines: ~15 (15.6%)
- Code lines: ~68


## Dependencies and Imports

- `import logging`
- `import os`
- `import time`
- `from io import BytesIO`
- `from common.decorator import singleton`
- `from azure.storage.blob import ContainerClient`
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
- Imports from `io`
- Imports from `common.decorator`
- Imports from `azure.storage.blob`
- Imports from `common`
- Potential test file: `test_azure_sas_conn.py`

## Keywords

1, ANY, AZURE, All, Apache, Authors, BASIS, BytesIO, CONDITIONS, CONTAINER_URL, ContainerClient, Copyright, Exception, Fail, False, GET, InfiniFlow, KIND, LICENSE, License, Licensed, None, Python, RAGFlowAzureSasBlob, Reserved, Rights, SAS_TOKEN, See, The, Unless, Version, WARRANTIES, WITHOUT, You, __close__, __init__, __open__, get, get_presigned_url, health, obj_exist, put, rm, singleton

---
*Generated by RAGFlow Repository Documentation Generator*
