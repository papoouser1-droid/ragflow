# Documentation: rag/app/one.py

## File Metadata

- **Path**: `rag/app/one.py`
- **Size**: 5979 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `rag/app/one.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `io`
- `re`
- `deepdoc.parser.utils`
- `rag.app`
- `rag.nlp`
- `deepdoc.parser`
- `deepdoc.parser.figure_parser`
- `rag.app.naive`
- `sys`
- `timeit`

### Classes Defined

This file defines 1 class(es):

#### Class: `Pdf` (line 28)

**Methods**: __call__

### Functions Defined

This file defines 3 function(s):

#### Function: `chunk` (line 64)

**Parameters**: filename, binary, from_page, to_page, lang, callback

**Docstring**: Supported file formats are docx, pdf, excel, txt.
One file forms a chunk which maintains original text order....

#### Function: `__call__` (line 29)

**Parameters**: self, filename, binary, from_page, to_page, zoomin, callback

#### Function: `dummy` (line 162)

**Parameters**: prog, msg

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
import re

from deepdoc.parser.utils import get_text
from rag.app import naive
from rag.nlp import rag_tokenizer, tokenize
from deepdoc.parser import PdfParser, ExcelParser, HtmlParser
from deepdoc.parser.figure_parser import vision_figure_parser_docx_wrapper
from rag.app.naive import by_plaintext, PARSERS

class Pdf(PdfParser):
    def __call__(self, filename, binary=None, from_page=0,
                 to_page=100000, zoomin=3, callback=None):
        from timeit import default_timer as timer
        start = timer()
        callback(msg="OCR started")
        self.__images__(
            filename if not binary else binary,
            zoomin,
            from_page,
            to_page,
            callback
        )
        callback(msg="OCR finished ({:.2f}s)".format(timer() - start))

        start = timer()
        self._layouts_rec(zoomin, drop=False)
        callback(0.63, "Layout analysis ({:.2f}s)".format(timer() - start))
        logging.debug("layouts cost: {}s".format(timer() - start))

        start = timer()
        self._table_transformer_job(zoomin)
        callback(0.65, "Table analysis ({:.2f}s)".format(timer() - start))

        start = timer()
        self._text_merge()
        callback(0.67, "Text merged ({:.2f}s)".format(timer() - start))
        tbls = self._extract_table_figure(True, zoomin, True, True)
        self._concat_downward()

        sections = [(b["text"], self.get_position(b, zoomin))
                    for i, b in enumerate(self.boxes)]
        return [(txt, "") for txt, _ in sorted(sections, key=lambda x: (
            x[-1][0][0], x[-1][0][3], x[-1][0][1]))], tbls


def chunk(filename, binary=None, from_page=0, to_page=100000,
          lang="Chinese", callback=None, **kwargs):
    """
        Supported file formats are docx, pdf, excel, txt.
        One file forms a chunk which maintains original text order.
    """
    parser_config = kwargs.get(
        "parser_config", {
            "chunk_token_num": 512, "delimiter": "\n!?。；！？", "layout_recognize": "DeepDOC"})
    eng = lang.lower() == "english"  # is_english(cks)

    if re.search(r"\.docx$", filename, re.IGNORECASE):
        callback(0.1, "Start to parse.")
        sections, tbls = naive.Docx()(filename, binary)
        tbls=vision_figure_parser_docx_wrapper(sections=sections,tbls=tbls,callback=callback,**kwargs)
        sections = [s for s, _ in sections if s]
        for (_, html), _ in tbls:
            sections.append(html)
        callback(0.8, "Finish parsing.")

    elif re.search(r"\.pdf$", filename, re.IGNORECASE):
        layout_recognizer = parser_config.get("layout_recognize", "DeepDOC")

        if isinstance(layout_recognizer, bool):
            layout_recognizer = "DeepDOC" if layout_recognizer else "Plain Text"

        name = layout_recognizer.strip().lower()
        parser = PARSERS.get(name, by_plaintext)
        callback(0.1, "Start to parse.")

        sections, tbls, pdf_parser = parser(
            filename = filename,
            binary = binary,
            from_page = from_page,
            to_page = to_page,
            lang = lang,
            callback = callback,
            pdf_cls = Pdf,
            **kwargs
        )

        if not sections and not tbls:
            return []

        if name in ["tcadp", "docling", "mineru"]:
            parser_config["chunk_token_num"] = 0
        
        callback(0.8, "Finish parsing.")
        
        for (img, rows), poss in tbls:
            if not rows:
                continue
            sections.append((rows if isinstance(rows, str) else rows[0],
                             [(p[0] + 1 - from_page, p[1], p[2], p[3], p[4]) for p in poss]))
        sections = [s for s, _ in sections if s]

    elif re.search(r"\.xlsx?$", filename, re.IGNORECASE):
        callback(0.1, "Start to parse.")
        excel_parser = ExcelParser()
        sections = excel_parser.html(binary, 1000000000)

    elif re.search(r"\.(txt|md|markdown)$", filename, re.IGNORECASE):
        callback(0.1, "Start to parse.")
        txt = get_text(filename, binary)
        sections = txt.split("\n")
        sections = [s for s in sections if s]
        callback(0.8, "Finish parsing.")

    elif re.search(r"\.(htm|html)$", filename, re.IGNORECASE):
        callback(0.1, "Start to parse.")
        sections = HtmlParser()(filename, binary)
        sections = [s for s in sections if s]
        callback(0.8, "Finish parsing.")

    elif re.search(r"\.doc$", filename, re.IGNORECASE):
        callback(0.1, "Start to parse.")
        binary = BytesIO(binary)
        doc_parsed = parser.from_buffer(binary)
        sections = doc_parsed['content'].split('\n')
        sections = [s for s in sections if s]
        callback(0.8, "Finish parsing.")

    else:
        raise NotImplementedError(
            "file type not supported yet(doc, docx, pdf, txt supported)")

    doc = {
        "docnm_kwd": filename,
        "title_tks": rag_tokenizer.tokenize(re.sub(r"\.[a-zA-Z]+$", "", filename))
    }
    doc["title_sm_tks"] = rag_tokenizer.fine_grained_tokenize(doc["title_tks"])
    tokenize(doc, "\n".join(sections), eng)
    return [doc]


if __name__ == "__main__":
    import sys

    def dummy(prog=None, msg=""):
        pass

    chunk(sys.argv[1], from_page=0, to_page=10, callback=dummy)

```

## Detailed Analysis

### File Role in Repository

The file `rag/app/one.py` is located in the `rag/app` directory.

This file is part of the **RAG (Retrieval-Augmented Generation)** core engine.

### Architecture Context

Files in this location typically handle concerns related to app.

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
- [audio.py](audio.py_docs.md)
- [book.py](book.py_docs.md)
- [email.py](email.py_docs.md)
- [laws.py](laws.py_docs.md)
- [manual.py](manual.py_docs.md)
- [naive.py](naive.py_docs.md)
- [paper.py](paper.py_docs.md)
- [picture.py](picture.py_docs.md)
- [presentation.py](presentation.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
