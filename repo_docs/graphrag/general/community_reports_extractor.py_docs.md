# File Documentation: graphrag/general/community_reports_extractor.py

## File Metadata

- **Path**: `graphrag/general/community_reports_extractor.py`
- **Extension**: `.py`
- **Lines**: 180
- **Characters**: 7,391
- **Size**: 7,391 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

## Original Source

```python
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

## High-Level Overview

# Copyright (c) 2024 Microsoft Corporation.
# Licensed under the MIT License
"""
"""

## Detailed Walkthrough

### Classes (2)

- `CommunityReportsResult`: Class definition
- `CommunityReportsExtractor`: Class definition

### Imports (19)

- `import logging`
- `import json`
- `import os`
- `import re`
- `from typing import Callable`
- `from dataclasses import dataclass`
- `import networkx as nx`
- `import pandas as pd`
- `from api.db.services.task_service import has_canceled`
- `from common.exceptions import TaskCanceledException`

## Code Structure Analysis

- Total lines: 180
- Blank lines: 21 (11.7%)
- Comment lines: ~7 (3.9%)
- Code lines: ~152


## Dependencies and Imports

- `import logging`
- `import json`
- `import os`
- `import re`
- `from typing import Callable`
- `from dataclasses import dataclass`
- `import networkx as nx`
- `import pandas as pd`
- `from api.db.services.task_service import has_canceled`
- `from common.exceptions import TaskCanceledException`
- `from common.connection_utils import timeout`
- `from graphrag.general import leiden`
- `from graphrag.general.community_report_prompt import COMMUNITY_REPORT_PROMPT`
- `from graphrag.general.extractor import Extractor`
- `from graphrag.general.leiden import add_community_info2graph`
- `from rag.llm.chat_model import Base as CompletionLLM`
- `from graphrag.utils import perform_variable_replacements, dict_has_keys_with_types, chat_limiter`
- `from common.token_utils import num_tokens_from_string`
- `import trio`

## Design & Architecture

This file is located in the `graphrag` directory, specifically within `graphrag/general`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 8 loop(s) - consider algorithmic complexity
- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `graphrag/general/` directory
- Imports from `dataclasses`
- Imports from `api.db.services.task_service`
- Imports from `common.exceptions`
- Imports from `common.connection_utils`
- Potential test file: `test_community_reports_extractor.py`

## Keywords

Base, COMMUNITY_REPORT_PROMPT, Callable, Communities, Community, CommunityReportsExtractor, CommunityReportsResult, CompletionLLM, Copyright, Corporation, DataFrame, ENABLE_TIMEOUT_ASSERTION, Exception, Extractor, Failed, Graph, Init, JSON, JSONDecodeError, LLM, Level, License, Licensed, MIT, Microsoft, None, Output, Python, Reference, Report, Response, Task, TaskCanceledException, __call__, __init__, _get_text_output, dataclass, definition, extract_community_report, finding_explanation, finding_summary, timeout

---
*Generated by RAGFlow Repository Documentation Generator*
