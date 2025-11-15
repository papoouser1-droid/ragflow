# Documentation: graphrag/general/smoke.py

## File Metadata

- **Path**: `graphrag/general/smoke.py`
- **Size**: 3176 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `graphrag/general/smoke.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `argparse`
- `json`
- `logging`
- `networkx`
- `trio`
- `common.constants`
- `api.db.services.document_service`
- `api.db.services.knowledgebase_service`
- `api.db.services.llm_service`
- `api.db.services.user_service`
- `graphrag.general.graph_extractor`
- `graphrag.general.index`
- `common`

### Functions Defined

This file defines 1 function(s):

#### Function: `callback` (line 35)

**Parameters**: prog, msg

## Original Source Code

```py
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

## Detailed Analysis

### File Role in Repository

The file `graphrag/general/smoke.py` is located in the `graphrag/general` directory.

This file is part of the **Graph RAG** knowledge graph system.

### Architecture Context

Files in this location typically handle concerns related to general.

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
- [community_report_prompt.py](community_report_prompt.py_docs.md)
- [community_reports_extractor.py](community_reports_extractor.py_docs.md)
- [entity_embedding.py](entity_embedding.py_docs.md)
- [extractor.py](extractor.py_docs.md)
- [graph_extractor.py](graph_extractor.py_docs.md)
- [graph_prompt.py](graph_prompt.py_docs.md)
- [index.py](index.py_docs.md)
- [leiden.py](leiden.py_docs.md)
- [mind_map_extractor.py](mind_map_extractor.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
