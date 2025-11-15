# File Documentation: deepdoc/vision/seeit.py

## File Metadata

- **Path**: `deepdoc/vision/seeit.py`
- **Extension**: `.py`
- **Lines**: 88
- **Characters**: 2,883
- **Size**: 2,883 bytes
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
import os
import PIL
from PIL import ImageDraw


def save_results(image_list, results, labels, output_dir='output/', threshold=0.5):
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)
    for idx, im in enumerate(image_list):
        im = draw_box(im, results[idx], labels, threshold=threshold)

        out_path = os.path.join(output_dir, f"{idx}.jpg")
        im.save(out_path, quality=95)
        logging.debug("save result to: " + out_path)


def draw_box(im, result, labels, threshold=0.5):
    draw_thickness = min(im.size) // 320
    draw = ImageDraw.Draw(im)
    color_list = get_color_map_list(len(labels))
    clsid2color = {n.lower():color_list[i] for i,n in enumerate(labels)}
    result = [r for r in result if r["score"] >= threshold]

    for dt in result:
        color = tuple(clsid2color[dt["type"]])
        xmin, ymin, xmax, ymax = dt["bbox"]
        draw.line(
            [(xmin, ymin), (xmin, ymax), (xmax, ymax), (xmax, ymin),
             (xmin, ymin)],
            width=draw_thickness,
            fill=color)

        # draw label
        text = "{} {:.4f}".format(dt["type"], dt["score"])
        tw, th = imagedraw_textsize_c(draw, text)
        draw.rectangle(
            [(xmin + 1, ymin - th), (xmin + tw + 1, ymin)], fill=color)
        draw.text((xmin + 1, ymin - th), text, fill=(255, 255, 255))
    return im


def get_color_map_list(num_classes):
    """
    Args:
        num_classes (int): number of class
    Returns:
        color_map (list): RGB color list
    """
    color_map = num_classes * [0, 0, 0]
    for i in range(0, num_classes):
        j = 0
        lab = i
        while lab:
            color_map[i * 3] |= (((lab >> 0) & 1) << (7 - j))
            color_map[i * 3 + 1] |= (((lab >> 1) & 1) << (7 - j))
            color_map[i * 3 + 2] |= (((lab >> 2) & 1) << (7 - j))
            j += 1
            lab >>= 3
    color_map = [color_map[i:i + 3] for i in range(0, len(color_map), 3)]
    return color_map


def imagedraw_textsize_c(draw, text):
    if int(PIL.__version__.split('.')[0]) < 10:
        tw, th = draw.textsize(text)
    else:
        left, top, right, bottom = draw.textbbox((0, 0), text)
        tw, th = right - left, bottom - top

    return tw, th

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


### Functions (4)

- `save_results()`: Function definition
- `draw_box()`: Function definition
- `get_color_map_list()`: Function definition
- `imagedraw_textsize_c()`: Function definition

### Imports (4)

- `import logging`
- `import os`
- `import PIL`
- `from PIL import ImageDraw`

## Code Structure Analysis

- Total lines: 88
- Blank lines: 14 (15.9%)
- Comment lines: ~18 (20.5%)
- Code lines: ~56


## Dependencies and Imports

- `import logging`
- `import os`
- `import PIL`
- `from PIL import ImageDraw`

## Design & Architecture

This file is located in the `deepdoc` directory, specifically within `deepdoc/vision`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 8 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `deepdoc/vision/` directory
- Imports from `PIL`
- Potential test file: `test_seeit.py`

## Keywords

ANY, All, Apache, Args, Authors, BASIS, CONDITIONS, Copyright, Draw, ImageDraw, InfiniFlow, KIND, LICENSE, License, Licensed, PIL, Python, RGB, Reserved, Returns, Rights, See, The, Unless, Version, WARRANTIES, WITHOUT, You, draw_box, get_color_map_list, imagedraw_textsize_c, save_results

---
*Generated by RAGFlow Repository Documentation Generator*
