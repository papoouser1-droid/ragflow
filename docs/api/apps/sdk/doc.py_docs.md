# Documentation: api/apps/sdk/doc.py

## File Metadata

- **Path**: `api/apps/sdk/doc.py`
- **Size**: 54536 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `api/apps/sdk/doc.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `datetime`
- `logging`
- `pathlib`
- `re`
- `io`
- `xxhash`
- `flask`
- `peewee`
- `pydantic`
- `api.constants`
- `api.db`
- `api.db.db_models`
- `api.db.services.document_service`
- `api.db.services.file2document_service`
- `api.db.services.file_service`
- `api.db.services.knowledgebase_service`
- `api.db.services.llm_service`
- `api.db.services.tenant_llm_service`
- `api.db.services.task_service`
- `api.db.services.dialog_service`

### Classes Defined

This file defines 1 class(es):

#### Class: `Chunk` (line 50)

**Methods**: validate_positions

### Functions Defined

This file defines 13 function(s):

#### Function: `upload` (line 72)

**Parameters**: dataset_id, tenant_id

**Docstring**: Upload documents to a dataset.
---
tags:
  - Documents
security:
  - ApiKeyAuth: []
parameters:
  - in: path
    name: dataset_id
    type: string
    required: true
    description: ID of the dataset...

#### Function: `update_doc` (line 182)

**Parameters**: tenant_id, dataset_id, document_id

**Docstring**: Update a document within a dataset.
---
tags:
  - Documents
security:
  - ApiKeyAuth: []
parameters:
  - in: path
    name: dataset_id
    type: string
    required: true
    description: ID of the da...

#### Function: `download` (line 362)

**Parameters**: tenant_id, dataset_id, document_id

**Docstring**: Download a document from a dataset.
---
tags:
  - Documents
security:
  - ApiKeyAuth: []
produces:
  - application/octet-stream
parameters:
  - in: path
    name: dataset_id
    type: string
    requi...

#### Function: `list_docs` (line 422)

**Parameters**: dataset_id, tenant_id

**Docstring**: List documents in a dataset.
---
tags:
  - Documents
security:
  - ApiKeyAuth: []
parameters:
  - in: path
    name: dataset_id
    type: string
    required: true
    description: ID of the dataset.
...

#### Function: `delete` (line 592)

**Parameters**: tenant_id, dataset_id

**Docstring**: Delete documents from a dataset.
---
tags:
  - Documents
security:
  - ApiKeyAuth: []
parameters:
  - in: path
    name: dataset_id
    type: string
    required: true
    description: ID of the datas...

#### Function: `parse` (line 702)

**Parameters**: tenant_id, dataset_id

**Docstring**: Start parsing documents into chunks.
---
tags:
  - Chunks
security:
  - ApiKeyAuth: []
parameters:
  - in: path
    name: dataset_id
    type: string
    required: true
    description: ID of the data...

#### Function: `stop_parsing` (line 785)

**Parameters**: tenant_id, dataset_id

**Docstring**: Stop parsing documents into chunks.
---
tags:
  - Chunks
security:
  - ApiKeyAuth: []
parameters:
  - in: path
    name: dataset_id
    type: string
    required: true
    description: ID of the datas...

#### Function: `list_chunks` (line 856)

**Parameters**: tenant_id, dataset_id, document_id

**Docstring**: List chunks of a document.
---
tags:
  - Chunks
security:
  - ApiKeyAuth: []
parameters:
  - in: path
    name: dataset_id
    type: string
    required: true
    description: ID of the dataset.
  - i...

#### Function: `add_chunk` (line 1026)

**Parameters**: tenant_id, dataset_id, document_id

**Docstring**: Add a chunk to a document.
---
tags:
  - Chunks
security:
  - ApiKeyAuth: []
parameters:
  - in: path
    name: dataset_id
    type: string
    required: true
    description: ID of the dataset.
  - i...

#### Function: `rm_chunk` (line 1155)

**Parameters**: tenant_id, dataset_id, document_id

**Docstring**: Remove chunks from a document.
---
tags:
  - Chunks
security:
  - ApiKeyAuth: []
parameters:
  - in: path
    name: dataset_id
    type: string
    required: true
    description: ID of the dataset.
 ...

#### Function: `update_chunk` (line 1226)

**Parameters**: tenant_id, dataset_id, document_id, chunk_id

**Docstring**: Update a chunk within a document.
---
tags:
  - Chunks
security:
  - ApiKeyAuth: []
parameters:
  - in: path
    name: dataset_id
    type: string
    required: true
    description: ID of the dataset...

#### Function: `retrieval_test` (line 1330)

**Parameters**: tenant_id

**Docstring**: Retrieve chunks based on a query.
---
tags:
  - Retrieval
security:
  - ApiKeyAuth: []
parameters:
  - in: body
    name: body
    description: Retrieval parameters.
    required: true
    schema:
   ...

#### Function: `validate_positions` (line 63)

**Parameters**: cls, value

## Original Source Code

```py
#
#  Copyright 2024 The InfiniFlow Authors. All Rights Reserved.
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
import datetime
import logging
import pathlib
import re
from io import BytesIO

import xxhash
from flask import request, send_file
from peewee import OperationalError
from pydantic import BaseModel, Field, validator

from api.constants import FILE_NAME_LEN_LIMIT
from api.db import FileType
from api.db.db_models import File, Task
from api.db.services.document_service import DocumentService
from api.db.services.file2document_service import File2DocumentService
from api.db.services.file_service import FileService
from api.db.services.knowledgebase_service import KnowledgebaseService
from api.db.services.llm_service import LLMBundle
from api.db.services.tenant_llm_service import TenantLLMService
from api.db.services.task_service import TaskService, queue_tasks
from api.db.services.dialog_service import meta_filter, convert_conditions
from api.utils.api_utils import check_duplicate_ids, construct_json_result, get_error_data_result, get_parser_config, get_result, server_error_response, token_required
from rag.app.qa import beAdoc, rmPrefix
from rag.app.tag import label_question
from rag.nlp import rag_tokenizer, search
from rag.prompts.generator import cross_languages, keyword_extraction
from common.string_utils import remove_redundant_spaces
from common.constants import RetCode, LLMType, ParserType, TaskStatus, FileSource
from common import settings

MAXIMUM_OF_UPLOADING_FILES = 256


class Chunk(BaseModel):
    id: str = ""
    content: str = ""
    document_id: str = ""
    docnm_kwd: str = ""
    important_keywords: list = Field(default_factory=list)
    questions: list = Field(default_factory=list)
    question_tks: str = ""
    image_id: str = ""
    available: bool = True
    positions: list[list[int]] = Field(default_factory=list)

    @validator("positions")
    def validate_positions(cls, value):
        for sublist in value:
            if len(sublist) != 5:
                raise ValueError("Each sublist in positions must have a length of 5")
        return value


@manager.route("/datasets/<dataset_id>/documents", methods=["POST"])  # noqa: F821
@token_required
def upload(dataset_id, tenant_id):
    """
    Upload documents to a dataset.
    ---
    tags:
      - Documents
    security:
      - ApiKeyAuth: []
    parameters:
      - in: path
        name: dataset_id
        type: string
        required: true
        description: ID of the dataset.
      - in: header
        name: Authorization
        type: string
        required: true
        description: Bearer token for authentication.
      - in: formData
        name: file
        type: file
        required: true
        description: Document files to upload.
      - in: formData
        name: parent_path
        type: string
        description: Optional nested path under the parent folder. Uses '/' separators.
    responses:
      200:
        description: Successfully uploaded documents.
        schema:
          type: object
          properties:
            data:
              type: array
              items:
                type: object
                properties:
                  id:
                    type: string
                    description: Document ID.
                  name:
                    type: string
                    description: Document name.
                  chunk_count:
                    type: integer
                    description: Number of chunks.
                  token_count:
                    type: integer
                    description: Number of tokens.
                  dataset_id:
                    type: string
                    description: ID of the dataset.
                  chunk_method:
                    type: string
                    description: Chunking method used.
                  run:
                    type: string
                    description: Processing status.
    """
    if "file" not in request.files:
        return get_error_data_result(message="No file part!", code=RetCode.ARGUMENT_ERROR)
    file_objs = request.files.getlist("file")
    for file_obj in file_objs:
        if file_obj.filename == "":
            return get_result(message="No file selected!", code=RetCode.ARGUMENT_ERROR)
        if len(file_obj.filename.encode("utf-8")) > FILE_NAME_LEN_LIMIT:
            return get_result(message=f"File name must be {FILE_NAME_LEN_LIMIT} bytes or less.", code=RetCode.ARGUMENT_ERROR)
    """
    # total size
    total_size = 0
    for file_obj in file_objs:
        file_obj.seek(0, os.SEEK_END)
        total_size += file_obj.tell()
        file_obj.seek(0)
    MAX_TOTAL_FILE_SIZE = 10 * 1024 * 1024
    if total_size > MAX_TOTAL_FILE_SIZE:
        return get_result(
            message=f"Total file size exceeds 10MB limit! ({total_size / (1024 * 1024):.2f} MB)",
            code=RetCode.ARGUMENT_ERROR,
        )
    """
    e, kb = KnowledgebaseService.get_by_id(dataset_id)
    if not e:
        raise LookupError(f"Can't find the dataset with ID {dataset_id}!")
    err, files = FileService.upload_document(kb, file_objs, tenant_id, parent_path=request.form.get("parent_path"))
    if err:
        return get_result(message="\n".join(err), code=RetCode.SERVER_ERROR)
    # rename key's name
    renamed_doc_list = []
    for file in files:
        doc = file[0]
        key_mapping = {
            "chunk_num": "chunk_count",
            "kb_id": "dataset_id",
            "token_num": "token_count",
            "parser_id": "chunk_method",
        }
        renamed_doc = {}
        for key, value in doc.items():
            new_key = key_mapping.get(key, key)
            renamed_doc[new_key] = value
        renamed_doc["run"] = "UNSTART"
        renamed_doc_list.append(renamed_doc)
    return get_result(data=renamed_doc_list)


@manager.route("/datasets/<dataset_id>/documents/<document_id>", methods=["PUT"])  # noqa: F821
@token_required
def update_doc(tenant_id, dataset_id, document_id):
    """
    Update a document within a dataset.
    ---
    tags:
      - Documents
    security:
      - ApiKeyAuth: []
    parameters:
      - in: path
        name: dataset_id
        type: string
        required: true
        description: ID of the dataset.
      - in: path
        name: document_id
        type: string
        required: true
        description: ID of the document to update.
      - in: header
        name: Authorization
        type: string
        required: true
        description: Bearer token for authentication.
      - in: body
        name: body
        description: Document update parameters.
        required: true
        schema:
          type: object
          properties:
            name:
              type: string
              description: New name of the document.
            parser_config:
              type: object
              description: Parser configuration.
            chunk_method:
              type: string
              description: Chunking method.
            enabled:
              type: boolean
              description: Document status.
    responses:
      200:
        description: Document updated successfully.
        schema:
          type: object
    """
    req = request.json
    if not KnowledgebaseService.query(id=dataset_id, tenant_id=tenant_id):
        return get_error_data_result(message="You don't own the dataset.")
    e, kb = KnowledgebaseService.get_by_id(dataset_id)
    if not e:
        return get_error_data_result(message="Can't find this knowledgebase!")
    doc = DocumentService.query(kb_id=dataset_id, id=document_id)
    if not doc:
        return get_error_data_result(message="The dataset doesn't own the document.")
    doc = doc[0]
    if "chunk_count" in req:
        if req["chunk_count"] != doc.chunk_num:
            return get_error_data_result(message="Can't change `chunk_count`.")
    if "token_count" in req:
        if req["token_count"] != doc.token_num:
            return get_error_data_result(message="Can't change `token_count`.")
    if "progress" in req:
        if req["progress"] != doc.progress:
            return get_error_data_result(message="Can't change `progress`.")

    if "meta_fields" in req:
        if not isinstance(req["meta_fields"], dict):
            return get_error_data_result(message="meta_fields must be a dictionary")
        DocumentService.update_meta_fields(document_id, req["meta_fields"])

    if "name" in req and req["name"] != doc.name:
        if len(req["name"].encode("utf-8")) > FILE_NAME_LEN_LIMIT:
            return get_result(
                message=f"File name must be {FILE_NAME_LEN_LIMIT} bytes or less.",
                code=RetCode.ARGUMENT_ERROR,
            )
        if pathlib.Path(req["name"].lower()).suffix != pathlib.Path(doc.name.lower()).suffix:
            return get_result(
                message="The extension of file can't be changed",
                code=RetCode.ARGUMENT_ERROR,
            )
        for d in DocumentService.query(name=req["name"], kb_id=doc.kb_id):
            if d.name == req["name"]:
                return get_error_data_result(message="Duplicated document name in the same dataset.")
        if not DocumentService.update_by_id(document_id, {"name": req["name"]}):
            return get_error_data_result(message="Database error (Document rename)!")

        informs = File2DocumentService.get_by_document_id(document_id)
        i

... [Content truncated - file is 54536 bytes] ...

    description: Availability status of the chunk.
      - in: header
        name: Authorization
        type: string
        required: true
        description: Bearer token for authentication.
    responses:
      200:
        description: Chunk updated successfully.
        schema:
          type: object
    """
    chunk = settings.docStoreConn.get(chunk_id, search.index_name(tenant_id), [dataset_id])
    if chunk is None:
        return get_error_data_result(f"Can't find this chunk {chunk_id}")
    if not KnowledgebaseService.accessible(kb_id=dataset_id, user_id=tenant_id):
        return get_error_data_result(message=f"You don't own the dataset {dataset_id}.")
    doc = DocumentService.query(id=document_id, kb_id=dataset_id)
    if not doc:
        return get_error_data_result(message=f"You don't own the document {document_id}.")
    doc = doc[0]
    req = request.json
    if "content" in req:
        content = req["content"]
    else:
        content = chunk.get("content_with_weight", "")
    d = {"id": chunk_id, "content_with_weight": content}
    d["content_ltks"] = rag_tokenizer.tokenize(d["content_with_weight"])
    d["content_sm_ltks"] = rag_tokenizer.fine_grained_tokenize(d["content_ltks"])
    if "important_keywords" in req:
        if not isinstance(req["important_keywords"], list):
            return get_error_data_result("`important_keywords` should be a list")
        d["important_kwd"] = req.get("important_keywords", [])
        d["important_tks"] = rag_tokenizer.tokenize(" ".join(req["important_keywords"]))
    if "questions" in req:
        if not isinstance(req["questions"], list):
            return get_error_data_result("`questions` should be a list")
        d["question_kwd"] = [str(q).strip() for q in req.get("questions", []) if str(q).strip()]
        d["question_tks"] = rag_tokenizer.tokenize("\n".join(req["questions"]))
    if "available" in req:
        d["available_int"] = int(req["available"])
    if "positions" in req:
        if not isinstance(req["positions"], list):
            return get_error_data_result("`positions` should be a list")
        d["position_int"] = req["positions"]
    embd_id = DocumentService.get_embd_id(document_id)
    embd_mdl = TenantLLMService.model_instance(tenant_id, LLMType.EMBEDDING.value, embd_id)
    if doc.parser_id == ParserType.QA:
        arr = [t for t in re.split(r"[\n\t]", d["content_with_weight"]) if len(t) > 1]
        if len(arr) != 2:
            return get_error_data_result(message="Q&A must be separated by TAB/ENTER key.")
        q, a = rmPrefix(arr[0]), rmPrefix(arr[1])
        d = beAdoc(d, arr[0], arr[1], not any([rag_tokenizer.is_chinese(t) for t in q + a]))

    v, c = embd_mdl.encode([doc.name, d["content_with_weight"] if not d.get("question_kwd") else "\n".join(d["question_kwd"])])
    v = 0.1 * v[0] + 0.9 * v[1] if doc.parser_id != ParserType.QA else v[1]
    d["q_%d_vec" % len(v)] = v.tolist()
    settings.docStoreConn.update({"id": chunk_id}, d, search.index_name(tenant_id), dataset_id)
    return get_result()


@manager.route("/retrieval", methods=["POST"])  # noqa: F821
@token_required
def retrieval_test(tenant_id):
    """
    Retrieve chunks based on a query.
    ---
    tags:
      - Retrieval
    security:
      - ApiKeyAuth: []
    parameters:
      - in: body
        name: body
        description: Retrieval parameters.
        required: true
        schema:
          type: object
          properties:
            dataset_ids:
              type: array
              items:
                type: string
              required: true
              description: List of dataset IDs to search in.
            question:
              type: string
              required: true
              description: Query string.
            document_ids:
              type: array
              items:
                type: string
              description: List of document IDs to filter.
            similarity_threshold:
              type: number
              format: float
              description: Similarity threshold.
            vector_similarity_weight:
              type: number
              format: float
              description: Vector similarity weight.
            top_k:
              type: integer
              description: Maximum number of chunks to return.
            highlight:
              type: boolean
              description: Whether to highlight matched content.
            metadata_condition:
              type: object
              description: metadata filter condition.
      - in: header
        name: Authorization
        type: string
        required: true
        description: Bearer token for authentication.
    responses:
      200:
        description: Retrieval results.
        schema:
          type: object
          properties:
            chunks:
              type: array
              items:
                type: object
                properties:
                  id:
                    type: string
                    description: Chunk ID.
                  content:
                    type: string
                    description: Chunk content.
                  document_id:
                    type: string
                    description: ID of the document.
                  dataset_id:
                    type: string
                    description: ID of the dataset.
                  similarity:
                    type: number
                    format: float
                    description: Similarity score.
    """
    req = request.json
    if not req.get("dataset_ids"):
        return get_error_data_result("`dataset_ids` is required.")
    kb_ids = req["dataset_ids"]
    if not isinstance(kb_ids, list):
        return get_error_data_result("`dataset_ids` should be a list")
    for id in kb_ids:
        if not KnowledgebaseService.accessible(kb_id=id, user_id=tenant_id):
            return get_error_data_result(f"You don't own the dataset {id}.")
    kbs = KnowledgebaseService.get_by_ids(kb_ids)
    embd_nms = list(set([TenantLLMService.split_model_name_and_factory(kb.embd_id)[0] for kb in kbs]))  # remove vendor suffix for comparison
    if len(embd_nms) != 1:
        return get_result(
            message='Datasets use different embedding models."',
            code=RetCode.DATA_ERROR,
        )
    if "question" not in req:
        return get_error_data_result("`question` is required.")
    page = int(req.get("page", 1))
    size = int(req.get("page_size", 30))
    question = req["question"]
    doc_ids = req.get("document_ids", [])
    use_kg = req.get("use_kg", False)
    langs = req.get("cross_languages", [])
    if not isinstance(doc_ids, list):
        return get_error_data_result("`documents` should be a list")
    doc_ids_list = KnowledgebaseService.list_documents_by_ids(kb_ids)
    for doc_id in doc_ids:
        if doc_id not in doc_ids_list:
            return get_error_data_result(f"The datasets don't own the document {doc_id}")
    if not doc_ids:
        metadata_condition = req.get("metadata_condition", {})
        metas = DocumentService.get_meta_by_kbs(kb_ids)
        doc_ids = meta_filter(metas, convert_conditions(metadata_condition))
    similarity_threshold = float(req.get("similarity_threshold", 0.2))
    vector_similarity_weight = float(req.get("vector_similarity_weight", 0.3))
    top = int(req.get("top_k", 1024))
    if req.get("highlight") == "False" or req.get("highlight") == "false":
        highlight = False
    else:
        highlight = True
    try:
        tenant_ids = list(set([kb.tenant_id for kb in kbs]))
        e, kb = KnowledgebaseService.get_by_id(kb_ids[0])
        if not e:
            return get_error_data_result(message="Dataset not found!")
        embd_mdl = LLMBundle(kb.tenant_id, LLMType.EMBEDDING, llm_name=kb.embd_id)

        rerank_mdl = None
        if req.get("rerank_id"):
            rerank_mdl = LLMBundle(kb.tenant_id, LLMType.RERANK, llm_name=req["rerank_id"])

        if langs:
            question = cross_languages(kb.tenant_id, None, question, langs)

        if req.get("keyword", False):
            chat_mdl = LLMBundle(kb.tenant_id, LLMType.CHAT)
            question += keyword_extraction(chat_mdl, question)

        ranks = settings.retriever.retrieval(
            question,
            embd_mdl,
            tenant_ids,
            kb_ids,
            page,
            size,
            similarity_threshold,
            vector_similarity_weight,
            top,
            doc_ids,
            rerank_mdl=rerank_mdl,
            highlight=highlight,
            rank_feature=label_question(question, kbs),
        )
        if use_kg:
            ck = settings.kg_retriever.retrieval(question, [k.tenant_id for k in kbs], kb_ids, embd_mdl, LLMBundle(kb.tenant_id, LLMType.CHAT))
            if ck["content_with_weight"]:
                ranks["chunks"].insert(0, ck)

        for c in ranks["chunks"]:
            c.pop("vector", None)

        ##rename keys
        renamed_chunks = []
        for chunk in ranks["chunks"]:
            key_mapping = {
                "chunk_id": "id",
                "content_with_weight": "content",
                "doc_id": "document_id",
                "important_kwd": "important_keywords",
                "question_kwd": "questions",
                "docnm_kwd": "document_keyword",
                "kb_id": "dataset_id",
            }
            rename_chunk = {}
            for key, value in chunk.items():
                new_key = key_mapping.get(key, key)
                rename_chunk[new_key] = value
            renamed_chunks.append(rename_chunk)
        ranks["chunks"] = renamed_chunks
        return get_result(data=ranks)
    except Exception as e:
        if str(e).find("not_found") > 0:
            return get_result(
                message="No chunk found! Check the chunk status please!",
                code=RetCode.DATA_ERROR,
            )
        return server_error_response(e)

```

## Detailed Analysis

### File Role in Repository

The file `api/apps/sdk/doc.py` is located in the `api/apps/sdk` directory.

This file is part of the **API/Backend** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to sdk.

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

- [agents.py](agents.py_docs.md)
- [chat.py](chat.py_docs.md)
- [dataset.py](dataset.py_docs.md)
- [dify_retrieval.py](dify_retrieval.py_docs.md)
- [files.py](files.py_docs.md)
- [session.py](session.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
