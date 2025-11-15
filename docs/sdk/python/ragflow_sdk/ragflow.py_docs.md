# Documentation: sdk/python/ragflow_sdk/ragflow.py

## File Metadata

- **Path**: `sdk/python/ragflow_sdk/ragflow.py`
- **Size**: 10326 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sdk/python/ragflow_sdk/ragflow.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `typing`
- `requests`
- `modules.agent`
- `modules.chat`
- `modules.chunk`
- `modules.dataset`

### Classes Defined

This file defines 1 class(es):

#### Class: `RAGFlow` (line 26)

**Methods**: __init__, post, get, delete, put, create_dataset, delete_datasets, get_dataset, list_datasets, create_chat, delete_chats, list_chats, retrieve, list_agents, create_agent, update_agent, delete_agent

### Functions Defined

This file defines 17 function(s):

#### Function: `__init__` (line 27)

**Parameters**: self, api_key, base_url, version

**Docstring**: api_url: http://<host_address>/api/v1...

#### Function: `post` (line 35)

**Parameters**: self, path, json, stream, files

#### Function: `get` (line 39)

**Parameters**: self, path, params, json

#### Function: `delete` (line 43)

**Parameters**: self, path, json

#### Function: `put` (line 47)

**Parameters**: self, path, json

#### Function: `create_dataset` (line 51)

**Parameters**: self, name, avatar, description, embedding_model, permission, chunk_method, parser_config

#### Function: `delete_datasets` (line 78)

**Parameters**: self, ids

#### Function: `get_dataset` (line 84)

**Parameters**: self, name

#### Function: `list_datasets` (line 90)

**Parameters**: self, page, page_size, orderby, desc, id, name

#### Function: `create_chat` (line 110)

**Parameters**: self, name, avatar, dataset_ids, llm, prompt

#### Function: `delete_chats` (line 162)

**Parameters**: self, ids

#### Function: `list_chats` (line 168)

**Parameters**: self, page, page_size, orderby, desc, id, name

#### Function: `retrieve` (line 188)

**Parameters**: self, dataset_ids, document_ids, question, page, page_size, similarity_threshold, vector_similarity_weight, top_k, rerank_id, keyword, cross_languages, metadata_condition

#### Function: `list_agents` (line 230)

**Parameters**: self, page, page_size, orderby, desc, id, title

#### Function: `create_agent` (line 250)

**Parameters**: self, title, dsl, description

#### Function: `update_agent` (line 262)

**Parameters**: self, agent_id, title, description, dsl

#### Function: `delete_agent` (line 280)

**Parameters**: self, agent_id

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

from typing import Optional

import requests

from .modules.agent import Agent
from .modules.chat import Chat
from .modules.chunk import Chunk
from .modules.dataset import DataSet


class RAGFlow:
    def __init__(self, api_key, base_url, version="v1"):
        """
        api_url: http://<host_address>/api/v1
        """
        self.user_key = api_key
        self.api_url = f"{base_url}/api/{version}"
        self.authorization_header = {"Authorization": "{} {}".format("Bearer", self.user_key)}

    def post(self, path, json=None, stream=False, files=None):
        res = requests.post(url=self.api_url + path, json=json, headers=self.authorization_header, stream=stream, files=files)
        return res

    def get(self, path, params=None, json=None):
        res = requests.get(url=self.api_url + path, params=params, headers=self.authorization_header, json=json)
        return res

    def delete(self, path, json):
        res = requests.delete(url=self.api_url + path, json=json, headers=self.authorization_header)
        return res

    def put(self, path, json):
        res = requests.put(url=self.api_url + path, json=json, headers=self.authorization_header)
        return res

    def create_dataset(
        self,
        name: str,
        avatar: Optional[str] = None,
        description: Optional[str] = None,
        embedding_model: Optional[str] = None,
        permission: str = "me",
        chunk_method: str = "naive",
        parser_config: Optional[DataSet.ParserConfig] = None,
    ) -> DataSet:
        payload = {
            "name": name,
            "avatar": avatar,
            "description": description,
            "embedding_model": embedding_model,
            "permission": permission,
            "chunk_method": chunk_method,
        }
        if parser_config is not None:
            payload["parser_config"] = parser_config.to_json()

        res = self.post("/datasets", payload)
        res = res.json()
        if res.get("code") == 0:
            return DataSet(self, res["data"])
        raise Exception(res["message"])

    def delete_datasets(self, ids: list[str] | None = None):
        res = self.delete("/datasets", {"ids": ids})
        res = res.json()
        if res.get("code") != 0:
            raise Exception(res["message"])

    def get_dataset(self, name: str):
        _list = self.list_datasets(name=name)
        if len(_list) > 0:
            return _list[0]
        raise Exception("Dataset %s not found" % name)

    def list_datasets(self, page: int = 1, page_size: int = 30, orderby: str = "create_time", desc: bool = True, id: str | None = None, name: str | None = None) -> list[DataSet]:
        res = self.get(
            "/datasets",
            {
                "page": page,
                "page_size": page_size,
                "orderby": orderby,
                "desc": desc,
                "id": id,
                "name": name,
            },
        )
        res = res.json()
        result_list = []
        if res.get("code") == 0:
            for data in res["data"]:
                result_list.append(DataSet(self, data))
            return result_list
        raise Exception(res["message"])

    def create_chat(self, name: str, avatar: str = "", dataset_ids=None, llm: Chat.LLM | None = None, prompt: Chat.Prompt | None = None) -> Chat:
        if dataset_ids is None:
            dataset_ids = []
        dataset_list = []
        for id in dataset_ids:
            dataset_list.append(id)

        if llm is None:
            llm = Chat.LLM(
                self,
                {
                    "model_name": None,
                    "temperature": 0.1,
                    "top_p": 0.3,
                    "presence_penalty": 0.4,
                    "frequency_penalty": 0.7,
                    "max_tokens": 512,
                },
            )
        if prompt is None:
            prompt = Chat.Prompt(
                self,
                {
                    "similarity_threshold": 0.2,
                    "keywords_similarity_weight": 0.7,
                    "top_n": 8,
                    "top_k": 1024,
                    "variables": [{"key": "knowledge", "optional": True}],
                    "rerank_model": "",
                    "empty_response": None,
                    "opener": None,
                    "show_quote": True,
                    "prompt": None,
                },
            )
            if prompt.opener is None:
                prompt.opener = "Hi! I'm your assistant. What can I do for you?"
            if prompt.prompt is None:
                prompt.prompt = (
                    "You are an intelligent assistant. Please summarize the content of the knowledge base to answer the question. "
                    "Please list the data in the knowledge base and answer in detail. When all knowledge base content is irrelevant to the question, "
                    "your answer must include the sentence 'The answer you are looking for is not found in the knowledge base!' "
                    "Answers need to consider chat history.\nHere is the knowledge base:\n{knowledge}\nThe above is the knowledge base."
                )

        temp_dict = {"name": name, "avatar": avatar, "dataset_ids": dataset_list if dataset_list else [], "llm": llm.to_json(), "prompt": prompt.to_json()}
        res = self.post("/chats", temp_dict)
        res = res.json()
        if res.get("code") == 0:
            return Chat(self, res["data"])
        raise Exception(res["message"])

    def delete_chats(self, ids: list[str] | None = None):
        res = self.delete("/chats", {"ids": ids})
        res = res.json()
        if res.get("code") != 0:
            raise Exception(res["message"])

    def list_chats(self, page: int = 1, page_size: int = 30, orderby: str = "create_time", desc: bool = True, id: str | None = None, name: str | None = None) -> list[Chat]:
        res = self.get(
            "/chats",
            {
                "page": page,
                "page_size": page_size,
                "orderby": orderby,
                "desc": desc,
                "id": id,
                "name": name,
            },
        )
        res = res.json()
        result_list = []
        if res.get("code") == 0:
            for data in res["data"]:
                result_list.append(Chat(self, data))
            return result_list
        raise Exception(res["message"])

    def retrieve(
        self,
        dataset_ids,
        document_ids=None,
        question="",
        page=1,
        page_size=30,
        similarity_threshold=0.2,
        vector_similarity_weight=0.3,
        top_k=1024,
        rerank_id: str | None = None,
        keyword: bool = False,
        cross_languages: list[str]|None = None,
        metadata_condition: dict | None = None,
    ):
        if document_ids is None:
            document_ids = []
        data_json = {
            "page": page,
            "page_size": page_size,
            "similarity_threshold": similarity_threshold,
            "vector_similarity_weight": vector_similarity_weight,
            "top_k": top_k,
            "rerank_id": rerank_id,
            "keyword": keyword,
            "question": question,
            "dataset_ids": dataset_ids,
            "document_ids": document_ids,
            "cross_languages": cross_languages,
            "metadata_condition": metadata_condition
        }
        # Send a POST request to the backend service (using requests library as an example, actual implementation may vary)
        res = self.post("/retrieval", json=data_json)
        res = res.json()
        if res.get("code") == 0:
            chunks = []
            for chunk_data in res["data"].get("chunks"):
                chunk = Chunk(self, chunk_data)
                chunks.append(chunk)
            return chunks
        raise Exception(res.get("message"))

    def list_agents(self, page: int = 1, page_size: int = 30, orderby: str = "update_time", desc: bool = True, id: str | None = None, title: str | None = None) -> list[Agent]:
        res = self.get(
            "/agents",
            {
                "page": page,
                "page_size": page_size,
                "orderby": orderby,
                "desc": desc,
                "id": id,
                "title": title,
            },
        )
        res = res.json()
        result_list = []
        if res.get("code") == 0:
            for data in res["data"]:
                result_list.append(Agent(self, data))
            return result_list
        raise Exception(res["message"])

    def create_agent(self, title: str, dsl: dict, description: str | None = None) -> None:
        req = {"title": title, "dsl": dsl}

        if description is not None:
            req["description"] = description

        res = self.post("/agents", req)
        res = res.json()

        if res.get("code") != 0:
            raise Exception(res["message"])

    def update_agent(self, agent_id: str, title: str | None = None, description: str | None = None, dsl: dict | None = None) -> None:
        req = {}

        if title is not None:
            req["title"] = title

        if description is not None:
            req["description"] = description

        if dsl is not None:
            req["dsl"] = dsl

        res = self.put(f"/agents/{agent_id}", req)
        res = res.json()

        if res.get("code") != 0:
            raise Exception(res["message"])

    def delete_agent(self, agent_id: str) -> None:
        res = self.delete(f"/agents/{agent_id}", {})
        res = res.json()

        if res.get("code") != 0:
            raise Exception(res["message"])

```

## Detailed Analysis

### File Role in Repository

The file `sdk/python/ragflow_sdk/ragflow.py` is located in the `sdk/python/ragflow_sdk` directory.

### Architecture Context

Files in this location typically handle concerns related to ragflow_sdk.

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


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
