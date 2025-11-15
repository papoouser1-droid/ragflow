# Documentation: graphrag/general/community_reports_extractor.py

## File Metadata

- **Path**: `graphrag/general/community_reports_extractor.py`
- **Size**: 7391 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `graphrag/general/community_reports_extractor.py`.

## Python Module Overview

### Module Docstring

```
Reference:
 - [graphrag](https://github.com/microsoft/graphrag)
```

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `json`
- `os`
- `re`
- `typing`
- `dataclasses`
- `networkx`
- `pandas`
- `api.db.services.task_service`
- `common.exceptions`
- `common.connection_utils`
- `graphrag.general`
- `graphrag.general.community_report_prompt`
- `graphrag.general.extractor`
- `graphrag.general.leiden`
- `rag.llm.chat_model`
- `graphrag.utils`
- `common.token_utils`
- `trio`

### Classes Defined

This file defines 2 class(es):

#### Class: `CommunityReportsResult` (line 31)

**Docstring**: Community reports result class definition....

#### Class: `CommunityReportsExtractor` (line 38)

**Docstring**: Community reports extractor class definition....

**Methods**: __init__, _get_text_output

### Functions Defined

This file defines 4 function(s):

#### Function: `__init__` (line 45)

**Parameters**: self, llm_invoker, max_report_length

#### Function: `_get_text_output` (line 161)

**Parameters**: self, parsed_output

#### Function: `finding_summary` (line 166)

**Parameters**: finding

#### Function: `finding_explanation` (line 171)

**Parameters**: finding

## Original Source Code

```py
# Copyright (c) 2024 Microsoft Corporation.
# Licensed under the MIT License
"""
Reference:
 - [graphrag](https://github.com/microsoft/graphrag)
"""

import logging
import json
import os
import re
from typing import Callable
from dataclasses import dataclass
import networkx as nx
import pandas as pd

from api.db.services.task_service import has_canceled
from common.exceptions import TaskCanceledException
from common.connection_utils import timeout
from graphrag.general import leiden
from graphrag.general.community_report_prompt import COMMUNITY_REPORT_PROMPT
from graphrag.general.extractor import Extractor
from graphrag.general.leiden import add_community_info2graph
from rag.llm.chat_model import Base as CompletionLLM
from graphrag.utils import perform_variable_replacements, dict_has_keys_with_types, chat_limiter
from common.token_utils import num_tokens_from_string
import trio


@dataclass
class CommunityReportsResult:
    """Community reports result class definition."""

    output: list[str]
    structured_output: list[dict]


class CommunityReportsExtractor(Extractor):
    """Community reports extractor class definition."""

    _extraction_prompt: str
    _output_formatter_prompt: str
    _max_report_length: int

    def __init__(
            self,
            llm_invoker: CompletionLLM,
            max_report_length: int | None = None,
    ):
        super().__init__(llm_invoker)
        """Init method definition."""
        self._llm = llm_invoker
        self._extraction_prompt = COMMUNITY_REPORT_PROMPT
        self._max_report_length = max_report_length or 1500

    async def __call__(self, graph: nx.Graph, callback: Callable | None = None, task_id: str = ""):
        enable_timeout_assertion = os.environ.get("ENABLE_TIMEOUT_ASSERTION")
        for node_degree in graph.degree:
            graph.nodes[str(node_degree[0])]["rank"] = int(node_degree[1])

        communities: dict[str, dict[str, list]] = leiden.run(graph, {})
        total = sum([len(comm.items()) for _, comm in communities.items()])
        res_str = []
        res_dict = []
        over, token_count = 0, 0
        @timeout(120)
        async def extract_community_report(community):
            nonlocal res_str, res_dict, over, token_count
            if task_id:
                if has_canceled(task_id):
                    logging.info(f"Task {task_id} cancelled during community report extraction.")
                    raise TaskCanceledException(f"Task {task_id} was cancelled")

            cm_id, cm = community
            weight = cm["weight"]
            ents = cm["nodes"]
            if len(ents) < 2:
                return
            ent_list = [{"entity": ent, "description": graph.nodes[ent]["description"]} for ent in ents]
            ent_df = pd.DataFrame(ent_list)

            rela_list = []
            k = 0
            for i in range(0, len(ents)):
                if k >= 10000:
                    break
                for j in range(i + 1, len(ents)):
                    if k >= 10000:
                        break
                    edge = graph.get_edge_data(ents[i], ents[j])
                    if edge is None:
                        continue
                    rela_list.append({"source": ents[i], "target": ents[j], "description": edge["description"]})
                    k += 1
            rela_df = pd.DataFrame(rela_list)

            prompt_variables = {
                "entity_df": ent_df.to_csv(index_label="id"),
                "relation_df": rela_df.to_csv(index_label="id")
            }
            text = perform_variable_replacements(self._extraction_prompt, variables=prompt_variables)
            async with chat_limiter:
                try:
                    with trio.move_on_after(180 if enable_timeout_assertion else 1000000000) as cancel_scope:
                        if task_id and has_canceled(task_id):
                            logging.info(f"Task {task_id} cancelled before LLM call.")
                            raise TaskCanceledException(f"Task {task_id} was cancelled")
                        response = await trio.to_thread.run_sync( self._chat, text, [{"role": "user", "content": "Output:"}], {}, task_id)
                    if cancel_scope.cancelled_caught:
                        logging.warning("extract_community_report._chat timeout, skipping...")
                        return
                except Exception as e:
                    logging.error(f"extract_community_report._chat failed: {e}")
                    return
            token_count += num_tokens_from_string(text + response)
            response = re.sub(r"^[^\{]*", "", response)
            response = re.sub(r"[^\}]*$", "", response)
            response = re.sub(r"\{\{", "{", response)
            response = re.sub(r"\}\}", "}", response)
            logging.debug(response)
            try:
                response = json.loads(response)
            except json.JSONDecodeError as e:
                logging.error(f"Failed to parse JSON response: {e}")
                logging.error(f"Response content: {response}")
                return
            if not dict_has_keys_with_types(response, [
                        ("title", str),
                        ("summary", str),
                        ("findings", list),
                        ("rating", float),
                        ("rating_explanation", str),
                    ]):
                return
            response["weight"] = weight
            response["entities"] = ents
            add_community_info2graph(graph, ents, response["title"])
            res_str.append(self._get_text_output(response))
            res_dict.append(response)
            over += 1
            if callback:
                callback(msg=f"Communities: {over}/{total}, used tokens: {token_count}")

        st = trio.current_time()
        async with trio.open_nursery() as nursery:
            for level, comm in communities.items():
                logging.info(f"Level {level}: Community: {len(comm.keys())}")
                for community in comm.items():
                    if task_id and has_canceled(task_id):
                        logging.info(f"Task {task_id} cancelled before community processing.")
                        raise TaskCanceledException(f"Task {task_id} was cancelled")
                    nursery.start_soon(extract_community_report, community)
        if callback:
            callback(msg=f"Community reports done in {trio.current_time() - st:.2f}s, used tokens: {token_count}")

        return CommunityReportsResult(
            structured_output=res_dict,
            output=res_str,
        )

    def _get_text_output(self, parsed_output: dict) -> str:
        title = parsed_output.get("title", "Report")
        summary = parsed_output.get("summary", "")
        findings = parsed_output.get("findings", [])

        def finding_summary(finding: dict):
            if isinstance(finding, str):
                return finding
            return finding.get("summary")

        def finding_explanation(finding: dict):
            if isinstance(finding, str):
                return ""
            return finding.get("explanation")

        report_sections = "\n\n".join(
            f"## {finding_summary(f)}\n\n{finding_explanation(f)}" for f in findings
        )
        return f"# {title}\n\n{summary}\n\n{report_sections}"

```

## Detailed Analysis

### File Role in Repository

The file `graphrag/general/community_reports_extractor.py` is located in the `graphrag/general` directory.

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
- [entity_embedding.py](entity_embedding.py_docs.md)
- [extractor.py](extractor.py_docs.md)
- [graph_extractor.py](graph_extractor.py_docs.md)
- [graph_prompt.py](graph_prompt.py_docs.md)
- [index.py](index.py_docs.md)
- [leiden.py](leiden.py_docs.md)
- [mind_map_extractor.py](mind_map_extractor.py_docs.md)
- [mind_map_prompt.py](mind_map_prompt.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
