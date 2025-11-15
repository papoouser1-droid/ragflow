# Documentation: graphrag/light/graph_extractor.py

## File Metadata

- **Path**: `graphrag/light/graph_extractor.py`
- **Size**: 6185 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `graphrag/light/graph_extractor.py`.

## Python Module Overview

### Module Docstring

```
Reference:
 - [graphrag](https://github.com/microsoft/graphrag)
```

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `re`
- `dataclasses`
- `typing`
- `networkx`
- `trio`
- `graphrag.general.extractor`
- `graphrag.light.graph_prompt`
- `graphrag.utils`
- `rag.llm.chat_model`
- `common.token_utils`

### Classes Defined

This file defines 2 class(es):

#### Class: `GraphExtractionResult` (line 24)

**Docstring**: Unipartite graph extraction result class definition....

#### Class: `GraphExtractor` (line 31)

**Methods**: __init__

### Functions Defined

This file defines 1 function(s):

#### Function: `__init__` (line 34)

**Parameters**: self, llm_invoker, language, entity_types, example_number, max_gleanings

## Original Source Code

```py
# Copyright (c) 2024 Microsoft Corporation.
# Licensed under the MIT License
"""
Reference:
 - [graphrag](https://github.com/microsoft/graphrag)
"""

import logging
import re
from dataclasses import dataclass
from typing import Any

import networkx as nx
import trio

from graphrag.general.extractor import ENTITY_EXTRACTION_MAX_GLEANINGS, Extractor
from graphrag.light.graph_prompt import PROMPTS
from graphrag.utils import chat_limiter, pack_user_ass_to_openai_messages, split_string_by_multi_markers
from rag.llm.chat_model import Base as CompletionLLM
from common.token_utils import num_tokens_from_string


@dataclass
class GraphExtractionResult:
    """Unipartite graph extraction result class definition."""

    output: nx.Graph
    source_docs: dict[Any, Any]


class GraphExtractor(Extractor):
    _max_gleanings: int

    def __init__(
        self,
        llm_invoker: CompletionLLM,
        language: str | None = "English",
        entity_types: list[str] | None = None,
        example_number: int = 2,
        max_gleanings: int | None = None,
    ):
        super().__init__(llm_invoker, language, entity_types)
        """Init method definition."""
        self._max_gleanings = max_gleanings if max_gleanings is not None else ENTITY_EXTRACTION_MAX_GLEANINGS
        self._example_number = example_number
        examples = "\n".join(PROMPTS["entity_extraction_examples"][: int(self._example_number)])

        example_context_base = dict(
            tuple_delimiter=PROMPTS["DEFAULT_TUPLE_DELIMITER"],
            record_delimiter=PROMPTS["DEFAULT_RECORD_DELIMITER"],
            completion_delimiter=PROMPTS["DEFAULT_COMPLETION_DELIMITER"],
            entity_types=",".join(self._entity_types),
            language=self._language,
        )
        # add example's format
        examples = examples.format(**example_context_base)

        self._entity_extract_prompt = PROMPTS["entity_extraction"]
        self._context_base = dict(
            tuple_delimiter=PROMPTS["DEFAULT_TUPLE_DELIMITER"],
            record_delimiter=PROMPTS["DEFAULT_RECORD_DELIMITER"],
            completion_delimiter=PROMPTS["DEFAULT_COMPLETION_DELIMITER"],
            entity_types=",".join(self._entity_types),
            examples=examples,
            language=self._language,
        )

        self._continue_prompt = PROMPTS["entity_continue_extraction"].format(**self._context_base)
        self._if_loop_prompt = PROMPTS["entity_if_loop_extraction"]

        self._left_token_count = llm_invoker.max_length - num_tokens_from_string(self._entity_extract_prompt.format(**self._context_base, input_text=""))
        self._left_token_count = max(llm_invoker.max_length * 0.6, self._left_token_count)

    async def _process_single_content(self, chunk_key_dp: tuple[str, str], chunk_seq: int, num_chunks: int, out_results, task_id=""):
        token_count = 0
        chunk_key = chunk_key_dp[0]
        content = chunk_key_dp[1]
        hint_prompt = self._entity_extract_prompt.format(**self._context_base, input_text=content)

        gen_conf = {}
        final_result = ""
        glean_result = ""
        if_loop_result = ""
        history = []
        logging.info(f"Start processing for {chunk_key}: {content[:25]}...")
        if self.callback:
            self.callback(msg=f"Start processing for {chunk_key}: {content[:25]}...")
        async with chat_limiter:
            final_result = await trio.to_thread.run_sync(self._chat, "", [{"role": "user", "content": hint_prompt}], gen_conf, task_id)
        token_count += num_tokens_from_string(hint_prompt + final_result)
        history = pack_user_ass_to_openai_messages(hint_prompt, final_result, self._continue_prompt)
        for now_glean_index in range(self._max_gleanings):
            async with chat_limiter:
                # glean_result = await trio.to_thread.run_sync(lambda: self._chat(hint_prompt, history, gen_conf))
                glean_result = await trio.to_thread.run_sync(self._chat, "", history, gen_conf, task_id)
            history.extend([{"role": "assistant", "content": glean_result}])
            token_count += num_tokens_from_string("\n".join([m["content"] for m in history]) + hint_prompt + self._continue_prompt)
            final_result += glean_result
            if now_glean_index == self._max_gleanings - 1:
                break

            history.extend([{"role": "user", "content": self._if_loop_prompt}])
            async with chat_limiter:
                if_loop_result = await trio.to_thread.run_sync(self._chat, "", history, gen_conf, task_id)
            token_count += num_tokens_from_string("\n".join([m["content"] for m in history]) + if_loop_result + self._if_loop_prompt)
            if_loop_result = if_loop_result.strip().strip('"').strip("'").lower()
            if if_loop_result != "yes":
                break
            history.extend([{"role": "assistant", "content": if_loop_result}, {"role": "user", "content": self._continue_prompt}])

        logging.info(f"Completed processing for {chunk_key}: {content[:25]}... after {now_glean_index} gleanings, {token_count} tokens.")
        if self.callback:
            self.callback(msg=f"Completed processing for {chunk_key}: {content[:25]}... after {now_glean_index} gleanings, {token_count} tokens.")
        records = split_string_by_multi_markers(
            final_result,
            [self._context_base["record_delimiter"], self._context_base["completion_delimiter"]],
        )
        rcds = []
        for record in records:
            record = re.search(r"\((.*)\)", record)
            if record is None:
                continue
            rcds.append(record.group(1))
        records = rcds
        maybe_nodes, maybe_edges = self._entities_and_relations(chunk_key, records, self._context_base["tuple_delimiter"])
        out_results.append((maybe_nodes, maybe_edges, token_count))
        if self.callback:
            self.callback(
                0.5 + 0.1 * len(out_results) / num_chunks,
                msg=f"Entities extraction of chunk {chunk_seq} {len(out_results)}/{num_chunks} done, {len(maybe_nodes)} nodes, {len(maybe_edges)} edges, {token_count} tokens.",
            )

```

## Detailed Analysis

### File Role in Repository

The file `graphrag/light/graph_extractor.py` is located in the `graphrag/light` directory.

This file is part of the **Graph RAG** knowledge graph system.

### Architecture Context

Files in this location typically handle concerns related to light.

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
- [graph_prompt.py](graph_prompt.py_docs.md)
- [smoke.py](smoke.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
