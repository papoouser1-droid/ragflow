# Documentation: common/data_source/html_utils.py

## File Metadata

- **Path**: `common/data_source/html_utils.py`
- **Size**: 7875 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `common/data_source/html_utils.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `re`
- `copy`
- `dataclasses`
- `io`
- `typing`
- `bs4`
- `common.data_source.config`
- `trafilatura`
- `trafilatura.settings`

### Classes Defined

This file defines 1 class(es):

#### Class: `ParsedHTML` (line 18)

### Functions Defined

This file defines 7 function(s):

#### Function: `strip_excessive_newlines_and_spaces` (line 23)

**Parameters**: document

#### Function: `strip_newlines` (line 33)

**Parameters**: document

#### Function: `format_element_text` (line 38)

**Parameters**: element_text, link_href

#### Function: `parse_html_with_trafilatura` (line 51)

**Parameters**: html_content

**Docstring**: Parse HTML content using trafilatura....

#### Function: `format_document_soup` (line 66)

**Parameters**: document, table_cell_separator

**Docstring**: Format html to a flat text document.

The following goals:
- Newlines from within the HTML are removed (as browser would ignore them as well).
- Repeated newlines/spaces are removed (as browsers would...

#### Function: `parse_html_page_basic` (line 162)

**Parameters**: text

#### Function: `web_html_cleanup` (line 167)

**Parameters**: page_content, mintlify_cleanup_enabled, additional_element_types_to_discard

## Original Source Code

```py
import logging
import re
from copy import copy
from dataclasses import dataclass
from io import BytesIO
from typing import IO

import bs4

from common.data_source.config import HTML_BASED_CONNECTOR_TRANSFORM_LINKS_STRATEGY, \
    HtmlBasedConnectorTransformLinksStrategy, WEB_CONNECTOR_IGNORED_CLASSES, WEB_CONNECTOR_IGNORED_ELEMENTS, \
    PARSE_WITH_TRAFILATURA

MINTLIFY_UNWANTED = ["sticky", "hidden"]


@dataclass
class ParsedHTML:
    title: str | None
    cleaned_text: str


def strip_excessive_newlines_and_spaces(document: str) -> str:
    # collapse repeated spaces into one
    document = re.sub(r" +", " ", document)
    # remove trailing spaces
    document = re.sub(r" +[\n\r]", "\n", document)
    # remove repeated newlines
    document = re.sub(r"[\n\r]+", "\n", document)
    return document.strip()


def strip_newlines(document: str) -> str:
    # HTML might contain newlines which are just whitespaces to a browser
    return re.sub(r"[\n\r]+", " ", document)


def format_element_text(element_text: str, link_href: str | None) -> str:
    element_text_no_newlines = strip_newlines(element_text)

    if (
        not link_href
        or HTML_BASED_CONNECTOR_TRANSFORM_LINKS_STRATEGY
        == HtmlBasedConnectorTransformLinksStrategy.STRIP
    ):
        return element_text_no_newlines

    return f"[{element_text_no_newlines}]({link_href})"


def parse_html_with_trafilatura(html_content: str) -> str:
    """Parse HTML content using trafilatura."""
    import trafilatura  # type: ignore
    from trafilatura.settings import use_config  # type: ignore

    config = use_config()
    config.set("DEFAULT", "include_links", "True")
    config.set("DEFAULT", "include_tables", "True")
    config.set("DEFAULT", "include_images", "True")
    config.set("DEFAULT", "include_formatting", "True")

    extracted_text = trafilatura.extract(html_content, config=config)
    return strip_excessive_newlines_and_spaces(extracted_text) if extracted_text else ""


def format_document_soup(
    document: bs4.BeautifulSoup, table_cell_separator: str = "\t"
) -> str:
    """Format html to a flat text document.

    The following goals:
    - Newlines from within the HTML are removed (as browser would ignore them as well).
    - Repeated newlines/spaces are removed (as browsers would ignore them).
    - Newlines only before and after headlines and paragraphs or when explicit (br or pre tag)
    - Table columns/rows are separated by newline
    - List elements are separated by newline and start with a hyphen
    """
    text = ""
    list_element_start = False
    verbatim_output = 0
    in_table = False
    last_added_newline = False
    link_href: str | None = None

    for e in document.descendants:
        verbatim_output -= 1
        if isinstance(e, bs4.element.NavigableString):
            if isinstance(e, (bs4.element.Comment, bs4.element.Doctype)):
                continue
            element_text = e.text
            if in_table:
                # Tables are represented in natural language with rows separated by newlines
                # Can't have newlines then in the table elements
                element_text = element_text.replace("\n", " ").strip()

            # Some tags are translated to spaces but in the logic underneath this section, we
            # translate them to newlines as a browser should render them such as with br
            # This logic here avoids a space after newline when it shouldn't be there.
            if last_added_newline and element_text.startswith(" "):
                element_text = element_text[1:]
                last_added_newline = False

            if element_text:
                content_to_add = (
                    element_text
                    if verbatim_output > 0
                    else format_element_text(element_text, link_href)
                )

                # Don't join separate elements without any spacing
                if (text and not text[-1].isspace()) and (
                    content_to_add and not content_to_add[0].isspace()
                ):
                    text += " "

                text += content_to_add

                list_element_start = False
        elif isinstance(e, bs4.element.Tag):
            # table is standard HTML element
            if e.name == "table":
                in_table = True
            # tr is for rows
            elif e.name == "tr" and in_table:
                text += "\n"
            # td for data cell, th for header
            elif e.name in ["td", "th"] and in_table:
                text += table_cell_separator
            elif e.name == "/table":
                in_table = False
            elif in_table:
                # don't handle other cases while in table
                pass
            elif e.name == "a":
                href_value = e.get("href", None)
                # mostly for typing, having multiple hrefs is not valid HTML
                link_href = (
                    href_value[0] if isinstance(href_value, list) else href_value
                )
            elif e.name == "/a":
                link_href = None
            elif e.name in ["p", "div"]:
                if not list_element_start:
                    text += "\n"
            elif e.name in ["h1", "h2", "h3", "h4"]:
                text += "\n"
                list_element_start = False
                last_added_newline = True
            elif e.name == "br":
                text += "\n"
                list_element_start = False
                last_added_newline = True
            elif e.name == "li":
                text += "\n- "
                list_element_start = True
            elif e.name == "pre":
                if verbatim_output <= 0:
                    verbatim_output = len(list(e.childGenerator()))
    return strip_excessive_newlines_and_spaces(text)


def parse_html_page_basic(text: str | BytesIO | IO[bytes]) -> str:
    soup = bs4.BeautifulSoup(text, "html.parser")
    return format_document_soup(soup)


def web_html_cleanup(
    page_content: str | bs4.BeautifulSoup,
    mintlify_cleanup_enabled: bool = True,
    additional_element_types_to_discard: list[str] | None = None,
) -> ParsedHTML:
    if isinstance(page_content, str):
        soup = bs4.BeautifulSoup(page_content, "html.parser")
    else:
        soup = page_content

    title_tag = soup.find("title")
    title = None
    if title_tag and title_tag.text:
        title = title_tag.text
        title_tag.extract()

    # Heuristics based cleaning of elements based on css classes
    unwanted_classes = copy(WEB_CONNECTOR_IGNORED_CLASSES)
    if mintlify_cleanup_enabled:
        unwanted_classes.extend(MINTLIFY_UNWANTED)
    for undesired_element in unwanted_classes:
        [
            tag.extract()
            for tag in soup.find_all(
                class_=lambda x: x and undesired_element in x.split()
            )
        ]

    for undesired_tag in WEB_CONNECTOR_IGNORED_ELEMENTS:
        [tag.extract() for tag in soup.find_all(undesired_tag)]

    if additional_element_types_to_discard:
        for undesired_tag in additional_element_types_to_discard:
            [tag.extract() for tag in soup.find_all(undesired_tag)]

    soup_string = str(soup)
    page_text = ""

    if PARSE_WITH_TRAFILATURA:
        try:
            page_text = parse_html_with_trafilatura(soup_string)
            if not page_text:
                raise ValueError("Empty content returned by trafilatura.")
        except Exception as e:
            logging.info(f"Trafilatura parsing failed: {e}. Falling back on bs4.")
            page_text = format_document_soup(soup)
    else:
        page_text = format_document_soup(soup)

    # 200B is ZeroWidthSpace which we don't care for
    cleaned_text = page_text.replace("\u200b", "")

    return ParsedHTML(title=title, cleaned_text=cleaned_text)

```

## Detailed Analysis

### File Role in Repository

The file `common/data_source/html_utils.py` is located in the `common/data_source` directory.

### Architecture Context

Files in this location typically handle concerns related to data_source.

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
- [blob_connector.py](blob_connector.py_docs.md)
- [config.py](config.py_docs.md)
- [confluence_connector.py](confluence_connector.py_docs.md)
- [discord_connector.py](discord_connector.py_docs.md)
- [dropbox_connector.py](dropbox_connector.py_docs.md)
- [exceptions.py](exceptions.py_docs.md)
- [file_types.py](file_types.py_docs.md)
- [gmail_connector.py](gmail_connector.py_docs.md)
- [interfaces.py](interfaces.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
