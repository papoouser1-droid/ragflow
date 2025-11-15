# Documentation: rag/utils/oss_conn.py

## File Metadata

- **Path**: `rag/utils/oss_conn.py`
- **Size**: 6077 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `rag/utils/oss_conn.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `boto3`
- `botocore.exceptions`
- `botocore.config`
- `time`
- `io`
- `common.decorator`
- `common`

### Classes Defined

This file defines 1 class(es):

#### Class: `RAGFlowOSS` (line 27)

**Methods**: __init__, use_default_bucket, use_prefix_path, __open__, __close__, bucket_exists, health, get_properties, list, put, rm, get, obj_exist, get_presigned_url

### Functions Defined

This file defines 16 function(s):

#### Function: `__init__` (line 28)

**Parameters**: self

#### Function: `use_default_bucket` (line 40)

**Parameters**: method

#### Function: `use_prefix_path` (line 48)

**Parameters**: method

#### Function: `__open__` (line 55)

**Parameters**: self

#### Function: `__close__` (line 75)

**Parameters**: self

#### Function: `bucket_exists` (line 80)

**Parameters**: self, bucket

#### Function: `health` (line 90)

**Parameters**: self

#### Function: `get_properties` (line 101)

**Parameters**: self, bucket, key

#### Function: `list` (line 104)

**Parameters**: self, bucket, dir, recursive

#### Function: `put` (line 109)

**Parameters**: self, bucket, fnm, binary, tenant_id

#### Function: `rm` (line 126)

**Parameters**: self, bucket, fnm, tenant_id

#### Function: `get` (line 134)

**Parameters**: self, bucket, fnm, tenant_id

#### Function: `obj_exist` (line 148)

**Parameters**: self, bucket, fnm, tenant_id

#### Function: `get_presigned_url` (line 160)

**Parameters**: self, bucket, fnm, expires, tenant_id

#### Function: `wrapper` (line 41)

**Parameters**: self, bucket

#### Function: `wrapper` (line 49)

**Parameters**: self, bucket, fnm

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

import logging
import boto3
from botocore.exceptions import ClientError
from botocore.config import Config
import time
from io import BytesIO
from common.decorator import singleton
from common import settings


@singleton
class RAGFlowOSS:
    def __init__(self):
        self.conn = None
        self.oss_config = settings.OSS
        self.access_key = self.oss_config.get('access_key', None)
        self.secret_key = self.oss_config.get('secret_key', None)
        self.endpoint_url = self.oss_config.get('endpoint_url', None)
        self.region = self.oss_config.get('region', None)
        self.bucket = self.oss_config.get('bucket', None)
        self.prefix_path = self.oss_config.get('prefix_path', None)
        self.__open__()

    @staticmethod
    def use_default_bucket(method):
        def wrapper(self, bucket, *args, **kwargs):
            # If there is a default bucket, use the default bucket
            actual_bucket = self.bucket if self.bucket else bucket
            return method(self, actual_bucket, *args, **kwargs)
        return wrapper
    
    @staticmethod
    def use_prefix_path(method):
        def wrapper(self, bucket, fnm, *args, **kwargs):
            # If the prefix path is set, use the prefix path
            fnm = f"{self.prefix_path}/{fnm}" if self.prefix_path else fnm
            return method(self, bucket, fnm, *args, **kwargs)
        return wrapper

    def __open__(self):
        try:
            if self.conn:
                self.__close__()
        except Exception:
            pass

        try:
            # Reference：https://help.aliyun.com/zh/oss/developer-reference/use-amazon-s3-sdks-to-access-oss
            self.conn = boto3.client(
                's3',
                region_name=self.region,
                aws_access_key_id=self.access_key,
                aws_secret_access_key=self.secret_key,
                endpoint_url=self.endpoint_url,
                config=Config(s3={"addressing_style": "virtual"}, signature_version='v4')
            )
        except Exception:
            logging.exception(f"Fail to connect at region {self.region}")

    def __close__(self):
        del self.conn
        self.conn = None

    @use_default_bucket
    def bucket_exists(self, bucket):
        try:
            logging.debug(f"head_bucket bucketname {bucket}")
            self.conn.head_bucket(Bucket=bucket)
            exists = True
        except ClientError:
            logging.exception(f"head_bucket error {bucket}")
            exists = False
        return exists

    def health(self):
        bucket = self.bucket
        fnm = "txtxtxtxt1"
        fnm, binary = f"{self.prefix_path}/{fnm}" if self.prefix_path else fnm, b"_t@@@1"
        if not self.bucket_exists(bucket):
            self.conn.create_bucket(Bucket=bucket)
            logging.debug(f"create bucket {bucket} ********")

        r = self.conn.upload_fileobj(BytesIO(binary), bucket, fnm)
        return r

    def get_properties(self, bucket, key):
        return {}

    def list(self, bucket, dir, recursive=True):
        return []

    @use_prefix_path
    @use_default_bucket
    def put(self, bucket, fnm, binary, tenant_id=None):
        logging.debug(f"bucket name {bucket}; filename :{fnm}:")
        for _ in range(1):
            try:
                if not self.bucket_exists(bucket):
                    self.conn.create_bucket(Bucket=bucket)
                    logging.info(f"create bucket {bucket} ********")
                r = self.conn.upload_fileobj(BytesIO(binary), bucket, fnm)

                return r
            except Exception:
                logging.exception(f"Fail put {bucket}/{fnm}")
                self.__open__()
                time.sleep(1)

    @use_prefix_path
    @use_default_bucket
    def rm(self, bucket, fnm, tenant_id=None):
        try:
            self.conn.delete_object(Bucket=bucket, Key=fnm)
        except Exception:
            logging.exception(f"Fail rm {bucket}/{fnm}")

    @use_prefix_path
    @use_default_bucket
    def get(self, bucket, fnm, tenant_id=None):
        for _ in range(1):
            try:
                r = self.conn.get_object(Bucket=bucket, Key=fnm)
                object_data = r['Body'].read()
                return object_data
            except Exception:
                logging.exception(f"fail get {bucket}/{fnm}")
                self.__open__()
                time.sleep(1)
        return None

    @use_prefix_path
    @use_default_bucket
    def obj_exist(self, bucket, fnm, tenant_id=None):
        try:
            if self.conn.head_object(Bucket=bucket, Key=fnm):
                return True
        except ClientError as e:
            if e.response['Error']['Code'] == '404':
                return False
            else:
                raise

    @use_prefix_path
    @use_default_bucket
    def get_presigned_url(self, bucket, fnm, expires, tenant_id=None):
        for _ in range(10):
            try:
                r = self.conn.generate_presigned_url('get_object',
                                                     Params={'Bucket': bucket,
                                                             'Key': fnm},
                                                     ExpiresIn=expires)

                return r
            except Exception:
                logging.exception(f"fail get url {bucket}/{fnm}")
                self.__open__()
                time.sleep(1)
        return None


```

## Detailed Analysis

### File Role in Repository

The file `rag/utils/oss_conn.py` is located in the `rag/utils` directory.

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
- [azure_spn_conn.py](azure_spn_conn.py_docs.md)
- [base64_image.py](base64_image.py_docs.md)
- [doc_store_conn.py](doc_store_conn.py_docs.md)
- [es_conn.py](es_conn.py_docs.md)
- [file_utils.py](file_utils.py_docs.md)
- [infinity_conn.py](infinity_conn.py_docs.md)
- [mcp_tool_call_conn.py](mcp_tool_call_conn.py_docs.md)
- [minio_conn.py](minio_conn.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
