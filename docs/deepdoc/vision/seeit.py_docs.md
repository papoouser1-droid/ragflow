# Documentation: deepdoc/vision/seeit.py

## File Metadata

- **Path**: `deepdoc/vision/seeit.py`
- **Size**: 2883 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `deepdoc/vision/seeit.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `os`
- `PIL`
- `PIL`

### Functions Defined

This file defines 4 function(s):

#### Function: `save_results` (line 23)

**Parameters**: image_list, results, labels, output_dir, threshold

#### Function: `draw_box` (line 34)

**Parameters**: im, result, labels, threshold

#### Function: `get_color_map_list` (line 59)

**Parameters**: num_classes

**Docstring**: Args:
    num_classes (int): number of class
Returns:
    color_map (list): RGB color list...

#### Function: `imagedraw_textsize_c` (line 80)

**Parameters**: draw, text

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

## Detailed Analysis

### File Role in Repository

The file `deepdoc/vision/seeit.py` is located in the `deepdoc/vision` directory.

This file is part of the **Document Processing** system.

### Architecture Context

Files in this location typically handle concerns related to vision.

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
- [layout_recognizer.py](layout_recognizer.py_docs.md)
- [ocr.py](ocr.py_docs.md)
- [operators.py](operators.py_docs.md)
- [postprocess.py](postprocess.py_docs.md)
- [recognizer.py](recognizer.py_docs.md)
- [t_ocr.py](t_ocr.py_docs.md)
- [t_recognizer.py](t_recognizer.py_docs.md)
- [table_structure_recognizer.py](table_structure_recognizer.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
