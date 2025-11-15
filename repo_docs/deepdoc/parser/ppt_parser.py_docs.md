# File Documentation: deepdoc/parser/ppt_parser.py

## File Metadata

- **Path**: `deepdoc/parser/ppt_parser.py`
- **Extension**: `.py`
- **Lines**: 100
- **Characters**: 3,600
- **Size**: 3,600 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

## Original Source

```python
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

## High-Level Overview

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

- `RAGFlowPptParser`: Class definition

### Imports (3)

- `import logging`
- `from io import BytesIO`
- `from pptx import Presentation`

## Code Structure Analysis

- Total lines: 100
- Blank lines: 13 (13.0%)
- Comment lines: ~20 (20.0%)
- Code lines: ~67


## Dependencies and Imports

- `import logging`
- `from io import BytesIO`
- `from pptx import Presentation`

## Design & Architecture

This file is located in the `deepdoc` directory, specifically within `deepdoc/parser`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 7 loop(s) - consider algorithmic complexity

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
- Imports from `io`
- Imports from `pptx`
- Potential test file: `test_ppt_parser.py`

## Keywords

ANY, All, Apache, Authors, BASIS, BytesIO, CONDITIONS, Copyright, Error, Exception, First, Handle, InfiniFlow, KIND, LICENSE, License, Licensed, None, NotImplementedError, Presentation, Python, RAGFlowPptParser, Reserved, Rights, Safely, See, The, Unless, Version, WARRANTIES, WITHOUT, You, __call__, __extract, __get_bulleted_text, __init__

---
*Generated by RAGFlow Repository Documentation Generator*
