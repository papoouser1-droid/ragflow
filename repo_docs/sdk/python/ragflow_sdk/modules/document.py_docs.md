# File Documentation: sdk/python/ragflow_sdk/modules/document.py

## File Metadata

- **Path**: `sdk/python/ragflow_sdk/modules/document.py`
- **Extension**: `.py`
- **Lines**: 102
- **Characters**: 3,780
- **Size**: 3,780 bytes
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

import json

from .base import Base
from .chunk import Chunk


class Document(Base):
    class ParserConfig(Base):
        def __init__(self, rag, res_dict):
            super().__init__(rag, res_dict)

    def __init__(self, rag, res_dict):
        self.id = ""
        self.name = ""
        self.thumbnail = None
        self.dataset_id = None
        self.chunk_method = "naive"
        self.parser_config = {"pages": [[1, 1000000]]}
        self.source_type = "local"
        self.type = ""
        self.created_by = ""
        self.size = 0
        self.token_count = 0
        self.chunk_count = 0
        self.progress = 0.0
        self.progress_msg = ""
        self.process_begin_at = None
        self.process_duration = 0.0
        self.run = "0"
        self.status = "1"
        self.meta_fields = {}
        for k in list(res_dict.keys()):
            if k not in self.__dict__:
                res_dict.pop(k)
        super().__init__(rag, res_dict)

    def update(self, update_message: dict):
        if "meta_fields" in update_message:
            if not isinstance(update_message["meta_fields"], dict):
                raise Exception("meta_fields must be a dictionary")
        res = self.put(f"/datasets/{self.dataset_id}/documents/{self.id}", update_message)
        res = res.json()
        if res.get("code") != 0:
            raise Exception(res["message"])

        self._update_from_dict(self.rag, res.get("data", {}))
        return self

    def download(self):
        res = self.get(f"/datasets/{self.dataset_id}/documents/{self.id}")
        error_keys = set(["code", "message"])
        try:
            response = res.json()
            actual_keys = set(response.keys())
            if actual_keys == error_keys:
                raise Exception(res.get("message"))
            else:
                return res.content
        except json.JSONDecodeError:
            return res.content

    def list_chunks(self, page=1, page_size=30, keywords="", id=""):
        data = {"keywords": keywords, "page": page, "page_size": page_size, "id": id}
        res = self.get(f"/datasets/{self.dataset_id}/documents/{self.id}/chunks", data)
        res = res.json()
        if res.get("code") == 0:
            chunks = []
            for data in res["data"].get("chunks"):
                chunk = Chunk(self.rag, data)
                chunks.append(chunk)
            return chunks
        raise Exception(res.get("message"))

    def add_chunk(self, content: str, important_keywords: list[str] = [], questions: list[str] = []):
        res = self.post(f"/datasets/{self.dataset_id}/documents/{self.id}/chunks", {"content": content, "important_keywords": important_keywords, "questions": questions})
        res = res.json()
        if res.get("code") == 0:
            return Chunk(self.rag, res["data"].get("chunk"))
        raise Exception(res.get("message"))

    def delete_chunks(self, ids: list[str] | None = None):
        res = self.rm(f"/datasets/{self.dataset_id}/documents/{self.id}/chunks", {"chunk_ids": ids})
        res = res.json()
        if res.get("code") != 0:
            raise Exception(res.get("message"))

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

- `Document`: Class definition

### Imports (3)

- `import json`
- `from .base import Base`
- `from .chunk import Chunk`

## Code Structure Analysis

- Total lines: 102
- Blank lines: 12 (11.8%)
- Comment lines: ~15 (14.7%)
- Code lines: ~75


## Dependencies and Imports

- `import json`
- `from .base import Base`
- `from .chunk import Chunk`

## Design & Architecture

This file is located in the `sdk` directory, specifically within `sdk/python/ragflow_sdk/modules`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 3 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `sdk/python/ragflow_sdk/modules/` directory
- Imports from `.base`
- Imports from `.chunk`
- Potential test file: `test_document.py`

## Keywords

ANY, All, Apache, Authors, BASIS, Base, CONDITIONS, Chunk, Copyright, Document, Exception, InfiniFlow, JSONDecodeError, KIND, LICENSE, License, Licensed, None, ParserConfig, Python, Reserved, Rights, See, The, Unless, Version, WARRANTIES, WITHOUT, You, __init__, add_chunk, delete_chunks, download, list_chunks, update

---
*Generated by RAGFlow Repository Documentation Generator*
