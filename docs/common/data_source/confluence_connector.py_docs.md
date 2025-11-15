# Documentation: common/data_source/confluence_connector.py

## File Metadata

- **Path**: `common/data_source/confluence_connector.py`
- **Size**: 81000 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `common/data_source/confluence_connector.py`.

## Python Module Overview

### Module Docstring

```
Confluence connector
```

### Imports and Dependencies

This module imports the following dependencies:

- `copy`
- `json`
- `logging`
- `time`
- `datetime`
- `pathlib`
- `typing`
- `requests`
- `typing_extensions`
- `urllib.parse`
- `bs4`
- `atlassian.errors`
- `atlassian`
- `requests.exceptions`
- `common.data_source.config`
- `common.data_source.exceptions`
- `common.data_source.html_utils`
- `common.data_source.interfaces`
- `common.data_source.models`
- `common.data_source.utils`

### Classes Defined

This file defines 4 class(es):

#### Class: `ConfluenceCheckpoint` (line 54)

#### Class: `ConfluenceRateLimitError` (line 59)

#### Class: `OnyxConfluence` (line 63)

**Docstring**: This is a custom Confluence class that:

A. overrides the default Confluence class to add a custom CQL method.
B.
This is necessary because the default Confluence class does not properly support cql e...

**Methods**: __init__, _renew_credentials, _make_oauth2_dict, _probe_connection, _initialize_connection, _initialize_connection_helper, _make_rate_limited_confluence_method, __getattr__, _try_one_by_one_for_paginated_url, _paginate_url, build_cql_url, paginated_cql_retrieval, paginated_page_retrieval, cql_paginate_all_expansions, paginated_cql_user_retrieval, paginated_groups_by_user_retrieval, paginated_groups_retrieval, paginated_group_members_retrieval, get_all_space_permissions_server, get_current_user

#### Class: `ConfluenceConnector` (line 1272)

**Methods**: __init__, set_allow_images, _adjust_start_for_query, _is_newer_than_start, confluence_client, low_timeout_confluence_client, set_credentials_provider, load_credentials, _construct_page_cql_query, _construct_attachment_query, _get_comment_string_for_page_id, _convert_page_to_document, _fetch_page_attachments, _fetch_document_batches, _build_page_retrieval_url, load_from_checkpoint, build_dummy_checkpoint, validate_checkpoint_json, retrieve_all_slim_docs, retrieve_all_slim_docs_perm_sync, _retrieve_all_slim_docs, validate_connector_settings

### Functions Defined

This file defines 57 function(s):

#### Function: `get_user_email_from_username__server` (line 853)

**Parameters**: confluence_client, user_name

#### Function: `_get_user` (line 873)

**Parameters**: confluence_client, user_id

**Docstring**: Get Confluence Display Name based on the account-id or userkey value

Args:
    user_id (str): The user id (i.e: the account-id or userkey)
    confluence_client (Confluence): The Confluence Client

R...

#### Function: `sanitize_attachment_title` (line 903)

**Parameters**: title

**Docstring**: Sanitize the attachment title to be a valid HTML attribute....

#### Function: `extract_text_from_confluence_html` (line 910)

**Parameters**: confluence_client, confluence_object, fetched_titles

**Docstring**: Parse a Confluence html page and replace the 'user Id' by the real
    User Display Name

Args:
    confluence_object (dict): The confluence object as a dict
    confluence_client (Confluence): Conflu...

#### Function: `_remove_macro_stylings` (line 1025)

**Parameters**: soup

#### Function: `get_page_restrictions` (line 1037)

**Parameters**: confluence_client, page_id, page_restrictions, ancestors

**Docstring**: Get page access restrictions for a Confluence page.
This functionality requires Enterprise Edition.

Args:
    confluence_client: OnyxConfluence client instance
    page_id: The ID of the page
    pag...

#### Function: `get_all_space_permissions` (line 1074)

**Parameters**: confluence_client, is_cloud

**Docstring**: Get access permissions for all spaces in Confluence.
This functionality requires Enterprise Edition.

Args:
    confluence_client: OnyxConfluence client instance
    is_cloud: Whether this is a Conflu...

#### Function: `_make_attachment_link` (line 1106)

**Parameters**: confluence_client, attachment, parent_content_id

#### Function: `_process_image_attachment` (line 1131)

**Parameters**: confluence_client, attachment, raw_bytes, media_type

**Docstring**: Process an image attachment by saving it without generating a summary....

#### Function: `process_attachment` (line 1141)

**Parameters**: confluence_client, attachment, parent_content_id, allow_images

**Docstring**: Processes a Confluence attachment. If it's a document, extracts text,
or if it's an image, stores it for later analysis. Returns a structured result....

#### Function: `convert_attachment_to_content` (line 1242)

**Parameters**: confluence_client, attachment, page_id, allow_images

**Docstring**: Facade function which:
  1. Validates attachment type
  2. Extracts content or stores image for later processing
  3. Returns (content_text, stored_file_name) or None if we should skip it...

#### Function: `__init__` (line 77)

**Parameters**: self, is_cloud, url, credentials_provider, timeout, scoped_token, confluence_user_profiles_override

#### Function: `_renew_credentials` (line 126)

**Parameters**: self

**Docstring**: credential_json - the current json credentials
Returns a tuple
1. The up to date credentials
2. True if the credentials were updated

This method is intended to be used within a distributed lock.
Lock...

#### Function: `_make_oauth2_dict` (line 196)

**Parameters**: credentials

#### Function: `_probe_connection` (line 206)

**Parameters**: self

#### Function: `_initialize_connection` (line 299)

**Parameters**: self

**Docstring**: Called externally to init the connection in a thread safe manner....

#### Function: `_initialize_connection_helper` (line 312)

**Parameters**: self, credentials

**Docstring**: Called internally to init the connection. Distributed locking
to prevent multiple threads from modifying the credentials
must be handled around this function....

#### Function: `_make_rate_limited_confluence_method` (line 353)

**Parameters**: self, name, credential_provider

#### Function: `__getattr__` (line 418)

**Parameters**: self, name

**Docstring**: Dynamically intercept attribute/method access....

#### Function: `_try_one_by_one_for_paginated_url` (line 442)

**Parameters**: self, url_suffix, initial_start, limit

**Docstring**: Go through `limit` items, starting at `initial_start` one by one (e.g. using
`limit=1` for each call).

If we encounter an error, we skip the item and try the next one. We will return
the items we wer...

#### Function: `_paginate_url` (line 498)

**Parameters**: self, url_suffix, limit, next_page_callback, force_offset_pagination

**Docstring**: This will paginate through the top level query....

#### Function: `build_cql_url` (line 649)

**Parameters**: self, cql, expand

#### Function: `paginated_cql_retrieval` (line 653)

**Parameters**: self, cql, expand, limit

**Docstring**: The content/search endpoint can be used to fetch pages, attachments, and comments....

#### Function: `paginated_page_retrieval` (line 665)

**Parameters**: self, cql_url, limit, next_page_callback

**Docstring**: Error handling (and testing) wrapper for _paginate_url,
because the current approach to page retrieval involves handling the
next page links manually....

#### Function: `cql_paginate_all_expansions` (line 685)

**Parameters**: self, cql, expand, limit

**Docstring**: This function will paginate through the top level query first, then
paginate through all of the expansions....

#### Function: `paginated_cql_user_retrieval` (line 712)

**Parameters**: self, expand, limit

**Docstring**: The search/user endpoint can be used to fetch users.
It's a separate endpoint from the content/search endpoint used only for users.
Otherwise it's very similar to the content/search endpoint....

#### Function: `paginated_groups_by_user_retrieval` (line 755)

**Parameters**: self, user_id, limit

**Docstring**: This is not an SQL like query.
It's a confluence specific endpoint that can be used to fetch groups....

#### Function: `paginated_groups_retrieval` (line 772)

**Parameters**: self, limit

**Docstring**: This is not an SQL like query.
It's a confluence specific endpoint that can be used to fetch groups....

#### Function: `paginated_group_members_retrieval` (line 782)

**Parameters**: self, group_name, limit

**Docstring**: This is not an SQL like query.
It's a confluence specific endpoint that can be used to fetch the members of a group.
THIS DOESN'T WORK FOR SERVER because it breaks when there is a slash in the group n...

#### Function: `get_all_space_permissions_server` (line 796)

**Parameters**: self, space_key

**Docstring**: This is a confluence server specific method that can be used to
fetch the permissions of a space.
This is better logging than calling the get_space_permissions method
because it returns a jsonrpc resp...

#### Function: `get_current_user` (line 826)

**Parameters**: self, expand

**Docstring**: Implements a method that isn't in the third party client.

Get information about the current user
:param expand: OPTIONAL expand for get status of user.
        Possible param is "status". Results are...

#### Function: `__init__` (line 1278)

**Parameters**: self, wiki_base, is_cloud, space, page_id, index_recursively, cql_query, batch_size, continue_on_failure, labels_to_skip, timezone_offset, time_buffer_seconds, scoped_token

#### Function: `set_allow_images` (line 1358)

**Parameters**: self, value

#### Function: `_adjust_start_for_query` (line 1362)

**Parameters**: self, start

#### Function: `_is_newer_than_start` (line 1371)

**Parameters**: self, doc_time, start

#### Function: `confluence_client` (line 1381)

**Parameters**: self

#### Function: `low_timeout_confluence_client` (line 1387)

**Parameters**: self

#### Function: `set_credentials_provider` (line 1392)

**Parameters**: self, credentials_provider

#### Function: `load_credentials` (line 1422)

**Parameters**: self, credentials

#### Function: `_construct_page_cql_query` (line 1425)

**Parameters**: self, start, end

**Docstring**: Constructs a CQL query for use in the confluence API. See
https://developer.atlassian.com/server/confluence/advanced-searching-using-cql/
for more information. This is JUST the CQL, not the full URL u...

#### Function: `_construct_attachment_query` (line 1453)

**Parameters**: self, confluence_page_id, start, end

#### Function: `_get_comment_string_for_page_id` (line 1478)

**Parameters**: self, page_id

#### Function: `_convert_page_to_document` (line 1496)

**Parameters**: self, page

**Docstring**: Converts a Confluence page to a Document object.
Includes the page content, comments, and attachments....

#### Function: `_fetch_page_attachments` (line 1579)

**Parameters**: self, page, start, end

**Docstring**: Inline attachments are added directly to the document as text or image sections by
this function. The returned documents/connectorfailures are for non-inline attachments
and those at the end of the pa...

#### Function: `_fetch_document_batches` (line 1719)

**Parameters**: self, checkpoint, start, end

**Docstring**: Yields batches of Documents. For each page:
 - Create a Document with 1 Section for the page text/comments
 - Then fetch attachments. For each attachment:
     - Attempt to convert it with convert_att...

#### Function: `_build_page_retrieval_url` (line 1776)

**Parameters**: self, start, end, limit

**Docstring**: Builds the full URL used to retrieve pages from the confluence API.
This can be used as input to the confluence client's _paginate_url
or paginated_page_retrieval methods....

#### Function: `load_from_checkpoint` (line 1794)

**Parameters**: self, start, end, checkpoint

#### Function: `build_dummy_checkpoint` (line 1814)

**Parameters**: self

#### Function: `validate_checkpoint_json` (line 1818)

**Parameters**: self, checkpoint_json

#### Function: `retrieve_all_slim_docs` (line 1822)

**Parameters**: self, start, end, callback

#### Function: `retrieve_all_slim_docs_perm_sync` (line 1835)

**Parameters**: self, start, end, callback

**Docstring**: Return 'slim' docs (IDs + minimal permission data).
Does not fetch actual text. Used primarily for incremental permission sync....

#### Function: `_retrieve_all_slim_docs` (line 1852)

**Parameters**: self, start, end, callback, include_permissions

#### Function: `validate_connector_settings` (line 1954)

**Parameters**: self

#### Function: `wrapped_call` (line 356)

**Parameters**: None

#### Function: `_traverse_and_update` (line 696)

**Parameters**: data

#### Function: `store_next_page_url` (line 1742)

**Parameters**: next_page_url

#### Function: `get_external_access` (line 1868)

**Parameters**: doc_id, restrictions, ancestors

## Original Source Code

```py


"""Confluence connector"""
import copy
import json
import logging
import time
from datetime import datetime, timezone, timedelta
from pathlib import Path
from typing import Any, cast, Iterator, Callable, Generator

import requests
from typing_extensions import override
from urllib.parse import quote

import bs4
from atlassian.errors import ApiError
from atlassian import Confluence
from requests.exceptions import HTTPError

from common.data_source.config import INDEX_BATCH_SIZE, DocumentSource, CONTINUE_ON_CONNECTOR_FAILURE, \
    CONFLUENCE_CONNECTOR_LABELS_TO_SKIP, CONFLUENCE_TIMEZONE_OFFSET, CONFLUENCE_CONNECTOR_USER_PROFILES_OVERRIDE, \
    CONFLUENCE_SYNC_TIME_BUFFER_SECONDS, \
    OAUTH_CONFLUENCE_CLOUD_CLIENT_ID, OAUTH_CONFLUENCE_CLOUD_CLIENT_SECRET, _DEFAULT_PAGINATION_LIMIT, \
    _PROBLEMATIC_EXPANSIONS, _REPLACEMENT_EXPANSIONS, _USER_NOT_FOUND, _COMMENT_EXPANSION_FIELDS, \
    _ATTACHMENT_EXPANSION_FIELDS, _PAGE_EXPANSION_FIELDS, ONE_DAY, ONE_HOUR, _RESTRICTIONS_EXPANSION_FIELDS, \
    _SLIM_DOC_BATCH_SIZE, CONFLUENCE_CONNECTOR_ATTACHMENT_SIZE_THRESHOLD
from common.data_source.exceptions import (
    ConnectorMissingCredentialError,
    ConnectorValidationError,
    InsufficientPermissionsError,
    UnexpectedValidationError, CredentialExpiredError
)
from common.data_source.html_utils import format_document_soup
from common.data_source.interfaces import (
    ConnectorCheckpoint,
    CredentialsConnector,
    SecondsSinceUnixEpoch,
    SlimConnectorWithPermSync, StaticCredentialsProvider, CheckpointedConnector, SlimConnector,
    CredentialsProviderInterface, ConfluenceUser, IndexingHeartbeatInterface, AttachmentProcessingResult,
    CheckpointOutput
)
from common.data_source.models import ConnectorFailure, Document, TextSection, ImageSection, BasicExpertInfo, \
    DocumentFailure, GenerateSlimDocumentOutput, SlimDocument, ExternalAccess
from common.data_source.utils import load_all_docs_from_checkpoint_connector, scoped_url, \
    process_confluence_user_profiles_override, confluence_refresh_tokens, run_with_timeout, _handle_http_error, \
    update_param_in_path, get_start_param_from_url, build_confluence_document_id, datetime_from_string, \
    is_atlassian_date_error, validate_attachment_filetype
from rag.utils.redis_conn import RedisDB, REDIS_CONN

_USER_ID_TO_DISPLAY_NAME_CACHE: dict[str, str | None] = {}
_USER_EMAIL_CACHE: dict[str, str | None] = {}

class ConfluenceCheckpoint(ConnectorCheckpoint):

    next_page_url: str | None


class ConfluenceRateLimitError(Exception):
    pass


class OnyxConfluence:
    """
    This is a custom Confluence class that:

    A. overrides the default Confluence class to add a custom CQL method.
    B.
    This is necessary because the default Confluence class does not properly support cql expansions.
    All methods are automatically wrapped with handle_confluence_rate_limit.
    """

    CREDENTIAL_PREFIX = "connector:confluence:credential"
    CREDENTIAL_TTL = 300  # 5 min
    PROBE_TIMEOUT = 5  # 5 seconds

    def __init__(
        self,
        is_cloud: bool,
        url: str,
        credentials_provider: CredentialsProviderInterface,
        timeout: int | None = None,
        scoped_token: bool = False,
        # should generally not be passed in, but making it overridable for
        # easier testing
        confluence_user_profiles_override: list[dict[str, str]] | None = (
            CONFLUENCE_CONNECTOR_USER_PROFILES_OVERRIDE
        ),
    ) -> None:
        self.base_url = url  #'/'.join(url.rstrip("/").split("/")[:-1])
        url = scoped_url(url, "confluence") if scoped_token else url

        self._is_cloud = is_cloud
        self._url = url.rstrip("/")
        self._credentials_provider = credentials_provider
        self.scoped_token = scoped_token
        self.redis_client: RedisDB | None = None
        self.static_credentials: dict[str, Any] | None = None
        if self._credentials_provider.is_dynamic():
            self.redis_client = REDIS_CONN
        else:
            self.static_credentials = self._credentials_provider.get_credentials()

        self._confluence = Confluence(url)
        self.credential_key: str = (
            self.CREDENTIAL_PREFIX
            + f":credential_{self._credentials_provider.get_provider_key()}"
        )

        self._kwargs: Any = None

        self.shared_base_kwargs: dict[str, str | int | bool] = {
            "api_version": "cloud" if is_cloud else "latest",
            "backoff_and_retry": True,
            "cloud": is_cloud,
        }
        if timeout:
            self.shared_base_kwargs["timeout"] = timeout

        self._confluence_user_profiles_override = (
            process_confluence_user_profiles_override(confluence_user_profiles_override)
            if confluence_user_profiles_override
            else None
        )

    def _renew_credentials(self) -> tuple[dict[str, Any], bool]:
        """credential_json - the current json credentials
        Returns a tuple
        1. The up to date credentials
        2. True if the credentials were updated

        This method is intended to be used within a distributed lock.
        Lock, call this, update credentials if the tokens were refreshed, then release
        """
        # static credentials are preloaded, so no locking/redis required
        if self.static_credentials:
            return self.static_credentials, False

        if not self.redis_client:
            raise RuntimeError("self.redis_client is None")

        # dynamic credentials need locking
        # check redis first, then fallback to the DB
        credential_raw = self.redis_client.get(self.credential_key)
        if credential_raw is not None:
            credential_bytes = cast(bytes, credential_raw)
            credential_str = credential_bytes.decode("utf-8")
            credential_json: dict[str, Any] = json.loads(credential_str)
        else:
            credential_json = self._credentials_provider.get_credentials()

        if "confluence_refresh_token" not in credential_json:
            # static credentials ... cache them permanently and return
            self.static_credentials = credential_json
            return credential_json, False

        if not OAUTH_CONFLUENCE_CLOUD_CLIENT_ID:
            raise RuntimeError("OAUTH_CONFLUENCE_CLOUD_CLIENT_ID must be set!")

        if not OAUTH_CONFLUENCE_CLOUD_CLIENT_SECRET:
            raise RuntimeError("OAUTH_CONFLUENCE_CLOUD_CLIENT_SECRET must be set!")

        # check if we should refresh tokens. we're deciding to refresh halfway
        # to expiration
        now = datetime.now(timezone.utc)
        created_at = datetime.fromisoformat(credential_json["created_at"])
        expires_in: int = credential_json["expires_in"]
        renew_at = created_at + timedelta(seconds=expires_in // 2)
        if now <= renew_at:
            # cached/current credentials are reasonably up to date
            return credential_json, False

        # we need to refresh
        logging.info("Renewing Confluence Cloud credentials...")
        new_credentials = confluence_refresh_tokens(
            OAUTH_CONFLUENCE_CLOUD_CLIENT_ID,
            OAUTH_CONFLUENCE_CLOUD_CLIENT_SECRET,
            credential_json["cloud_id"],
            credential_json["confluence_refresh_token"],
        )

        # store the new credentials to redis and to the db thru the provider
        # redis: we use a 5 min TTL because we are given a 10 minute grace period
        # when keys are rotated. it's easier to expire the cached credentials
        # reasonably frequently rather than trying to handle strong synchronization
        # between the db and redis everywhere the credentials might be updated
        new_credential_str = json.dumps(new_credentials)
        self.redis_client.set(
            self.credential_key, new_credential_str, nx=True, ex=self.CREDENTIAL_TTL
        )
        self._credentials_provider.set_credentials(new_credentials)

        return new_credentials, True

    @staticmethod
    def _make_oauth2_dict(credentials: dict[str, Any]) -> dict[str, Any]:
        oauth2_dict: dict[str, Any] = {}
        if "confluence_refresh_token" in credentials:
            oauth2_dict["client_id"] = OAUTH_CONFLUENCE_CLOUD_CLIENT_ID
            oauth2_dict["token"] = {}
            oauth2_dict["token"]["access_token"] = credentials[
                "confluence_access_token"
            ]
        return oauth2_dict

    def _probe_connection(
        self,
        **kwargs: Any,
    ) -> None:
        merged_kwargs = {**self.shared_base_kwargs, **kwargs}
        # add special timeout to make sure that we don't hang indefinitely
        merged_kwargs["timeout"] = self.PROBE_TIMEOUT

        with self._credentials_provider:
            credentials, _ = self._renew_credentials()
            if self.scoped_token:
                # v2 endpoint doesn't always work with scoped tokens, use v1
                token = credentials["confluence_access_token"]
                probe_url = f"{self.base_url}/rest/api/space?limit=1"
                import requests

                logging.info(f"First and Last 5 of token: {token[:5]}...{token[-5:]}")

                try:
                    r = requests.get(
                        probe_url,
                        headers={"Authorization": f"Bearer {token}"},
                        timeout=10,
                    )
                    r.raise_for_status()
                except HTTPError as e:
                    if e.response.status_code == 403:
                        logging.warning(
                            "scoped token authenticated but not valid for probe endpoint (spaces)"
                        )
                    else:
                        if "WWW-Authenticate" in e.response.headers:
                            logging.warning(
                                f"WWW-Authenticate: {e.response.headers['WWW-Authenticate']}"
                            )
                            l

... [Content truncated - file is 81000 bytes] ...

results is returned
            if checkpoint.next_page_url and checkpoint.next_page_url != page_query_url:
                return checkpoint

        checkpoint.has_more = False
        return checkpoint

    def _build_page_retrieval_url(
        self,
        start: SecondsSinceUnixEpoch | None,
        end: SecondsSinceUnixEpoch | None,
        limit: int,
    ) -> str:
        """
        Builds the full URL used to retrieve pages from the confluence API.
        This can be used as input to the confluence client's _paginate_url
        or paginated_page_retrieval methods.
        """
        page_query = self._construct_page_cql_query(start, end)
        cql_url = self.confluence_client.build_cql_url(
            page_query, expand=",".join(_PAGE_EXPANSION_FIELDS)
        )
        return update_param_in_path(cql_url, "limit", str(limit))

    @override
    def load_from_checkpoint(
        self,
        start: SecondsSinceUnixEpoch,
        end: SecondsSinceUnixEpoch,
        checkpoint: ConfluenceCheckpoint,
    ) -> CheckpointOutput[ConfluenceCheckpoint]:
        end += ONE_DAY  # handle time zone weirdness
        try:
            return self._fetch_document_batches(checkpoint, start, end)
        except Exception as e:
            if is_atlassian_date_error(e) and start is not None:
                logging.warning(
                    "Confluence says we provided an invalid 'updated' field. This may indicate"
                    "a real issue, but can also appear during edge cases like daylight"
                    f"savings time changes. Retrying with a 1 hour offset. Error: {e}"
                )
                return self._fetch_document_batches(checkpoint, start - ONE_HOUR, end)
            raise

    @override
    def build_dummy_checkpoint(self) -> ConfluenceCheckpoint:
        return ConfluenceCheckpoint(has_more=True, next_page_url=None)

    @override
    def validate_checkpoint_json(self, checkpoint_json: str) -> ConfluenceCheckpoint:
        return ConfluenceCheckpoint.model_validate_json(checkpoint_json)

    @override
    def retrieve_all_slim_docs(
        self,
        start: SecondsSinceUnixEpoch | None = None,
        end: SecondsSinceUnixEpoch | None = None,
        callback: IndexingHeartbeatInterface | None = None,
    ) -> GenerateSlimDocumentOutput:
        return self._retrieve_all_slim_docs(
            start=start,
            end=end,
            callback=callback,
            include_permissions=False,
        )

    def retrieve_all_slim_docs_perm_sync(
        self,
        start: SecondsSinceUnixEpoch | None = None,
        end: SecondsSinceUnixEpoch | None = None,
        callback: IndexingHeartbeatInterface | None = None,
    ) -> GenerateSlimDocumentOutput:
        """
        Return 'slim' docs (IDs + minimal permission data).
        Does not fetch actual text. Used primarily for incremental permission sync.
        """
        return self._retrieve_all_slim_docs(
            start=start,
            end=end,
            callback=callback,
            include_permissions=True,
        )

    def _retrieve_all_slim_docs(
        self,
        start: SecondsSinceUnixEpoch | None = None,
        end: SecondsSinceUnixEpoch | None = None,
        callback: IndexingHeartbeatInterface | None = None,
        include_permissions: bool = True,
    ) -> GenerateSlimDocumentOutput:
        doc_metadata_list: list[SlimDocument] = []
        restrictions_expand = ",".join(_RESTRICTIONS_EXPANSION_FIELDS)

        space_level_access_info: dict[str, ExternalAccess] = {}
        if include_permissions:
            space_level_access_info = get_all_space_permissions(
                self.confluence_client, self.is_cloud
            )

        def get_external_access(
            doc_id: str, restrictions: dict[str, Any], ancestors: list[dict[str, Any]]
        ) -> ExternalAccess | None:
            return get_page_restrictions(
                self.confluence_client, doc_id, restrictions, ancestors
            ) or space_level_access_info.get(page_space_key)

        # Query pages
        page_query = self.base_cql_page_query + self.cql_label_filter
        for page in self.confluence_client.cql_paginate_all_expansions(
            cql=page_query,
            expand=restrictions_expand,
            limit=_SLIM_DOC_BATCH_SIZE,
        ):
            page_id = page["id"]
            page_restrictions = page.get("restrictions") or {}
            page_space_key = page.get("space", {}).get("key")
            page_ancestors = page.get("ancestors", [])

            page_id = build_confluence_document_id(
                self.wiki_base, page["_links"]["webui"], self.is_cloud
            )
            doc_metadata_list.append(
                SlimDocument(
                    id=page_id,
                    external_access=(
                        get_external_access(page_id, page_restrictions, page_ancestors)
                        if include_permissions
                        else None
                    ),
                )
            )

            # Query attachments for each page
            attachment_query = self._construct_attachment_query(page["id"])
            for attachment in self.confluence_client.cql_paginate_all_expansions(
                cql=attachment_query,
                expand=restrictions_expand,
                limit=_SLIM_DOC_BATCH_SIZE,
            ):
                # If you skip images, you'll skip them in the permission sync
                attachment["metadata"].get("mediaType", "")
                if not validate_attachment_filetype(
                    attachment,
                ):
                    continue

                attachment_restrictions = attachment.get("restrictions", {})
                if not attachment_restrictions:
                    attachment_restrictions = page_restrictions or {}

                attachment_space_key = attachment.get("space", {}).get("key")
                if not attachment_space_key:
                    attachment_space_key = page_space_key

                attachment_id = build_confluence_document_id(
                    self.wiki_base,
                    attachment["_links"]["webui"],
                    self.is_cloud,
                )
                doc_metadata_list.append(
                    SlimDocument(
                        id=attachment_id,
                        external_access=(
                            get_external_access(
                                attachment_id, attachment_restrictions, []
                            )
                            if include_permissions
                            else None
                        ),
                    )
                )

            if len(doc_metadata_list) > _SLIM_DOC_BATCH_SIZE:
                yield doc_metadata_list[:_SLIM_DOC_BATCH_SIZE]
                doc_metadata_list = doc_metadata_list[_SLIM_DOC_BATCH_SIZE:]

                if callback and callback.should_stop():
                    raise RuntimeError(
                        "retrieve_all_slim_docs_perm_sync: Stop signal detected"
                    )
                if callback:
                    callback.progress("retrieve_all_slim_docs_perm_sync", 1)

        yield doc_metadata_list

    def validate_connector_settings(self) -> None:
        try:
            spaces = self.low_timeout_confluence_client.get_all_spaces(limit=1)
        except HTTPError as e:
            status_code = e.response.status_code if e.response else None
            if status_code == 401:
                raise CredentialExpiredError(
                    "Invalid or expired Confluence credentials (HTTP 401)."
                )
            elif status_code == 403:
                raise InsufficientPermissionsError(
                    "Insufficient permissions to access Confluence resources (HTTP 403)."
                )
            raise UnexpectedValidationError(
                f"Unexpected Confluence error (status={status_code}): {e}"
            )
        except Exception as e:
            raise UnexpectedValidationError(
                f"Unexpected error while validating Confluence settings: {e}"
            )

        if self.space:
            try:
                self.low_timeout_confluence_client.get_space(self.space)
            except ApiError as e:
                raise ConnectorValidationError(
                    "Invalid Confluence space key provided"
                ) from e

        if not spaces or not spaces.get("results"):
            raise ConnectorValidationError(
                "No Confluence spaces found. Either your credentials lack permissions, or "
                "there truly are no spaces in this Confluence instance."
            )



if __name__ == "__main__":
    import os

    # base url
    wiki_base = os.environ["CONFLUENCE_URL"]

    # auth stuff
    username = os.environ["CONFLUENCE_USERNAME"]
    access_token = os.environ["CONFLUENCE_ACCESS_TOKEN"]
    is_cloud = os.environ["CONFLUENCE_IS_CLOUD"].lower() == "true"

    # space + page
    space = os.environ["CONFLUENCE_SPACE_KEY"]
    # page_id = os.environ["CONFLUENCE_PAGE_ID"]

    confluence_connector = ConfluenceConnector(
        wiki_base=wiki_base,
        space=space,
        is_cloud=is_cloud,
        # page_id=page_id,
    )

    credentials_provider = StaticCredentialsProvider(
        None,
        DocumentSource.CONFLUENCE,
        {
            "confluence_username": username,
            "confluence_access_token": access_token,
        },
    )
    confluence_connector.set_credentials_provider(credentials_provider)

    start = 0.0
    end = datetime.now().timestamp()

    # Fetch all `SlimDocuments`.
    for slim_doc in confluence_connector.retrieve_all_slim_docs_perm_sync():
        print(slim_doc)

    # Fetch all `Documents`.
    for doc in load_all_docs_from_checkpoint_connector(
        connector=confluence_connector,
        start=start,
        end=end,
    ):
        print(doc)

```

## Detailed Analysis

### File Role in Repository

The file `common/data_source/confluence_connector.py` is located in the `common/data_source` directory.

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
- [discord_connector.py](discord_connector.py_docs.md)
- [dropbox_connector.py](dropbox_connector.py_docs.md)
- [exceptions.py](exceptions.py_docs.md)
- [file_types.py](file_types.py_docs.md)
- [gmail_connector.py](gmail_connector.py_docs.md)
- [html_utils.py](html_utils.py_docs.md)
- [interfaces.py](interfaces.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
