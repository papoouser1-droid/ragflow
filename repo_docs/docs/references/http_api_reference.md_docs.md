# File Documentation: docs/references/http_api_reference.md

## File Metadata

- **Path**: `docs/references/http_api_reference.md`
- **Extension**: `.md`
- **Lines**: 4,474
- **Characters**: 130,121
- **Size**: 130,173 bytes
- **Purpose**: Documentation - Markdown documentation file

## Original Source

```markdown
---
sidebar_position: 4
slug: /http_api_reference
---

# HTTP API

A complete reference for RAGFlow's RESTful API. Before proceeding, please ensure you [have your RAGFlow API key ready for authentication](https://ragflow.io/docs/dev/acquire_ragflow_api_key).

---

## ERROR CODES

---

| Code | Message               | Description                |
| ---- | --------------------- | -------------------------- |
| 400  | Bad Request           | Invalid request parameters |
| 401  | Unauthorized          | Unauthorized access        |
| 403  | Forbidden             | Access denied              |
| 404  | Not Found             | Resource not found         |
| 500  | Internal Server Error | Server internal error      |
| 1001 | Invalid Chunk ID      | Invalid Chunk ID           |
| 1002 | Chunk Update Failed   | Chunk update failed        |

---

## OpenAI-Compatible API

---

### Create chat completion

**POST** `/api/v1/chats_openai/{chat_id}/chat/completions`

Creates a model response for a given chat conversation.

This API follows the same request and response format as OpenAI's API. It allows you to interact with the model in a manner similar to how you would with [OpenAI's API](https://platform.openai.com/docs/api-reference/chat/create).

#### Request

- Method: POST
- URL: `/api/v1/chats_openai/{chat_id}/chat/completions`
- Headers:
  - `'content-Type: application/json'`
  - `'Authorization: Bearer <YOUR_API_KEY>'`
- Body:
  - `"model"`: `string`
  - `"messages"`: `object list`
  - `"stream"`: `boolean`

##### Request example

```bash
curl --request POST \
     --url http://{address}/api/v1/chats_openai/{chat_id}/chat/completions \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer <YOUR_API_KEY>' \
     --data '{
        "model": "model",
        "messages": [{"role": "user", "content": "Say this is a test!"}],
        "stream": true
      }'
```

##### Request Parameters

- `model` (*Body parameter*) `string`, *Required*  
  The model used to generate the response. The server will parse this automatically, so you can set it to any value for now.

- `messages` (*Body parameter*) `list[object]`, *Required*  
  A list of historical chat messages used to generate the response. This must contain at least one message with the `user` role.

- `stream` (*Body parameter*) `boolean`  
  Whether to receive the response as a stream. Set this to `false` explicitly if you prefer to receive the entire response in one go instead of as a stream.

#### Response

Stream:

```json
data:{
    "id": "chatcmpl-3b0397f277f511f0b47f729e3aa55728",
    "choices": [
        {
            "delta": {
                "content": "Hello! It seems like you're just greeting me. If you have a specific",
                "role": "assistant",
                "function_call": null,
                "tool_calls": null,
                "reasoning_content": null
            },
            "finish_reason": null,
            "index": 0,
            "logprobs": null
        }
    ],
    "created": 1755084508,
    "model": "model",
    "object": "chat.completion.chunk",
    "system_fingerprint": "",
    "usage": null
}

data:{"id": "chatcmpl-3b0397f277f511f0b47f729e3aa55728", "choices": [{"delta": {"content": " question or need information, feel free to ask, and I'll do my best", "role": "assistant", "function_call": null, "tool_calls": null, "reasoning_content": null}, "finish_reason": null, "index": 0, "logprobs": null}], "created": 1755084508, "model": "model", "object": "chat.completion.chunk", "system_fingerprint": "", "usage": null}

data:{"id": "chatcmpl-3b0397f277f511f0b47f729e3aa55728", "choices": [{"delta": {"content": " to assist you based on the knowledge base provided.", "role": "assistant", "function_call": null, "tool_calls": null, "reasoning_content": null}, "finish_reason": null, "index": 0, "logprobs": null}], "created": 1755084508, "model": "model", "object": "chat.completion.chunk", "system_fingerprint": "", "usage": null}

data:{"id": "chatcmpl-3b0397f277f511f0b47f729e3aa55728", "choices": [{"delta": {"content": null, "role": "assistant", "function_call": null, "tool_calls": null, "reasoning_content": null}, "finish_reason": "stop", "index": 0, "logprobs": null}], "created": 1755084508, "model": "model", "object": "chat.completion.chunk", "system_fingerprint": "", "usage": {"prompt_tokens": 5, "completion_tokens": 188, "total_tokens": 193}}

data:[DONE]
```

Non-stream:

```json
{
    "choices": [
        {
            "finish_reason": "stop",
            "index": 0,
            "logprobs": null,
            "message": {
                "content": "Hello! I'm your smart assistant. What can I do for you?",
                "role": "assistant"
            }
        }
    ],
    "created": 1755084403,
    "id": "chatcmpl-3b0397f277f511f0b47f729e3aa55728",
    "model": "model",
    "object": "chat.completion",
    "usage": {
        "completion_tokens": 55,
        "completion_tokens_details": {
            "accepted_prediction_tokens": 55,
            "reasoning_tokens": 5,
            "rejected_prediction_tokens": 0
        },
        "prompt_tokens": 5,
        "total_tokens": 60
    }
}
```

Failure:

```json
{
  "code": 102,
  "message": "The last content of this conversation is not from user."
}
```

---

### Create agent completion

**POST** `/api/v1/agents_openai/{agent_id}/chat/completions`

Creates a model response for a given chat conversation.

This API follows the same request and response format as OpenAI's API. It allows you to interact with the model in a manner similar to how you would with [OpenAI's API](https://platform.openai.com/docs/api-reference/chat/create).

#### Request

- Method: POST
- URL: `/api/v1/agents_openai/{agent_id}/chat/completions`
- Headers:
  - `'content-Type: application/json'`
  - `'Authorization: Bearer <YOUR_API_KEY>'`
- Body:
  - `"model"`: `string`
  - `"messages"`: `object list`
  - `"stream"`: `boolean`

##### Request example

```bash
curl --request POST \
     --url http://{address}/api/v1/agents_openai/{agent_id}/chat/completions \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer <YOUR_API_KEY>' \
     --data '{
        "model": "model",
        "messages": [{"role": "user", "content": "Say this is a test!"}],
        "stream": true
      }'
```

##### Request Parameters

- `model` (*Body parameter*) `string`, *Required*  
  The model used to generate the response. The server will parse this automatically, so you can set it to any value for now.

- `messages` (*Body parameter*) `list[object]`, *Required*  
  A list of historical chat messages used to generate the response. This must contain at least one message with the `user` role.

- `stream` (*Body parameter*) `boolean`  
  Whether to receive the response as a stream. Set this to `false` explicitly if you prefer to receive the entire response in one go instead of as a stream.

- `session_id` (*Body parameter*) `string`  
  Agent session id.

#### Response

Stream:

```json
...

data: {
    "id": "c39f6f9c83d911f0858253708ecb6573",
    "object": "chat.completion.chunk",
    "model": "d1f79142831f11f09cc51795b9eb07c0",
    "choices": [
        {
            "delta": {
                "content": " terminal"
            },
            "finish_reason": null,
            "index": 0
        }
    ]
}

data: {
    "id": "c39f6f9c83d911f0858253708ecb6573",
    "object": "chat.completion.chunk",
    "model": "d1f79142831f11f09cc51795b9eb07c0",
    "choices": [
        {
            "delta": {
                "content": "."
            },
            "finish_reason": null,
            "index": 0
        }
    ]
}

data: {
    "id": "c39f6f9c83d911f0858253708ecb6573",
    "object": "chat.completion.chunk",
    "model": "d1f79142831f11f09cc51795b9eb07c0",
    "choices": [
        {
            "delta": {
                "content": "",
                "reference": {
                    "chunks": {
                        "20": {
                            "id": "4b8935ac0a22deb1",
                            "content": "```cd /usr/ports/editors/neovim/ && make install```## Android[Termux](https://github.com/termux/termux-app) offers a Neovim package.",
                            "document_id": "4bdd2ff65e1511f0907f09f583941b45",
                            "document_name": "INSTALL22.md",
                            "dataset_id": "456ce60c5e1511f0907f09f583941b45",
                            "image_id": "",
                            "positions": [
                                [
                                    12,
                                    11,
                                    11,
                                    11,
                                    11
                                ]
                            ],
                            "url": null,
                            "similarity": 0.5697155305154673,
                            "vector_similarity": 0.7323851005515574,
                            "term_similarity": 0.5000000005,
                            "doc_type": ""
                        }
                    },
                    "doc_aggs": {
                        "INSTALL22.md": {
                            "doc_name": "INSTALL22.md",
                            "doc_id": "4bdd2ff65e1511f0907f09f583941b45",
                            "count": 3
                        },
                        "INSTALL.md": {
                            "doc_name": "INSTALL.md",
                            "doc_id": "4bd7fdd85e1511f0907f09f583941b45",
                            "count": 2
                        },
                        "INSTALL(1).md": {
                            "doc_name": "INSTALL(1).md",
                            "doc_id": "4bdfb42e5e1511f0907f09f583941b45",
                            "count": 2
                        },
                        "INSTALL3.md": {
                            "doc_name": "INSTALL3.md",
                            "doc_id": "4bdab5825e1511f0907f09f583941b45",
                            "count": 1
                        }
                    }
                }
            },
            "finish_reason": null,
            "index": 0
        }
    ]
}

data: [DONE]
```

Non-stream:

```json
{
    "choices": [
        {
            "finish_reason": "stop",
            "index": 0,
            "logprobs": null,
            "message": {
                "content": "\nTo install Neovim, the process varies depending on your operating system:\n\n### For Windows:\n1. **Download from GitHub**: \n   - Visit the [Neovim releases page](https://github.com/neovim/neovim/releases)\n   - Download the latest Windows installer (nvim-win64.msi)\n   - Run the installer and follow the prompts\n\n2. **Using winget** (Windows Package Manager):\n...",
                "reference": {
                    "chunks": {
                        "20": {
                            "content": "```cd /usr/ports/editors/neovim/ && make install```## Android[Termux](https://github.com/termux/termux-app) offers a Neovim package.",
                            "dataset_id": "456ce60c5e1511f0907f09f583941b45",
                            "doc_type": "",
                            "document_id": "4bdd2ff65e1511f0907f09f583941b45",
                            "document_name": "INSTALL22.md",
                            "id": "4b8935ac0a22deb1",
                            "image_id": "",
                            "positions": [
                                [
                                    12,
                                    11,
                                    11,
                                    11,
                                    11
                                ]
                            ],
                            "similarity": 0.5697155305154673,
                            "term_similarity": 0.5000000005,
                            "url": null,
                            "vector_similarity": 0.7323851005515574
                        }
                    },
                    "doc_aggs": {
                        "INSTALL(1).md": {
                            "count": 2,
                            "doc_id": "4bdfb42e5e1511f0907f09f583941b45",
                            "doc_name": "INSTALL(1).md"
                        },
                        "INSTALL.md": {
                            "count": 2,
                            "doc_id": "4bd7fdd85e1511f0907f09f583941b45",
                            "doc_name": "INSTALL.md"
                        },
                        "INSTALL22.md": {
                            "count": 3,
                            "doc_id": "4bdd2ff65e1511f0907f09f583941b45",
                            "doc_name": "INSTALL22.md"
                        },
                        "INSTALL3.md": {
                            "count": 1,
                            "doc_id": "4bdab5825e1511f0907f09f583941b45",
                            "doc_name": "INSTALL3.md"
                        }
                    }
                },
                "role": "assistant"
            }
        }
    ],
    "created": null,
    "id": "c39f6f9c83d911f0858253708ecb6573",
    "model": "d1f79142831f11f09cc51795b9eb07c0",
    "object": "chat.completion",
    "param": null,
    "usage": {
        "completion_tokens": 415,
        "completion_tokens_details": {
            "accepted_prediction_tokens": 0,
            "reasoning_tokens": 0,
            "rejected_prediction_tokens": 0
        },
        "prompt_tokens": 6,
        "total_tokens": 421
    }
}
```

Failure:

```json
{
  "code": 102,
  "message": "The last content of this conversation is not from user."
}
```

## DATASET MANAGEMENT

---

### Create dataset

**POST** `/api/v1/datasets`

Creates a dataset.

#### Request

- Method: POST
- URL: `/api/v1/datasets`
- Headers:
  - `'content-Type: application/json'`
  - `'Authorization: Bearer <YOUR_API_KEY>'`
- Body:
  - `"name"`: `string`
  - `"avatar"`: `string`
  - `"description"`: `string`
  - `"embedding_model"`: `string`
  - `"permission"`: `string`
  - `"chunk_method"`: `string`
  - `"parser_config"`: `object`

##### Request example

```bash
curl --request POST \
     --url http://{address}/api/v1/datasets \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer <YOUR_API_KEY>' \
     --data '{
      "name": "test_1"
      }'
```

##### Request parameters

- `"name"`: (*Body parameter*), `string`, *Required*  
  The unique name of the dataset to create. It must adhere to the following requirements:  
  - Basic Multilingual Plane (BMP) only
  - Maximum 128 characters
  - Case-insensitive

- `"avatar"`: (*Body parameter*), `string`  
  Base64 encoding of the avatar.
  - Maximum 65535 characters

- `"description"`: (*Body parameter*), `string`  
  A brief description of the dataset to create.
  - Maximum 65535 characters

- `"embedding_model"`: (*Body parameter*), `string`  
  The name of the embedding model to use. For example: `"BAAI/bge-large-zh-v1.5@BAAI"`
  - Maximum 255 characters
  - Must follow `model_name@model_factory` format

- `"permission"`: (*Body parameter*), `string`  
  Specifies who can access the dataset to create. Available options:  
  - `"me"`: (Default) Only you can manage the dataset.
  - `"team"`: All team members can manage the dataset.

- `"chunk_method"`: (*Body parameter*), `enum<string>`  
  The chunking method of the dataset to create. Available options:  
  - `"naive"`: General (default)
  - `"book"`: Book
  - `"email"`: Email
  - `"laws"`: Laws
  - `"manual"`: Manual
  - `"one"`: One
  - `"paper"`: Paper
  - `"picture"`: Picture
  - `"presentation"`: Presentation
  - `"qa"`: Q&A
  - `"table"`: Table
  - `"tag"`: Tag

- `"parser_config"`: (*Body parameter*), `object`  
  The configuration settings for the dataset parser. The attributes in this JSON object vary with the selected `"chunk_method"`:  
  - If `"chunk_method"` is `"naive"`, the `"parser_config"` object contains the following attributes:
    - `"auto_keywords"`: `int`
      - Defaults to `0`
      - Minimum: `0`
      - Maximum: `32`
    - `"auto_questions"`: `int`
      - Defaults to `0`
      - Minimum: `0`
      - Maximum: `10`
    - `"chunk_token_num"`: `int`
      - Defaults to `512`
      - Minimum: `1`
      - Maximum: `2048`
    - `"delimiter"`: `string`
      - Defaults to `"\n"`.
    - `"html4excel"`: `bool` Indicates whether to convert Excel documents into HTML format.
      - Defaults to `false`
    - `"layout_recognize"`: `string`
      - Defaults to `DeepDOC`
    - `"tag_kb_ids"`: `array<string>` refer to [Use tag set](https://ragflow.io/docs/dev/use_tag_sets)
      - Must include a list of dataset IDs, where each dataset is parsed using the ​​Tag Chunking Method
    - `"task_page_size"`: `int` For PDF only.
      - Defaults to `12`
      - Minimum: `1`
    - `"raptor"`: `object` RAPTOR-specific settings.
      - Defaults to: `{"use_raptor": false}`
    - `"graphrag"`: `object` GRAPHRAG-specific settings.
      - Defaults to: `{"use_graphrag": false}`
  - If `"chunk_method"` is `"qa"`, `"manuel"`, `"paper"`, `"book"`, `"laws"`, or `"presentation"`, the `"parser_config"` object contains the following attribute:  
    - `"raptor"`: `object` RAPTOR-specific settings.
      - Defaults to: `{"use_raptor": false}`.
  - If `"chunk_method"` is `"table"`, `"picture"`, `"one"`, or `"email"`, `"parser_config"` is an empty JSON object.

#### Response

Success:

```json
{
    "code": 0,
    "data": {
        "avatar": null,
        "chunk_count": 0,
        "chunk_method": "naive",
        "create_date": "Mon, 28 Apr 2025 18:40:41 GMT",
        "create_time": 1745836841611,
        "created_by": "3af81804241d11f0a6a79f24fc270c7f",
        "description": null,
        "document_count": 0,
        "embedding_model": "BAAI/bge-large-zh-v1.5@BAAI",
        "id": "3b4de7d4241d11f0a6a79f24fc270c7f",
        "language": "English",
        "name": "RAGFlow example",
        "pagerank": 0,
        "parser_config": {
            "chunk_token_num": 128, 
            "delimiter": "\\n!?;。；！？", 
            "html4excel": false, 
            "layout_recognize": "DeepDOC", 
            "raptor": {
                "use_raptor": false
                }
            },
        "permission": "me",
        "similarity_threshold": 0.2,
        "status": "1",
        "tenant_id": "3af81804241d11f0a6a79f24fc270c7f",
        "token_num": 0,
        "update_date": "Mon, 28 Apr 2025 18:40:41 GMT",
        "update_time": 1745836841611,
        "vector_similarity_weight": 0.3,
    },
}
```

Failure:

```json
{
    "code": 101,
    "message": "Dataset name 'RAGFlow example' already exists"
}
```

---

### Delete datasets

**DELETE** `/api/v1/datasets`

Deletes datasets by ID.

#### Request

- Method: DELETE
- URL: `/api/v1/datasets`
- Headers:
  - `'content-Type: application/json'`
  - `'Authorization: Bearer <YOUR_API_KEY>'`
  - Body:
    - `"ids"`: `list[string]` or `null`

##### Request example

```bash
curl --request DELETE \
     --url http://{address}/api/v1/datasets \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer <YOUR_API_KEY>' \
     --data '{
     "ids": ["d94a8dc02c9711f0930f7fbc369eab6d", "e94a8dc02c9711f0930f7fbc369eab6e"]
     }'
```

##### Request parameters

- `"ids"`: (*Body parameter*), `list[string]` or `null`,   *Required*  
  Specifies the datasets to delete:
  - If `null`, all datasets will be deleted.
  - If an array of IDs, only the specified datasets will be deleted.
  - If an empty array, no datasets will be deleted.

#### Response

Success:

```json
{
    "code": 0 
}
```

Failure:

```json
{
    "code": 102,
    "message": "You don't own the dataset."
}
```

---

### Update dataset

**PUT** `/api/v1/datasets/{dataset_id}`

Updates configurations for a specified dataset.

#### Request

- Method: PUT
- URL: `/api/v1/datasets/{dataset_id}`
- Headers:
  - `'content-Type: application/json'`
  - `'Authorization: Bearer <YOUR_API_KEY>'`
- Body:
  - `"name"`: `string`
  - `"avatar"`: `string`
  - `"description"`: `string`
  - `"embedding_model"`: `string`
  - `"permission"`: `string`
  - `"chunk_method"`: `string`
  - `"pagerank"`: `int`
  - `"parser_config"`: `object`

##### Request example

```bash
curl --request PUT \
     --url http://{address}/api/v1/datasets/{dataset_id} \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer <YOUR_API_KEY>' \
     --data '
     {
          "name": "updated_dataset"
     }'
```

##### Request parameters

- `dataset_id`: (*Path parameter*)  
  The ID of the dataset to update.
- `"name"`: (*Body parameter*), `string`  
  The revised name of the dataset.
  - Basic Multilingual Plane (BMP) only
  - Maximum 128 characters
  - Case-insensitive
- `"avatar"`: (*Body parameter*), `string`  
  The updated base64 encoding of the avatar.
  - Maximum 65535 characters
- `"embedding_model"`: (*Body parameter*), `string`  
  The updated embedding model name.  
  - Ensure that `"chunk_count"` is `0` before updating `"embedding_model"`.
  - Maximum 255 characters
  - Must follow `model_name@model_factory` format
- `"permission"`: (*Body parameter*), `string`  
  The updated dataset permission. Available options:  
  - `"me"`: (Default) Only you can manage the dataset.
  - `"team"`: All team members can manage the dataset.
- `"pagerank"`: (*Body parameter*), `int`  
  refer to [Set page rank](https://ragflow.io/docs/dev/set_page_rank)
  - Default: `0`
  - Minimum: `0`
  - Maximum: `100`
- `"chunk_method"`: (*Body parameter*), `enum<string>`  
  The chunking method for the dataset. Available options:  
  - `"naive"`: General (default)
  - `"book"`: Book
  - `"email"`: Email
  - `"laws"`: Laws
  - `"manual"`: Manual
  - `"one"`: One
  - `"paper"`: Paper
  - `"picture"`: Picture
  - `"presentation"`: Presentation
  - `"qa"`: Q&A
  - `"table"`: Table
  - `"tag"`: Tag
- `"parser_config"`: (*Body parameter*), `object`  
  The configuration settings for the dataset parser. The attributes in this JSON object vary with the selected `"chunk_method"`:  
  - If `"chunk_method"` is `"naive"`, the `"parser_config"` object contains the following attributes:
    - `"auto_keywords"`: `int`
      - Defaults to `0`
      - Minimum: `0`
      - Maximum: `32`
    - `"auto_questions"`: `int`
      - Defaults to `0`
      - Minimum: `0`
      - Maximum: `10`
    - `"chunk_token_num"`: `int`
      - Defaults to `512`
      - Minimum: `1`
      - Maximum: `2048`
    - `"delimiter"`: `string`
      - Defaults to `"\n"`.
    - `"html4excel"`: `bool` Indicates whether to convert Excel documents into HTML format.
      - Defaults to `false`
    - `"layout_recognize"`: `string`
      - Defaults to `DeepDOC`
    - `"tag_kb_ids"`: `array<string>` refer to [Use tag set](https://ragflow.io/docs/dev/use_tag_sets)
      - Must include a list of dataset IDs, where each dataset is parsed using the ​​Tag Chunking Method
    - `"task_page_size"`: `int` For PDF only.
      - Defaults to `12`
      - Minimum: `1`
    - `"raptor"`: `object` RAPTOR-specific settings.
      - Defaults to: `{"use_raptor": false}`
    - `"graphrag"`: `object` GRAPHRAG-specific settings.
      - Defaults to: `{"use_graphrag": false}`
  - If `"chunk_method"` is `"qa"`, `"manuel"`, `"paper"`, `"book"`, `"laws"`, or `"presentation"`, the `"parser_config"` object contains the following attribute:  
    - `"raptor"`: `object` RAPTOR-specific settings.
      - Defaults to: `{"use_raptor": false}`.
  - If `"chunk_method"` is `"table"`, `"picture"`, `"one"`, or `"email"`, `"parser_config"` is an empty JSON object.

#### Response

Success:

```json
{
    "code": 0 
}
```

Failure:

```json
{
    "code": 102,
    "message": "Can't change tenant_id."
}
```

---

### List datasets

**GET** `/api/v1/datasets?page={page}&page_size={page_size}&orderby={orderby}&desc={desc}&name={dataset_name}&id={dataset_id}`

Lists datasets.

#### Request

- Method: GET
- URL: `/api/v1/datasets?page={page}&page_size={page_size}&orderby={orderby}&desc={desc}&name={dataset_name}&id={dataset_id}`
- Headers:
  - `'Authorization: Bearer <YOUR_API_KEY>'`

##### Request example

```bash
curl --request GET \
     --url http://{address}/api/v1/datasets?page={page}&page_size={page_size}&orderby={orderby}&desc={desc}&name={dataset_name}&id={dataset_id} \
     --header 'Authorization: Bearer <YOUR_API_KEY>'
```

##### Request parameters

- `page`: (*Filter parameter*)  
  Specifies the page on which the datasets will be displayed. Defaults to `1`.
- `page_size`: (*Filter parameter*)  
  The number of datasets on each page. Defaults to `30`.
- `orderby`: (*Filter parameter*)  
  The field by which datasets should be sorted. Available options:
  - `create_time` (default)
  - `update_time`
- `desc`: (*Filter parameter*)  
  Indicates whether the retrieved datasets should be sorted in descending order. Defaults to `true`.
- `name`: (*Filter parameter*)  
  The name of the dataset to retrieve.
- `id`: (*Filter parameter*)  
  The ID of the dataset to retrieve.

#### Response

Success:

```json
{
    "code": 0,
    "data": [
        {
            "avatar": "",
            "chunk_count": 59,
            "create_date": "Sat, 14 Sep 2024 01:12:37 GMT",
            "create_time": 1726276357324,
            "created_by": "69736c5e723611efb51b0242ac120007",
            "description": null,
            "document_count": 1,
            "embedding_model": "BAAI/bge-large-zh-v1.5",
            "id": "6e211ee0723611efa10a0242ac120007",
            "language": "English",
            "name": "mysql",
            "chunk_method": "naive",
            "parser_config": {
                "chunk_token_num": 8192,
                "delimiter": "\\n",
                "entity_types": [
                    "organization",
                    "person",
                    "location",
                    "event",
                    "time"
                ]
            },
            "permission": "me",
            "similarity_threshold": 0.2,
            "status": "1",
            "tenant_id": "69736c5e723611efb51b0242ac120007",
            "token_num": 12744,
            "update_date": "Thu, 10 Oct 2024 04:07:23 GMT",
            "update_time": 1728533243536,
            "vector_similarity_weight": 0.3
        }
    ],
    "total": 1
}
```

Failure:

```json
{
    "code": 102,
    "message": "The dataset doesn't exist"
}
```

 ---

### Get knowledge graph

**GET** `/api/v1/datasets/{dataset_id}/knowledge_graph`

Retrieves the knowledge graph of a specified dataset.

#### Request

- Method: GET
- URL: `/api/v1/datasets/{dataset_id}/knowledge_graph`
- Headers:
  - `'Authorization: Bearer <YOUR_API_KEY>'`

##### Request example

```bash
curl --request GET \
     --url http://{address}/api/v1/datasets/{dataset_id}/knowledge_graph \
     --header 'Authorization: Bearer <YOUR_API_KEY>'
```

##### Request parameters

- `dataset_id`: (*Path parameter*)  
  The ID of the target dataset.

#### Response

Success:

```json
{
    "code": 0,
    "data": {
        "graph": {
            "directed": false,
            "edges": [
                {
                    "description": "The notice is a document issued to convey risk warnings and operational alerts.<SEP>The notice is a specific instance of a notification document issued under the risk warning framework.",
                    "keywords": ["9", "8"],
                    "source": "notice",
                    "source_id": ["8a46cdfe4b5c11f0a5281a58e595aa1c"],
                    "src_id": "xxx",
                    "target": "xxx",
                    "tgt_id": "xxx",
                    "weight": 17.0
                }
            ],
            "graph": {
                "source_id": ["8a46cdfe4b5c11f0a5281a58e595aa1c", "8a7eb6424b5c11f0a5281a58e595aa1c"]
            },
            "multigraph": false,
            "nodes": [
                {
                    "description": "xxx",
                    "entity_name": "xxx",
                    "entity_type": "ORGANIZATION",
                    "id": "xxx",
                    "pagerank": 0.10804906590624092,
                    "rank": 3,
                    "source_id": ["8a7eb6424b5c11f0a5281a58e595aa1c"]
                }
            ]
        },
        "mind_map": {}
    }
}
```

Failure:

```json
{
    "code": 102,
    "message": "The dataset doesn't exist"
}
```

---

### Delete knowledge graph

**DELETE** `/api/v1/datasets/{dataset_id}/knowledge_graph`

Removes the knowledge graph of a specified dataset.

#### Request

- Method: DELETE
- URL: `/api/v1/datasets/{dataset_id}/knowledge_graph`
- Headers:
  - `'Authorization: Bearer <YOUR_API_KEY>'`

##### Request example

```bash
curl --request DELETE \
     --url http://{address}/api/v1/datasets/{dataset_id}/knowledge_graph \
     --header 'Authorization: Bearer <YOUR_API_KEY>'
```

##### Request parameters

- `dataset_id`: (*Path parameter*)  
  The ID of the target dataset.

#### Response

Success:

```json
{
    "code": 0,
    "data": true
}
```

Failure:

```json
{
    "code": 102,
    "message": "The dataset doesn't exist"
}
```

---

### Construct knowledge graph

**POST** `/api/v1/datasets/{dataset_id}/run_graphrag`

Constructs a knowledge graph from a specified dataset.

#### Request

- Method: POST
- URL: `/api/v1/datasets/{dataset_id}/run_graphrag`
- Headers:
  - `'Authorization: Bearer <YOUR_API_KEY>'`

##### Request example

```bash
curl --request POST \
     --url http://{address}/api/v1/datasets/{dataset_id}/run_graphrag \
     --header 'Authorization: Bearer <YOUR_API_KEY>'
```

##### Request parameters

- `dataset_id`: (*Path parameter*)  
  The ID of the target dataset.

#### Response

Success:

```json
{
    "code":0,
    "data":{
      "graphrag_task_id":"e498de54bfbb11f0ba028f704583b57b"
    }
}
```

Failure:

```json
{
    "code": 102,
    "message": "Invalid Dataset ID"
}
```

---

### Get knowledge graph construction status

**GET** `/api/v1/datasets/{dataset_id}/trace_graphrag`

Retrieves the knowledge graph construction status for a specified dataset.

#### Request

- Method: GET
- URL: `/api/v1/datasets/{dataset_id}/trace_graphrag`
- Headers:
  - `'Authorization: Bearer <YOUR_API_KEY>'`

##### Request example

```bash
curl --request GET \
     --url http://{address}/api/v1/datasets/{dataset_id}/trace_graphrag \
     --header 'Authorization: Bearer <YOUR_API_KEY>'
```

##### Request parameters

- `dataset_id`: (*Path parameter*)  
  The ID of the target dataset.

#### Response

Success:

```json
{
    "code":0,
    "data":{
        "begin_at":"Wed, 12 Nov 2025 19:36:56 GMT",
        "chunk_ids":"",
        "create_date":"Wed, 12 Nov 2025 19:36:56 GMT",
        "create_time":1762947416350,
        "digest":"39e43572e3dcd84f",
        "doc_id":"44661c10bde211f0bc93c164a47ffc40",
        "from_page":100000000,
        "id":"e498de54bfbb11f0ba028f704583b57b",
        "priority":0,
        "process_duration":2.45419,
        "progress":1.0,
        "progress_msg":"19:36:56 created task graphrag\n19:36:57 Task has been received.\n19:36:58 [GraphRAG] doc:083661febe2411f0bc79456921e5745f has no available chunks, skip generation.\n19:36:58 [GraphRAG] build_subgraph doc:44661c10bde211f0bc93c164a47ffc40 start (chunks=1, timeout=10000000000s)\n19:36:58 Graph already contains 44661c10bde211f0bc93c164a47ffc40\n19:36:58 [GraphRAG] build_subgraph doc:44661c10bde211f0bc93c164a47ffc40 empty\n19:36:58 [GraphRAG] kb:33137ed0bde211f0bc93c164a47ffc40 no subgraphs generated successfully, end.\n19:36:58 Knowledge Graph done (0.72s)","retry_count":1,
        "task_type":"graphrag",
        "to_page":100000000,
        "update_date":"Wed, 12 Nov 2025 19:36:58 GMT",
        "update_time":1762947418454
    }
}
```

Failure:

```json
{
    "code": 102,
    "message": "Invalid Dataset ID"
}
```

---

### Construct RAPTOR

**POST** `/api/v1/datasets/{dataset_id}/run_raptor`

Construct a RAPTOR from a specified dataset.

#### Request

- Method: POST
- URL: `/api/v1/datasets/{dataset_id}/run_raptor`
- Headers:
  - `'Authorization: Bearer <YOUR_API_KEY>'`

##### Request example

```bash
curl --request POST \
     --url http://{address}/api/v1/datasets/{dataset_id}/run_raptor \
     --header 'Authorization: Bearer <YOUR_API_KEY>'
```

##### Request parameters

- `dataset_id`: (*Path parameter*)  
  The ID of the target dataset.

#### Response

Success:

```json
{
    "code":0,
    "data":{
        "raptor_task_id":"50d3c31cbfbd11f0ba028f704583b57b"
    }
}
```

Failure:

```json
{
    "code": 102,
    "message": "Invalid Dataset ID"
}
```

---

### Get RAPTOR construction status

**GET** `/api/v1/datasets/{dataset_id}/trace_raptor`

Retrieves the RAPTOR construction status for a specified dataset.

#### Request

- Method: GET
- URL: `/api/v1/datasets/{dataset_id}/trace_raptor`
- Headers:
  - `'Authorization: Bearer <YOUR_API_KEY>'`

##### Request example

```bash
curl --request GET \
     --url http://{address}/api/v1/datasets/{dataset_id}/trace_raptor \
     --header 'Authorization: Bearer <YOUR_API_KEY>'
```

##### Request parameters

- `dataset_id`: (*Path parameter*)  
  The ID of the target dataset.

#### Response

Success:

```json
{
    "code":0,
    "data":{
        "begin_at":"Wed, 12 Nov 2025 19:47:07 GMT",
        "chunk_ids":"",
        "create_date":"Wed, 12 Nov 2025 19:47:07 GMT",
        "create_time":1762948027427,
        "digest":"8b279a6248cb8fc6",
        "doc_id":"44661c10bde211f0bc93c164a47ffc40",
        "from_page":100000000,
        "id":"50d3c31cbfbd11f0ba028f704583b57b",
        "priority":0,
        "process_duration":0.948244,
        "progress":1.0,
        "progress_msg":"19:47:07 created task raptor\n19:47:07 Task has been received.\n19:47:07 Processing...\n19:47:07 Processing...\n19:47:07 Indexing done (0.01s).\n19:47:07 Task done (0.29s)",
        "retry_count":1,
        "task_type":"raptor",
        "to_page":100000000,
        "update_date":"Wed, 12 Nov 2025 19:47:07 GMT",
        "update_time":1762948027948
    }
}
```

Failure:

```json
{
    "code": 102,
    "message": "Invalid Dataset ID"
}
```

---

## FILE MANAGEMENT WITHIN DATASET

---

### Upload documents

**POST** `/api/v1/datasets/{dataset_id}/documents`

Uploads documents to a specified dataset.

#### Request

- Method: POST
- URL: `/api/v1/datasets/{dataset_id}/documents`
- Headers:
  - `'Content-Type: multipart/form-data'`
  - `'Authorization: Bearer <YOUR_API_KEY>'`
- Form:
  - `'file=@{FILE_PATH}'`

##### Request example

```bash
curl --request POST \
     --url http://{address}/api/v1/datasets/{dataset_id}/documents \
     --header 'Content-Type: multipart/form-data' \
     --header 'Authorization: Bearer <YOUR_API_KEY>' \
     --form 'file=@./test1.txt' \
     --form 'file=@./test2.pdf'
```

##### Request parameters

- `dataset_id`: (*Path parameter*)  
  The ID of the dataset to which the documents will be uploaded.
- `'file'`: (*Body parameter*)  
  A document to upload.

#### Response

Success:

```json
{
    "code": 0,
    "data": [
        {
            "chunk_method": "naive",
            "created_by": "69736c5e723611efb51b0242ac120007",
            "dataset_id": "527fa74891e811ef9c650242ac120006",
            "id": "b330ec2e91ec11efbc510242ac120004",
            "location": "1.txt",
            "name": "1.txt",
            "parser_config": {
                "chunk_token_num": 128,
                "delimiter": "\\n",
                "html4excel": false,
                "layout_recognize": true,
                "raptor": {
                    "use_raptor": false
                }
            },
            "run": "UNSTART",
            "size": 17966,
            "thumbnail": "",
            "type": "doc"
        }
    ]
}
```

Failure:

```json
{
    "code": 101,
    "message": "No file part!"
}
```

---

### Update document

**PUT** `/api/v1/datasets/{dataset_id}/documents/{document_id}`

Updates configurations for a specified document.

#### Request

- Method: PUT
- URL: `/api/v1/datasets/{dataset_id}/documents/{document_id}`
- Headers:
  - `'content-Type: application/json'`
  - `'Authorization: Bearer <YOUR_API_KEY>'`
- Body:
  - `"name"`:`string`
  - `"meta_fields"`:`object`
  - `"chunk_method"`:`string`
  - `"parser_config"`:`object`

##### Request example

```bash
curl --request PUT \
     --url http://{address}/api/v1/datasets/{dataset_id}/documents/{document_id} \
     --header 'Authorization: Bearer <YOUR_API_KEY>' \
     --header 'Content-Type: application/json' \
     --data '
     {
          "name": "manual.txt", 
          "chunk_method": "manual", 
          "parser_config": {"chunk_token_num": 128}
     }'

```

##### Request parameters

- `dataset_id`: (*Path parameter*)  
  The ID of the associated dataset.
- `document_id`: (*Path parameter*)  
  The ID of the document to update.
- `"name"`: (*Body parameter*), `string`
- `"meta_fields"`: (*Body parameter*), `dict[str, Any]` The meta fields of the document.
- `"chunk_method"`: (*Body parameter*), `string`  
  The parsing method to apply to the document:  
  - `"naive"`: General
  - `"manual`: Manual
  - `"qa"`: Q&A
  - `"table"`: Table
  - `"paper"`: Paper
  - `"book"`: Book
  - `"laws"`: Laws
  - `"presentation"`: Presentation
  - `"picture"`: Picture
  - `"one"`: One
  - `"email"`: Email
- `"parser_config"`: (*Body parameter*), `object`  
  The configuration settings for the dataset parser. The attributes in this JSON object vary with the selected `"chunk_method"`:  
  - If `"chunk_method"` is `"naive"`, the `"parser_config"` object contains the following attributes:
    - `"chunk_token_num"`: Defaults to `256`.
    - `"layout_recognize"`: Defaults to `true`.
    - `"html4excel"`: Indicates whether to convert Excel documents into HTML format. Defaults to `false`.
    - `"delimiter"`: Defaults to `"\n"`.
    - `"task_page_size"`: Defaults to `12`. For PDF only.
    - `"raptor"`: RAPTOR-specific settings. Defaults to: `{"use_raptor": false}`.
  - If `"chunk_method"` is `"qa"`, `"manuel"`, `"paper"`, `"book"`, `"laws"`, or `"presentation"`, the `"parser_config"` object contains the following attribute:
    - `"raptor"`: RAPTOR-specific settings. Defaults to: `{"use_raptor": false}`.
  - If `"chunk_method"` is `"table"`, `"picture"`, `"one"`, or `"email"`, `"parser_config"` is an empty JSON object.
- `"enabled"`: (*Body parameter*), `integer`  
  Whether the document should be **available** in the knowledge base.  
  - `1` → （available）  
  - `0` → （unavailable）  

#### Response

Success:

```json
{
    "code": 0
}
```

Failure:

```json
{
    "code": 102,
    "message": "The dataset does not have the document."
}
```

---

### Download document

**GET** `/api/v1/datasets/{dataset_id}/documents/{document_id}`

Downloads a document from a specified dataset.

#### Request

- Method: GET
- URL: `/api/v1/datasets/{dataset_id}/documents/{document_id}`
- Headers:
  - `'Authorization: Bearer <YOUR_API_KEY>'`
- Output:
  - `'{PATH_TO_THE_FILE}'`

##### Request example

```bash
curl --request GET \
     --url http://{address}/api/v1/datasets/{dataset_id}/documents/{document_id} \
     --header 'Authorization: Bearer <YOUR_API_KEY>' \
     --output ./ragflow.txt
```

##### Request parameters

- `dataset_id`: (*Path parameter*)  
  The associated dataset ID.
- `documents_id`: (*Path parameter*)  
  The ID of the document to download.

#### Response

Success:

```json
This is a test to verify the file download feature.
```

Failure:

```json
{
    "code": 102,
    "message": "You do not own the dataset 7898da028a0511efbf750242ac1220005."
}
```

---

### List documents

**GET** `/api/v1/datasets/{dataset_id}/documents?page={page}&page_size={page_size}&orderby={orderby}&desc={desc}&keywords={keywords}&id={document_id}&name={document_name}&create_time_from={timestamp}&create_time_to={timestamp}&suffix={file_suffix}&run={run_status}`

Lists documents in a specified dataset.

#### Request

- Method: GET
- URL: `/api/v1/datasets/{dataset_id}/documents?page={page}&page_size={page_size}&orderby={orderby}&desc={desc}&keywords={keywords}&id={document_id}&name={document_name}&create_time_from={timestamp}&create_time_to={timestamp}&suffix={file_suffix}&run={run_status}`
- Headers:
  - `'content-Type: application/json'`
  - `'Authorization: Bearer <YOUR_API_KEY>'`

##### Request examples

**A basic request with pagination:**
```bash
curl --request GET \
     --url http://{address}/api/v1/datasets/{dataset_id}/documents?page=1&page_size=10 \
     --header 'Authorization: Bearer <YOUR_API_KEY>'
```

##### Request parameters

- `dataset_id`: (*Path parameter*)  
  The associated dataset ID.
- `keywords`: (*Filter parameter*), `string`  
  The keywords used to match document titles.
- `page`: (*Filter parameter*), `integer`
  Specifies the page on which the documents will be displayed. Defaults to `1`.
- `page_size`: (*Filter parameter*), `integer`  
  The maximum number of documents on each page. Defaults to `30`.
- `orderby`: (*Filter parameter*), `string`  
  The field by which documents should be sorted. Available options:
  - `create_time` (default)
  - `update_time`
- `desc`: (*Filter parameter*), `boolean`  
  Indicates whether the retrieved documents should be sorted in descending order. Defaults to `true`.
- `id`: (*Filter parameter*), `string`  
  The ID of the document to retrieve.
- `create_time_from`: (*Filter parameter*), `integer`  
  Unix timestamp for filtering documents created after this time. 0 means no filter. Defaults to `0`.
- `create_time_to`: (*Filter parameter*), `integer`  
  Unix timestamp for filtering documents created before this time. 0 means no filter. Defaults to `0`.
- `suffix`: (*Filter parameter*), `array[string]`  
  Filter by file suffix. Supports multiple values, e.g., `pdf`, `txt`, and `docx`. Defaults to all suffixes.
- `run`: (*Filter parameter*), `array[string]`  
  Filter by document processing status. Supports numeric, text, and mixed formats:  
  - Numeric format: `["0", "1", "2", "3", "4"]`
  - Text format: `[UNSTART, RUNNING, CANCEL, DONE, FAIL]`
  - Mixed format: `[UNSTART, 1, DONE]` (mixing numeric and text formats)
  - Status mapping:
    - `0` / `UNSTART`: Document not yet processed
    - `1` / `RUNNING`: Document is currently being processed
    - `2` / `CANCEL`: Document processing was cancelled
    - `3` / `DONE`: Document processing completed successfully
    - `4` / `FAIL`: Document processing failed  
  Defaults to all statuses.

##### Usage examples

**A request with multiple filtering parameters**

```bash
curl --request GET \
     --url 'http://{address}/api/v1/datasets/{dataset_id}/documents?suffix=pdf&run=DONE&page=1&page_size=10' \
     --header 'Authorization: Bearer <YOUR_API_KEY>'
```

#### Response

Success:

```json
{
    "code": 0,
    "data": {
        "docs": [
            {
                "chunk_count": 0,
                "create_date": "Mon, 14 Oct 2024 09:11:01 GMT",
                "create_time": 1728897061948,
                "created_by": "69736c5e723611efb51b0242ac120007",
                "id": "3bcfbf8a8a0c11ef8aba0242ac120006",
                "knowledgebase_id": "7898da028a0511efbf750242ac120005",
                "location": "Test_2.txt",
                "name": "Test_2.txt",
                "parser_config": {
                    "chunk_token_count": 128,
                    "delimiter": "\n",
                    "layout_recognize": true,
                    "task_page_size": 12
                },
                "chunk_method": "naive",
                "process_begin_at": null,
                "process_duration": 0.0,
                "progress": 0.0,
                "progress_msg": "",
                "run": "UNSTART",
                "size": 7,
                "source_type": "local",
                "status": "1",
                "thumbnail": null,
                "token_count": 0,
                "type": "doc",
                "update_date": "Mon, 14 Oct 2024 09:11:01 GMT",
                "update_time": 1728897061948
            }
        ],
        "total_datasets": 1
    }
}
```

Failure:

```json
{
    "code": 102,
    "message": "You don't own the dataset 7898da028a0511efbf750242ac1220005. "
}
```

---

### Delete documents

**DELETE** `/api/v1/datasets/{dataset_id}/documents`

Deletes documents by ID.

#### Request

- Method: DELETE
- URL: `/api/v1/datasets/{dataset_id}/documents`
- Headers:
  - `'Content-Type: application/json'`
  - `'Authorization: Bearer <YOUR_API_KEY>'`
- Body:
  - `"ids"`: `list[string]`

##### Request example

```bash
curl --request DELETE \
     --url http://{address}/api/v1/datasets/{dataset_id}/documents \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer <YOUR_API_KEY>' \
     --data '
     {
          "ids": ["id_1","id_2"]
     }'
```

##### Request parameters

- `dataset_id`: (*Path parameter*)  
  The associated dataset ID.
- `"ids"`: (*Body parameter*), `list[string]`  
  The IDs of the documents to delete. If it is not specified, all documents in the specified dataset will be deleted.

#### Response

Success:

```json
{
    "code": 0
}.
```

Failure:

```json
{
    "code": 102,
    "message": "You do not own the dataset 7898da028a0511efbf750242ac1220005."
}
```

---

### Parse documents

**POST** `/api/v1/datasets/{dataset_id}/chunks`

Parses documents in a specified dataset.

#### Request

- Method: POST
- URL: `/api/v1/datasets/{dataset_id}/chunks`
- Headers:
  - `'content-Type: application/json'`
  - `'Authorization: Bearer <YOUR_API_KEY>'`
- Body:
  - `"document_ids"`: `list[string]`

##### Request example

```bash
curl --request POST \
     --url http://{address}/api/v1/datasets/{dataset_id}/chunks \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer <YOUR_API_KEY>' \
     --data '
     {
          "document_ids": ["97a5f1c2759811efaa500242ac120004","97ad64b6759811ef9fc30242ac120004"]
     }'
```

##### Request parameters

- `dataset_id`: (*Path parameter*)  
  The dataset ID.
- `"document_ids"`: (*Body parameter*), `list[string]`, *Required*  
  The IDs of the documents to parse.

#### Response

Success:

```json
{
    "code": 0
}
```

Failure:

```json
{
    "code": 102,
    "message": "`document_ids` is required"
}
```

---

### Stop parsing documents

**DELETE** `/api/v1/datasets/{dataset_id}/chunks`

Stops parsing specified documents.

#### Request

- Method: DELETE
- URL: `/api/v1/datasets/{dataset_id}/chunks`
- Headers:
  - `'content-Type: application/json'`
  - `'Authorization: Bearer <YOUR_API_KEY>'`
- Body:
  - `"document_ids"`: `list[string]`

##### Request example

```bash
curl --request DELETE \
     --url http://{address}/api/v1/datasets/{dataset_id}/chunks \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer <YOUR_API_KEY>' \
     --data '
     {
          "document_ids": ["97a5f1c2759811efaa500242ac120004","97ad64b6759811ef9fc30242ac120004"]
     }'
```

##### Request parameters

- `dataset_id`: (*Path parameter*)  
  The associated dataset ID.
- `"document_ids"`: (*Body parameter*), `list[string]`, *Required*  
  The IDs of the documents for which the parsing should be stopped.

#### Response

Success:

```json
{
    "code": 0
}
```

Failure:

```json
{
    "code": 102,
    "message": "`document_ids` is required"
}
```

---

## CHUNK MANAGEMENT WITHIN DATASET

---

### Add chunk

**POST** `/api/v1/datasets/{dataset_id}/documents/{document_id}/chunks`

Adds a chunk to a specified document in a specified dataset.

#### Request

- Method: POST
- URL: `/api/v1/datasets/{dataset_id}/documents/{document_id}/chunks`
- Headers:
  - `'content-Type: application/json'`
  - `'Authorization: Bearer <YOUR_API_KEY>'`
- Body:
  - `"content"`: `string`
  - `"important_keywords"`: `list[string]`

##### Request example

```bash
curl --request POST \
     --url http://{address}/api/v1/datasets/{dataset_id}/documents/{document_id}/chunks \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer <YOUR_API_KEY>' \
     --data '
     {
          "content": "<CHUNK_CONTENT_HERE>"
     }'
```

##### Request parameters

- `dataset_id`: (*Path parameter*)  
  The associated dataset ID.
- `document_ids`: (*Path parameter*)  
  The associated document ID.
- `"content"`: (*Body parameter*), `string`, *Required*  
  The text content of the chunk.
- `"important_keywords`(*Body parameter*), `list[string]`  
  The key terms or phrases to tag with the chunk.
- `"questions"`(*Body parameter*), `list[string]`
  If there is a given question, the embedded chunks will be based on them

#### Response

Success:

```json
{
    "code": 0,
    "data": {
        "chunk": {
            "content": "who are you",
            "create_time": "2024-12-30 16:59:55",
            "create_timestamp": 1735549195.969164,
            "dataset_id": "72f36e1ebdf411efb7250242ac120006",
            "document_id": "61d68474be0111ef98dd0242ac120006",
            "id": "12ccdc56e59837e5",
            "important_keywords": [],
            "questions": []
        }
    }
}
```

Failure:

```json
{
    "code": 102,
    "message": "`content` is required"
}
```

---

### List chunks

**GET** `/api/v1/datasets/{dataset_id}/documents/{document_id}/chunks?keywords={keywords}&page={page}&page_size={page_size}&id={id}`

Lists chunks in a specified document.

#### Request

- Method: GET
- URL: `/api/v1/datasets/{dataset_id}/documents/{document_id}/chunks?keywords={keywords}&page={page}&page_size={page_size}&id={chunk_id}`
- Headers:
  - `'Authorization: Bearer <YOUR_API_KEY>'`

##### Request example

```bash
curl --request GET \
     --url http://{address}/api/v1/datasets/{dataset_id}/documents/{document_id}/chunks?keywords={keywords}&page={page}&page_size={page_size}&id={chunk_id} \
     --header 'Authorization: Be

[... Content truncated for brevity ...]
```

## High-Level Overview

# HTTP API

## Detailed Walkthrough

This is a documentation file. See the 'Original Source' section for full content.

## Code Structure Analysis

- Total lines: 4474
- Blank lines: 747 (16.7%)
- Comment lines: ~274 (6.1%)
- Code lines: ~3453


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `docs` directory, specifically within `docs/references`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 48 loop(s) - consider algorithmic complexity
- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `docs/references/` directory
- Potential test file: `test_http_api_reference.md`

## Keywords

AGENT, API, ASSISTANT, Access, Add, Adds, Agent, All, Also, Android, Answer, Answer_0, Answers, Any, Apr, Asks, Assistant, Authorization, Available, BAAI, BMP, Bad, Base64, Basic, Bearer, Before, Begin, Body, Book, CANCEL, CHAT, CHUNK, CHUNK_CONTENT_HERE, CMAKE_BUILD_TYPE, CODES, Can, Canvas, Case, Chat, Check, Chunk, Chunking, Code, Compatible, Configurations, Construct, Constructs, Content, Controls, Converse...

---
*Generated by RAGFlow Repository Documentation Generator*
