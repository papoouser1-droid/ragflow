# File Documentation: deepdoc/vision/t_ocr.py

## File Metadata

- **Path**: `deepdoc/vision/t_ocr.py`
- **Extension**: `.py`
- **Lines**: 94
- **Characters**: 3,220
- **Size**: 3,220 bytes
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

import os
import sys
sys.path.insert(
    0,
    os.path.abspath(
        os.path.join(
            os.path.dirname(
                os.path.abspath(__file__)),
            '../../')))

from deepdoc.vision.seeit import draw_box
from deepdoc.vision import OCR, init_in_out
import argparse
import numpy as np
import trio

# os.environ['CUDA_VISIBLE_DEVICES'] = '0,2' #2 gpus, uncontinuous
os.environ['CUDA_VISIBLE_DEVICES'] = '0' #1 gpu
# os.environ['CUDA_VISIBLE_DEVICES'] = '' #cpu


def main(args):
    import torch.cuda

    cuda_devices = torch.cuda.device_count()
    limiter = [trio.CapacityLimiter(1) for _ in range(cuda_devices)] if cuda_devices > 1 else None
    ocr = OCR()
    images, outputs = init_in_out(args)

    def __ocr(i, id, img):
        print("Task {} start".format(i))
        bxs = ocr(np.array(img), id)
        bxs = [(line[0], line[1][0]) for line in bxs]
        bxs = [{
            "text": t,
            "bbox": [b[0][0], b[0][1], b[1][0], b[-1][1]],
            "type": "ocr",
            "score": 1} for b, t in bxs if b[0][0] <= b[1][0] and b[0][1] <= b[-1][1]]
        img = draw_box(images[i], bxs, ["ocr"], 1.)
        img.save(outputs[i], quality=95)
        with open(outputs[i] + ".txt", "w+", encoding='utf-8') as f:
            f.write("\n".join([o["text"] for o in bxs]))

        print("Task {} done".format(i))

    async def __ocr_thread(i, id, img, limiter = None):
        if limiter:
            async with limiter:
                print("Task {} use device {}".format(i, id))
                await trio.to_thread.run_sync(lambda: __ocr(i, id, img))
        else:
            __ocr(i, id, img)

    async def __ocr_launcher():
        if cuda_devices > 1:
            async with trio.open_nursery() as nursery:
                for i, img in enumerate(images):
                    nursery.start_soon(__ocr_thread, i, i % cuda_devices, img, limiter[i % cuda_devices])
                    await trio.sleep(0.1)
        else:
            for i, img in enumerate(images):
                await __ocr_thread(i, 0, img)

    trio.run(__ocr_launcher)

    print("OCR tasks are all done")


if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument('--inputs',
                        help="Directory where to store images or PDFs, or a file path to a single image or PDF",
                        required=True)
    parser.add_argument('--output_dir', help="Directory where to store the output images. Default: './ocr_outputs'",
                        default="./ocr_outputs")
    args = parser.parse_args()
    main(args)

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

- `main()`: Function definition

### Imports (7)

- `import os`
- `import sys`
- `from deepdoc.vision.seeit import draw_box`
- `from deepdoc.vision import OCR, init_in_out`
- `import argparse`
- `import numpy as np`
- `import trio`

## Code Structure Analysis

- Total lines: 94
- Blank lines: 15 (16.0%)
- Comment lines: ~17 (18.1%)
- Code lines: ~62


## Dependencies and Imports

- `import os`
- `import sys`
- `from deepdoc.vision.seeit import draw_box`
- `from deepdoc.vision import OCR, init_in_out`
- `import argparse`
- `import numpy as np`
- `import trio`

## Design & Architecture

This file is located in the `deepdoc` directory, specifically within `deepdoc/vision`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 7 loop(s) - consider algorithmic complexity
- Uses asynchronous patterns for better performance

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
- Imports from `deepdoc.vision.seeit`
- Imports from `deepdoc.vision`
- Potential test file: `test_t_ocr.py`

## Keywords

ANY, All, Apache, ArgumentParser, Authors, BASIS, CONDITIONS, CUDA_VISIBLE_DEVICES, CapacityLimiter, Copyright, Default, Directory, InfiniFlow, KIND, LICENSE, License, Licensed, None, OCR, PDF, PDFs, Python, Reserved, Rights, See, Task, The, True, Unless, Version, WARRANTIES, WITHOUT, You, __ocr, __ocr_launcher, __ocr_thread, main

---
*Generated by RAGFlow Repository Documentation Generator*
