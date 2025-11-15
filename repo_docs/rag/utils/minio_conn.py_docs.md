# File Documentation: rag/utils/minio_conn.py

## File Metadata

- **Path**: `rag/utils/minio_conn.py`
- **Extension**: `.py`
- **Lines**: 178
- **Characters**: 6,165
- **Size**: 6,165 bytes
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
import time
from minio import Minio
from minio.commonconfig import CopySource
from minio.error import S3Error
from io import BytesIO
from common.decorator import singleton
from common import settings


@singleton
class RAGFlowMinio:
    def __init__(self):
        self.conn = None
        self.__open__()

    def __open__(self):
        try:
            if self.conn:
                self.__close__()
        except Exception:
            pass

        try:
            self.conn = Minio(settings.MINIO["host"],
                              access_key=settings.MINIO["user"],
                              secret_key=settings.MINIO["password"],
                              secure=False
                              )
        except Exception:
            logging.exception(
                "Fail to connect %s " % settings.MINIO["host"])

    def __close__(self):
        del self.conn
        self.conn = None

    def health(self):
        bucket, fnm, binary = "txtxtxtxt1", "txtxtxtxt1", b"_t@@@1"
        if not self.conn.bucket_exists(bucket):
            self.conn.make_bucket(bucket)
        r = self.conn.put_object(bucket, fnm,
                                 BytesIO(binary),
                                 len(binary)
                                 )
        return r

    def put(self, bucket, fnm, binary, tenant_id=None):
        for _ in range(3):
            try:
                if not self.conn.bucket_exists(bucket):
                    self.conn.make_bucket(bucket)

                r = self.conn.put_object(bucket, fnm,
                                         BytesIO(binary),
                                         len(binary)
                                         )
                return r
            except Exception:
                logging.exception(f"Fail to put {bucket}/{fnm}:")
                self.__open__()
                time.sleep(1)

    def rm(self, bucket, fnm, tenant_id=None):
        try:
            self.conn.remove_object(bucket, fnm)
        except Exception:
            logging.exception(f"Fail to remove {bucket}/{fnm}:")

    def get(self, bucket, filename, tenant_id=None):
        for _ in range(1):
            try:
                r = self.conn.get_object(bucket, filename)
                return r.read()
            except Exception:
                logging.exception(f"Fail to get {bucket}/{filename}")
                self.__open__()
                time.sleep(1)
        return None

    def obj_exist(self, bucket, filename, tenant_id=None):
        try:
            if not self.conn.bucket_exists(bucket):
                return False
            if self.conn.stat_object(bucket, filename):
                return True
            else:
                return False
        except S3Error as e:
            if e.code in ["NoSuchKey", "NoSuchBucket", "ResourceNotFound"]:
                return False
        except Exception:
            logging.exception(f"obj_exist {bucket}/{filename} got exception")
            return False

    def bucket_exists(self, bucket):
        try:
            if not self.conn.bucket_exists(bucket):
                return False
            else:
                return True
        except S3Error as e:
            if e.code in ["NoSuchKey", "NoSuchBucket", "ResourceNotFound"]:
                return False
        except Exception:
            logging.exception(f"bucket_exist {bucket} got exception")
            return False

    def get_presigned_url(self, bucket, fnm, expires, tenant_id=None):
        for _ in range(10):
            try:
                return self.conn.get_presigned_url("GET", bucket, fnm, expires)
            except Exception:
                logging.exception(f"Fail to get_presigned {bucket}/{fnm}:")
                self.__open__()
                time.sleep(1)
        return None

    def remove_bucket(self, bucket):
        try:
            if self.conn.bucket_exists(bucket):
                objects_to_delete = self.conn.list_objects(bucket, recursive=True)
                for obj in objects_to_delete:
                    self.conn.remove_object(bucket, obj.object_name)
                self.conn.remove_bucket(bucket)
        except Exception:
            logging.exception(f"Fail to remove bucket {bucket}")

    def copy(self, src_bucket, src_path, dest_bucket, dest_path):
        try:
            if not self.conn.bucket_exists(dest_bucket):
                self.conn.make_bucket(dest_bucket)

            try:
                self.conn.stat_object(src_bucket, src_path)
            except Exception as e:
                logging.exception(f"Source object not found: {src_bucket}/{src_path}, {e}")
                return False

            self.conn.copy_object(
                dest_bucket,
                dest_path,
                CopySource(src_bucket, src_path),
            )
            return True

        except Exception:
            logging.exception(f"Fail to copy {src_bucket}/{src_path} -> {dest_bucket}/{dest_path}")
            return False

    def move(self, src_bucket, src_path, dest_bucket, dest_path):
        try:
            if self.copy(src_bucket, src_path, dest_bucket, dest_path):
                self.rm(src_bucket, src_path)
                return True
            else:
                logging.error(f"Copy failed, move aborted: {src_bucket}/{src_path}")
                return False
        except Exception:
            logging.exception(f"Fail to move {src_bucket}/{src_path} -> {dest_bucket}/{dest_path}")
            return False

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

- `RAGFlowMinio`: Class definition

### Imports (8)

- `import logging`
- `import time`
- `from minio import Minio`
- `from minio.commonconfig import CopySource`
- `from minio.error import S3Error`
- `from io import BytesIO`
- `from common.decorator import singleton`
- `from common import settings`

## Code Structure Analysis

- Total lines: 178
- Blank lines: 21 (11.8%)
- Comment lines: ~15 (8.4%)
- Code lines: ~142


## Dependencies and Imports

- `import logging`
- `import time`
- `from minio import Minio`
- `from minio.commonconfig import CopySource`
- `from minio.error import S3Error`
- `from io import BytesIO`
- `from common.decorator import singleton`
- `from common import settings`

## Design & Architecture

This file is located in the `rag` directory, specifically within `rag/utils`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 5 loop(s) - consider algorithmic complexity

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
- Imports from `minio`
- Imports from `minio.commonconfig`
- Imports from `minio.error`
- Imports from `io`
- Imports from `common.decorator`
- Potential test file: `test_minio_conn.py`

## Keywords

1, ANY, All, Apache, Authors, BASIS, BytesIO, CONDITIONS, Copy, CopySource, Copyright, Exception, Fail, False, GET, InfiniFlow, KIND, LICENSE, License, Licensed, MINIO, Minio, NoSuchBucket, NoSuchKey, None, Python, RAGFlowMinio, Reserved, ResourceNotFound, Rights, S3Error, See, Source, The, True, Unless, Version, WARRANTIES, WITHOUT, You, __close__, __init__, __open__, bucket_exists, copy, get, get_presigned_url, health, move, obj_exist...

---
*Generated by RAGFlow Repository Documentation Generator*
