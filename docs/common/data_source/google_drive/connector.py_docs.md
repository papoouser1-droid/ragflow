# Documentation: common/data_source/google_drive/connector.py

## File Metadata

- **Path**: `common/data_source/google_drive/connector.py`
- **Size**: 60384 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `common/data_source/google_drive/connector.py`.

## Python Module Overview

### Module Docstring

```
Google Drive connector
```

### Imports and Dependencies

This module imports the following dependencies:

- `copy`
- `json`
- `logging`
- `os`
- `sys`
- `threading`
- `collections.abc`
- `enum`
- `functools`
- `typing`
- `urllib.parse`
- `google.auth.exceptions`
- `google.oauth2.credentials`
- `google.oauth2.service_account`
- `googleapiclient.errors`
- `typing_extensions`
- `common.data_source.config`
- `common.data_source.exceptions`
- `common.data_source.google_drive.doc_conversion`
- `common.data_source.google_drive.file_retrieval`

### Classes Defined

This file defines 4 class(es):

#### Class: `CredentialedRetrievalMethod` (line 98)

**Methods**: __call__

#### Class: `DriveIdStatus` (line 108)

#### Class: `GoogleDriveConnector` (line 114)

**Methods**: __init__, set_allow_images, primary_admin_email, google_domain, creds, load_credentials, _update_traversed_parent_ids, _get_all_user_emails, get_all_drive_ids, _get_all_drives_for_user, make_drive_id_getter, _impersonate_user_for_retrieval, _manage_service_account_retrieval, _determine_retrieval_ids, _oauth_retrieval_drives, _oauth_retrieval_folders, _load_from_checkpoint, _checkpointed_retrieval, _oauth_retrieval_all_files, _manage_oauth_retrieval, _fetch_drive_items, _extract_docs_from_google_drive, load_from_checkpoint, load_from_checkpoint_with_perm_sync, _extract_slim_docs_from_google_drive, retrieve_all_slim_docs_perm_sync, validate_connector_settings, build_dummy_checkpoint, validate_checkpoint_json

#### Class: `CheckpointOutputWrapper` (line 1174)

**Docstring**: Wraps a CheckpointOutput generator to give things back in a more digestible format.
The connector format is easier for the connector implementor (e.g. it enforces exactly
one new checkpoint is returne...

**Methods**: __init__, __call__

### Functions Defined

This file defines 45 function(s):

#### Function: `_extract_str_list_from_comma_str` (line 54)

**Parameters**: string

#### Function: `_extract_ids_from_urls` (line 60)

**Parameters**: urls

#### Function: `_clean_requested_drive_ids` (line 64)

**Parameters**: requested_drive_ids, requested_folder_ids, all_drive_ids_available

#### Function: `add_retrieval_info` (line 80)

**Parameters**: drive_files, user_email, completion_stage, parent_id

#### Function: `get_credentials_from_env` (line 1141)

**Parameters**: email, oauth

#### Function: `yield_all_docs_from_checkpoint_connector` (line 1214)

**Parameters**: connector, start, end

#### Function: `__call__` (line 99)

**Parameters**: self, field_type, checkpoint, start, end

#### Function: `__init__` (line 115)

**Parameters**: self, include_shared_drives, include_my_drives, include_files_shared_with_me, shared_drive_urls, my_drive_emails, shared_folder_urls, specific_user_emails, batch_size

#### Function: `set_allow_images` (line 173)

**Parameters**: self, value

#### Function: `primary_admin_email` (line 177)

**Parameters**: self

#### Function: `google_domain` (line 183)

**Parameters**: self

#### Function: `creds` (line 189)

**Parameters**: self

#### Function: `load_credentials` (line 195)

**Parameters**: self, credentials

#### Function: `_update_traversed_parent_ids` (line 217)

**Parameters**: self, folder_id

#### Function: `_get_all_user_emails` (line 220)

**Parameters**: self

#### Function: `get_all_drive_ids` (line 251)

**Parameters**: self

#### Function: `_get_all_drives_for_user` (line 254)

**Parameters**: self, user_email

#### Function: `make_drive_id_getter` (line 272)

**Parameters**: self, drive_ids, checkpoint

#### Function: `_impersonate_user_for_retrieval` (line 324)

**Parameters**: self, user_email, field_type, checkpoint, get_new_drive_id, sorted_filtered_folder_ids, start, end

#### Function: `_manage_service_account_retrieval` (line 510)

**Parameters**: self, field_type, checkpoint, start, end

**Docstring**: The current implementation of the service account retrieval does some
initial setup work using the primary admin email, then runs MAX_DRIVE_WORKERS
concurrent threads, each of which impersonates a dif...

#### Function: `_determine_retrieval_ids` (line 602)

**Parameters**: self, checkpoint, next_stage

#### Function: `_oauth_retrieval_drives` (line 637)

**Parameters**: self, field_type, drive_service, drive_ids_to_retrieve, checkpoint, start, end

#### Function: `_oauth_retrieval_folders` (line 689)

**Parameters**: self, field_type, drive_service, drive_ids_to_retrieve, folder_ids_to_retrieve, checkpoint, start, end

**Docstring**: If there are any remaining folder ids to retrieve found earlier in the
retrieval process, we recursively descend the file tree and retrieve all
files in the folder(s)....

#### Function: `_load_from_checkpoint` (line 742)

**Parameters**: self, start, end, checkpoint, include_permissions

**Docstring**: Entrypoint for the connector; first run is with an empty checkpoint....

#### Function: `_checkpointed_retrieval` (line 771)

**Parameters**: self, retrieval_method, field_type, checkpoint, start, end

#### Function: `_oauth_retrieval_all_files` (line 798)

**Parameters**: self, field_type, drive_service, start, end, page_token

#### Function: `_manage_oauth_retrieval` (line 832)

**Parameters**: self, field_type, checkpoint, start, end

#### Function: `_fetch_drive_items` (line 909)

**Parameters**: self, field_type, checkpoint, start, end

#### Function: `_extract_docs_from_google_drive` (line 926)

**Parameters**: self, checkpoint, start, end, include_permissions

**Docstring**: Retrieves and converts Google Drive files to documents....

#### Function: `load_from_checkpoint` (line 1020)

**Parameters**: self, start, end, checkpoint

#### Function: `load_from_checkpoint_with_perm_sync` (line 1029)

**Parameters**: self, start, end, checkpoint

#### Function: `_extract_slim_docs_from_google_drive` (line 1037)

**Parameters**: self, checkpoint, start, end, callback

#### Function: `retrieve_all_slim_docs_perm_sync` (line 1074)

**Parameters**: self, start, end, callback

#### Function: `validate_connector_settings` (line 1095)

**Parameters**: self

#### Function: `build_dummy_checkpoint` (line 1127)

**Parameters**: self

#### Function: `validate_checkpoint_json` (line 1137)

**Parameters**: self, checkpoint_json

#### Function: `__init__` (line 1182)

**Parameters**: self

#### Function: `__call__` (line 1185)

**Parameters**: self, checkpoint_connector_generator

#### Function: `get_available_drive_id` (line 289)

**Parameters**: thread_id

#### Function: `_yield_from_drive` (line 646)

**Parameters**: drive_id, drive_start

#### Function: `_yield_from_folder_crawl` (line 708)

**Parameters**: folder_id, folder_start

#### Function: `_inner_wrapper` (line 1194)

**Parameters**: checkpoint_connector_generator

#### Function: `_yield_from_drive` (line 408)

**Parameters**: drive_id, drive_start

#### Function: `_yield_from_folder_crawl` (line 461)

**Parameters**: folder_id, folder_start

#### Function: `_yield_batch` (line 958)

**Parameters**: files_batch

## Original Source Code

```py
"""Google Drive connector"""

import copy
import json
import logging
import os
import sys
import threading
from collections.abc import Callable, Generator, Iterator
from enum import Enum
from functools import partial
from typing import Any, Protocol, cast
from urllib.parse import urlparse

from google.auth.exceptions import RefreshError  # type: ignore  # type: ignore
from google.oauth2.credentials import Credentials as OAuthCredentials  # type: ignore  # type: ignore  # type: ignore
from google.oauth2.service_account import Credentials as ServiceAccountCredentials  # type: ignore  # type: ignore
from googleapiclient.errors import HttpError  # type: ignore  # type: ignore
from typing_extensions import override

from common.data_source.config import GOOGLE_DRIVE_CONNECTOR_SIZE_THRESHOLD, INDEX_BATCH_SIZE, SLIM_BATCH_SIZE, DocumentSource
from common.data_source.exceptions import ConnectorMissingCredentialError, ConnectorValidationError, CredentialExpiredError, InsufficientPermissionsError
from common.data_source.google_drive.doc_conversion import PermissionSyncContext, build_slim_document, convert_drive_item_to_document, onyx_document_id_from_drive_file
from common.data_source.google_drive.file_retrieval import (
    DriveFileFieldType,
    crawl_folders_for_files,
    get_all_files_for_oauth,
    get_all_files_in_my_drive_and_shared,
    get_files_in_shared_drive,
    get_root_folder_id,
)
from common.data_source.google_drive.model import DriveRetrievalStage, GoogleDriveCheckpoint, GoogleDriveFileType, RetrievedDriveFile, StageCompletion
from common.data_source.google_util.auth import get_google_creds
from common.data_source.google_util.constant import DB_CREDENTIALS_PRIMARY_ADMIN_KEY, MISSING_SCOPES_ERROR_STR, USER_FIELDS
from common.data_source.google_util.oauth_flow import ensure_oauth_token_dict
from common.data_source.google_util.resource import GoogleDriveService, get_admin_service, get_drive_service
from common.data_source.google_util.util import GoogleFields, execute_paginated_retrieval, get_file_owners
from common.data_source.google_util.util_threadpool_concurrency import ThreadSafeDict
from common.data_source.interfaces import (
    CheckpointedConnectorWithPermSync,
    IndexingHeartbeatInterface,
    SlimConnectorWithPermSync,
)
from common.data_source.models import CheckpointOutput, ConnectorFailure, Document, EntityFailure, GenerateSlimDocumentOutput, SecondsSinceUnixEpoch
from common.data_source.utils import datetime_from_string, parallel_yield, run_functions_tuples_in_parallel

MAX_DRIVE_WORKERS = int(os.environ.get("MAX_DRIVE_WORKERS", 4))
SHARED_DRIVE_PAGES_PER_CHECKPOINT = 2
MY_DRIVE_PAGES_PER_CHECKPOINT = 2
OAUTH_PAGES_PER_CHECKPOINT = 2
FOLDERS_PER_CHECKPOINT = 1


def _extract_str_list_from_comma_str(string: str | None) -> list[str]:
    if not string:
        return []
    return [s.strip() for s in string.split(",") if s.strip()]


def _extract_ids_from_urls(urls: list[str]) -> list[str]:
    return [urlparse(url).path.strip("/").split("/")[-1] for url in urls]


def _clean_requested_drive_ids(
    requested_drive_ids: set[str],
    requested_folder_ids: set[str],
    all_drive_ids_available: set[str],
) -> tuple[list[str], list[str]]:
    invalid_requested_drive_ids = requested_drive_ids - all_drive_ids_available
    filtered_folder_ids = requested_folder_ids - all_drive_ids_available
    if invalid_requested_drive_ids:
        logging.warning(f"Some shared drive IDs were not found. IDs: {invalid_requested_drive_ids}")
        logging.warning("Checking for folder access instead...")
        filtered_folder_ids.update(invalid_requested_drive_ids)

    valid_requested_drive_ids = requested_drive_ids - invalid_requested_drive_ids
    return sorted(valid_requested_drive_ids), sorted(filtered_folder_ids)


def add_retrieval_info(
    drive_files: Iterator[GoogleDriveFileType | str],
    user_email: str,
    completion_stage: DriveRetrievalStage,
    parent_id: str | None = None,
) -> Iterator[RetrievedDriveFile | str]:
    for file in drive_files:
        if isinstance(file, str):
            yield file
            continue
        yield RetrievedDriveFile(
            drive_file=file,
            user_email=user_email,
            parent_id=parent_id,
            completion_stage=completion_stage,
        )


class CredentialedRetrievalMethod(Protocol):
    def __call__(
        self,
        field_type: DriveFileFieldType,
        checkpoint: GoogleDriveCheckpoint,
        start: SecondsSinceUnixEpoch | None = None,
        end: SecondsSinceUnixEpoch | None = None,
    ) -> Iterator[RetrievedDriveFile]: ...


class DriveIdStatus(str, Enum):
    AVAILABLE = "available"
    IN_PROGRESS = "in_progress"
    FINISHED = "finished"


class GoogleDriveConnector(SlimConnectorWithPermSync, CheckpointedConnectorWithPermSync):
    def __init__(
        self,
        include_shared_drives: bool = False,
        include_my_drives: bool = False,
        include_files_shared_with_me: bool = False,
        shared_drive_urls: str | None = None,
        my_drive_emails: str | None = None,
        shared_folder_urls: str | None = None,
        specific_user_emails: str | None = None,
        batch_size: int = INDEX_BATCH_SIZE,
    ) -> None:
        if not any(
            (
                include_shared_drives,
                include_my_drives,
                include_files_shared_with_me,
                shared_folder_urls,
                my_drive_emails,
                shared_drive_urls,
            )
        ):
            raise ConnectorValidationError(
                "Nothing to index. Please specify at least one of the following: include_shared_drives, include_my_drives, include_files_shared_with_me, shared_folder_urls, or my_drive_emails"
            )

        specific_requests_made = False
        if bool(shared_drive_urls) or bool(my_drive_emails) or bool(shared_folder_urls):
            specific_requests_made = True
        self.specific_requests_made = specific_requests_made

        # NOTE: potentially modified in load_credentials if using service account
        self.include_files_shared_with_me = False if specific_requests_made else include_files_shared_with_me
        self.include_my_drives = False if specific_requests_made else include_my_drives
        self.include_shared_drives = False if specific_requests_made else include_shared_drives

        shared_drive_url_list = _extract_str_list_from_comma_str(shared_drive_urls)
        self._requested_shared_drive_ids = set(_extract_ids_from_urls(shared_drive_url_list))

        self._requested_my_drive_emails = set(_extract_str_list_from_comma_str(my_drive_emails))

        shared_folder_url_list = _extract_str_list_from_comma_str(shared_folder_urls)
        self._requested_folder_ids = set(_extract_ids_from_urls(shared_folder_url_list))
        self._specific_user_emails = _extract_str_list_from_comma_str(specific_user_emails)

        self._primary_admin_email: str | None = None

        self._creds: OAuthCredentials | ServiceAccountCredentials | None = None
        self._creds_dict: dict[str, Any] | None = None

        # ids of folders and shared drives that have been traversed
        self._retrieved_folder_and_drive_ids: set[str] = set()

        self.allow_images = False

        self.size_threshold = GOOGLE_DRIVE_CONNECTOR_SIZE_THRESHOLD

        self.logger = logging.getLogger(self.__class__.__name__)

    def set_allow_images(self, value: bool) -> None:
        self.allow_images = value

    @property
    def primary_admin_email(self) -> str:
        if self._primary_admin_email is None:
            raise RuntimeError("Primary admin email missing, should not call this property before calling load_credentials")
        return self._primary_admin_email

    @property
    def google_domain(self) -> str:
        if self._primary_admin_email is None:
            raise RuntimeError("Primary admin email missing, should not call this property before calling load_credentials")
        return self._primary_admin_email.split("@")[-1]

    @property
    def creds(self) -> OAuthCredentials | ServiceAccountCredentials:
        if self._creds is None:
            raise RuntimeError("Creds missing, should not call this property before calling load_credentials")
        return self._creds

    # TODO: ensure returned new_creds_dict is actually persisted when this is called?
    def load_credentials(self, credentials: dict[str, Any]) -> dict[str, Any] | None:
        try:
            self._primary_admin_email = credentials[DB_CREDENTIALS_PRIMARY_ADMIN_KEY]
        except KeyError:
            raise ValueError("Credentials json missing primary admin key")

        self._creds, new_creds_dict = get_google_creds(
            credentials=credentials,
            source=DocumentSource.GOOGLE_DRIVE,
        )

        # Service account connectors don't have a specific setting determining whether
        # to include "shared with me" for each user, so we default to true unless the connector
        # is in specific folders/drives mode. Note that shared files are only picked up during
        # the My Drive stage, so this does nothing if the connector is set to only index shared drives.
        if isinstance(self._creds, ServiceAccountCredentials) and not self.specific_requests_made:
            self.include_files_shared_with_me = True

        self._creds_dict = new_creds_dict

        return new_creds_dict

    def _update_traversed_parent_ids(self, folder_id: str) -> None:
        self._retrieved_folder_and_drive_ids.add(folder_id)

    def _get_all_user_emails(self) -> list[str]:
        if self._specific_user_emails:
            return self._specific_user_emails

        # Start with primary admin email
        user_emails = [self.primary_admin_email]

        # Only fetch additional users if using service account
        if isinstance(self.creds, OAuthCredentials):
            return user_emails

        admin_service = get_admin_service(
          

... [Content truncated - file is 60384 bytes] ...

   # TODO: move everything to load_from_checkpoint
                # and only fetch permissions if needed
                PermissionSyncContext(
                    primary_admin_email=self.primary_admin_email,
                    google_domain=self.google_domain,
                ),
            ):
                slim_batch.append(doc)
            if len(slim_batch) >= SLIM_BATCH_SIZE:
                yield slim_batch
                slim_batch = []
                if callback:
                    if callback.should_stop():
                        raise RuntimeError("_extract_slim_docs_from_google_drive: Stop signal detected")
                    callback.progress("_extract_slim_docs_from_google_drive", 1)
        yield slim_batch

    def retrieve_all_slim_docs_perm_sync(
        self,
        start: SecondsSinceUnixEpoch | None = None,
        end: SecondsSinceUnixEpoch | None = None,
        callback: IndexingHeartbeatInterface | None = None,
    ) -> GenerateSlimDocumentOutput:
        try:
            checkpoint = self.build_dummy_checkpoint()
            while checkpoint.completion_stage != DriveRetrievalStage.DONE:
                yield from self._extract_slim_docs_from_google_drive(
                    checkpoint=checkpoint,
                    start=start,
                    end=end,
                )
            self.logger.info("Drive perm sync: Slim doc retrieval complete")

        except Exception as e:
            if MISSING_SCOPES_ERROR_STR in str(e):
                raise PermissionError() from e
            raise e

    def validate_connector_settings(self) -> None:
        if self._creds is None:
            raise ConnectorMissingCredentialError("Google Drive credentials not loaded.")

        if self._primary_admin_email is None:
            raise ConnectorValidationError("Primary admin email not found in credentials. Ensure DB_CREDENTIALS_PRIMARY_ADMIN_KEY is set.")

        try:
            drive_service = get_drive_service(self._creds, self._primary_admin_email)
            drive_service.files().list(pageSize=1, fields="files(id)").execute()

            if isinstance(self._creds, ServiceAccountCredentials):
                # default is ~17mins of retries, don't do that here since this is called from
                # the UI
                get_root_folder_id(drive_service)

        except HttpError as e:
            status_code = e.resp.status if e.resp else None
            if status_code == 401:
                raise CredentialExpiredError("Invalid or expired Google Drive credentials (401).")
            elif status_code == 403:
                raise InsufficientPermissionsError("Google Drive app lacks required permissions (403). Please ensure the necessary scopes are granted and Drive apps are enabled.")
            else:
                raise ConnectorValidationError(f"Unexpected Google Drive error (status={status_code}): {e}")

        except Exception as e:
            # Check for scope-related hints from the error message
            if MISSING_SCOPES_ERROR_STR in str(e):
                raise InsufficientPermissionsError("Google Drive credentials are missing required scopes.")
            raise ConnectorValidationError(f"Unexpected error during Google Drive validation: {e}")

    @override
    def build_dummy_checkpoint(self) -> GoogleDriveCheckpoint:
        return GoogleDriveCheckpoint(
            retrieved_folder_and_drive_ids=set(),
            completion_stage=DriveRetrievalStage.START,
            completion_map=ThreadSafeDict(),
            all_retrieved_file_ids=set(),
            has_more=True,
        )

    @override
    def validate_checkpoint_json(self, checkpoint_json: str) -> GoogleDriveCheckpoint:
        return GoogleDriveCheckpoint.model_validate_json(checkpoint_json)


def get_credentials_from_env(email: str, oauth: bool = False) -> dict:
    try:
        if oauth:
            raw_credential_string = os.environ["GOOGLE_DRIVE_OAUTH_CREDENTIALS_JSON_STR"]
        else:
            raw_credential_string = os.environ["GOOGLE_DRIVE_SERVICE_ACCOUNT_JSON_STR"]
    except KeyError:
        raise ValueError("Missing Google Drive credentials in environment variables")

    try:
        credential_dict = json.loads(raw_credential_string)
    except json.JSONDecodeError:
        raise ValueError("Invalid JSON in Google Drive credentials")

    if oauth:
        credential_dict = ensure_oauth_token_dict(credential_dict, DocumentSource.GOOGLE_DRIVE)

    refried_credential_string = json.dumps(credential_dict)

    DB_CREDENTIALS_DICT_TOKEN_KEY = "google_tokens"
    DB_CREDENTIALS_DICT_SERVICE_ACCOUNT_KEY = "google_service_account_key"
    DB_CREDENTIALS_PRIMARY_ADMIN_KEY = "google_primary_admin"
    DB_CREDENTIALS_AUTHENTICATION_METHOD = "authentication_method"

    cred_key = DB_CREDENTIALS_DICT_TOKEN_KEY if oauth else DB_CREDENTIALS_DICT_SERVICE_ACCOUNT_KEY

    return {
        cred_key: refried_credential_string,
        DB_CREDENTIALS_PRIMARY_ADMIN_KEY: email,
        DB_CREDENTIALS_AUTHENTICATION_METHOD: "uploaded",
    }


class CheckpointOutputWrapper:
    """
    Wraps a CheckpointOutput generator to give things back in a more digestible format.
    The connector format is easier for the connector implementor (e.g. it enforces exactly
    one new checkpoint is returned AND that the checkpoint is at the end), thus the different
    formats.
    """

    def __init__(self) -> None:
        self.next_checkpoint: GoogleDriveCheckpoint | None = None

    def __call__(
        self,
        checkpoint_connector_generator: CheckpointOutput,
    ) -> Generator[
        tuple[Document | None, ConnectorFailure | None, GoogleDriveCheckpoint | None],
        None,
        None,
    ]:
        # grabs the final return value and stores it in the `next_checkpoint` variable
        def _inner_wrapper(
            checkpoint_connector_generator: CheckpointOutput,
        ) -> CheckpointOutput:
            self.next_checkpoint = yield from checkpoint_connector_generator
            return self.next_checkpoint  # not used

        for document_or_failure in _inner_wrapper(checkpoint_connector_generator):
            if isinstance(document_or_failure, Document):
                yield document_or_failure, None, None
            elif isinstance(document_or_failure, ConnectorFailure):
                yield None, document_or_failure, None
            else:
                raise ValueError(f"Invalid document_or_failure type: {type(document_or_failure)}")

        if self.next_checkpoint is None:
            raise RuntimeError("Checkpoint is None. This should never happen - the connector should always return a checkpoint.")

        yield None, None, self.next_checkpoint


def yield_all_docs_from_checkpoint_connector(
    connector: GoogleDriveConnector,
    start: SecondsSinceUnixEpoch,
    end: SecondsSinceUnixEpoch,
) -> Iterator[Document | ConnectorFailure]:
    num_iterations = 0

    checkpoint = connector.build_dummy_checkpoint()
    while checkpoint.has_more:
        doc_batch_generator = CheckpointOutputWrapper()(connector.load_from_checkpoint(start, end, checkpoint))
        for document, failure, next_checkpoint in doc_batch_generator:
            if failure is not None:
                yield failure
            if document is not None:
                yield document
            if next_checkpoint is not None:
                checkpoint = next_checkpoint

        num_iterations += 1
        if num_iterations > 100_000:
            raise RuntimeError("Too many iterations. Infinite loop?")


if __name__ == "__main__":
    import time

    logging.basicConfig(level=logging.DEBUG)

    try:
        # Get credentials from environment
        email = os.environ.get("GOOGLE_DRIVE_PRIMARY_ADMIN_EMAIL", "yongtengrey@gmail.com")
        creds = get_credentials_from_env(email, oauth=True)
        print("Credentials loaded successfully")
        print(f"{creds=}")

        connector = GoogleDriveConnector(
            include_shared_drives=False,
            shared_drive_urls=None,
            include_my_drives=True,
            my_drive_emails="yongtengrey@gmail.com",
            shared_folder_urls="https://drive.google.com/drive/folders/1fAKwbmf3U2oM139ZmnOzgIZHGkEwnpfy",
            include_files_shared_with_me=False,
            specific_user_emails=None,
        )
        print("GoogleDriveConnector initialized successfully")
        connector.load_credentials(creds)
        print("Credentials loaded into connector successfully")

        print("Google Drive connector is ready to use!")
        max_fsize = 0
        biggest_fsize = 0
        num_errors = 0
        docs_processed = 0
        start_time = time.time()
        with open("stats.txt", "w") as f:
            for num, doc_or_failure in enumerate(yield_all_docs_from_checkpoint_connector(connector, 0, time.time())):
                if num % 200 == 0:
                    f.write(f"Processed {num} files\n")
                    f.write(f"Max file size: {max_fsize / 1000_000:.2f} MB\n")
                    f.write(f"Time so far: {time.time() - start_time:.2f} seconds\n")
                    f.write(f"Docs per minute: {num / (time.time() - start_time) * 60:.2f}\n")
                    biggest_fsize = max(biggest_fsize, max_fsize)
                if isinstance(doc_or_failure, Document):
                    docs_processed += 1
                    max_fsize = max(max_fsize, doc_or_failure.size_bytes)
                    print(f"{doc_or_failure=}")
                elif isinstance(doc_or_failure, ConnectorFailure):
                    num_errors += 1
            print(f"Num errors: {num_errors}")
            print(f"Biggest file size: {biggest_fsize / 1000_000:.2f} MB")
            print(f"Time taken: {time.time() - start_time:.2f} seconds")
            print(f"Total documents produced: {docs_processed}")

    except Exception as e:
        print(f"Error: {e}")
        import traceback

        traceback.print_exc()
        sys.exit(1)

```

## Detailed Analysis

### File Role in Repository

The file `common/data_source/google_drive/connector.py` is located in the `common/data_source/google_drive` directory.

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
- [constant.py](constant.py_docs.md)
- [doc_conversion.py](doc_conversion.py_docs.md)
- [file_retrieval.py](file_retrieval.py_docs.md)
- [model.py](model.py_docs.md)
- [section_extraction.py](section_extraction.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
