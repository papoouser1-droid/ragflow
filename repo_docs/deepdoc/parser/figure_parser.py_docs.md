# File Documentation: deepdoc/parser/figure_parser.py

## File Metadata

- **Path**: `deepdoc/parser/figure_parser.py`
- **Extension**: `.py`
- **Lines**: 148
- **Characters**: 5,868
- **Size**: 5,868 bytes
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

- `VisionFigureParser`: Class definition

### Functions (3)

- `vision_figure_parser_figure_data_wrapper()`: Function definition
- `vision_figure_parser_docx_wrapper()`: Function definition
- `vision_figure_parser_pdf_wrapper()`: Function definition

### Imports (7)

- `from concurrent.futures import ThreadPoolExecutor, as_completed`
- `from PIL import Image`
- `from common.constants import LLMType`
- `from api.db.services.llm_service import LLMBundle`
- `from common.connection_utils import timeout`
- `from rag.app.picture import vision_llm_chunk as picture_vision_llm_chunk`
- `from rag.prompts.generator import vision_llm_figure_describe_prompt`

## Code Structure Analysis

- Total lines: 148
- Blank lines: 25 (16.9%)
- Comment lines: ~16 (10.8%)
- Code lines: ~107


## Dependencies and Imports

- `from concurrent.futures import ThreadPoolExecutor, as_completed`
- `from PIL import Image`
- `from common.constants import LLMType`
- `from api.db.services.llm_service import LLMBundle`
- `from common.connection_utils import timeout`
- `from rag.app.picture import vision_llm_chunk as picture_vision_llm_chunk`
- `from rag.prompts.generator import vision_llm_figure_describe_prompt`

## Design & Architecture

This file is located in the `deepdoc` directory, specifically within `deepdoc/parser`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 8 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `deepdoc/parser/` directory
- Imports from `concurrent.futures`
- Imports from `PIL`
- Imports from `common.constants`
- Imports from `api.db.services.llm_service`
- Imports from `common.connection_utils`
- Potential test file: `test_figure_parser.py`

## Keywords

ANY, All, Apache, Attempting, Authors, BASIS, CONDITIONS, Copyright, Exception, IMAGE2TEXT, Image, InfiniFlow, KIND, LICENSE, LLMBundle, LLMType, License, Licensed, None, PIL, Python, Reserved, Rights, See, Should, Skipping, The, ThreadPoolExecutor, Unexpected, Unless, Version, VisionFigureParser, Visual, WARRANTIES, WITHOUT, You, __call__, __init__, _assemble, _extract_figures_info, is_figure_item, process, timeout, vision_figure_parser_docx_wrapper, vision_figure_parser_figure_data_wrapper, vision_figure_parser_pdf_wrapper

---
*Generated by RAGFlow Repository Documentation Generator*
