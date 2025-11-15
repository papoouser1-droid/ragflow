# Documentation: conf/llm_factories.json

## File Metadata

- **Path**: `conf/llm_factories.json`
- **Size**: 174898 bytes
- **Type**: .json
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `conf/llm_factories.json`.

## Original Source Code

```json
{
    "factory_llm_infos": [
        {
            "name": "OpenAI",
            "logo": "",
            "tags": "LLM,TEXT EMBEDDING,TTS,TEXT RE-RANK,SPEECH2TEXT,MODERATION",
            "status": "1",
            "rank": "999",
            "llm": [
                {
                    "llm_name": "gpt-5",
                    "tags": "LLM,CHAT,400k,IMAGE2TEXT",
                    "max_tokens": 400000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "gpt-5-mini",
                    "tags": "LLM,CHAT,400k,IMAGE2TEXT",
                    "max_tokens": 400000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "gpt-5-nano",
                    "tags": "LLM,CHAT,400k,IMAGE2TEXT",
                    "max_tokens": 400000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "gpt-5-chat-latest",
                    "tags": "LLM,CHAT,400k,IMAGE2TEXT",
                    "max_tokens": 400000,
                    "model_type": "chat",
                    "is_tools": false
                },
                {
                    "llm_name": "gpt-4.1",
                    "tags": "LLM,CHAT,1M,IMAGE2TEXT",
                    "max_tokens": 1047576,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "gpt-4.1-mini",
                    "tags": "LLM,CHAT,1M,IMAGE2TEXT",
                    "max_tokens": 1047576,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "gpt-4.1-nano",
                    "tags": "LLM,CHAT,1M,IMAGE2TEXT",
                    "max_tokens": 1047576,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "gpt-4.5-preview",
                    "tags": "LLM,CHAT,128K",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "o3",
                    "tags": "LLM,CHAT,200K,IMAGE2TEXT",
                    "max_tokens": 200000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "o4-mini",
                    "tags": "LLM,CHAT,200K,IMAGE2TEXT",
                    "max_tokens": 200000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "o4-mini-high",
                    "tags": "LLM,CHAT,200K,IMAGE2TEXT",
                    "max_tokens": 200000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "gpt-4o-mini",
                    "tags": "LLM,CHAT,128K,IMAGE2TEXT",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "gpt-4o",
                    "tags": "LLM,CHAT,128K,IMAGE2TEXT",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "gpt-3.5-turbo",
                    "tags": "LLM,CHAT,4K",
                    "max_tokens": 4096,
                    "model_type": "chat",
                    "is_tools": false
                },
                {
                    "llm_name": "gpt-3.5-turbo-16k-0613",
                    "tags": "LLM,CHAT,16k",
                    "max_tokens": 16385,
                    "model_type": "chat",
                    "is_tools": false
                },
                {
                    "llm_name": "text-embedding-ada-002",
                    "tags": "TEXT EMBEDDING,8K",
                    "max_tokens": 8191,
                    "model_type": "embedding",
                    "is_tools": false
                },
                {
                    "llm_name": "text-embedding-3-small",
                    "tags": "TEXT EMBEDDING,8K",
                    "max_tokens": 8191,
                    "model_type": "embedding",
                    "is_tools": false
                },
                {
                    "llm_name": "text-embedding-3-large",
                    "tags": "TEXT EMBEDDING,8K",
                    "max_tokens": 8191,
                    "model_type": "embedding",
                    "is_tools": false
                },
                {
                    "llm_name": "whisper-1",
                    "tags": "SPEECH2TEXT",
                    "max_tokens": 26214400,
                    "model_type": "speech2text",
                    "is_tools": false
                },
                {
                    "llm_name": "gpt-4",
                    "tags": "LLM,CHAT,8K",
                    "max_tokens": 8191,
                    "model_type": "chat",
                    "is_tools": false
                },
                {
                    "llm_name": "gpt-4-turbo",
                    "tags": "LLM,CHAT,8K",
                    "max_tokens": 8191,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "gpt-4-32k",
                    "tags": "LLM,CHAT,32K",
                    "max_tokens": 32768,
                    "model_type": "chat",
                    "is_tools": false
                },
                {
                    "llm_name": "tts-1",
                    "tags": "TTS",
                    "max_tokens": 2048,
                    "model_type": "tts",
                    "is_tools": false
                }
            ]
        },
        {
            "name": "xAI",
            "logo": "",
            "tags": "LLM",
            "status": "1",
            "rank": "930",
            "llm": [
                {
                    "llm_name": "grok-4",
                    "tags": "LLM,CHAT,256k",
                    "max_tokens": 256000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "grok-3",
                    "tags": "LLM,CHAT,130k",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "grok-3-fast",
                    "tags": "LLM,CHAT,130k",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "grok-3-mini",
                    "tags": "LLM,CHAT,130k",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "grok-3-mini-mini-fast",
                    "tags": "LLM,CHAT,130k",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "grok-2-vision",
                    "tags": "LLM,CHAT,IMAGE2TEXT,32k",
                    "max_tokens": 32768,
                    "model_type": "image2text",
                    "is_tools": true
                }
            ]
        },
        {
            "name": "TokenPony",
            "logo": "",
            "tags": "LLM",
            "status": "1",
            "llm": [
                {
                    "llm_name": "qwen3-8b",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "deepseek-v3-0324",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-32b",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "kimi-k2-instruct-0905",
                    "tags": "LLM,CHAT,256K",
                    "max_tokens": 256000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "deepseek-r1-0528",
                    "tags": "LLM,CHAT,164k",
                    "max_tokens": 164000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-coder-480b",
                    "tags": "LLM,CHAT,1024k",
                    "max_tokens": 1024000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "glm-4.5",
                    "tags": "LLM,CHAT,131K",
                    "max_tokens": 131000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "deepseek-v3.1",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
               

... [Content truncated - file is 174898 bytes] ...

                   "tags": "LLM,CHAT,8000",
                    "max_tokens": 8000,
                    "model_type": "chat",
                    "is_tools": true
                }
            ]
        },
        {
            "name": "DeerAPI",
            "logo": "",
            "tags": "LLM,TEXT EMBEDDING,IMAGE2TEXT",
            "status": "1",
            "llm": [
                {
                    "llm_name": "gpt-5-chat-latest",
                    "tags": "LLM,CHAT,400k",
                    "max_tokens": 400000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "chatgpt-4o-latest",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "gpt-5-mini",
                    "tags": "LLM,CHAT,400k",
                    "max_tokens": 400000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "gpt-5-nano",
                    "tags": "LLM,CHAT,400k",
                    "max_tokens": 400000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "gpt-5",
                    "tags": "LLM,CHAT,400k",
                    "max_tokens": 400000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "gpt-4.1-mini",
                    "tags": "LLM,CHAT,1M",
                    "max_tokens": 1047576,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "gpt-4.1-nano",
                    "tags": "LLM,CHAT,1M",
                    "max_tokens": 1047576,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "gpt-4.1",
                    "tags": "LLM,CHAT,1M",
                    "max_tokens": 1047576,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "gpt-4o-mini",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "o4-mini-2025-04-16",
                    "tags": "LLM,CHAT,200k",
                    "max_tokens": 200000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "o3-pro-2025-06-10",
                    "tags": "LLM,CHAT,200k",
                    "max_tokens": 200000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "claude-opus-4-1-20250805",
                    "tags": "LLM,CHAT,200k,IMAGE2TEXT",
                    "max_tokens": 200000,
                    "model_type": "image2text",
                    "is_tools": true
                },
                {
                    "llm_name": "claude-opus-4-1-20250805-thinking",
                    "tags": "LLM,CHAT,200k,IMAGE2TEXT",
                    "max_tokens": 200000,
                    "model_type": "image2text",
                    "is_tools": true
                },
                {
                    "llm_name": "claude-sonnet-4-20250514",
                    "tags": "LLM,CHAT,200k,IMAGE2TEXT",
                    "max_tokens": 200000,
                    "model_type": "image2text",
                    "is_tools": true
                },
                {
                    "llm_name": "claude-sonnet-4-20250514-thinking",
                    "tags": "LLM,CHAT,200k,IMAGE2TEXT",
                    "max_tokens": 200000,
                    "model_type": "image2text",
                    "is_tools": true
                },
                {
                    "llm_name": "claude-3-7-sonnet-latest",
                    "tags": "LLM,CHAT,200k",
                    "max_tokens": 200000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "claude-3-5-haiku-latest",
                    "tags": "LLM,CHAT,200k",
                    "max_tokens": 200000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "gemini-2.5-pro",
                    "tags": "LLM,CHAT,1M,IMAGE2TEXT",
                    "max_tokens": 1000000,
                    "model_type": "image2text",
                    "is_tools": true
                },
                {
                    "llm_name": "gemini-2.5-flash",
                    "tags": "LLM,CHAT,1M,IMAGE2TEXT",
                    "max_tokens": 1000000,
                    "model_type": "image2text",
                    "is_tools": true
                },
                {
                    "llm_name": "gemini-2.5-flash-lite",
                    "tags": "LLM,CHAT,1M,IMAGE2TEXT",
                    "max_tokens": 1000000,
                    "model_type": "image2text",
                    "is_tools": true
                },
                {
                    "llm_name": "gemini-2.0-flash",
                    "tags": "LLM,CHAT,1M,IMAGE2TEXT",
                    "max_tokens": 1000000,
                    "model_type": "image2text",
                    "is_tools": true
                },
                {
                    "llm_name": "grok-4-0709",
                    "tags": "LLM,CHAT,131k",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "grok-3",
                    "tags": "LLM,CHAT,131k",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "grok-3-mini",
                    "tags": "LLM,CHAT,131k",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "grok-2-image-1212",
                    "tags": "LLM,CHAT,32k,IMAGE2TEXT",
                    "max_tokens": 32768,
                    "model_type": "image2text",
                    "is_tools": true
                },
                {
                    "llm_name": "deepseek-v3.1",
                    "tags": "LLM,CHAT,64k",
                    "max_tokens": 64000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "deepseek-v3",
                    "tags": "LLM,CHAT,64k",
                    "max_tokens": 64000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "deepseek-r1-0528",
                    "tags": "LLM,CHAT,164k",
                    "max_tokens": 164000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "deepseek-chat",
                    "tags": "LLM,CHAT,32k",
                    "max_tokens": 32000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "deepseek-reasoner",
                    "tags": "LLM,CHAT,64k",
                    "max_tokens": 64000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-30b-a3b",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-coder-plus-2025-07-22",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "text-embedding-ada-002",
                    "tags": "TEXT EMBEDDING,8K",
                    "max_tokens": 8191,
                    "model_type": "embedding",
                    "is_tools": false
                },
                {
                    "llm_name": "text-embedding-3-small",
                    "tags": "TEXT EMBEDDING,8K",
                    "max_tokens": 8191,
                    "model_type": "embedding",
                    "is_tools": false
                },
                {
                    "llm_name": "text-embedding-3-large",
                    "tags": "TEXT EMBEDDING,8K",
                    "max_tokens": 8191,
                    "model_type": "embedding",
                    "is_tools": false
                },
                {
                    "llm_name": "whisper-1",
                    "tags": "SPEECH2TEXT",
                    "max_tokens": 26214400,
                    "model_type": "speech2text",
                    "is_tools": false
                },
                {
                    "llm_name": "tts-1",
                    "tags": "TTS",
                    "max_tokens": 2048,
                    "model_type": "tts",
                    "is_tools": false
                }
            ]
        }
    ]
}
```

## Detailed Analysis

### File Role in Repository

The file `conf/llm_factories.json` is located in the `conf` directory.

### Architecture Context

Files in this location typically handle concerns related to conf.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [infinity_mapping.json](infinity_mapping.json_docs.md)
- [mapping.json](mapping.json_docs.md)
- [os_mapping.json](os_mapping.json_docs.md)
- [private.pem](private.pem_docs.md)
- [public.pem](public.pem_docs.md)
- [service_conf.yaml](service_conf.yaml_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
