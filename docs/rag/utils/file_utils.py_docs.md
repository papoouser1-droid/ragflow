# Documentation: rag/utils/file_utils.py

## File Metadata

- **Path**: `rag/utils/file_utils.py`
- **Size**: 8438 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `rag/utils/file_utils.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `io`
- `hashlib`
- `zipfile`
- `requests`
- `requests.exceptions`
- `io`
- `typing`
- `PyPDF2`
- `docx`
- `olefile`

### Functions Defined

This file defines 12 function(s):

#### Function: `_is_zip` (line 28)

**Parameters**: h

#### Function: `_is_pdf` (line 31)

**Parameters**: h

#### Function: `_is_ole` (line 34)

**Parameters**: h

#### Function: `_sha10` (line 37)

**Parameters**: b

#### Function: `_guess_ext` (line 40)

**Parameters**: b

#### Function: `_extract_ole10native_payload` (line 62)

**Parameters**: data

#### Function: `extract_embed_file` (line 85)

**Parameters**: target

**Docstring**: Only extract the 'first layer' of embedding, returning raw (filename, bytes)....

#### Function: `extract_links_from_docx` (line 149)

**Parameters**: docx_bytes

**Docstring**: Extract all hyperlinks from a Word (.docx) document binary stream.

Args:
    docx_bytes (bytes): Raw bytes of a .docx file.

Returns:
    set[str]: A set of unique hyperlink URLs....

#### Function: `extract_links_from_pdf` (line 173)

**Parameters**: pdf_bytes

**Docstring**: Extract all clickable hyperlinks from a PDF binary stream.

Args:
    pdf_bytes (bytes): Raw bytes of a PDF file.

Returns:
    set[str]: A set of unique hyperlink URLs (unordered)....

#### Function: `_get_session` (line 201)

**Parameters**: headers

**Docstring**: Get or create a global reusable session....

#### Function: `extract_html` (line 218)

**Parameters**: url, timeout, headers, max_retries

**Docstring**: Extract the full HTML page as raw bytes from a given URL.
Automatically reuses a persistent HTTP session and applies robust timeout & retry logic.

Args:
    url (str): Target webpage URL.
    timeout...

#### Function: `push` (line 94)

**Parameters**: b, name_hint

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

import io
import hashlib
import zipfile
import requests
from requests.exceptions import Timeout, RequestException
from io import BytesIO
from typing import List, Union, Tuple, Optional, Dict
import PyPDF2
from docx import Document
import olefile

def _is_zip(h: bytes) -> bool:
    return h.startswith(b"PK\x03\x04") or h.startswith(b"PK\x05\x06") or h.startswith(b"PK\x07\x08")

def _is_pdf(h: bytes) -> bool:
    return h.startswith(b"%PDF-")

def _is_ole(h: bytes) -> bool:
    return h.startswith(b"\xD0\xCF\x11\xE0\xA1\xB1\x1A\xE1")

def _sha10(b: bytes) -> str:
    return hashlib.sha256(b).hexdigest()[:10]

def _guess_ext(b: bytes) -> str:
    h = b[:8]
    if _is_zip(h):
        try:
            with zipfile.ZipFile(io.BytesIO(b), "r") as z:
                names = [n.lower() for n in z.namelist()]
                if any(n.startswith("word/") for n in names):
                    return ".docx"
                if any(n.startswith("ppt/") for n in names):
                    return ".pptx"
                if any(n.startswith("xl/") for n in names):
                    return ".xlsx"
        except Exception:
            pass
        return ".zip"
    if _is_pdf(h):
        return ".pdf"
    if _is_ole(h):
        return ".doc"
    return ".bin"

# Try to extract the real embedded payload from OLE's Ole10Native
def _extract_ole10native_payload(data: bytes) -> bytes:
    try:
        pos = 0
        if len(data) < 4:
            return data
        _ = int.from_bytes(data[pos:pos+4], "little")
        pos += 4
        # filename/src/tmp (NUL-terminated ANSI)
        for _ in range(3):
            z = data.index(b"\x00", pos)
            pos = z + 1
        # skip unknown 4 bytes
        pos += 4
        if pos + 4 > len(data):
            return data
        size = int.from_bytes(data[pos:pos+4], "little")
        pos += 4
        if pos + size <= len(data):
            return data[pos:pos+size]
    except Exception:
        pass
    return data

def extract_embed_file(target: Union[bytes, bytearray]) -> List[Tuple[str, bytes]]:
    """
    Only extract the 'first layer' of embedding, returning raw (filename, bytes).
    """
    top = bytes(target)
    head = top[:8]
    out: List[Tuple[str, bytes]] = []
    seen = set()

    def push(b: bytes, name_hint: str = ""):
        h10 = _sha10(b)
        if h10 in seen:
            return
        seen.add(h10)
        ext = _guess_ext(b)
        # If name_hint has an extension use its basename; else fallback to guessed ext
        if "." in name_hint:
            fname = name_hint.split("/")[-1]
        else:
            fname = f"{h10}{ext}"
        out.append((fname, b))

    # OOXML/ZIP container (docx/xlsx/pptx)
    if _is_zip(head):
        try:
            with zipfile.ZipFile(io.BytesIO(top), "r") as z:
                embed_dirs = (
                    "word/embeddings/", "word/objects/", "word/activex/",
                    "xl/embeddings/", "ppt/embeddings/"
                )
                for name in z.namelist():
                    low = name.lower()
                    if any(low.startswith(d) for d in embed_dirs):
                        try:
                            b = z.read(name)
                            push(b, name)
                        except Exception:
                            pass
        except Exception:
            pass
        return out

    # OLE container (doc/ppt/xls)
    if _is_ole(head):
        try:
            with olefile.OleFileIO(io.BytesIO(top)) as ole:
                for entry in ole.listdir():
                    p = "/".join(entry)
                    try:
                        data = ole.openstream(entry).read()
                    except Exception:
                        continue
                    if not data:
                        continue
                    if "Ole10Native" in p or "ole10native" in p.lower():
                        data = _extract_ole10native_payload(data)
                    push(data, p)
        except Exception:
            pass
        return out

    return out


def extract_links_from_docx(docx_bytes: bytes):
    """
    Extract all hyperlinks from a Word (.docx) document binary stream.

    Args:
        docx_bytes (bytes): Raw bytes of a .docx file.

    Returns:
        set[str]: A set of unique hyperlink URLs.
    """
    links = set()
    with BytesIO(docx_bytes) as bio:
        document = Document(bio)

        # Each relationship may represent a hyperlink, image, footer, etc.
        for rel in document.part.rels.values():
            if rel.reltype == (
                "http://schemas.openxmlformats.org/officeDocument/2006/relationships/hyperlink"
            ):
                links.add(rel.target_ref)

    return links


def extract_links_from_pdf(pdf_bytes: bytes):
    """
    Extract all clickable hyperlinks from a PDF binary stream.

    Args:
        pdf_bytes (bytes): Raw bytes of a PDF file.

    Returns:
        set[str]: A set of unique hyperlink URLs (unordered).
    """
    links = set()
    with BytesIO(pdf_bytes) as bio:
        pdf = PyPDF2.PdfReader(bio)

        for page in pdf.pages:
            annots = page.get("/Annots")
            if not annots or isinstance(annots, PyPDF2.generic.IndirectObject):
                continue
            for annot in annots:
                obj = annot.get_object()
                a = obj.get("/A")
                if a and a.get("/URI"):
                    links.add(a["/URI"])

    return links


_GLOBAL_SESSION: Optional[requests.Session] = None
def _get_session(headers: Optional[Dict[str, str]] = None) -> requests.Session:
    """Get or create a global reusable session."""
    global _GLOBAL_SESSION
    if _GLOBAL_SESSION is None:
        _GLOBAL_SESSION = requests.Session()
        _GLOBAL_SESSION.headers.update({
            "User-Agent": (
                "Mozilla/5.0 (X11; Linux x86_64) "
                "AppleWebKit/537.36 (KHTML, like Gecko) "
                "Chrome/121.0 Safari/537.36"
            )
        })
    if headers:
        _GLOBAL_SESSION.headers.update(headers)
    return _GLOBAL_SESSION


def extract_html(
    url: str,
    timeout: float = 60.0,
    headers: Optional[Dict[str, str]] = None,
    max_retries: int = 2,
) -> Tuple[Optional[bytes], Dict[str, str]]:
    """
    Extract the full HTML page as raw bytes from a given URL.
    Automatically reuses a persistent HTTP session and applies robust timeout & retry logic.

    Args:
        url (str): Target webpage URL.
        timeout (float): Request timeout in seconds (applies to connect + read).
        headers (dict, optional): Extra HTTP headers.
        max_retries (int): Number of retries on timeout or transient errors.

    Returns:
        tuple(bytes|None, dict):
            - html_bytes: Raw HTML content (or None if failed)
            - metadata: HTTP info (status_code, content_type, final_url, error if any)
    """
    session = _get_session(headers=headers)
    metadata = {"final_url": url, "status_code": "", "content_type": "", "error": ""}

    for attempt in range(1, max_retries + 1):
        try:
            resp = session.get(url, timeout=timeout)
            resp.raise_for_status()

            html_bytes = resp.content
            metadata.update({
                "final_url": resp.url,
                "status_code": str(resp.status_code),
                "content_type": resp.headers.get("Content-Type", ""),
            })
            return html_bytes, metadata

        except Timeout:
            metadata["error"] = f"Timeout after {timeout}s (attempt {attempt}/{max_retries})"
            if attempt >= max_retries:
                continue
        except RequestException as e:
            metadata["error"] = f"Request failed: {e}"
            continue

    return None, metadata
```

## Detailed Analysis

### File Role in Repository

The file `rag/utils/file_utils.py` is located in the `rag/utils` directory.

This file is part of the **RAG (Retrieval-Augmented Generation)** core engine.

### Architecture Context

Files in this location typically handle concerns related to utils.

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
- [azure_sas_conn.py](azure_sas_conn.py_docs.md)
- [azure_spn_conn.py](azure_spn_conn.py_docs.md)
- [base64_image.py](base64_image.py_docs.md)
- [doc_store_conn.py](doc_store_conn.py_docs.md)
- [es_conn.py](es_conn.py_docs.md)
- [infinity_conn.py](infinity_conn.py_docs.md)
- [mcp_tool_call_conn.py](mcp_tool_call_conn.py_docs.md)
- [minio_conn.py](minio_conn.py_docs.md)
- [opendal_conn.py](opendal_conn.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
