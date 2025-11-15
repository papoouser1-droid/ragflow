# Documentation: deepdoc/parser/figure_parser.py

## File Metadata

- **Path**: `deepdoc/parser/figure_parser.py`
- **Size**: 5868 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `deepdoc/parser/figure_parser.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `concurrent.futures`
- `PIL`
- `common.constants`
- `api.db.services.llm_service`
- `common.connection_utils`
- `rag.app.picture`
- `rag.prompts.generator`

### Classes Defined

This file defines 1 class(es):

#### Class: `VisionFigureParser` (line 81)

**Methods**: __init__, _extract_figures_info, _assemble, __call__

### Functions Defined

This file defines 9 function(s):

#### Function: `vision_figure_parser_figure_data_wrapper` (line 27)

**Parameters**: figures_data_without_positions

#### Function: `vision_figure_parser_docx_wrapper` (line 38)

**Parameters**: sections, tbls, callback

#### Function: `vision_figure_parser_pdf_wrapper` (line 55)

**Parameters**: tbls, callback

#### Function: `__init__` (line 82)

**Parameters**: self, vision_model, figures_data

#### Function: `_extract_figures_info` (line 88)

**Parameters**: self, figures_data

#### Function: `_assemble` (line 106)

**Parameters**: self

#### Function: `__call__` (line 123)

**Parameters**: self

#### Function: `is_figure_item` (line 62)

**Parameters**: item

#### Function: `process` (line 127)

**Parameters**: figure_idx, figure_binary

## Original Source Code

```py
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
from concurrent.futures import ThreadPoolExecutor, as_completed

from PIL import Image

from common.constants import LLMType
from api.db.services.llm_service import LLMBundle
from common.connection_utils import timeout
from rag.app.picture import vision_llm_chunk as picture_vision_llm_chunk
from rag.prompts.generator import vision_llm_figure_describe_prompt


def vision_figure_parser_figure_data_wrapper(figures_data_without_positions):
    return [
        (
            (figure_data[1], [figure_data[0]]),
            [(0, 0, 0, 0, 0)],
        )
        for figure_data in figures_data_without_positions
        if isinstance(figure_data[1], Image.Image)
    ]


def vision_figure_parser_docx_wrapper(sections,tbls,callback=None,**kwargs):
    try:
        vision_model = LLMBundle(kwargs["tenant_id"], LLMType.IMAGE2TEXT)
        callback(0.7, "Visual model detected. Attempting to enhance figure extraction...")
    except Exception:
        vision_model = None
    if vision_model:
        figures_data = vision_figure_parser_figure_data_wrapper(sections)
        try:
            docx_vision_parser = VisionFigureParser(vision_model=vision_model, figures_data=figures_data, **kwargs)
            boosted_figures = docx_vision_parser(callback=callback)
            tbls.extend(boosted_figures)
        except Exception as e:
            callback(0.8, f"Visual model error: {e}. Skipping figure parsing enhancement.")
    return tbls


def vision_figure_parser_pdf_wrapper(tbls, callback=None, **kwargs):
    try:
        vision_model = LLMBundle(kwargs["tenant_id"], LLMType.IMAGE2TEXT)
        callback(0.7, "Visual model detected. Attempting to enhance figure extraction...")
    except Exception:
        vision_model = None
    if vision_model:
        def is_figure_item(item):
            return (
                isinstance(item[0][0], Image.Image) and
                isinstance(item[0][1], list)
            )
        figures_data = [item for item in tbls if is_figure_item(item)]
        try:
            docx_vision_parser = VisionFigureParser(vision_model=vision_model, figures_data=figures_data, **kwargs)
            boosted_figures = docx_vision_parser(callback=callback)
            tbls = [item for item in tbls if not is_figure_item(item)]
            tbls.extend(boosted_figures)
        except Exception as e:
            callback(0.8, f"Visual model error: {e}. Skipping figure parsing enhancement.")
    return tbls


shared_executor = ThreadPoolExecutor(max_workers=10)


class VisionFigureParser:
    def __init__(self, vision_model, figures_data, *args, **kwargs):
        self.vision_model = vision_model
        self._extract_figures_info(figures_data)
        assert len(self.figures) == len(self.descriptions)
        assert not self.positions or (len(self.figures) == len(self.positions))

    def _extract_figures_info(self, figures_data):
        self.figures = []
        self.descriptions = []
        self.positions = []

        for item in figures_data:
            # position
            if len(item) == 2 and isinstance(item[0], tuple) and len(item[0]) == 2 and isinstance(item[1], list) and isinstance(item[1][0], tuple) and len(item[1][0]) == 5:
                img_desc = item[0]
                assert len(img_desc) == 2 and isinstance(img_desc[0], Image.Image) and isinstance(img_desc[1], list), "Should be (figure, [description])"
                self.figures.append(img_desc[0])
                self.descriptions.append(img_desc[1])
                self.positions.append(item[1])
            else:
                assert len(item) == 2 and isinstance(item[0], Image.Image) and isinstance(item[1], list), f"Unexpected form of figure data: get {len(item)=}, {item=}"
                self.figures.append(item[0])
                self.descriptions.append(item[1])

    def _assemble(self):
        self.assembled = []
        self.has_positions = len(self.positions) != 0
        for i in range(len(self.figures)):
            figure = self.figures[i]
            desc = self.descriptions[i]
            pos = self.positions[i] if self.has_positions else None

            figure_desc = (figure, desc)

            if pos is not None:
                self.assembled.append((figure_desc, pos))
            else:
                self.assembled.append((figure_desc,))

        return self.assembled

    def __call__(self, **kwargs):
        callback = kwargs.get("callback", lambda prog, msg: None)

        @timeout(30, 3)
        def process(figure_idx, figure_binary):
            description_text = picture_vision_llm_chunk(
                binary=figure_binary,
                vision_model=self.vision_model,
                prompt=vision_llm_figure_describe_prompt(),
                callback=callback,
            )
            return figure_idx, description_text

        futures = []
        for idx, img_binary in enumerate(self.figures or []):
            futures.append(shared_executor.submit(process, idx, img_binary))

        for future in as_completed(futures):
            figure_num, txt = future.result()
            if txt:
                self.descriptions[figure_num] = txt + "\n".join(self.descriptions[figure_num])

        self._assemble()

        return self.assembled

```

## Detailed Analysis

### File Role in Repository

The file `deepdoc/parser/figure_parser.py` is located in the `deepdoc/parser` directory.

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
- [html_parser.py](html_parser.py_docs.md)
- [json_parser.py](json_parser.py_docs.md)
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
