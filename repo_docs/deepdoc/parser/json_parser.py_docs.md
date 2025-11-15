# File Documentation: deepdoc/parser/json_parser.py

## File Metadata

- **Path**: `deepdoc/parser/json_parser.py`
- **Extension**: `.py`
- **Lines**: 180
- **Characters**: 6,388
- **Size**: 6,388 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

## Original Source

```python
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

## High-Level Overview

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

## Detailed Walkthrough

### Classes (1)

- `RAGFlowJsonParser`: Class definition

### Imports (3)

- `import json`
- `from typing import Any`
- `from rag.nlp import find_codec`

## Code Structure Analysis

- Total lines: 180
- Blank lines: 28 (15.6%)
- Comment lines: ~33 (18.3%)
- Code lines: ~119


## Dependencies and Imports

- `import json`
- `from typing import Any`
- `from rag.nlp import find_codec`

## Design & Architecture

This file is located in the `deepdoc` directory, specifically within `deepdoc/parser`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 12 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `deepdoc/parser/` directory
- Imports from `rag.nlp`
- Potential test file: `test_json_parser.py`

## Keywords

ANY, Add, All, Any, Apache, Authors, BASIS, Base, CONDITIONS, Calculate, Chunk, Convert, Copyright, False, InfiniFlow, Iterate, JSON, JSONDecodeError, KIND, LICENSE, License, Licensed, None, Process, Python, RAGFlowJsonParser, Remove, Reserved, Rights, See, Set, Split, Splits, The, True, Unless, Version, WARRANTIES, WITHOUT, You, __call__, __init__, _is_valid_json, _json_size, _json_split, _list_to_dict_preprocessing, _parse_json, _parse_jsonl, _set_nested_dict, is_jsonl_format...

---
*Generated by RAGFlow Repository Documentation Generator*
