# Documentation: common/data_source/models.py

## File Metadata

- **Path**: `common/data_source/models.py`
- **Size**: 8173 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `common/data_source/models.py`.

## Python Module Overview

### Module Docstring

```
Data model definitions for all connectors
```

### Imports and Dependencies

This module imports the following dependencies:

- `dataclasses`
- `datetime`
- `typing`
- `typing_extensions`
- `pydantic`

### Classes Defined

This file defines 26 class(es):

#### Class: `ExternalAccess` (line 10)

**Methods**: __str__, num_entries, public, empty

#### Class: `ExtractionResult` (line 68)

**Docstring**: Structured result from text and image extraction from various file types....

#### Class: `TextSection` (line 76)

**Docstring**: Text section model...

#### Class: `ImageSection` (line 82)

**Docstring**: Image section model...

#### Class: `Document` (line 88)

**Docstring**: Document model...

#### Class: `BasicExpertInfo` (line 99)

**Docstring**: Expert information model...

**Methods**: get_semantic_name

#### Class: `SlimDocument` (line 120)

**Docstring**: Simplified document model (contains only ID and permission info)...

#### Class: `ConnectorCheckpoint` (line 126)

**Docstring**: Connector checkpoint model...

#### Class: `DocumentFailure` (line 131)

**Docstring**: Document processing failure information...

#### Class: `EntityFailure` (line 137)

**Docstring**: Entity processing failure information...

#### Class: `ConnectorFailure` (line 143)

**Docstring**: Connector failure information...

#### Class: `GmailCredentials` (line 154)

**Docstring**: Gmail authentication credentials model...

#### Class: `GmailThread` (line 160)

**Docstring**: Gmail thread data model...

#### Class: `GmailMessage` (line 166)

**Docstring**: Gmail message data model...

#### Class: `NotionPage` (line 174)

**Docstring**: Represents a Notion Page object...

#### Class: `NotionBlock` (line 185)

**Docstring**: Represents a Notion Block object...

#### Class: `NotionSearchResponse` (line 192)

**Docstring**: Represents the response from the Notion Search API...

#### Class: `NotionCredentials` (line 199)

**Docstring**: Notion authentication credentials model...

#### Class: `ChannelTopicPurposeType` (line 205)

**Docstring**: Slack channel topic or purpose...

#### Class: `ChannelType` (line 212)

**Docstring**: Slack channel...

#### Class: `AttachmentType` (line 240)

**Docstring**: Slack message attachment...

#### Class: `BotProfileType` (line 251)

**Docstring**: Slack bot profile...

#### Class: `MessageType` (line 261)

**Docstring**: Slack message...

#### Class: `SlackCheckpoint` (line 279)

**Docstring**: Slack checkpoint...

#### Class: `SlackMessageFilterReason` (line 289)

**Docstring**: Slack message filter reason...

#### Class: `ProcessedSlackMessage` (line 295)

**Docstring**: Processed Slack message...

**Methods**: __init__

### Functions Defined

This file defines 7 function(s):

#### Function: `__str__` (line 23)

**Parameters**: self

**Docstring**: Prevent extremely long logs...

#### Function: `num_entries` (line 40)

**Parameters**: self

#### Function: `public` (line 44)

**Parameters**: cls

#### Function: `empty` (line 52)

**Parameters**: cls

**Docstring**: A helper function that returns an *empty* set of external user-emails and group-ids, and sets `is_public` to `False`.
This effectively makes the document in question "private" or inaccessible to anyon...

#### Function: `get_semantic_name` (line 106)

**Parameters**: self

**Docstring**: Get semantic name for display...

#### Function: `__init__` (line 297)

**Parameters**: self, doc, thread_or_message_ts, filter_reason, failure

#### Function: `truncate_set` (line 26)

**Parameters**: s, max_len

## Original Source Code

```py
"""Data model definitions for all connectors"""
from dataclasses import dataclass
from datetime import datetime
from typing import Any, Optional, List, Sequence, NamedTuple
from typing_extensions import TypedDict, NotRequired
from pydantic import BaseModel


@dataclass(frozen=True)
class ExternalAccess:

    # arbitrary limit to prevent excessively large permissions sets
    # not internally enforced ... the caller can check this before using the instance
    MAX_NUM_ENTRIES = 5000

    # Emails of external users with access to the doc externally
    external_user_emails: set[str]
    # Names or external IDs of groups with access to the doc
    external_user_group_ids: set[str]
    # Whether the document is public in the external system or Onyx
    is_public: bool

    def __str__(self) -> str:
        """Prevent extremely long logs"""

        def truncate_set(s: set[str], max_len: int = 100) -> str:
            s_str = str(s)
            if len(s_str) > max_len:
                return f"{s_str[:max_len]}... ({len(s)} items)"
            return s_str

        return (
            f"ExternalAccess("
            f"external_user_emails={truncate_set(self.external_user_emails)}, "
            f"external_user_group_ids={truncate_set(self.external_user_group_ids)}, "
            f"is_public={self.is_public})"
        )

    @property
    def num_entries(self) -> int:
        return len(self.external_user_emails) + len(self.external_user_group_ids)

    @classmethod
    def public(cls) -> "ExternalAccess":
        return cls(
            external_user_emails=set(),
            external_user_group_ids=set(),
            is_public=True,
        )

    @classmethod
    def empty(cls) -> "ExternalAccess":
        """
        A helper function that returns an *empty* set of external user-emails and group-ids, and sets `is_public` to `False`.
        This effectively makes the document in question "private" or inaccessible to anyone else.

        This is especially helpful to use when you are performing permission-syncing, and some document's permissions aren't able
        to be determined (for whatever reason). Setting its `ExternalAccess` to "private" is a feasible fallback.
        """

        return cls(
            external_user_emails=set(),
            external_user_group_ids=set(),
            is_public=False,
        )


class ExtractionResult(NamedTuple):
    """Structured result from text and image extraction from various file types."""

    text_content: str
    embedded_images: Sequence[tuple[bytes, str]]
    metadata: dict[str, Any]


class TextSection(BaseModel):
    """Text section model"""
    link: str
    text: str


class ImageSection(BaseModel):
    """Image section model"""
    link: str
    image_file_id: str


class Document(BaseModel):
    """Document model"""
    id: str
    source: str
    semantic_identifier: str
    extension: str
    blob: bytes
    doc_updated_at: datetime
    size_bytes: int


class BasicExpertInfo(BaseModel):
    """Expert information model"""
    display_name: Optional[str] = None
    first_name: Optional[str] = None
    last_name: Optional[str] = None
    email: Optional[str] = None

    def get_semantic_name(self) -> str:
        """Get semantic name for display"""
        if self.display_name:
            return self.display_name
        elif self.first_name and self.last_name:
            return f"{self.first_name} {self.last_name}"
        elif self.first_name:
            return self.first_name
        elif self.last_name:
            return self.last_name
        else:
            return "Unknown"


class SlimDocument(BaseModel):
    """Simplified document model (contains only ID and permission info)"""
    id: str
    external_access: Optional[Any] = None


class ConnectorCheckpoint(BaseModel):
    """Connector checkpoint model"""
    has_more: bool = True


class DocumentFailure(BaseModel):
    """Document processing failure information"""
    document_id: str
    document_link: str


class EntityFailure(BaseModel):
    """Entity processing failure information"""
    entity_id: str
    missed_time_range: tuple[datetime, datetime]


class ConnectorFailure(BaseModel):
    """Connector failure information"""
    failed_document: Optional[DocumentFailure] = None
    failed_entity: Optional[EntityFailure] = None
    failure_message: str
    exception: Optional[Exception] = None

    model_config = {"arbitrary_types_allowed": True}


# Gmail Models
class GmailCredentials(BaseModel):
    """Gmail authentication credentials model"""
    primary_admin_email: str
    credentials: dict[str, Any]


class GmailThread(BaseModel):
    """Gmail thread data model"""
    id: str
    messages: list[dict[str, Any]]


class GmailMessage(BaseModel):
    """Gmail message data model"""
    id: str
    payload: dict[str, Any]
    label_ids: Optional[list[str]] = None


# Notion Models
class NotionPage(BaseModel):
    """Represents a Notion Page object"""
    id: str
    created_time: str
    last_edited_time: str
    archived: bool
    properties: dict[str, Any]
    url: str
    database_name: Optional[str] = None  # Only applicable to database type pages


class NotionBlock(BaseModel):
    """Represents a Notion Block object"""
    id: str  # Used for the URL
    text: str
    prefix: str  # How this block should be joined with existing text


class NotionSearchResponse(BaseModel):
    """Represents the response from the Notion Search API"""
    results: list[dict[str, Any]]
    next_cursor: Optional[str]
    has_more: bool = False


class NotionCredentials(BaseModel):
    """Notion authentication credentials model"""
    integration_token: str


# Slack Models
class ChannelTopicPurposeType(TypedDict):
    """Slack channel topic or purpose"""
    value: str
    creator: str
    last_set: int


class ChannelType(TypedDict):
    """Slack channel"""
    id: str
    name: str
    is_channel: bool
    is_group: bool
    is_im: bool
    created: int
    creator: str
    is_archived: bool
    is_general: bool
    unlinked: int
    name_normalized: str
    is_shared: bool
    is_ext_shared: bool
    is_org_shared: bool
    pending_shared: List[str]
    is_pending_ext_shared: bool
    is_member: bool
    is_private: bool
    is_mpim: bool
    updated: int
    topic: ChannelTopicPurposeType
    purpose: ChannelTopicPurposeType
    previous_names: List[str]
    num_members: int


class AttachmentType(TypedDict):
    """Slack message attachment"""
    service_name: NotRequired[str]
    text: NotRequired[str]
    fallback: NotRequired[str]
    thumb_url: NotRequired[str]
    thumb_width: NotRequired[int]
    thumb_height: NotRequired[int]
    id: NotRequired[int]


class BotProfileType(TypedDict):
    """Slack bot profile"""
    id: NotRequired[str]
    deleted: NotRequired[bool]
    name: NotRequired[str]
    updated: NotRequired[int]
    app_id: NotRequired[str]
    team_id: NotRequired[str]


class MessageType(TypedDict):
    """Slack message"""
    type: str
    user: str
    text: str
    ts: str
    attachments: NotRequired[List[AttachmentType]]
    bot_id: NotRequired[str]
    app_id: NotRequired[str]
    bot_profile: NotRequired[BotProfileType]
    thread_ts: NotRequired[str]
    subtype: NotRequired[str]


# Thread message list
ThreadType = List[MessageType]


class SlackCheckpoint(TypedDict):
    """Slack checkpoint"""
    channel_ids: List[str] | None
    channel_completion_map: dict[str, str]
    current_channel: ChannelType | None
    current_channel_access: Any | None
    seen_thread_ts: List[str]
    has_more: bool


class SlackMessageFilterReason(str):
    """Slack message filter reason"""
    BOT = "bot"
    DISALLOWED = "disallowed"


class ProcessedSlackMessage:
    """Processed Slack message"""
    def __init__(self, doc=None, thread_or_message_ts=None, filter_reason=None, failure=None):
        self.doc = doc
        self.thread_or_message_ts = thread_or_message_ts
        self.filter_reason = filter_reason
        self.failure = failure


# Type aliases for type hints
SecondsSinceUnixEpoch = float
GenerateDocumentsOutput = Any
GenerateSlimDocumentOutput = Any
CheckpointOutput = Any

```

## Detailed Analysis

### File Role in Repository

The file `common/data_source/models.py` is located in the `common/data_source` directory.

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
