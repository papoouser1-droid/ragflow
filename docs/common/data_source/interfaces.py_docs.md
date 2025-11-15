# Documentation: common/data_source/interfaces.py

## File Metadata

- **Path**: `common/data_source/interfaces.py`
- **Size**: 13005 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `common/data_source/interfaces.py`.

## Python Module Overview

### Module Docstring

```
Interface definitions
```

### Imports and Dependencies

This module imports the following dependencies:

- `abc`
- `uuid`
- `abc`
- `enum`
- `types`
- `typing`
- `anthropic`
- `common.data_source.models`

### Classes Defined

This file defines 16 class(es):

#### Class: `LoadConnector` (line 20)

**Docstring**: Load connector interface...

**Methods**: load_credentials, load_from_state, validate_connector_settings

#### Class: `PollConnector` (line 39)

**Docstring**: Poll connector interface...

**Methods**: poll_source

#### Class: `CredentialsConnector` (line 48)

**Docstring**: Credentials connector interface...

**Methods**: load_credentials

#### Class: `SlimConnectorWithPermSync` (line 57)

**Docstring**: Simplified connector interface (with permission sync)...

**Methods**: retrieve_all_slim_docs_perm_sync

#### Class: `CheckpointedConnectorWithPermSync` (line 71)

**Docstring**: Checkpointed connector interface (with permission sync)...

**Methods**: load_from_checkpoint, load_from_checkpoint_with_perm_sync, build_dummy_checkpoint, validate_checkpoint_json

#### Class: `CredentialsProviderInterface` (line 108)

**Methods**: __enter__, __exit__, get_tenant_id, get_provider_key, get_credentials, set_credentials, is_dynamic

#### Class: `StaticCredentialsProvider` (line 156)

**Docstring**: Implementation (a very simple one!) to handle static credentials....

**Methods**: __init__, __enter__, __exit__, get_tenant_id, get_provider_key, get_credentials, set_credentials, is_dynamic

#### Class: `BaseConnector` (line 203)

**Methods**: load_credentials, parse_metadata, validate_connector_settings, validate_perm_sync, set_allow_images, build_dummy_checkpoint

#### Class: `CheckpointedConnector` (line 264)

**Methods**: load_from_checkpoint, build_dummy_checkpoint, validate_checkpoint_json

#### Class: `CheckpointOutputWrapper` (line 303)

**Docstring**: Wraps a CheckpointOutput generator to give things back in a more digestible format,
specifically for Document outputs.
The connector format is easier for the connector implementor (e.g. it enforces ex...

**Methods**: __init__, __call__

#### Class: `SlimConnector` (line 349)

**Methods**: retrieve_all_slim_docs

#### Class: `ConfluenceUser` (line 357)

#### Class: `TokenResponse` (line 367)

#### Class: `OnyxExtensionType` (line 375)

#### Class: `AttachmentProcessingResult` (line 382)

**Docstring**: A container for results after processing a Confluence attachment.
'text' is the textual content of the attachment.
'file_name' is the final file name used in FileStore to store the content.
'error' ho...

#### Class: `IndexingHeartbeatInterface` (line 398)

**Docstring**: Defines a callback interface to be passed to
to run_indexing_entrypoint....

**Methods**: should_stop, progress

### Functions Defined

This file defines 40 function(s):

#### Function: `load_credentials` (line 24)

**Parameters**: self, credentials

**Docstring**: Load credentials...

#### Function: `load_from_state` (line 29)

**Parameters**: self

**Docstring**: Load documents from state...

#### Function: `validate_connector_settings` (line 34)

**Parameters**: self

**Docstring**: Validate connector settings...

#### Function: `poll_source` (line 43)

**Parameters**: self, start, end

**Docstring**: Poll source to get documents...

#### Function: `load_credentials` (line 52)

**Parameters**: self, credentials

**Docstring**: Load credentials...

#### Function: `retrieve_all_slim_docs_perm_sync` (line 61)

**Parameters**: self, start, end, callback

**Docstring**: Retrieve all simplified documents (with permission sync)...

#### Function: `load_from_checkpoint` (line 75)

**Parameters**: self, start, end, checkpoint

**Docstring**: Load documents from checkpoint...

#### Function: `load_from_checkpoint_with_perm_sync` (line 85)

**Parameters**: self, start, end, checkpoint

**Docstring**: Load documents from checkpoint (with permission sync)...

#### Function: `build_dummy_checkpoint` (line 95)

**Parameters**: self

**Docstring**: Build dummy checkpoint...

#### Function: `validate_checkpoint_json` (line 100)

**Parameters**: self, checkpoint_json

**Docstring**: Validate checkpoint JSON...

#### Function: `__enter__` (line 110)

**Parameters**: self

#### Function: `__exit__` (line 114)

**Parameters**: self, exc_type, exc_value, traceback

#### Function: `get_tenant_id` (line 123)

**Parameters**: self

#### Function: `get_provider_key` (line 127)

**Parameters**: self

**Docstring**: a unique key that the connector can use to lock around a credential
that might be used simultaneously.

Will typically be the credential id, but can also just be something random
in cases when there i...

#### Function: `get_credentials` (line 137)

**Parameters**: self

#### Function: `set_credentials` (line 141)

**Parameters**: self, credential_json

#### Function: `is_dynamic` (line 145)

**Parameters**: self

**Docstring**: If dynamic, the credentials may change during usage ... maening the client
needs to use the locking features of the credentials provider to operate
correctly.

If static, the client can simply referen...

#### Function: `__init__` (line 161)

**Parameters**: self, tenant_id, connector_name, credential_json

#### Function: `__enter__` (line 173)

**Parameters**: self

#### Function: `__exit__` (line 176)

**Parameters**: self, exc_type, exc_value, traceback

#### Function: `get_tenant_id` (line 184)

**Parameters**: self

#### Function: `get_provider_key` (line 187)

**Parameters**: self

#### Function: `get_credentials` (line 190)

**Parameters**: self

#### Function: `set_credentials` (line 193)

**Parameters**: self, credential_json

#### Function: `is_dynamic` (line 196)

**Parameters**: self

#### Function: `load_credentials` (line 209)

**Parameters**: self, credentials

#### Function: `parse_metadata` (line 213)

**Parameters**: metadata

**Docstring**: Parse the metadata for a document/chunk into a string to pass to Generative AI as additional context...

#### Function: `validate_connector_settings` (line 230)

**Parameters**: self

**Docstring**: Override this if your connector needs to validate credentials or settings.
Raise an exception if invalid, otherwise do nothing.

Default is a no-op (always successful)....

#### Function: `validate_perm_sync` (line 238)

**Parameters**: self

**Docstring**: Don't override this; add a function to perm_sync_valid.py in the ee package
to do permission sync validation...

#### Function: `set_allow_images` (line 251)

**Parameters**: self, value

**Docstring**: Implement if the underlying connector wants to skip/allow image downloading
based on the application level image analysis setting....

#### Function: `build_dummy_checkpoint` (line 255)

**Parameters**: self

#### Function: `load_from_checkpoint` (line 266)

**Parameters**: self, start, end, checkpoint

**Docstring**: Yields back documents or failures. Final return is the new checkpoint.

Final return can be access via either:

```
try:
    for document_or_failure in connector.load_from_checkpoint(start, end, check...

#### Function: `build_dummy_checkpoint` (line 294)

**Parameters**: self

#### Function: `validate_checkpoint_json` (line 298)

**Parameters**: self, checkpoint_json

**Docstring**: Validate the checkpoint json and return the checkpoint object...

#### Function: `__init__` (line 312)

**Parameters**: self

#### Function: `__call__` (line 315)

**Parameters**: self, checkpoint_connector_generator

#### Function: `retrieve_all_slim_docs` (line 351)

**Parameters**: self

#### Function: `should_stop` (line 403)

**Parameters**: self

**Docstring**: Signal to stop the looping function in flight....

#### Function: `progress` (line 407)

**Parameters**: self, tag, amount

**Docstring**: Send progress updates to the caller.
Amount can be a positive number to indicate progress or <= 0
just to act as a keep-alive....

#### Function: `_inner_wrapper` (line 324)

**Parameters**: checkpoint_connector_generator

## Original Source Code

```py
"""Interface definitions"""
import abc
import uuid
from abc import ABC, abstractmethod
from enum import IntFlag, auto
from types import TracebackType
from typing import Any, Dict, Generator, TypeVar, Generic, Callable, TypeAlias

from anthropic import BaseModel

from common.data_source.models import (
    Document,
    SlimDocument,
    ConnectorCheckpoint,
    ConnectorFailure,
    SecondsSinceUnixEpoch, GenerateSlimDocumentOutput
)


class LoadConnector(ABC):
    """Load connector interface"""

    @abstractmethod
    def load_credentials(self, credentials: Dict[str, Any]) -> Dict[str, Any] | None:
        """Load credentials"""
        pass

    @abstractmethod
    def load_from_state(self) -> Generator[list[Document], None, None]:
        """Load documents from state"""
        pass

    @abstractmethod
    def validate_connector_settings(self) -> None:
        """Validate connector settings"""
        pass


class PollConnector(ABC):
    """Poll connector interface"""

    @abstractmethod
    def poll_source(self, start: SecondsSinceUnixEpoch, end: SecondsSinceUnixEpoch) -> Generator[list[Document], None, None]:
        """Poll source to get documents"""
        pass


class CredentialsConnector(ABC):
    """Credentials connector interface"""

    @abstractmethod
    def load_credentials(self, credentials: Dict[str, Any]) -> Dict[str, Any] | None:
        """Load credentials"""
        pass


class SlimConnectorWithPermSync(ABC):
    """Simplified connector interface (with permission sync)"""

    @abstractmethod
    def retrieve_all_slim_docs_perm_sync(
        self,
        start: SecondsSinceUnixEpoch | None = None,
        end: SecondsSinceUnixEpoch | None = None,
        callback: Any = None,
    ) -> Generator[list[SlimDocument], None, None]:
        """Retrieve all simplified documents (with permission sync)"""
        pass


class CheckpointedConnectorWithPermSync(ABC):
    """Checkpointed connector interface (with permission sync)"""

    @abstractmethod
    def load_from_checkpoint(
        self,
        start: SecondsSinceUnixEpoch,
        end: SecondsSinceUnixEpoch,
        checkpoint: ConnectorCheckpoint,
    ) -> Generator[Document | ConnectorFailure, None, ConnectorCheckpoint]:
        """Load documents from checkpoint"""
        pass

    @abstractmethod
    def load_from_checkpoint_with_perm_sync(
        self,
        start: SecondsSinceUnixEpoch,
        end: SecondsSinceUnixEpoch,
        checkpoint: ConnectorCheckpoint,
    ) -> Generator[Document | ConnectorFailure, None, ConnectorCheckpoint]:
        """Load documents from checkpoint (with permission sync)"""
        pass

    @abstractmethod
    def build_dummy_checkpoint(self) -> ConnectorCheckpoint:
        """Build dummy checkpoint"""
        pass

    @abstractmethod
    def validate_checkpoint_json(self, checkpoint_json: str) -> ConnectorCheckpoint:
        """Validate checkpoint JSON"""
        pass


T = TypeVar("T", bound="CredentialsProviderInterface")


class CredentialsProviderInterface(abc.ABC, Generic[T]):
    @abc.abstractmethod
    def __enter__(self) -> T:
        raise NotImplementedError

    @abc.abstractmethod
    def __exit__(
        self,
        exc_type: type[BaseException] | None,
        exc_value: BaseException | None,
        traceback: TracebackType | None,
    ) -> None:
        raise NotImplementedError

    @abc.abstractmethod
    def get_tenant_id(self) -> str | None:
        raise NotImplementedError

    @abc.abstractmethod
    def get_provider_key(self) -> str:
        """a unique key that the connector can use to lock around a credential
        that might be used simultaneously.

        Will typically be the credential id, but can also just be something random
        in cases when there is nothing to lock (aka static credentials)
        """
        raise NotImplementedError

    @abc.abstractmethod
    def get_credentials(self) -> dict[str, Any]:
        raise NotImplementedError

    @abc.abstractmethod
    def set_credentials(self, credential_json: dict[str, Any]) -> None:
        raise NotImplementedError

    @abc.abstractmethod
    def is_dynamic(self) -> bool:
        """If dynamic, the credentials may change during usage ... maening the client
        needs to use the locking features of the credentials provider to operate
        correctly.

        If static, the client can simply reference the credentials once and use them
        through the entire indexing run.
        """
        raise NotImplementedError


class StaticCredentialsProvider(
    CredentialsProviderInterface["StaticCredentialsProvider"]
):
    """Implementation (a very simple one!) to handle static credentials."""

    def __init__(
        self,
        tenant_id: str | None,
        connector_name: str,
        credential_json: dict[str, Any],
    ):
        self._tenant_id = tenant_id
        self._connector_name = connector_name
        self._credential_json = credential_json

        self._provider_key = str(uuid.uuid4())

    def __enter__(self) -> "StaticCredentialsProvider":
        return self

    def __exit__(
        self,
        exc_type: type[BaseException] | None,
        exc_value: BaseException | None,
        traceback: TracebackType | None,
    ) -> None:
        pass

    def get_tenant_id(self) -> str | None:
        return self._tenant_id

    def get_provider_key(self) -> str:
        return self._provider_key

    def get_credentials(self) -> dict[str, Any]:
        return self._credential_json

    def set_credentials(self, credential_json: dict[str, Any]) -> None:
        self._credential_json = credential_json

    def is_dynamic(self) -> bool:
        return False


CT = TypeVar("CT", bound=ConnectorCheckpoint)


class BaseConnector(abc.ABC, Generic[CT]):
    REDIS_KEY_PREFIX = "da_connector_data:"
    # Common image file extensions supported across connectors
    IMAGE_EXTENSIONS = {".jpg", ".jpeg", ".png", ".webp", ".gif"}

    @abc.abstractmethod
    def load_credentials(self, credentials: dict[str, Any]) -> dict[str, Any] | None:
        raise NotImplementedError

    @staticmethod
    def parse_metadata(metadata: dict[str, Any]) -> list[str]:
        """Parse the metadata for a document/chunk into a string to pass to Generative AI as additional context"""
        custom_parser_req_msg = (
            "Specific metadata parsing required, connector has not implemented it."
        )
        metadata_lines = []
        for metadata_key, metadata_value in metadata.items():
            if isinstance(metadata_value, str):
                metadata_lines.append(f"{metadata_key}: {metadata_value}")
            elif isinstance(metadata_value, list):
                if not all([isinstance(val, str) for val in metadata_value]):
                    raise RuntimeError(custom_parser_req_msg)
                metadata_lines.append(f'{metadata_key}: {", ".join(metadata_value)}')
            else:
                raise RuntimeError(custom_parser_req_msg)
        return metadata_lines

    def validate_connector_settings(self) -> None:
        """
        Override this if your connector needs to validate credentials or settings.
        Raise an exception if invalid, otherwise do nothing.

        Default is a no-op (always successful).
        """

    def validate_perm_sync(self) -> None:
        """
        Don't override this; add a function to perm_sync_valid.py in the ee package
        to do permission sync validation
        """
        """
        validate_connector_settings_fn = fetch_ee_implementation_or_noop(
            "onyx.connectors.perm_sync_valid",
            "validate_perm_sync",
            noop_return_value=None,
        )
        validate_connector_settings_fn(self)"""

    def set_allow_images(self, value: bool) -> None:
        """Implement if the underlying connector wants to skip/allow image downloading
        based on the application level image analysis setting."""

    def build_dummy_checkpoint(self) -> CT:
        # TODO: find a way to make this work without type: ignore
        return ConnectorCheckpoint(has_more=True)  # type: ignore


CheckpointOutput: TypeAlias = Generator[Document | ConnectorFailure, None, CT]
LoadFunction = Callable[[CT], CheckpointOutput[CT]]


class CheckpointedConnector(BaseConnector[CT]):
    @abc.abstractmethod
    def load_from_checkpoint(
        self,
        start: SecondsSinceUnixEpoch,
        end: SecondsSinceUnixEpoch,
        checkpoint: CT,
    ) -> CheckpointOutput[CT]:
        """Yields back documents or failures. Final return is the new checkpoint.

        Final return can be access via either:

        ```
        try:
            for document_or_failure in connector.load_from_checkpoint(start, end, checkpoint):
                print(document_or_failure)
        except StopIteration as e:
            checkpoint = e.value  # Extracting the return value
            print(checkpoint)
        ```

        OR

        ```
        checkpoint = yield from connector.load_from_checkpoint(start, end, checkpoint)
        ```
        """
        raise NotImplementedError

    @abc.abstractmethod
    def build_dummy_checkpoint(self) -> CT:
        raise NotImplementedError

    @abc.abstractmethod
    def validate_checkpoint_json(self, checkpoint_json: str) -> CT:
        """Validate the checkpoint json and return the checkpoint object"""
        raise NotImplementedError


class CheckpointOutputWrapper(Generic[CT]):
    """
    Wraps a CheckpointOutput generator to give things back in a more digestible format,
    specifically for Document outputs.
    The connector format is easier for the connector implementor (e.g. it enforces exactly
    one new checkpoint is returned AND that the checkpoint is at the end), thus the different
    formats.
    """

    def __init__(self) -> None:
        self.next_checkpoint: CT | None = None

    def __call__(
        self,
        checkpoint_connector_generator: CheckpointOutput[CT],
    ) -> Generator[
        tuple[Document | None, ConnectorFailure | None, CT | None],
        None,
        None,
    ]:
        # grabs the final return value and stores it in the `next_checkpoint` variable
        def _inner_wrapper(
            checkpoint_connector_generator: CheckpointOutput[CT],
        ) -> CheckpointOutput[CT]:
            self.next_checkpoint = yield from checkpoint_connector_generator
            return self.next_checkpoint  # not used

        for document_or_failure in _inner_wrapper(checkpoint_connector_generator):
            if isinstance(document_or_failure, Document):
                yield document_or_failure, None, None
            elif isinstance(document_or_failure, ConnectorFailure):
                yield None, document_or_failure, None
            else:
                raise ValueError(
                    f"Invalid document_or_failure type: {type(document_or_failure)}"
                )

        if self.next_checkpoint is None:
            raise RuntimeError(
                "Checkpoint is None. This should never happen - the connector should always return a checkpoint."
            )

        yield None, None, self.next_checkpoint


# Slim connectors retrieve just the ids of documents
class SlimConnector(BaseConnector):
    @abc.abstractmethod
    def retrieve_all_slim_docs(
        self,
    ) -> GenerateSlimDocumentOutput:
        raise NotImplementedError


class ConfluenceUser(BaseModel):
    user_id: str  # accountId in Cloud, userKey in Server
    username: str | None  # Confluence Cloud doesn't give usernames
    display_name: str
    # Confluence Data Center doesn't give email back by default,
    # have to fetch it with a different endpoint
    email: str | None
    type: str


class TokenResponse(BaseModel):
    access_token: str
    expires_in: int
    token_type: str
    refresh_token: str
    scope: str


class OnyxExtensionType(IntFlag):
    Plain = auto()
    Document = auto()
    Multimedia = auto()
    All = Plain | Document | Multimedia


class AttachmentProcessingResult(BaseModel):
    """
    A container for results after processing a Confluence attachment.
    'text' is the textual content of the attachment.
    'file_name' is the final file name used in FileStore to store the content.
    'error' holds an exception or string if something failed.
    """

    text: str | None
    file_blob: bytes | bytearray | None
    file_name: str | None
    error: str | None = None

    model_config = {"arbitrary_types_allowed": True}


class IndexingHeartbeatInterface(ABC):
    """Defines a callback interface to be passed to
    to run_indexing_entrypoint."""

    @abstractmethod
    def should_stop(self) -> bool:
        """Signal to stop the looping function in flight."""

    @abstractmethod
    def progress(self, tag: str, amount: int) -> None:
        """Send progress updates to the caller.
        Amount can be a positive number to indicate progress or <= 0
        just to act as a keep-alive.
        """


```

## Detailed Analysis

### File Role in Repository

The file `common/data_source/interfaces.py` is located in the `common/data_source` directory.

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
- [html_utils.py](html_utils.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
