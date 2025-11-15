# File Documentation: api/db/__init__.py

## File Metadata

- **Path**: `api/db/__init__.py`
- **Extension**: `.py`
- **Lines**: 77
- **Characters**: 2,173
- **Size**: 2,173 bytes
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

from enum import IntEnum
from strenum import StrEnum


class UserTenantRole(StrEnum):
    OWNER = 'owner'
    ADMIN = 'admin'
    NORMAL = 'normal'
    INVITE = 'invite'


class TenantPermission(StrEnum):
    ME = 'me'
    TEAM = 'team'


class SerializedType(IntEnum):
    PICKLE = 1
    JSON = 2


class FileType(StrEnum):
    PDF = 'pdf'
    DOC = 'doc'
    VISUAL = 'visual'
    AURAL = 'aural'
    VIRTUAL = 'virtual'
    FOLDER = 'folder'
    OTHER = "other"

VALID_FILE_TYPES = {FileType.PDF, FileType.DOC, FileType.VISUAL, FileType.AURAL, FileType.VIRTUAL, FileType.FOLDER, FileType.OTHER}


class InputType(StrEnum):
    LOAD_STATE = "load_state"  # e.g. loading a current full state or a save state, such as from a file
    POLL = "poll"  # e.g. calling an API to get all documents in the last hour
    EVENT = "event"  # e.g. registered an endpoint as a listener, and processing connector events
    SLIM_RETRIEVAL = "slim_retrieval"


class CanvasCategory(StrEnum):
    Agent = "agent_canvas"
    DataFlow = "dataflow_canvas"


class PipelineTaskType(StrEnum):
    PARSE = "Parse"
    DOWNLOAD = "Download"
    RAPTOR = "RAPTOR"
    GRAPH_RAG = "GraphRAG"
    MINDMAP = "Mindmap"


VALID_PIPELINE_TASK_TYPES = {PipelineTaskType.PARSE, PipelineTaskType.DOWNLOAD, PipelineTaskType.RAPTOR, PipelineTaskType.GRAPH_RAG, PipelineTaskType.MINDMAP}


PIPELINE_SPECIAL_PROGRESS_FREEZE_TASK_TYPES = {PipelineTaskType.RAPTOR.lower(), PipelineTaskType.GRAPH_RAG.lower(), PipelineTaskType.MINDMAP.lower()}


KNOWLEDGEBASE_FOLDER_NAME=".knowledgebase"

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

### Classes (7)

- `UserTenantRole`: Class definition
- `TenantPermission`: Class definition
- `SerializedType`: Class definition
- `FileType`: Class definition
- `InputType`: Class definition
- `CanvasCategory`: Class definition
- `PipelineTaskType`: Class definition

### Imports (2)

- `from enum import IntEnum`
- `from strenum import StrEnum`

## Code Structure Analysis

- Total lines: 77
- Blank lines: 23 (29.9%)
- Comment lines: ~15 (19.5%)
- Code lines: ~39


## Dependencies and Imports

- `from enum import IntEnum`
- `from strenum import StrEnum`

## Design & Architecture

This file is located in the `api` directory, specifically within `api/db`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity

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

- Other files in `api/db/` directory
- Imports from `enum`
- Imports from `strenum`
- Potential test file: `test___init__.py`

## Keywords

ADMIN, ANY, API, AURAL, Agent, All, Apache, Authors, BASIS, CONDITIONS, CanvasCategory, Copyright, DOC, DOWNLOAD, DataFlow, Download, EVENT, FOLDER, FileType, GRAPH_RAG, GraphRAG, INVITE, InfiniFlow, InputType, IntEnum, JSON, KIND, KNOWLEDGEBASE_FOLDER_NAME, LICENSE, LOAD_STATE, License, Licensed, MINDMAP, Mindmap, NORMAL, OTHER, OWNER, PARSE, PDF, PICKLE, PIPELINE_SPECIAL_PROGRESS_FREEZE_TASK_TYPES, POLL, Parse, PipelineTaskType, Python, RAPTOR, Reserved, Rights, SLIM_RETRIEVAL, See...

---
*Generated by RAGFlow Repository Documentation Generator*
