# Documentation: deepdoc/parser/json_parser.py

## File Metadata

- **Path**: `deepdoc/parser/json_parser.py`
- **Size**: 6388 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `deepdoc/parser/json_parser.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `json`
- `typing`
- `rag.nlp`

### Classes Defined

This file defines 1 class(es):

#### Class: `RAGFlowJsonParser` (line 27)

**Methods**: __init__, __call__, _json_size, _set_nested_dict, _list_to_dict_preprocessing, _json_split, split_json, split_text, _parse_json, _parse_jsonl, is_jsonl_format, _is_valid_json

### Functions Defined

This file defines 12 function(s):

#### Function: `__init__` (line 28)

**Parameters**: self, max_chunk_size, min_chunk_size

#### Function: `__call__` (line 33)

**Parameters**: self, binary

#### Function: `_json_size` (line 44)

**Parameters**: data

**Docstring**: Calculate the size of the serialized JSON object....

#### Function: `_set_nested_dict` (line 49)

**Parameters**: d, path, value

**Docstring**: Set a value in a nested dictionary based on the given path....

#### Function: `_list_to_dict_preprocessing` (line 55)

**Parameters**: self, data

#### Function: `_json_split` (line 66)

**Parameters**: self, data, current_path, chunks

**Docstring**: Split json into maximum size dictionaries while preserving structure....

#### Function: `split_json` (line 99)

**Parameters**: self, json_data, convert_lists

**Docstring**: Splits JSON into a list of JSON chunks...

#### Function: `split_text` (line 117)

**Parameters**: self, json_data, convert_lists, ensure_ascii

**Docstring**: Splits JSON into a list of JSON formatted strings...

#### Function: `_parse_json` (line 130)

**Parameters**: self, content

#### Function: `_parse_jsonl` (line 140)

**Parameters**: self, content

#### Function: `is_jsonl_format` (line 154)

**Parameters**: self, txt, sample_limit, threshold

#### Function: `_is_valid_json` (line 174)

**Parameters**: self, line

## Original Source Code

```py
# -*- coding: utf-8 -*-
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

# The following documents are mainly referenced, and only adaptation modifications have been made
# from https://github.com/langchain-ai/langchain/blob/master/libs/text-splitters/langchain_text_splitters/json.py

import json
from typing import Any

from rag.nlp import find_codec


class RAGFlowJsonParser:
    def __init__(self, max_chunk_size: int = 2000, min_chunk_size: int | None = None):
        super().__init__()
        self.max_chunk_size = max_chunk_size * 2
        self.min_chunk_size = min_chunk_size if min_chunk_size is not None else max(max_chunk_size - 200, 50)

    def __call__(self, binary):
        encoding = find_codec(binary)
        txt = binary.decode(encoding, errors="ignore")

        if self.is_jsonl_format(txt):
            sections = self._parse_jsonl(txt)
        else:
            sections = self._parse_json(txt)
        return sections

    @staticmethod
    def _json_size(data: dict) -> int:
        """Calculate the size of the serialized JSON object."""
        return len(json.dumps(data, ensure_ascii=False))

    @staticmethod
    def _set_nested_dict(d: dict, path: list[str], value: Any) -> None:
        """Set a value in a nested dictionary based on the given path."""
        for key in path[:-1]:
            d = d.setdefault(key, {})
        d[path[-1]] = value

    def _list_to_dict_preprocessing(self, data: Any) -> Any:
        if isinstance(data, dict):
            # Process each key-value pair in the dictionary
            return {k: self._list_to_dict_preprocessing(v) for k, v in data.items()}
        elif isinstance(data, list):
            # Convert the list to a dictionary with index-based keys
            return {str(i): self._list_to_dict_preprocessing(item) for i, item in enumerate(data)}
        else:
            # Base case: the item is neither a dict nor a list, so return it unchanged
            return data

    def _json_split(
        self,
        data,
        current_path: list[str] | None,
        chunks: list[dict] | None,
    ) -> list[dict]:
        """
        Split json into maximum size dictionaries while preserving structure.
        """
        current_path = current_path or []
        chunks = chunks or [{}]
        if isinstance(data, dict):
            for key, value in data.items():
                new_path = current_path + [key]
                chunk_size = self._json_size(chunks[-1])
                size = self._json_size({key: value})
                remaining = self.max_chunk_size - chunk_size

                if size < remaining:
                    # Add item to current chunk
                    self._set_nested_dict(chunks[-1], new_path, value)
                else:
                    if chunk_size >= self.min_chunk_size:
                        # Chunk is big enough, start a new chunk
                        chunks.append({})

                    # Iterate
                    self._json_split(value, new_path, chunks)
        else:
            # handle single item
            self._set_nested_dict(chunks[-1], current_path, data)
        return chunks

    def split_json(
        self,
        json_data,
        convert_lists: bool = False,
    ) -> list[dict]:
        """Splits JSON into a list of JSON chunks"""

        if convert_lists:
            preprocessed_data = self._list_to_dict_preprocessing(json_data)
            chunks = self._json_split(preprocessed_data, None, None)
        else:
            chunks = self._json_split(json_data, None, None)

        # Remove the last chunk if it's empty
        if not chunks[-1]:
            chunks.pop()
        return chunks

    def split_text(
        self,
        json_data: dict[str, Any],
        convert_lists: bool = False,
        ensure_ascii: bool = True,
    ) -> list[str]:
        """Splits JSON into a list of JSON formatted strings"""

        chunks = self.split_json(json_data=json_data, convert_lists=convert_lists)

        # Convert to string
        return [json.dumps(chunk, ensure_ascii=ensure_ascii) for chunk in chunks]

    def _parse_json(self, content: str) -> list[str]:
        sections = []
        try:
            json_data = json.loads(content)
            chunks = self.split_json(json_data, True)
            sections = [json.dumps(line, ensure_ascii=False) for line in chunks if line]
        except json.JSONDecodeError:
            pass
        return sections

    def _parse_jsonl(self, content: str) -> list[str]:
        lines = content.strip().splitlines()
        all_chunks = []
        for line in lines:
            if not line.strip():
                continue
            try:
                data = json.loads(line)
                chunks = self.split_json(data, convert_lists=True)
                all_chunks.extend(json.dumps(chunk, ensure_ascii=False) for chunk in chunks if chunk)
            except json.JSONDecodeError:
                continue
        return all_chunks

    def is_jsonl_format(self, txt: str, sample_limit: int = 10, threshold: float = 0.8) -> bool:
        lines = [line.strip() for line in txt.strip().splitlines() if line.strip()]
        if not lines:
            return False

        try:
            json.loads(txt)
            return False
        except json.JSONDecodeError:
            pass

        sample_limit = min(len(lines), sample_limit)
        sample_lines = lines[:sample_limit]
        valid_lines = sum(1 for line in sample_lines if self._is_valid_json(line))

        if not valid_lines:
            return False

        return (valid_lines / len(sample_lines)) >= threshold

    def _is_valid_json(self, line: str) -> bool:
        try:
            json.loads(line)
            return True
        except json.JSONDecodeError:
            return False

```

## Detailed Analysis

### File Role in Repository

The file `deepdoc/parser/json_parser.py` is located in the `deepdoc/parser` directory.

This file is part of the **Document Processing** system.

### Architecture Context

Files in this location typically handle concerns related to parser.

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
- [docling_parser.py](docling_parser.py_docs.md)
- [docx_parser.py](docx_parser.py_docs.md)
- [excel_parser.py](excel_parser.py_docs.md)
- [figure_parser.py](figure_parser.py_docs.md)
- [html_parser.py](html_parser.py_docs.md)
- [markdown_parser.py](markdown_parser.py_docs.md)
- [mineru_parser.py](mineru_parser.py_docs.md)
- [pdf_parser.py](pdf_parser.py_docs.md)
- [ppt_parser.py](ppt_parser.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
