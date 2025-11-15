# File Documentation: rag/utils/opendal_conn.py

## File Metadata

- **Path**: `rag/utils/opendal_conn.py`
- **Extension**: `.py`
- **Lines**: 118
- **Characters**: 4,419
- **Size**: 4,419 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

## Original Source

```python
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

## High-Level Overview

"""
"""

## Detailed Walkthrough

### Classes (1)

- `OpenDALStorage`: Class definition

### Functions (1)

- `get_opendal_config()`: Function definition

### Imports (6)

- `import opendal`
- `import logging`
- `import pymysql`
- `from urllib.parse import quote_plus`
- `from common.config_utils import get_base_config`
- `from common.decorator import singleton`

## Code Structure Analysis

- Total lines: 118
- Blank lines: 18 (15.3%)
- Comment lines: ~2 (1.7%)
- Code lines: ~98


## Dependencies and Imports

- `import opendal`
- `import logging`
- `import pymysql`
- `from urllib.parse import quote_plus`
- `from common.config_utils import get_base_config`
- `from common.decorator import singleton`

## Design & Architecture

This file is located in the `rag` directory, specifically within `rag/utils`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **SQL Operations**: Ensure parameterized queries to prevent SQL injection
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
- Imports from `urllib.parse`
- Imports from `common.config_utils`
- Imports from `common.decorator`
- Potential test file: `test_opendal_conn.py`

## Keywords

1, CREATE, CREATE_TABLE_SQL, CURRENT_TIMESTAMP, DEFAULT, Database, Default, EXISTS, Exception, Failed, GLOBAL, KEY, LONGBLOB, Loaded, NOT, None, OpenDAL, OpenDALStorage, Operator, PRIMARY, Python, SET, SET_MAX_ALLOWED_PACKET_SQL, TABLE, TIMESTAMP, Table, UPDATE, VARCHAR, __init__, get, get_opendal_config, health, init_db_config, init_opendal_mysql_table, obj_exist, put, rm, scan, singleton

---
*Generated by RAGFlow Repository Documentation Generator*
