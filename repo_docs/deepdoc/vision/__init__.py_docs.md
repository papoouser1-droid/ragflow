# File Documentation: deepdoc/vision/__init__.py

## File Metadata

- **Path**: `deepdoc/vision/__init__.py`
- **Extension**: `.py`
- **Lines**: 91
- **Characters**: 2,611
- **Size**: 2,611 bytes
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
import io
import sys
import threading

import pdfplumber

from .ocr import OCR
from .recognizer import Recognizer
from .layout_recognizer import AscendLayoutRecognizer
from .layout_recognizer import LayoutRecognizer4YOLOv10 as LayoutRecognizer
from .table_structure_recognizer import TableStructureRecognizer

LOCK_KEY_pdfplumber = "global_shared_lock_pdfplumber"
if LOCK_KEY_pdfplumber not in sys.modules:
    sys.modules[LOCK_KEY_pdfplumber] = threading.Lock()


def init_in_out(args):
    import os
    import traceback

    from PIL import Image

    from common.file_utils import traversal_files

    images = []
    outputs = []

    if not os.path.exists(args.output_dir):
        os.mkdir(args.output_dir)

    def pdf_pages(fnm, zoomin=3):
        nonlocal outputs, images
        with sys.modules[LOCK_KEY_pdfplumber]:
            pdf = pdfplumber.open(fnm)
            images = [p.to_image(resolution=72 * zoomin).annotated for i, p in enumerate(pdf.pages)]

        for i, page in enumerate(images):
            outputs.append(os.path.split(fnm)[-1] + f"_{i}.jpg")
        pdf.close()

    def images_and_outputs(fnm):
        nonlocal outputs, images
        if fnm.split(".")[-1].lower() == "pdf":
            pdf_pages(fnm)
            return
        try:
            fp = open(fnm, "rb")
            binary = fp.read()
            fp.close()
            images.append(Image.open(io.BytesIO(binary)).convert("RGB"))
            outputs.append(os.path.split(fnm)[-1])
        except Exception:
            traceback.print_exc()

    if os.path.isdir(args.inputs):
        for fnm in traversal_files(args.inputs):
            images_and_outputs(fnm)
    else:
        images_and_outputs(args.inputs)

    for i in range(len(outputs)):
        outputs[i] = os.path.join(args.output_dir, outputs[i])

    return images, outputs


__all__ = [
    "OCR",
    "Recognizer",
    "LayoutRecognizer",
    "AscendLayoutRecognizer",
    "TableStructureRecognizer",
    "init_in_out",
]

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


### Functions (1)

- `init_in_out()`: Function definition

### Imports (9)

- `import io`
- `import sys`
- `import threading`
- `import pdfplumber`
- `from .ocr import OCR`
- `from .recognizer import Recognizer`
- `from .layout_recognizer import AscendLayoutRecognizer`
- `from .layout_recognizer import LayoutRecognizer4YOLOv10 as LayoutRecognizer`
- `from .table_structure_recognizer import TableStructureRecognizer`

## Code Structure Analysis

- Total lines: 91
- Blank lines: 18 (19.8%)
- Comment lines: ~15 (16.5%)
- Code lines: ~58


## Dependencies and Imports

- `import io`
- `import sys`
- `import threading`
- `import pdfplumber`
- `from .ocr import OCR`
- `from .recognizer import Recognizer`
- `from .layout_recognizer import AscendLayoutRecognizer`
- `from .layout_recognizer import LayoutRecognizer4YOLOv10 as LayoutRecognizer`
- `from .table_structure_recognizer import TableStructureRecognizer`

## Design & Architecture

This file is located in the `deepdoc` directory, specifically within `deepdoc/vision`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 5 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `deepdoc/vision/` directory
- Imports from `.ocr`
- Imports from `.recognizer`
- Imports from `.layout_recognizer`
- Imports from `.layout_recognizer`
- Imports from `.table_structure_recognizer`
- Potential test file: `test___init__.py`

## Keywords

ANY, All, Apache, AscendLayoutRecognizer, Authors, BASIS, BytesIO, CONDITIONS, Copyright, Exception, Image, InfiniFlow, KIND, LICENSE, LOCK_KEY_pdfplumber, LayoutRecognizer, LayoutRecognizer4YOLOv10, License, Licensed, Lock, OCR, PIL, Python, RGB, Recognizer, Reserved, Rights, See, TableStructureRecognizer, The, Unless, Version, WARRANTIES, WITHOUT, You, images_and_outputs, init_in_out, pdf_pages

---
*Generated by RAGFlow Repository Documentation Generator*
