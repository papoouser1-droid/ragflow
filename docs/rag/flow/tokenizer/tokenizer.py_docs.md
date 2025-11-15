# Documentation: rag/flow/tokenizer/tokenizer.py

## File Metadata

- **Path**: `rag/flow/tokenizer/tokenizer.py`
- **Size**: 8154 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `rag/flow/tokenizer/tokenizer.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `random`
- `re`
- `numpy`
- `trio`
- `common.constants`
- `api.db.services.knowledgebase_service`
- `api.db.services.llm_service`
- `api.db.services.user_service`
- `common.connection_utils`
- `rag.flow.base`
- `rag.flow.tokenizer.schema`
- `rag.nlp`
- `common`
- `rag.svr.task_executor`
- `common.token_utils`

### Classes Defined

This file defines 2 class(es):

#### Class: `TokenizerParam` (line 35)

**Methods**: __init__, check, get_input_form

#### Class: `Tokenizer` (line 50)

### Functions Defined

This file defines 4 function(s):

#### Function: `__init__` (line 36)

**Parameters**: self

#### Function: `check` (line 42)

**Parameters**: self

#### Function: `get_input_form` (line 46)

**Parameters**: self

#### Function: `batch_encode` (line 80)

**Parameters**: txts

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
import logging
import random
import re

import numpy as np
import trio

from common.constants import LLMType
from api.db.services.knowledgebase_service import KnowledgebaseService
from api.db.services.llm_service import LLMBundle
from api.db.services.user_service import TenantService
from common.connection_utils import timeout
from rag.flow.base import ProcessBase, ProcessParamBase
from rag.flow.tokenizer.schema import TokenizerFromUpstream
from rag.nlp import rag_tokenizer
from common import settings
from rag.svr.task_executor import embed_limiter
from common.token_utils import truncate


class TokenizerParam(ProcessParamBase):
    def __init__(self):
        super().__init__()
        self.search_method = ["full_text", "embedding"]
        self.filename_embd_weight = 0.1
        self.fields = ["text"]

    def check(self):
        for v in self.search_method:
            self.check_valid_value(v.lower(), "Chunk method abnormal.", ["full_text", "embedding"])

    def get_input_form(self) -> dict[str, dict]:
        return {}


class Tokenizer(ProcessBase):
    component_name = "Tokenizer"

    async def _embedding(self, name, chunks):
        parts = sum(["full_text" in self._param.search_method, "embedding" in self._param.search_method])
        token_count = 0
        if self._canvas._kb_id:
            e, kb = KnowledgebaseService.get_by_id(self._canvas._kb_id)
            embedding_id = kb.embd_id
        else:
            e, ten = TenantService.get_by_id(self._canvas._tenant_id)
            embedding_id = ten.embd_id
        embedding_model = LLMBundle(self._canvas._tenant_id, LLMType.EMBEDDING, llm_name=embedding_id)
        texts = []
        for c in chunks:
            txt = ""
            if isinstance(self._param.fields, str):
                self._param.fields=[self._param.fields]
            for f in self._param.fields:
                f = c.get(f)
                if isinstance(f, str):
                    txt += f
                elif isinstance(f, list):
                    txt += "\n".join(f)
            texts.append(re.sub(r"</?(table|td|caption|tr|th)( [^<>]{0,12})?>", " ", txt))
        vts, c = embedding_model.encode([name])
        token_count += c
        tts = np.concatenate([vts[0] for _ in range(len(texts))], axis=0)

        @timeout(60)
        def batch_encode(txts):
            nonlocal embedding_model
            return embedding_model.encode([truncate(c, embedding_model.max_length - 10) for c in txts])

        cnts_ = np.array([])
        for i in range(0, len(texts), settings.EMBEDDING_BATCH_SIZE):
            async with embed_limiter:
                vts, c = await trio.to_thread.run_sync(lambda: batch_encode(texts[i : i + settings.EMBEDDING_BATCH_SIZE]))
            if len(cnts_) == 0:
                cnts_ = vts
            else:
                cnts_ = np.concatenate((cnts_, vts), axis=0)
            token_count += c
            if i % 33 == 32:
                self.callback(i * 1.0 / len(texts) / parts / settings.EMBEDDING_BATCH_SIZE + 0.5 * (parts - 1))

        cnts = cnts_
        title_w = float(self._param.filename_embd_weight)
        vects = (title_w * tts + (1 - title_w) * cnts) if len(tts) == len(cnts) else cnts

        assert len(vects) == len(chunks)
        for i, ck in enumerate(chunks):
            v = vects[i].tolist()
            ck["q_%d_vec" % len(v)] = v
        return chunks, token_count

    async def _invoke(self, **kwargs):
        try:
            from_upstream = TokenizerFromUpstream.model_validate(kwargs)
        except Exception as e:
            self.set_output("_ERROR", f"Input error: {str(e)}")
            return

        self.set_output("output_format", "chunks")
        parts = sum(["full_text" in self._param.search_method, "embedding" in self._param.search_method])
        if "full_text" in self._param.search_method:
            self.callback(random.randint(1, 5) / 100.0, "Start to tokenize.")
            if from_upstream.chunks:
                chunks = from_upstream.chunks
                for i, ck in enumerate(chunks):
                    ck["title_tks"] = rag_tokenizer.tokenize(re.sub(r"\.[a-zA-Z]+$", "", from_upstream.name))
                    ck["title_sm_tks"] = rag_tokenizer.fine_grained_tokenize(ck["title_tks"])
                    if ck.get("questions"):
                        ck["question_kwd"] = ck["questions"].split("\n")
                        ck["question_tks"] = rag_tokenizer.tokenize(str(ck["questions"]))
                    if ck.get("keywords"):
                        ck["important_kwd"] = ck["keywords"].split(",")
                        ck["important_tks"] = rag_tokenizer.tokenize(str(ck["keywords"]))
                    if ck.get("summary"):
                        ck["content_ltks"] = rag_tokenizer.tokenize(str(ck["summary"]))
                        ck["content_sm_ltks"] = rag_tokenizer.fine_grained_tokenize(ck["content_ltks"])
                    elif ck.get("text"):
                        ck["content_ltks"] = rag_tokenizer.tokenize(ck["text"])
                        ck["content_sm_ltks"] = rag_tokenizer.fine_grained_tokenize(ck["content_ltks"])
                    if i % 100 == 99:
                        self.callback(i * 1.0 / len(chunks) / parts)

            elif from_upstream.output_format in ["markdown", "text", "html"]:
                if from_upstream.output_format == "markdown":
                    payload = from_upstream.markdown_result
                elif from_upstream.output_format == "text":
                    payload = from_upstream.text_result
                else:
                    payload = from_upstream.html_result

                if not payload:
                    return ""

                ck = {"text": payload}
                if "full_text" in self._param.search_method:
                    ck["title_tks"] = rag_tokenizer.tokenize(re.sub(r"\.[a-zA-Z]+$", "", from_upstream.name))
                    ck["title_sm_tks"] = rag_tokenizer.fine_grained_tokenize(ck["title_tks"])
                    ck["content_ltks"] = rag_tokenizer.tokenize(payload)
                    ck["content_sm_ltks"] = rag_tokenizer.fine_grained_tokenize(ck["content_ltks"])
                chunks = [ck]
            else:
                chunks = from_upstream.json_result
                for i, ck in enumerate(chunks):
                    ck["title_tks"] = rag_tokenizer.tokenize(re.sub(r"\.[a-zA-Z]+$", "", from_upstream.name))
                    ck["title_sm_tks"] = rag_tokenizer.fine_grained_tokenize(ck["title_tks"])
                    if not ck.get("text"):
                        continue
                    ck["content_ltks"] = rag_tokenizer.tokenize(ck["text"])
                    ck["content_sm_ltks"] = rag_tokenizer.fine_grained_tokenize(ck["content_ltks"])
                    if i % 100 == 99:
                        self.callback(i * 1.0 / len(chunks) / parts)

            self.callback(1.0 / parts, "Finish tokenizing.")

        if "embedding" in self._param.search_method:
            self.callback(random.randint(1, 5) / 100.0 + 0.5 * (parts - 1), "Start embedding inference.")

            if from_upstream.name.strip() == "":
                logging.warning("Tokenizer: empty name provided from upstream, embedding may be not accurate.")

            chunks, token_count = await self._embedding(from_upstream.name, chunks)
            self.set_output("embedding_token_consumption", token_count)

            self.callback(1.0, "Finish embedding.")

        self.set_output("chunks", chunks)

```

## Detailed Analysis

### File Role in Repository

The file `rag/flow/tokenizer/tokenizer.py` is located in the `rag/flow/tokenizer` directory.

This file is part of the **RAG (Retrieval-Augmented Generation)** core engine.

### Architecture Context

Files in this location typically handle concerns related to tokenizer.

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
- [schema.py](schema.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
