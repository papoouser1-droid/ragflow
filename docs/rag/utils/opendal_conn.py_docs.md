# Documentation: rag/utils/opendal_conn.py

## File Metadata

- **Path**: `rag/utils/opendal_conn.py`
- **Size**: 4419 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `rag/utils/opendal_conn.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `opendal`
- `logging`
- `pymysql`
- `urllib.parse`
- `common.config_utils`
- `common.decorator`

### Classes Defined

This file defines 1 class(es):

#### Class: `OpenDALStorage` (line 52)

**Methods**: __init__, health, put, get, rm, scan, obj_exist, init_db_config, init_opendal_mysql_table

### Functions Defined

This file defines 10 function(s):

#### Function: `get_opendal_config` (line 23)

**Parameters**: None

#### Function: `__init__` (line 53)

**Parameters**: self

#### Function: `health` (line 63)

**Parameters**: self

#### Function: `put` (line 67)

**Parameters**: self, bucket, fnm, binary, tenant_id

#### Function: `get` (line 70)

**Parameters**: self, bucket, fnm, tenant_id

#### Function: `rm` (line 73)

**Parameters**: self, bucket, fnm, tenant_id

#### Function: `scan` (line 77)

**Parameters**: self, bucket, fnm, tenant_id

#### Function: `obj_exist` (line 80)

**Parameters**: self, bucket, fnm, tenant_id

#### Function: `init_db_config` (line 84)

**Parameters**: self

#### Function: `init_opendal_mysql_table` (line 104)

**Parameters**: self

## Original Source Code

```py
import opendal
import logging
import pymysql
from urllib.parse import quote_plus

from common.config_utils import get_base_config
from common.decorator import singleton


CREATE_TABLE_SQL = """
CREATE TABLE IF NOT EXISTS `{}` (
    `key` VARCHAR(255) PRIMARY KEY,
    `value` LONGBLOB,
    `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    `updated_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
"""
SET_MAX_ALLOWED_PACKET_SQL = """
SET GLOBAL max_allowed_packet={}
"""


def get_opendal_config():
    try:
        opendal_config = get_base_config('opendal', {})
        if opendal_config.get("scheme", "mysql") == 'mysql':
            mysql_config = get_base_config('mysql', {})
            max_packet = mysql_config.get("max_allowed_packet", 134217728)
            kwargs = {
                "scheme": "mysql",
                "host": mysql_config.get("host", "127.0.0.1"),
                "port": str(mysql_config.get("port", 3306)),
                "user": mysql_config.get("user", "root"),
                "password": mysql_config.get("password", ""),
                "database": mysql_config.get("name", "test_open_dal"),
                "table": opendal_config.get("config", {}).get("oss_table", "opendal_storage"),
                "max_allowed_packet": str(max_packet)
            }
            kwargs["connection_string"] = f"mysql://{kwargs['user']}:{quote_plus(kwargs['password'])}@{kwargs['host']}:{kwargs['port']}/{kwargs['database']}?max_allowed_packet={max_packet}"
        else:
            scheme = opendal_config.get("scheme")
            config_data = opendal_config.get("config", {})
            kwargs = {"scheme": scheme, **config_data}
        logging.info("Loaded OpenDAL configuration from yaml: %s", kwargs)
        return kwargs
    except Exception as e:
        logging.error("Failed to load OpenDAL configuration from yaml: %s", str(e))
        raise


@singleton
class OpenDALStorage:
    def __init__(self):
        self._kwargs = get_opendal_config()
        self._scheme = self._kwargs.get('scheme', 'mysql')
        if self._scheme == 'mysql':
            self.init_db_config()
            self.init_opendal_mysql_table()
        self._operator = opendal.Operator(**self._kwargs)

        logging.info("OpenDALStorage initialized successfully")

    def health(self):
        bucket, fnm, binary = "txtxtxtxt1", "txtxtxtxt1", b"_t@@@1"
        return self._operator.write(f"{bucket}/{fnm}", binary)

    def put(self, bucket, fnm, binary, tenant_id=None):
        self._operator.write(f"{bucket}/{fnm}", binary)

    def get(self, bucket, fnm, tenant_id=None):
        return self._operator.read(f"{bucket}/{fnm}")

    def rm(self, bucket, fnm, tenant_id=None):
        self._operator.delete(f"{bucket}/{fnm}")
        self._operator.__init__()

    def scan(self, bucket, fnm, tenant_id=None):
        return self._operator.scan(f"{bucket}/{fnm}")

    def obj_exist(self, bucket, fnm, tenant_id=None):
        return self._operator.exists(f"{bucket}/{fnm}")


    def init_db_config(self):
        try:
            conn = pymysql.connect(
                host=self._kwargs['host'],
                port=int(self._kwargs['port']),
                user=self._kwargs['user'],
                password=self._kwargs['password'],
                database=self._kwargs['database']
            )
            cursor = conn.cursor()
            max_packet = self._kwargs.get('max_allowed_packet', 4194304)  # Default to 4MB if not specified
            cursor.execute(SET_MAX_ALLOWED_PACKET_SQL.format(max_packet))
            conn.commit()
            cursor.close()
            conn.close()
            logging.info(f"Database configuration initialized with max_allowed_packet={max_packet}")
        except Exception as e:
            logging.error(f"Failed to initialize database configuration: {str(e)}")
            raise

    def init_opendal_mysql_table(self):
        conn = pymysql.connect(
            host=self._kwargs['host'],
            port=int(self._kwargs['port']),
            user=self._kwargs['user'],
            password=self._kwargs['password'],
            database=self._kwargs['database']
        )
        cursor = conn.cursor()
        cursor.execute(CREATE_TABLE_SQL.format(self._kwargs['table']))
        conn.commit()
        cursor.close()
        conn.close()
        logging.info(f"Table `{self._kwargs['table']}` initialized.")

```

## Detailed Analysis

### File Role in Repository

The file `rag/utils/opendal_conn.py` is located in the `rag/utils` directory.

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
