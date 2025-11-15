# Documentation: common/data_source/google_util/util.py

## File Metadata

- **Path**: `common/data_source/google_util/util.py`
- **Size**: 5400 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `common/data_source/google_util/util.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `socket`
- `collections.abc`
- `enum`
- `typing`
- `googleapiclient.errors`
- `common.data_source.google_drive.model`

### Classes Defined

This file defines 1 class(es):

#### Class: `GoogleFields` (line 13)

### Functions Defined

This file defines 5 function(s):

#### Function: `get_file_owners` (line 27)

**Parameters**: file, primary_admin_email

**Docstring**: Get the owners of a file if the attribute is present....

#### Function: `execute_paginated_retrieval` (line 38)

**Parameters**: retrieval_function, list_key, continue_on_404_or_403

#### Function: `execute_paginated_retrieval_with_max_pages` (line 54)

**Parameters**: retrieval_function, max_num_pages, list_key, continue_on_404_or_403

#### Function: `_execute_paginated_retrieval` (line 70)

**Parameters**: retrieval_function, list_key, continue_on_404_or_403, max_num_pages

**Docstring**: Execute a paginated retrieval from Google Drive API
Args:
    retrieval_function: The specific list function to call (e.g., service.files().list)
    list_key: If specified, each object returned by th...

#### Function: `_execute_single_retrieval` (line 112)

**Parameters**: retrieval_function, continue_on_404_or_403

**Docstring**: Execute a single retrieval from Google Drive API...

## Original Source Code

```py
import logging
import socket
from collections.abc import Callable, Iterator
from enum import Enum
from typing import Any

from googleapiclient.errors import HttpError  # type: ignore  # type: ignore

from common.data_source.google_drive.model import GoogleDriveFileType


# See https://developers.google.com/drive/api/reference/rest/v3/files/list for more
class GoogleFields(str, Enum):
    ID = "id"
    CREATED_TIME = "createdTime"
    MODIFIED_TIME = "modifiedTime"
    NAME = "name"
    SIZE = "size"
    PARENTS = "parents"


NEXT_PAGE_TOKEN_KEY = "nextPageToken"
PAGE_TOKEN_KEY = "pageToken"
ORDER_BY_KEY = "orderBy"


def get_file_owners(file: GoogleDriveFileType, primary_admin_email: str) -> list[str]:
    """
    Get the owners of a file if the attribute is present.
    """
    return [email for owner in file.get("owners", []) if (email := owner.get("emailAddress")) and email.split("@")[-1] == primary_admin_email.split("@")[-1]]


# included for type purposes; caller should not need to address
# Nones unless max_num_pages is specified. Use
# execute_paginated_retrieval_with_max_pages instead if you want
# the early stop + yield None after max_num_pages behavior.
def execute_paginated_retrieval(
    retrieval_function: Callable,
    list_key: str | None = None,
    continue_on_404_or_403: bool = False,
    **kwargs: Any,
) -> Iterator[GoogleDriveFileType]:
    for item in _execute_paginated_retrieval(
        retrieval_function,
        list_key,
        continue_on_404_or_403,
        **kwargs,
    ):
        if not isinstance(item, str):
            yield item


def execute_paginated_retrieval_with_max_pages(
    retrieval_function: Callable,
    max_num_pages: int,
    list_key: str | None = None,
    continue_on_404_or_403: bool = False,
    **kwargs: Any,
) -> Iterator[GoogleDriveFileType | str]:
    yield from _execute_paginated_retrieval(
        retrieval_function,
        list_key,
        continue_on_404_or_403,
        max_num_pages=max_num_pages,
        **kwargs,
    )


def _execute_paginated_retrieval(
    retrieval_function: Callable,
    list_key: str | None = None,
    continue_on_404_or_403: bool = False,
    max_num_pages: int | None = None,
    **kwargs: Any,
) -> Iterator[GoogleDriveFileType | str]:
    """Execute a paginated retrieval from Google Drive API
    Args:
        retrieval_function: The specific list function to call (e.g., service.files().list)
        list_key: If specified, each object returned by the retrieval function
                  will be accessed at the specified key and yielded from.
        continue_on_404_or_403: If True, the retrieval will continue even if the request returns a 404 or 403 error.
        max_num_pages: If specified, the retrieval will stop after the specified number of pages and yield None.
        **kwargs: Arguments to pass to the list function
    """
    if "fields" not in kwargs or "nextPageToken" not in kwargs["fields"]:
        raise ValueError("fields must contain nextPageToken for execute_paginated_retrieval")
    next_page_token = kwargs.get(PAGE_TOKEN_KEY, "")
    num_pages = 0
    while next_page_token is not None:
        if max_num_pages is not None and num_pages >= max_num_pages:
            yield next_page_token
            return
        num_pages += 1
        request_kwargs = kwargs.copy()
        if next_page_token:
            request_kwargs[PAGE_TOKEN_KEY] = next_page_token
        results = _execute_single_retrieval(
            retrieval_function,
            continue_on_404_or_403,
            **request_kwargs,
        )

        next_page_token = results.get(NEXT_PAGE_TOKEN_KEY)
        if list_key:
            for item in results.get(list_key, []):
                yield item
        else:
            yield results


def _execute_single_retrieval(
    retrieval_function: Callable,
    continue_on_404_or_403: bool = False,
    **request_kwargs: Any,
) -> GoogleDriveFileType:
    """Execute a single retrieval from Google Drive API"""
    try:
        results = retrieval_function(**request_kwargs).execute()
    except HttpError as e:
        if e.resp.status >= 500:
            results = retrieval_function()
        elif e.resp.status == 400:
            if "pageToken" in request_kwargs and "Invalid Value" in str(e) and "pageToken" in str(e):
                logging.warning(f"Invalid page token: {request_kwargs['pageToken']}, retrying from start of request")
                request_kwargs.pop("pageToken")
                return _execute_single_retrieval(
                    retrieval_function,
                    continue_on_404_or_403,
                    **request_kwargs,
                )
            logging.error(f"Error executing request: {e}")
            raise e
        elif e.resp.status == 404 or e.resp.status == 403:
            if continue_on_404_or_403:
                logging.debug(f"Error executing request: {e}")
                results = {}
            else:
                raise e
        elif e.resp.status == 429:
            results = retrieval_function()
        else:
            logging.exception("Error executing request:")
            raise e
    except (TimeoutError, socket.timeout) as error:
        logging.warning(
            "Timed out executing Google API request; retrying with backoff. Details: %s",
            error,
        )
        results = retrieval_function()

    return results

```

## Detailed Analysis

### File Role in Repository

The file `common/data_source/google_util/util.py` is located in the `common/data_source/google_util` directory.

### Architecture Context

Files in this location typically handle concerns related to google_util.

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
- [auth.py](auth.py_docs.md)
- [constant.py](constant.py_docs.md)
- [oauth_flow.py](oauth_flow.py_docs.md)
- [resource.py](resource.py_docs.md)
- [util_threadpool_concurrency.py](util_threadpool_concurrency.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
