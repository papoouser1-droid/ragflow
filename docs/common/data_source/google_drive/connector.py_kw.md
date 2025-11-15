# Keyword Map: common/data_source/google_drive/connector.py

## File Path and Links

- **Original File**: `common/data_source/google_drive/connector.py`
- **Documentation**: [connector.py_docs.md](./connector.py_docs.md)

## Keywords Extracted

This file contains 162 extracted keywords and identifiers:


### Class Definition

- **CheckpointOutputWrapper**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **CredentialedRetrievalMethod**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **DriveIdStatus**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **GoogleDriveConnector**: Referenced in this file (see [_docs.md](./connector.py_docs.md))

### Filename Component

- **connector**: Referenced in this file (see [_docs.md](./connector.py_docs.md))

### Function Definition

- **__call__**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **__init__**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_checkpointed_retrieval**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_clean_requested_drive_ids**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_determine_retrieval_ids**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_extract_docs_from_google_drive**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_extract_ids_from_urls**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_extract_slim_docs_from_google_drive**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_extract_str_list_from_comma_str**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_fetch_drive_items**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_get_all_drives_for_user**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_get_all_user_emails**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_impersonate_user_for_retrieval**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_inner_wrapper**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_load_from_checkpoint**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_manage_oauth_retrieval**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_manage_service_account_retrieval**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_oauth_retrieval_all_files**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_oauth_retrieval_drives**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_oauth_retrieval_folders**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_update_traversed_parent_ids**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_yield_batch**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_yield_from_drive**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_yield_from_folder_crawl**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **add_retrieval_info**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **build_dummy_checkpoint**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **creds**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **get_all_drive_ids**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **get_available_drive_id**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **get_credentials_from_env**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **google_domain**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **load_credentials**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **load_from_checkpoint**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **load_from_checkpoint_with_perm_sync**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **make_drive_id_getter**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **primary_admin_email**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **retrieve_all_slim_docs_perm_sync**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **set_allow_images**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **validate_checkpoint_json**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **validate_connector_settings**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **yield_all_docs_from_checkpoint_connector**: Referenced in this file (see [_docs.md](./connector.py_docs.md))

### Identifier

- **Any**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **Callable**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **CheckpointOutput**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **CheckpointedConnectorWithPermSync**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **ConnectorFailure**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **ConnectorMissingCredentialError**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **ConnectorValidationError**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **CredentialExpiredError**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **Credentials**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **DB_CREDENTIALS_PRIMARY_ADMIN_KEY**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **Document**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **DocumentSource**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **Drive**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **DriveFileFieldType**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **DriveRetrievalStage**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **EntityFailure**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **Enum**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **FOLDERS_PER_CHECKPOINT**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **GOOGLE_DRIVE_CONNECTOR_SIZE_THRESHOLD**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **GenerateSlimDocumentOutput**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **Generator**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **Google**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **GoogleDriveCheckpoint**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **GoogleDriveFileType**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **GoogleDriveService**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **GoogleFields**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **HttpError**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **INDEX_BATCH_SIZE**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **IndexingHeartbeatInterface**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **InsufficientPermissionsError**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **Iterator**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **MAX_DRIVE_WORKERS**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **MISSING_SCOPES_ERROR_STR**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **MY_DRIVE_PAGES_PER_CHECKPOINT**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **OAUTH_PAGES_PER_CHECKPOINT**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **OAuthCredentials**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **PermissionSyncContext**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **Protocol**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **RefreshError**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **RetrievedDriveFile**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **SHARED_DRIVE_PAGES_PER_CHECKPOINT**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **SLIM_BATCH_SIZE**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **SecondsSinceUnixEpoch**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **ServiceAccountCredentials**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **SlimConnectorWithPermSync**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **StageCompletion**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **ThreadSafeDict**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **USER_FIELDS**: Referenced in this file (see [_docs.md](./connector.py_docs.md))

### Imported Module

- **collections.abc**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **common.data_source.config**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **common.data_source.exceptions**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **common.data_source.google_drive.doc_conversion**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **common.data_source.google_drive.file_retrieval**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **common.data_source.google_drive.model**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **common.data_source.google_util.auth**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **common.data_source.google_util.constant**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **common.data_source.google_util.oauth_flow**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **common.data_source.google_util.resource**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **common.data_source.google_util.util**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **common.data_source.google_util.util_threadpool_concurrency**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **common.data_source.interfaces**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **common.data_source.models**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **common.data_source.utils**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **copy**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **enum**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **functools**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **google.auth.exceptions**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **google.oauth2.credentials**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **google.oauth2.service_account**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **googleapiclient.errors**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **json**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **logging**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **os**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **sys**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **threading**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **time**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **traceback**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **typing**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **typing_extensions**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **urllib.parse**: Referenced in this file (see [_docs.md](./connector.py_docs.md))

### Method In Checkpointoutputwrapper

- **__call__**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **__init__**: Referenced in this file (see [_docs.md](./connector.py_docs.md))

### Method In Credentialedretrievalmethod

- **__call__**: Referenced in this file (see [_docs.md](./connector.py_docs.md))

### Method In Googledriveconnector

- **__init__**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_checkpointed_retrieval**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_determine_retrieval_ids**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_extract_docs_from_google_drive**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_extract_slim_docs_from_google_drive**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_fetch_drive_items**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_get_all_drives_for_user**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_get_all_user_emails**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_impersonate_user_for_retrieval**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_load_from_checkpoint**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_manage_oauth_retrieval**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_manage_service_account_retrieval**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_oauth_retrieval_all_files**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_oauth_retrieval_drives**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_oauth_retrieval_folders**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **_update_traversed_parent_ids**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **build_dummy_checkpoint**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **creds**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **get_all_drive_ids**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **google_domain**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **load_credentials**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **load_from_checkpoint**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **load_from_checkpoint_with_perm_sync**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **make_drive_id_getter**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **primary_admin_email**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **retrieve_all_slim_docs_perm_sync**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **set_allow_images**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **validate_checkpoint_json**: Referenced in this file (see [_docs.md](./connector.py_docs.md))
- **validate_connector_settings**: Referenced in this file (see [_docs.md](./connector.py_docs.md))


## Keyword → Section Map

All keywords in this file can be found in the comprehensive documentation:
- [Full Documentation](./connector.py_docs.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
