# Documentation: common/data_source/google_drive/file_retrieval.py

## File Metadata

- **Path**: `common/data_source/google_drive/file_retrieval.py`
- **Size**: 13840 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `common/data_source/google_drive/file_retrieval.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `collections.abc`
- `datetime`
- `enum`
- `googleapiclient.discovery`
- `googleapiclient.errors`
- `common.data_source.google_drive.constant`
- `common.data_source.google_drive.model`
- `common.data_source.google_util.resource`
- `common.data_source.google_util.util`
- `common.data_source.models`

### Classes Defined

This file defines 1 class(es):

#### Class: `DriveFileFieldType` (line 24)

**Docstring**: Enum to specify which fields to retrieve from Google Drive files...

### Functions Defined

This file defines 9 function(s):

#### Function: `generate_time_range_filter` (line 32)

**Parameters**: start, end

#### Function: `_get_folders_in_parent` (line 46)

**Parameters**: service, parent_id

#### Function: `_get_fields_for_file_type` (line 70)

**Parameters**: field_type

**Docstring**: Get the appropriate fields string based on the field type enum...

#### Function: `_get_files_in_parent` (line 80)

**Parameters**: service, parent_id, field_type, start, end

#### Function: `crawl_folders_for_files` (line 107)

**Parameters**: service, parent_id, field_type, user_email, traversed_parent_ids, update_traversed_ids_func, start, end

**Docstring**: This function starts crawling from any folder. It is slower though....

#### Function: `get_files_in_shared_drive` (line 182)

**Parameters**: service, drive_id, field_type, max_num_pages, update_traversed_ids_func, cache_folders, start, end, page_token

#### Function: `get_all_files_in_my_drive_and_shared` (line 244)

**Parameters**: service, update_traversed_ids_func, field_type, include_shared_with_me, max_num_pages, start, end, cache_folders, page_token

#### Function: `get_all_files_for_oauth` (line 298)

**Parameters**: service, include_files_shared_with_me, include_my_drives, include_shared_drives, field_type, max_num_pages, start, end, page_token

#### Function: `get_root_folder_id` (line 343)

**Parameters**: service

## Original Source Code

```py
import logging
from collections.abc import Callable, Iterator
from datetime import datetime, timezone
from enum import Enum

from googleapiclient.discovery import Resource  # type: ignore
from googleapiclient.errors import HttpError  # type: ignore

from common.data_source.google_drive.constant import DRIVE_FOLDER_TYPE, DRIVE_SHORTCUT_TYPE
from common.data_source.google_drive.model import DriveRetrievalStage, GoogleDriveFileType, RetrievedDriveFile
from common.data_source.google_util.resource import GoogleDriveService
from common.data_source.google_util.util import ORDER_BY_KEY, PAGE_TOKEN_KEY, GoogleFields, execute_paginated_retrieval, execute_paginated_retrieval_with_max_pages
from common.data_source.models import SecondsSinceUnixEpoch

PERMISSION_FULL_DESCRIPTION = "permissions(id, emailAddress, type, domain, permissionDetails)"

FILE_FIELDS = "nextPageToken, files(mimeType, id, name, modifiedTime, webViewLink, shortcutDetails, owners(emailAddress), size)"
FILE_FIELDS_WITH_PERMISSIONS = f"nextPageToken, files(mimeType, id, name, {PERMISSION_FULL_DESCRIPTION}, permissionIds, modifiedTime, webViewLink, shortcutDetails, owners(emailAddress), size)"
SLIM_FILE_FIELDS = f"nextPageToken, files(mimeType, driveId, id, name, {PERMISSION_FULL_DESCRIPTION}, permissionIds, webViewLink, owners(emailAddress), modifiedTime)"

FOLDER_FIELDS = "nextPageToken, files(id, name, permissions, modifiedTime, webViewLink, shortcutDetails)"


class DriveFileFieldType(Enum):
    """Enum to specify which fields to retrieve from Google Drive files"""

    SLIM = "slim"  # Minimal fields for basic file info
    STANDARD = "standard"  # Standard fields including content metadata
    WITH_PERMISSIONS = "with_permissions"  # Full fields including permissions


def generate_time_range_filter(
    start: SecondsSinceUnixEpoch | None = None,
    end: SecondsSinceUnixEpoch | None = None,
) -> str:
    time_range_filter = ""
    if start is not None:
        time_start = datetime.fromtimestamp(start, tz=timezone.utc).isoformat()
        time_range_filter += f" and {GoogleFields.MODIFIED_TIME.value} > '{time_start}'"
    if end is not None:
        time_stop = datetime.fromtimestamp(end, tz=timezone.utc).isoformat()
        time_range_filter += f" and {GoogleFields.MODIFIED_TIME.value} <= '{time_stop}'"
    return time_range_filter


def _get_folders_in_parent(
    service: Resource,
    parent_id: str | None = None,
) -> Iterator[GoogleDriveFileType]:
    # Follow shortcuts to folders
    query = f"(mimeType = '{DRIVE_FOLDER_TYPE}' or mimeType = '{DRIVE_SHORTCUT_TYPE}')"
    query += " and trashed = false"

    if parent_id:
        query += f" and '{parent_id}' in parents"

    for file in execute_paginated_retrieval(
        retrieval_function=service.files().list,
        list_key="files",
        continue_on_404_or_403=True,
        corpora="allDrives",
        supportsAllDrives=True,
        includeItemsFromAllDrives=True,
        fields=FOLDER_FIELDS,
        q=query,
    ):
        yield file


def _get_fields_for_file_type(field_type: DriveFileFieldType) -> str:
    """Get the appropriate fields string based on the field type enum"""
    if field_type == DriveFileFieldType.SLIM:
        return SLIM_FILE_FIELDS
    elif field_type == DriveFileFieldType.WITH_PERMISSIONS:
        return FILE_FIELDS_WITH_PERMISSIONS
    else:  # DriveFileFieldType.STANDARD
        return FILE_FIELDS


def _get_files_in_parent(
    service: Resource,
    parent_id: str,
    field_type: DriveFileFieldType,
    start: SecondsSinceUnixEpoch | None = None,
    end: SecondsSinceUnixEpoch | None = None,
) -> Iterator[GoogleDriveFileType]:
    query = f"mimeType != '{DRIVE_FOLDER_TYPE}' and '{parent_id}' in parents"
    query += " and trashed = false"
    query += generate_time_range_filter(start, end)

    kwargs = {ORDER_BY_KEY: GoogleFields.MODIFIED_TIME.value}

    for file in execute_paginated_retrieval(
        retrieval_function=service.files().list,
        list_key="files",
        continue_on_404_or_403=True,
        corpora="allDrives",
        supportsAllDrives=True,
        includeItemsFromAllDrives=True,
        fields=_get_fields_for_file_type(field_type),
        q=query,
        **kwargs,
    ):
        yield file


def crawl_folders_for_files(
    service: Resource,
    parent_id: str,
    field_type: DriveFileFieldType,
    user_email: str,
    traversed_parent_ids: set[str],
    update_traversed_ids_func: Callable[[str], None],
    start: SecondsSinceUnixEpoch | None = None,
    end: SecondsSinceUnixEpoch | None = None,
) -> Iterator[RetrievedDriveFile]:
    """
    This function starts crawling from any folder. It is slower though.
    """
    logging.info("Entered crawl_folders_for_files with parent_id: " + parent_id)
    if parent_id not in traversed_parent_ids:
        logging.info("Parent id not in traversed parent ids, getting files")
        found_files = False
        file = {}
        try:
            for file in _get_files_in_parent(
                service=service,
                parent_id=parent_id,
                field_type=field_type,
                start=start,
                end=end,
            ):
                logging.info(f"Found file: {file['name']}, user email: {user_email}")
                found_files = True
                yield RetrievedDriveFile(
                    drive_file=file,
                    user_email=user_email,
                    parent_id=parent_id,
                    completion_stage=DriveRetrievalStage.FOLDER_FILES,
                )
            # Only mark a folder as done if it was fully traversed without errors
            # This usually indicates that the owner of the folder was impersonated.
            # In cases where this never happens, most likely the folder owner is
            # not part of the google workspace in question (or for oauth, the authenticated
            # user doesn't own the folder)
            if found_files:
                update_traversed_ids_func(parent_id)
        except Exception as e:
            if isinstance(e, HttpError) and e.status_code == 403:
                # don't yield an error here because this is expected behavior
                # when a user doesn't have access to a folder
                logging.debug(f"Error getting files in parent {parent_id}: {e}")
            else:
                logging.error(f"Error getting files in parent {parent_id}: {e}")
                yield RetrievedDriveFile(
                    drive_file=file,
                    user_email=user_email,
                    parent_id=parent_id,
                    completion_stage=DriveRetrievalStage.FOLDER_FILES,
                    error=e,
                )
    else:
        logging.info(f"Skipping subfolder files since already traversed: {parent_id}")

    for subfolder in _get_folders_in_parent(
        service=service,
        parent_id=parent_id,
    ):
        logging.info("Fetching all files in subfolder: " + subfolder["name"])
        yield from crawl_folders_for_files(
            service=service,
            parent_id=subfolder["id"],
            field_type=field_type,
            user_email=user_email,
            traversed_parent_ids=traversed_parent_ids,
            update_traversed_ids_func=update_traversed_ids_func,
            start=start,
            end=end,
        )


def get_files_in_shared_drive(
    service: Resource,
    drive_id: str,
    field_type: DriveFileFieldType,
    max_num_pages: int,
    update_traversed_ids_func: Callable[[str], None] = lambda _: None,
    cache_folders: bool = True,
    start: SecondsSinceUnixEpoch | None = None,
    end: SecondsSinceUnixEpoch | None = None,
    page_token: str | None = None,
) -> Iterator[GoogleDriveFileType | str]:
    kwargs = {ORDER_BY_KEY: GoogleFields.MODIFIED_TIME.value}
    if page_token:
        logging.info(f"Using page token: {page_token}")
        kwargs[PAGE_TOKEN_KEY] = page_token

    if cache_folders:
        # If we know we are going to folder crawl later, we can cache the folders here
        # Get all folders being queried and add them to the traversed set
        folder_query = f"mimeType = '{DRIVE_FOLDER_TYPE}'"
        folder_query += " and trashed = false"
        for folder in execute_paginated_retrieval(
            retrieval_function=service.files().list,
            list_key="files",
            continue_on_404_or_403=True,
            corpora="drive",
            driveId=drive_id,
            supportsAllDrives=True,
            includeItemsFromAllDrives=True,
            fields="nextPageToken, files(id)",
            q=folder_query,
        ):
            update_traversed_ids_func(folder["id"])

    # Get all files in the shared drive
    file_query = f"mimeType != '{DRIVE_FOLDER_TYPE}'"
    file_query += " and trashed = false"
    file_query += generate_time_range_filter(start, end)

    for file in execute_paginated_retrieval_with_max_pages(
        retrieval_function=service.files().list,
        max_num_pages=max_num_pages,
        list_key="files",
        continue_on_404_or_403=True,
        corpora="drive",
        driveId=drive_id,
        supportsAllDrives=True,
        includeItemsFromAllDrives=True,
        fields=_get_fields_for_file_type(field_type),
        q=file_query,
        **kwargs,
    ):
        # If we found any files, mark this drive as traversed. When a user has access to a drive,
        # they have access to all the files in the drive. Also not a huge deal if we re-traverse
        # empty drives.
        # NOTE: ^^ the above is not actually true due to folder restrictions:
        # https://support.google.com/a/users/answer/12380484?hl=en
        # So we may have to change this logic for people who use folder restrictions.
        update_traversed_ids_func(drive_id)
        yield file


def get_all_files_in_my_drive_and_shared(
    service: GoogleDriveService,
    update_traversed_ids_func: Callable,
    field_type: DriveFileFieldType,
    include_shared_with_me: bool,
    max_num_pages: int,
    start: SecondsSinceUnixEpoch | None = None,
    end: SecondsSinceUnixEpoch | None = None,
    cache_folders: bool = True,
    page_token: str | None = None,
) -> Iterator[GoogleDriveFileType | str]:
    kwargs = {ORDER_BY_KEY: GoogleFields.MODIFIED_TIME.value}
    if page_token:
        logging.info(f"Using page token: {page_token}")
        kwargs[PAGE_TOKEN_KEY] = page_token

    if cache_folders:
        # If we know we are going to folder crawl later, we can cache the folders here
        # Get all folders being queried and add them to the traversed set
        folder_query = f"mimeType = '{DRIVE_FOLDER_TYPE}'"
        folder_query += " and trashed = false"
        if not include_shared_with_me:
            folder_query += " and 'me' in owners"
        found_folders = False
        for folder in execute_paginated_retrieval(
            retrieval_function=service.files().list,
            list_key="files",
            corpora="user",
            fields=_get_fields_for_file_type(field_type),
            q=folder_query,
        ):
            update_traversed_ids_func(folder[GoogleFields.ID])
            found_folders = True
        if found_folders:
            update_traversed_ids_func(get_root_folder_id(service))

    # Then get the files
    file_query = f"mimeType != '{DRIVE_FOLDER_TYPE}'"
    file_query += " and trashed = false"
    if not include_shared_with_me:
        file_query += " and 'me' in owners"
    file_query += generate_time_range_filter(start, end)
    yield from execute_paginated_retrieval_with_max_pages(
        retrieval_function=service.files().list,
        max_num_pages=max_num_pages,
        list_key="files",
        continue_on_404_or_403=False,
        corpora="user",
        fields=_get_fields_for_file_type(field_type),
        q=file_query,
        **kwargs,
    )


def get_all_files_for_oauth(
    service: GoogleDriveService,
    include_files_shared_with_me: bool,
    include_my_drives: bool,
    # One of the above 2 should be true
    include_shared_drives: bool,
    field_type: DriveFileFieldType,
    max_num_pages: int,
    start: SecondsSinceUnixEpoch | None = None,
    end: SecondsSinceUnixEpoch | None = None,
    page_token: str | None = None,
) -> Iterator[GoogleDriveFileType | str]:
    kwargs = {ORDER_BY_KEY: GoogleFields.MODIFIED_TIME.value}
    if page_token:
        logging.info(f"Using page token: {page_token}")
        kwargs[PAGE_TOKEN_KEY] = page_token

    should_get_all = include_shared_drives and include_my_drives and include_files_shared_with_me
    corpora = "allDrives" if should_get_all else "user"

    file_query = f"mimeType != '{DRIVE_FOLDER_TYPE}'"
    file_query += " and trashed = false"
    file_query += generate_time_range_filter(start, end)

    if not should_get_all:
        if include_files_shared_with_me and not include_my_drives:
            file_query += " and not 'me' in owners"
        if not include_files_shared_with_me and include_my_drives:
            file_query += " and 'me' in owners"

    yield from execute_paginated_retrieval_with_max_pages(
        max_num_pages=max_num_pages,
        retrieval_function=service.files().list,
        list_key="files",
        continue_on_404_or_403=False,
        corpora=corpora,
        includeItemsFromAllDrives=should_get_all,
        supportsAllDrives=should_get_all,
        fields=_get_fields_for_file_type(field_type),
        q=file_query,
        **kwargs,
    )


# Just in case we need to get the root folder id
def get_root_folder_id(service: Resource) -> str:
    # we dont paginate here because there is only one root folder per user
    # https://developers.google.com/drive/api/guides/v2-to-v3-reference
    return service.files().get(fileId="root", fields=GoogleFields.ID.value).execute()[GoogleFields.ID.value]

```

## Detailed Analysis

### File Role in Repository

The file `common/data_source/google_drive/file_retrieval.py` is located in the `common/data_source/google_drive` directory.

### Architecture Context

Files in this location typically handle concerns related to google_drive.

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
- [connector.py](connector.py_docs.md)
- [constant.py](constant.py_docs.md)
- [doc_conversion.py](doc_conversion.py_docs.md)
- [model.py](model.py_docs.md)
- [section_extraction.py](section_extraction.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
