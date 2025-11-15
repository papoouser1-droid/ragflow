# File Documentation: api/apps/file2document_app.py

## File Metadata

- **Path**: `api/apps/file2document_app.py`
- **Extension**: `.py`
- **Lines**: 134
- **Characters**: 5,687
- **Size**: 5,687 bytes
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
#  limitations under the License
#

from pathlib import Path

from api.db.services.file2document_service import File2DocumentService
from api.db.services.file_service import FileService

from flask import request
from flask_login import login_required, current_user
from api.db.services.knowledgebase_service import KnowledgebaseService
from api.utils.api_utils import server_error_response, get_data_error_result, validate_request
from common.misc_utils import get_uuid
from common.constants import RetCode
from api.db import FileType
from api.db.services.document_service import DocumentService
from api.utils.api_utils import get_json_result


@manager.route('/convert', methods=['POST'])  # noqa: F821
@login_required
@validate_request("file_ids", "kb_ids")
def convert():
    req = request.json
    kb_ids = req["kb_ids"]
    file_ids = req["file_ids"]
    file2documents = []

    try:
        files = FileService.get_by_ids(file_ids)
        files_set = dict({file.id: file for file in files})
        for file_id in file_ids:
            file = files_set[file_id]
            if not file:
                return get_data_error_result(message="File not found!")
            file_ids_list = [file_id]
            if file.type == FileType.FOLDER.value:
                file_ids_list = FileService.get_all_innermost_file_ids(file_id, [])
            for id in file_ids_list:
                informs = File2DocumentService.get_by_file_id(id)
                # delete
                for inform in informs:
                    doc_id = inform.document_id
                    e, doc = DocumentService.get_by_id(doc_id)
                    if not e:
                        return get_data_error_result(message="Document not found!")
                    tenant_id = DocumentService.get_tenant_id(doc_id)
                    if not tenant_id:
                        return get_data_error_result(message="Tenant not found!")
                    if not DocumentService.remove_document(doc, tenant_id):
                        return get_data_error_result(
                            message="Database error (Document removal)!")
                File2DocumentService.delete_by_file_id(id)

                # insert
                for kb_id in kb_ids:
                    e, kb = KnowledgebaseService.get_by_id(kb_id)
                    if not e:
                        return get_data_error_result(
                            message="Can't find this knowledgebase!")
                    e, file = FileService.get_by_id(id)
                    if not e:
                        return get_data_error_result(
                            message="Can't find this file!")

                    doc = DocumentService.insert({
                        "id": get_uuid(),
                        "kb_id": kb.id,
                        "parser_id": FileService.get_parser(file.type, file.name, kb.parser_id),
                        "parser_config": kb.parser_config,
                        "created_by": current_user.id,
                        "type": file.type,
                        "name": file.name,
                        "suffix": Path(file.name).suffix.lstrip("."),
                        "location": file.location,
                        "size": file.size
                    })
                    file2document = File2DocumentService.insert({
                        "id": get_uuid(),
                        "file_id": id,
                        "document_id": doc.id,
                    })

                    file2documents.append(file2document.to_json())
        return get_json_result(data=file2documents)
    except Exception as e:
        return server_error_response(e)


@manager.route('/rm', methods=['POST'])  # noqa: F821
@login_required
@validate_request("file_ids")
def rm():
    req = request.json
    file_ids = req["file_ids"]
    if not file_ids:
        return get_json_result(
            data=False, message='Lack of "Files ID"', code=RetCode.ARGUMENT_ERROR)
    try:
        for file_id in file_ids:
            informs = File2DocumentService.get_by_file_id(file_id)
            if not informs:
                return get_data_error_result(message="Inform not found!")
            for inform in informs:
                if not inform:
                    return get_data_error_result(message="Inform not found!")
                File2DocumentService.delete_by_file_id(file_id)
                doc_id = inform.document_id
                e, doc = DocumentService.get_by_id(doc_id)
                if not e:
                    return get_data_error_result(message="Document not found!")
                tenant_id = DocumentService.get_tenant_id(doc_id)
                if not tenant_id:
                    return get_data_error_result(message="Tenant not found!")
                if not DocumentService.remove_document(doc, tenant_id):
                    return get_data_error_result(
                        message="Database error (Document removal)!")
        return get_json_result(data=True)
    except Exception as e:
        return server_error_response(e)

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
#  limitations under the License
#

## Detailed Walkthrough


### Functions (2)

- `convert()`: Function definition
- `rm()`: Function definition

### Imports (12)

- `from pathlib import Path`
- `from api.db.services.file2document_service import File2DocumentService`
- `from api.db.services.file_service import FileService`
- `from flask import request`
- `from flask_login import login_required, current_user`
- `from api.db.services.knowledgebase_service import KnowledgebaseService`
- `from api.utils.api_utils import server_error_response, get_data_error_result, validate_request`
- `from common.misc_utils import get_uuid`
- `from common.constants import RetCode`
- `from api.db import FileType`

## Code Structure Analysis

- Total lines: 134
- Blank lines: 12 (9.0%)
- Comment lines: ~17 (12.7%)
- Code lines: ~105


## Dependencies and Imports

- `from pathlib import Path`
- `from api.db.services.file2document_service import File2DocumentService`
- `from api.db.services.file_service import FileService`
- `from flask import request`
- `from flask_login import login_required, current_user`
- `from api.db.services.knowledgebase_service import KnowledgebaseService`
- `from api.utils.api_utils import server_error_response, get_data_error_result, validate_request`
- `from common.misc_utils import get_uuid`
- `from common.constants import RetCode`
- `from api.db import FileType`
- `from api.db.services.document_service import DocumentService`
- `from api.utils.api_utils import get_json_result`

## Design & Architecture

This file is located in the `api` directory, specifically within `api/apps`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 8 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `api/apps/` directory
- Imports from `pathlib`
- Imports from `api.db.services.file2document_service`
- Imports from `api.db.services.file_service`
- Imports from `flask`
- Imports from `flask_login`
- Potential test file: `test_file2document_app.py`

## Keywords

ANY, ARGUMENT_ERROR, All, Apache, Authors, BASIS, CONDITIONS, Can, Copyright, Database, Document, DocumentService, Exception, F821, FOLDER, False, File, File2DocumentService, FileService, FileType, Files, InfiniFlow, Inform, KIND, KnowledgebaseService, LICENSE, Lack, License, Licensed, POST, Path, Python, Reserved, RetCode, Rights, See, Tenant, The, True, Unless, Version, WARRANTIES, WITHOUT, You, convert, login_required, manager, rm, validate_request

---
*Generated by RAGFlow Repository Documentation Generator*
