# Documentation: deepdoc/parser/ppt_parser.py

## File Metadata

- **Path**: `deepdoc/parser/ppt_parser.py`
- **Size**: 3600 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `deepdoc/parser/ppt_parser.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `io`
- `pptx`

### Classes Defined

This file defines 1 class(es):

#### Class: `RAGFlowPptParser` (line 22)

**Methods**: __init__, __get_bulleted_text, __extract, __call__

### Functions Defined

This file defines 4 function(s):

#### Function: `__init__` (line 23)

**Parameters**: self

#### Function: `__get_bulleted_text` (line 26)

**Parameters**: self, paragraph

#### Function: `__extract` (line 33)

**Parameters**: self, shape

#### Function: `__call__` (line 77)

**Parameters**: self, fnm, from_page, to_page, callback

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

import logging
from io import BytesIO
from pptx import Presentation


class RAGFlowPptParser:
    def __init__(self):
        super().__init__()

    def __get_bulleted_text(self, paragraph):
        is_bulleted = bool(paragraph._p.xpath("./a:pPr/a:buChar")) or bool(paragraph._p.xpath("./a:pPr/a:buAutoNum")) or bool(paragraph._p.xpath("./a:pPr/a:buBlip"))
        if is_bulleted:
            return f"{'  '* paragraph.level}.{paragraph.text}"
        else:
            return paragraph.text

    def __extract(self, shape):
        try:
            # First try to get text content
            if hasattr(shape, 'has_text_frame') and shape.has_text_frame:
                text_frame = shape.text_frame
                texts = []
                for paragraph in text_frame.paragraphs:
                    if paragraph.text.strip():
                        texts.append(self.__get_bulleted_text(paragraph))
                return "\n".join(texts)

            # Safely get shape_type
            try:
                shape_type = shape.shape_type
            except NotImplementedError:
                # If shape_type is not available, try to get text content
                if hasattr(shape, 'text'):
                    return shape.text.strip()
                return ""

            # Handle table
            if shape_type == 19:
                tb = shape.table
                rows = []
                for i in range(1, len(tb.rows)):
                    rows.append("; ".join([tb.cell(
                        0, j).text + ": " + tb.cell(i, j).text for j in range(len(tb.columns)) if tb.cell(i, j)]))
                return "\n".join(rows)

            # Handle group shape
            if shape_type == 6:
                texts = []
                for p in sorted(shape.shapes, key=lambda x: (x.top // 10, x.left)):
                    t = self.__extract(p)
                    if t:
                        texts.append(t)
                return "\n".join(texts)

            return ""

        except Exception as e:
            logging.error(f"Error processing shape: {str(e)}")
            return ""

    def __call__(self, fnm, from_page, to_page, callback=None):
        ppt = Presentation(fnm) if isinstance(
            fnm, str) else Presentation(
            BytesIO(fnm))
        txts = []
        self.total_page = len(ppt.slides)
        for i, slide in enumerate(ppt.slides):
            if i < from_page:
                continue
            if i >= to_page:
                break
            texts = []
            for shape in sorted(
                    slide.shapes, key=lambda x: ((x.top if x.top is not None else 0) // 10, x.left if x.left is not None else 0)):
                try:
                    txt = self.__extract(shape)
                    if txt:
                        texts.append(txt)
                except Exception as e:
                    logging.exception(e)
            txts.append("\n".join(texts))

        return txts

```

## Detailed Analysis

### File Role in Repository

The file `deepdoc/parser/ppt_parser.py` is located in the `deepdoc/parser` directory.

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
- [json_parser.py](json_parser.py_docs.md)
- [markdown_parser.py](markdown_parser.py_docs.md)
- [mineru_parser.py](mineru_parser.py_docs.md)
- [pdf_parser.py](pdf_parser.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
