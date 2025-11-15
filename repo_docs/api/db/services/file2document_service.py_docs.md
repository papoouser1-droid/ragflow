# File Documentation: api/db/services/file2document_service.py

## File Metadata

- **Path**: `api/db/services/file2document_service.py`
- **Extension**: `.py`
- **Lines**: 97
- **Characters**: 3,547
- **Size**: 3,547 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

## Original Source

```python
#
#  Copyright 2024 The InfiniFlow Authors. All Rights Reserved.
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
from datetime import datetime

from common.constants import FileSource
from api.db.db_models import DB
from api.db.db_models import File, File2Document
from api.db.services.common_service import CommonService
from api.db.services.document_service import DocumentService
from common.time_utils import current_timestamp, datetime_format


class File2DocumentService(CommonService):
    model = File2Document

    @classmethod
    @DB.connection_context()
    def get_by_file_id(cls, file_id):
        objs = cls.model.select().where(cls.model.file_id == file_id)
        return objs

    @classmethod
    @DB.connection_context()
    def get_by_document_id(cls, document_id):
        objs = cls.model.select().where(cls.model.document_id == document_id)
        return objs

    @classmethod
    @DB.connection_context()
    def get_by_document_ids(cls, document_ids):
        objs = cls.model.select().where(cls.model.document_id.in_(document_ids))
        return list(objs.dicts())

    @classmethod
    @DB.connection_context()
    def insert(cls, obj):
        if not cls.save(**obj):
            raise RuntimeError("Database error (File)!")
        return File2Document(**obj)

    @classmethod
    @DB.connection_context()
    def delete_by_file_id(cls, file_id):
        return cls.model.delete().where(cls.model.file_id == file_id).execute()

    @classmethod
    @DB.connection_context()
    def delete_by_document_ids_or_file_ids(cls, document_ids, file_ids):
        if not document_ids:
            return cls.model.delete().where(cls.model.file_id.in_(file_ids)).execute()
        elif not file_ids:
            return cls.model.delete().where(cls.model.document_id.in_(document_ids)).execute()
        return cls.model.delete().where(cls.model.document_id.in_(document_ids) | cls.model.file_id.in_(file_ids)).execute()

    @classmethod
    @DB.connection_context()
    def delete_by_document_id(cls, doc_id):
        return cls.model.delete().where(cls.model.document_id == doc_id).execute()

    @classmethod
    @DB.connection_context()
    def update_by_file_id(cls, file_id, obj):
        obj["update_time"] = current_timestamp()
        obj["update_date"] = datetime_format(datetime.now())
        cls.model.update(obj).where(cls.model.id == file_id).execute()
        return File2Document(**obj)

    @classmethod
    @DB.connection_context()
    def get_storage_address(cls, doc_id=None, file_id=None):
        if doc_id:
            f2d = cls.get_by_document_id(doc_id)
        else:
            f2d = cls.get_by_file_id(file_id)
        if f2d:
            file = File.get_by_id(f2d[0].file_id)
            if not file.source_type or file.source_type == FileSource.LOCAL:
                return file.parent_id, file.location
            doc_id = f2d[0].document_id

        assert doc_id, "please specify doc_id"
        e, doc = DocumentService.get_by_id(doc_id)
        return doc.kb_id, doc.location

```

## High-Level Overview

#
#  Copyright 2024 The InfiniFlow Authors. All Rights Reserved.
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

- `File2DocumentService`: Class definition

### Imports (7)

- `from datetime import datetime`
- `from common.constants import FileSource`
- `from api.db.db_models import DB`
- `from api.db.db_models import File, File2Document`
- `from api.db.services.common_service import CommonService`
- `from api.db.services.document_service import DocumentService`
- `from common.time_utils import current_timestamp, datetime_format`

## Code Structure Analysis

- Total lines: 97
- Blank lines: 14 (14.4%)
- Comment lines: ~15 (15.5%)
- Code lines: ~68


## Dependencies and Imports

- `from datetime import datetime`
- `from common.constants import FileSource`
- `from api.db.db_models import DB`
- `from api.db.db_models import File, File2Document`
- `from api.db.services.common_service import CommonService`
- `from api.db.services.document_service import DocumentService`
- `from common.time_utils import current_timestamp, datetime_format`

## Design & Architecture

This file is located in the `api` directory, specifically within `api/db/services`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `api/db/services/` directory
- Imports from `datetime`
- Imports from `common.constants`
- Imports from `api.db.db_models`
- Imports from `api.db.db_models`
- Imports from `api.db.services.common_service`
- Potential test file: `test_file2document_service.py`

## Keywords

ANY, All, Apache, Authors, BASIS, CONDITIONS, CommonService, Copyright, DB, Database, DocumentService, File, File2Document, File2DocumentService, FileSource, InfiniFlow, KIND, LICENSE, LOCAL, License, Licensed, None, Python, Reserved, Rights, RuntimeError, See, The, Unless, Version, WARRANTIES, WITHOUT, You, classmethod, delete_by_document_id, delete_by_document_ids_or_file_ids, delete_by_file_id, get_by_document_id, get_by_document_ids, get_by_file_id, get_storage_address, insert, update_by_file_id

---
*Generated by RAGFlow Repository Documentation Generator*
