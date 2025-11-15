# File Documentation: graphrag/general/smoke.py

## File Metadata

- **Path**: `graphrag/general/smoke.py`
- **Extension**: `.py`
- **Lines**: 111
- **Characters**: 3,176
- **Size**: 3,176 bytes
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

import argparse
import json
import logging
import networkx as nx
import trio

from common.constants import LLMType
from api.db.services.document_service import DocumentService
from api.db.services.knowledgebase_service import KnowledgebaseService
from api.db.services.llm_service import LLMBundle
from api.db.services.user_service import TenantService
from graphrag.general.graph_extractor import GraphExtractor
from graphrag.general.index import update_graph, with_resolution, with_community
from common import settings

settings.init_settings()


def callback(prog=None, msg="Processing..."):
    logging.info(msg)


async def main():
    parser = argparse.ArgumentParser()
    parser.add_argument(
        "-t",
        "--tenant_id",
        default=False,
        help="Tenant ID",
        action="store",
        required=True,
    )
    parser.add_argument(
        "-d",
        "--doc_id",
        default=False,
        help="Document ID",
        action="store",
        required=True,
    )
    args = parser.parse_args()
    e, doc = DocumentService.get_by_id(args.doc_id)
    if not e:
        raise LookupError("Document not found.")
    kb_id = doc.kb_id

    chunks = [
        d["content_with_weight"]
        for d in settings.retriever.chunk_list(
            args.doc_id,
            args.tenant_id,
            [kb_id],
            max_count=6,
            fields=["content_with_weight"],
        )
    ]

    _, tenant = TenantService.get_by_id(args.tenant_id)
    llm_bdl = LLMBundle(args.tenant_id, LLMType.CHAT, tenant.llm_id)
    _, kb = KnowledgebaseService.get_by_id(kb_id)
    embed_bdl = LLMBundle(args.tenant_id, LLMType.EMBEDDING, kb.embd_id)

    graph, doc_ids = await update_graph(
        GraphExtractor,
        args.tenant_id,
        kb_id,
        args.doc_id,
        chunks,
        "English",
        llm_bdl,
        embed_bdl,
        callback,
    )
    print(json.dumps(nx.node_link_data(graph), ensure_ascii=False, indent=2))

    await with_resolution(
        args.tenant_id, kb_id, args.doc_id, llm_bdl, embed_bdl, callback
    )
    community_structure, community_reports = await with_community(
        args.tenant_id, kb_id, args.doc_id, llm_bdl, embed_bdl, callback
    )

    print(
        "------------------ COMMUNITY STRUCTURE--------------------\n",
        json.dumps(community_structure, ensure_ascii=False, indent=2),
    )
    print(
        "------------------ COMMUNITY REPORTS----------------------\n",
        community_reports,
    )


if __name__ == "__main__":
    trio.run(main)

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


### Functions (1)

- `callback()`: Function definition

### Imports (13)

- `import argparse`
- `import json`
- `import logging`
- `import networkx as nx`
- `import trio`
- `from common.constants import LLMType`
- `from api.db.services.document_service import DocumentService`
- `from api.db.services.knowledgebase_service import KnowledgebaseService`
- `from api.db.services.llm_service import LLMBundle`
- `from api.db.services.user_service import TenantService`

## Code Structure Analysis

- Total lines: 111
- Blank lines: 15 (13.5%)
- Comment lines: ~15 (13.5%)
- Code lines: ~81


## Dependencies and Imports

- `import argparse`
- `import json`
- `import logging`
- `import networkx as nx`
- `import trio`
- `from common.constants import LLMType`
- `from api.db.services.document_service import DocumentService`
- `from api.db.services.knowledgebase_service import KnowledgebaseService`
- `from api.db.services.llm_service import LLMBundle`
- `from api.db.services.user_service import TenantService`
- `from graphrag.general.graph_extractor import GraphExtractor`
- `from graphrag.general.index import update_graph, with_resolution, with_community`
- `from common import settings`

## Design & Architecture

This file is located in the `graphrag` directory, specifically within `graphrag/general`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 2 loop(s) - consider algorithmic complexity
- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `graphrag/general/` directory
- Imports from `common.constants`
- Imports from `api.db.services.document_service`
- Imports from `api.db.services.knowledgebase_service`
- Imports from `api.db.services.llm_service`
- Imports from `api.db.services.user_service`
- Potential test file: `test_smoke.py`

## Keywords

ANY, All, Apache, ArgumentParser, Authors, BASIS, CHAT, COMMUNITY, CONDITIONS, Copyright, Document, DocumentService, EMBEDDING, English, False, GraphExtractor, InfiniFlow, KIND, KnowledgebaseService, LICENSE, LLMBundle, LLMType, License, Licensed, LookupError, None, Processing, Python, REPORTS, Reserved, Rights, STRUCTURE, See, Tenant, TenantService, The, True, Unless, Version, WARRANTIES, WITHOUT, You, callback, main

---
*Generated by RAGFlow Repository Documentation Generator*
