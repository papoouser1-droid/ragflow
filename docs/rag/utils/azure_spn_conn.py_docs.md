# Documentation: rag/utils/azure_spn_conn.py

## File Metadata

- **Path**: `rag/utils/azure_spn_conn.py`
- **Size**: 3822 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `rag/utils/azure_spn_conn.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `os`
- `time`
- `common.decorator`
- `azure.identity`
- `azure.storage.filedatalake`
- `common`

### Classes Defined

This file defines 1 class(es):

#### Class: `RAGFlowAzureSpnBlob` (line 27)

**Methods**: __init__, __open__, __close__, health, put, rm, get, obj_exist, get_presigned_url

### Functions Defined

This file defines 9 function(s):

#### Function: `__init__` (line 28)

**Parameters**: self

#### Function: `__open__` (line 37)

**Parameters**: self

#### Function: `__close__` (line 50)

**Parameters**: self

#### Function: `health` (line 54)

**Parameters**: self

#### Function: `put` (line 60)

**Parameters**: self, bucket, fnm, binary

#### Function: `rm` (line 73)

**Parameters**: self, bucket, fnm

#### Function: `get` (line 79)

**Parameters**: self, bucket, fnm

#### Function: `obj_exist` (line 91)

**Parameters**: self, bucket, fnm

#### Function: `get_presigned_url` (line 99)

**Parameters**: self, bucket, fnm, expires

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

## Detailed Analysis

### File Role in Repository

The file `rag/utils/azure_spn_conn.py` is located in the `rag/utils` directory.

This file is part of the **RAG (Retrieval-Augmented Generation)** core engine.

### Architecture Context

Files in this location typically handle concerns related to utils.

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

- [__init__.py](__init__.py_docs.md)
- [azure_sas_conn.py](azure_sas_conn.py_docs.md)
- [base64_image.py](base64_image.py_docs.md)
- [doc_store_conn.py](doc_store_conn.py_docs.md)
- [es_conn.py](es_conn.py_docs.md)
- [file_utils.py](file_utils.py_docs.md)
- [infinity_conn.py](infinity_conn.py_docs.md)
- [mcp_tool_call_conn.py](mcp_tool_call_conn.py_docs.md)
- [minio_conn.py](minio_conn.py_docs.md)
- [opendal_conn.py](opendal_conn.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
