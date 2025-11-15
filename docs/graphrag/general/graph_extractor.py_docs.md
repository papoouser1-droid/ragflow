# Documentation: graphrag/general/graph_extractor.py

## File Metadata

- **Path**: `graphrag/general/graph_extractor.py`
- **Size**: 6331 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `graphrag/general/graph_extractor.py`.

## Python Module Overview

### Module Docstring

```
Reference:
 - [graphrag](https://github.com/microsoft/graphrag)
```

### Imports and Dependencies

This module imports the following dependencies:

- `re`
- `typing`
- `dataclasses`
- `tiktoken`
- `trio`
- `graphrag.general.extractor`
- `graphrag.general.graph_prompt`
- `graphrag.utils`
- `rag.llm.chat_model`
- `networkx`
- `common.token_utils`

### Classes Defined

This file defines 2 class(es):

#### Class: `GraphExtractionResult` (line 27)

**Docstring**: Unipartite graph extraction result class definition....

#### Class: `GraphExtractor` (line 34)

**Docstring**: Unipartite graph extractor class definition....

**Methods**: __init__

### Functions Defined

This file defines 1 function(s):

#### Function: `__init__` (line 51)

**Parameters**: self, llm_invoker, language, entity_types, tuple_delimiter_key, record_delimiter_key, input_text_key, entity_types_key, completion_delimiter_key, join_descriptions, max_gleanings, on_error

## Original Source Code

```py
# Copyright (c) 2024 Microsoft Corporation.
# Licensed under the MIT License
"""
Reference:
 - [graphrag](https://github.com/microsoft/graphrag)
"""

import re
from typing import Any
from dataclasses import dataclass
import tiktoken
import trio

from graphrag.general.extractor import Extractor, ENTITY_EXTRACTION_MAX_GLEANINGS
from graphrag.general.graph_prompt import GRAPH_EXTRACTION_PROMPT, CONTINUE_PROMPT, LOOP_PROMPT
from graphrag.utils import ErrorHandlerFn, perform_variable_replacements, chat_limiter, split_string_by_multi_markers
from rag.llm.chat_model import Base as CompletionLLM
import networkx as nx
from common.token_utils import num_tokens_from_string

DEFAULT_TUPLE_DELIMITER = "<|>"
DEFAULT_RECORD_DELIMITER = "##"
DEFAULT_COMPLETION_DELIMITER = "<|COMPLETE|>"


@dataclass
class GraphExtractionResult:
    """Unipartite graph extraction result class definition."""

    output: nx.Graph
    source_docs: dict[Any, Any]


class GraphExtractor(Extractor):
    """Unipartite graph extractor class definition."""

    _join_descriptions: bool
    _tuple_delimiter_key: str
    _record_delimiter_key: str
    _entity_types_key: str
    _input_text_key: str
    _completion_delimiter_key: str
    _entity_name_key: str
    _input_descriptions_key: str
    _extraction_prompt: str
    _summarization_prompt: str
    _loop_args: dict[str, Any]
    _max_gleanings: int
    _on_error: ErrorHandlerFn

    def __init__(
        self,
        llm_invoker: CompletionLLM,
        language: str | None = "English",
        entity_types: list[str] | None = None,
        tuple_delimiter_key: str | None = None,
        record_delimiter_key: str | None = None,
        input_text_key: str | None = None,
        entity_types_key: str | None = None,
        completion_delimiter_key: str | None = None,
        join_descriptions=True,
        max_gleanings: int | None = None,
        on_error: ErrorHandlerFn | None = None,
    ):
        super().__init__(llm_invoker, language, entity_types)
        """Init method definition."""
        # TODO: streamline construction
        self._llm = llm_invoker
        self._join_descriptions = join_descriptions
        self._input_text_key = input_text_key or "input_text"
        self._tuple_delimiter_key = tuple_delimiter_key or "tuple_delimiter"
        self._record_delimiter_key = record_delimiter_key or "record_delimiter"
        self._completion_delimiter_key = (
            completion_delimiter_key or "completion_delimiter"
        )
        self._entity_types_key = entity_types_key or "entity_types"
        self._extraction_prompt = GRAPH_EXTRACTION_PROMPT
        self._max_gleanings = (
            max_gleanings
            if max_gleanings is not None
            else ENTITY_EXTRACTION_MAX_GLEANINGS
        )
        self._on_error = on_error or (lambda _e, _s, _d: None)
        self.prompt_token_count = num_tokens_from_string(self._extraction_prompt)

        # Construct the looping arguments
        encoding = tiktoken.get_encoding("cl100k_base")
        yes = encoding.encode("YES")
        no = encoding.encode("NO")
        self._loop_args = {"logit_bias": {yes[0]: 100, no[0]: 100}, "max_tokens": 1}

        # Wire defaults into the prompt variables
        self._prompt_variables = {
            self._tuple_delimiter_key: DEFAULT_TUPLE_DELIMITER,
            self._record_delimiter_key: DEFAULT_RECORD_DELIMITER,
            self._completion_delimiter_key: DEFAULT_COMPLETION_DELIMITER,
            self._entity_types_key: ",".join(entity_types),
        }

    async def _process_single_content(self, chunk_key_dp: tuple[str, str], chunk_seq: int, num_chunks: int, out_results, task_id=""):
        token_count = 0
        chunk_key = chunk_key_dp[0]
        content = chunk_key_dp[1]
        variables = {
            **self._prompt_variables,
            self._input_text_key: content,
        }
        hint_prompt = perform_variable_replacements(self._extraction_prompt, variables=variables)
        async with chat_limiter:
            response = await trio.to_thread.run_sync(self._chat, hint_prompt, [{"role": "user", "content": "Output:"}], {}, task_id)
        token_count += num_tokens_from_string(hint_prompt + response)

        results = response or ""
        history = [{"role": "system", "content": hint_prompt}, {"role": "user", "content": response}]

        # Repeat to ensure we maximize entity count
        for i in range(self._max_gleanings):
            history.append({"role": "user", "content": CONTINUE_PROMPT})
            async with chat_limiter:
                response = await trio.to_thread.run_sync(lambda: self._chat("", history, {}))
            token_count += num_tokens_from_string("\n".join([m["content"] for m in history]) + response)
            results += response or ""

            # if this is the final glean, don't bother updating the continuation flag
            if i >= self._max_gleanings - 1:
                break
            history.append({"role": "assistant", "content": response})
            history.append({"role": "user", "content": LOOP_PROMPT})
            async with chat_limiter:
                continuation = await trio.to_thread.run_sync(lambda: self._chat("", history))
            token_count += num_tokens_from_string("\n".join([m["content"] for m in history]) + response)
            if continuation != "Y":
                break
            history.append({"role": "assistant", "content": "Y"})

        records = split_string_by_multi_markers(
            results,
            [self._prompt_variables[self._record_delimiter_key], self._prompt_variables[self._completion_delimiter_key]],
        )
        rcds = []
        for record in records:
            record = re.search(r"\((.*)\)", record)
            if record is None:
                continue
            rcds.append(record.group(1))
        records = rcds
        maybe_nodes, maybe_edges = self._entities_and_relations(chunk_key, records, self._prompt_variables[self._tuple_delimiter_key])
        out_results.append((maybe_nodes, maybe_edges, token_count))
        if self.callback:
            self.callback(0.5+0.1*len(out_results)/num_chunks, msg = f"Entities extraction of chunk {chunk_seq} {len(out_results)}/{num_chunks} done, {len(maybe_nodes)} nodes, {len(maybe_edges)} edges, {token_count} tokens.")

```

## Detailed Analysis

### File Role in Repository

The file `graphrag/general/graph_extractor.py` is located in the `graphrag/general` directory.

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
