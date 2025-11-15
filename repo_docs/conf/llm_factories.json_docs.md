# File Documentation: conf/llm_factories.json

## File Metadata

- **Path**: `conf/llm_factories.json`
- **Extension**: `.json`
- **Lines**: 4,844
- **Characters**: 174,898
- **Size**: 174,898 bytes
- **Purpose**: Data/Configuration - JSON data or configuration file

## Original Source

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
                    "is_tools": true
                },
                {
                    "llm_name": "hunyuan-a13b-instruct",
                    "tags": "LLM,CHAT,256k",
                    "max_tokens": 256000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-next-80b-a3b-instruct",
                    "tags": "LLM,CHAT,1024k",
                    "max_tokens": 1024000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "deepseek-v3.2-exp",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "deepseek-v3.1-terminus",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-vl-235b-a22b-instruct",
                    "tags": "LLM,CHAT,262k",
                    "max_tokens": 262000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-vl-30b-a3b-instruct",
                    "tags": "LLM,CHAT,262k",
                    "max_tokens": 262000,
                    "model_type": "chat",
                    "is_tools": true
                }
            ]
        },
        {
            "name": "Tongyi-Qianwen",
            "logo": "",
            "tags": "LLM,TEXT EMBEDDING,TEXT RE-RANK,TTS,SPEECH2TEXT,MODERATION",
            "status": "1",
            "rank": "950",
            "llm": [
                {
                    "llm_name": "Moonshot-Kimi-K2-Instruct",
                    "tags": "LLM,CHAT,128K",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "deepseek-r1",
                    "tags": "LLM,CHAT,64K",
                    "max_tokens": 65792,
                    "model_type": "chat",
                    "is_tools": false
                },
                {
                    "llm_name": "deepseek-v3",
                    "tags": "LLM,CHAT,64K",
                    "max_tokens": 65792,
                    "model_type": "chat",
                    "is_tools": false
                },
                {
                    "llm_name": "deepseek-r1-distill-qwen-1.5b",
                    "tags": "LLM,CHAT,32K",
                    "max_tokens": 32768,
                    "model_type": "chat",
                    "is_tools": false
                },
                {
                    "llm_name": "deepseek-r1-distill-qwen-7b",
                    "tags": "LLM,CHAT,32K",
                    "max_tokens": 32768,
                    "model_type": "chat",
                    "is_tools": false
                },
                {
                    "llm_name": "deepseek-r1-distill-qwen-14b",
                    "tags": "LLM,CHAT,32K",
                    "max_tokens": 32768,
                    "model_type": "chat",
                    "is_tools": false
                },
                {
                    "llm_name": "deepseek-r1-distill-qwen-32b",
                    "tags": "LLM,CHAT,32K",
                    "max_tokens": 32768,
                    "model_type": "chat",
                    "is_tools": false
                },
                {
                    "llm_name": "deepseek-r1-distill-llama-8b",
                    "tags": "LLM,CHAT,32K",
                    "max_tokens": 32768,
                    "model_type": "chat",
                    "is_tools": false
                },
                {
                    "llm_name": "deepseek-r1-distill-llama-70b",
                    "tags": "LLM,CHAT,32K",
                    "max_tokens": 32768,
                    "model_type": "chat",
                    "is_tools": false
                },
                {
                    "llm_name": "qwq-32b",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwq-plus",
                    "tags": "LLM,CHAT,132k",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen-plus-2025-07-28",
                    "tags": "LLM,CHAT,132k",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen-plus-2025-07-14",
                    "tags": "LLM,CHAT,132k",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwq-plus-latest",
                    "tags": "LLM,CHAT,132k",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen-flash",
                    "tags": "LLM,CHAT,1M",
                    "max_tokens": 1000000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen-flash-2025-07-28",
                    "tags": "LLM,CHAT,1M",
                    "max_tokens": 1000000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-max",
                    "tags": "LLM,CHAT,256k",
                    "max_tokens": 256000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-coder-480b-a35b-instruct",
                    "tags": "LLM,CHAT,256k",
                    "max_tokens": 256000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-30b-a3b-instruct-2507",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-30b-a3b-thinking-2507",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
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
                    "llm_name": "qwen3-vl-plus",
                    "tags": "LLM,CHAT,IMAGE2TEXT,256k",
                    "max_tokens": 256000,
                    "model_type": "image2text",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-vl-235b-a22b-instruct",
                    "tags": "LLM,CHAT,IMAGE2TEXT,128k",
                    "max_tokens": 128000,
                    "model_type": "image2text",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-vl-235b-a22b-thinking",
                    "tags": "LLM,CHAT,IMAGE2TEXT,128k",
                    "max_tokens": 128000,
                    "model_type": "image2text",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-235b-a22b-instruct-2507",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-235b-a22b-thinking-2507",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-235b-a22b",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-next-80b-a3b-instruct",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-next-80b-a3b-thinking",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-0.6b",
                    "tags": "LLM,CHAT,32k",
                    "max_tokens": 32000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-1.7b",
                    "tags": "LLM,CHAT,32k",
                    "max_tokens": 32000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-4b",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-8b",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen3-14b",
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
                    "llm_name": "qwen-long",
                    "tags": "LLM,CHAT,10000K",
                    "max_tokens": 1000000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen-turbo",
                    "tags": "LLM,CHAT,1M",
                    "max_tokens": 1000000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen-turbo-2025-04-28",
                    "tags": "LLM,CHAT,1M",
                    "max_tokens": 1000000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen-turbo-latest",
                    "tags": "LLM,CHAT,1M",
                    "max_tokens": 1000000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen-max",
                    "tags": "LLM,CHAT,32K",
                    "max_tokens": 32768,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen-plus",
                    "tags": "LLM,CHAT,132k",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen-plus-2025-04-28",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "qwen-plus-latest",
                    "tags": "LLM,CHAT,132k",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "text-embedding-v2",
                    "tags": "TEXT EMBEDDING,2K",
                    "max_tokens": 2048,
                    "model_type": "embedding"
                },
                {
                    "llm_name": "sambert-zhide-v1",
                    "tags": "TTS",
                    "max_tokens": 2048,
                    "model_type": "tts"
                },
                {
                    "llm_name": "sambert-zhiru-v1",
                    "tags": "TTS",
                    "max_tokens": 2048,
                    "model_type": "tts"
                },
                {
                    "llm_name": "text-embedding-v3",
                    "tags": "TEXT EMBEDDING,8K",
                    "max_tokens": 8192,
                    "model_type": "embedding"
                },
                {
                    "llm_name": "text-embedding-v4",
                    "tags": "TEXT EMBEDDING,8K",
                    "max_tokens": 8192,
                    "model_type": "embedding"
                },
                {
                    "llm_name": "qwen-vl-max",
                    "tags": "LLM,CHAT,IMAGE2TEXT",
                    "max_tokens": 765,
                    "model_type": "image2text",
                    "is_tools": false
                },
                {
                    "llm_name": "qwen-vl-plus",
                    "tags": "LLM,CHAT,IMAGE2TEXT",
                    "max_tokens": 765,
                    "model_type": "image2text",
                    "is_tools": false
                },
                {
                    "llm_name": "gte-rerank",
                    "tags": "RE-RANK,4k",
                    "max_tokens": 4000,
                    "model_type": "rerank"
                },
                {
                    "llm_name": "qwen-audio-asr",
                    "tags": "SPEECH2TEXT,8k",
                    "max_tokens": 8000,
                    "model_type": "speech2text"
                },
                {
                    "llm_name": "qwen-audio-asr-latest",
                    "tags": "SPEECH2TEXT,8k",
                    "max_tokens": 8000,
                    "model_type": "speech2text"
                },
                {
                    "llm_name": "qwen-audio-asr-1204",
                    "tags": "SPEECH2TEXT,8k",
                    "max_tokens": 8000,
                    "model_type": "speech2text"
                },
                {
                    "llm_name": "qianwen-deepresearch-30b-a3b-131k",
                    "tags": "LLM,CHAT,1M,AGENT,DEEPRESEARCH",
                    "max_tokens": 1000000,
                    "model_type": "chat",
                    "is_tools": true
                }
            ]
        },
        {
            "name": "ZHIPU-AI",
            "logo": "",
            "tags": "LLM,TEXT EMBEDDING,SPEECH2TEXT,MODERATION",
            "status": "1",
            "rank": "940",
            "llm": [
                {
                    "llm_name": "glm-4.5",
                    "tags": "LLM,CHAT,128K",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "glm-4.5-x",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "glm-4.5-air",
                    "tags": "LLM,CHAT,128K",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "glm-4.5-airx",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "glm-4.5-flash",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "glm-4.5v",
                    "tags": "LLM,IMAGE2TEXT,64,",
                    "max_tokens": 64000,
                    "model_type": "image2text",
                    "is_tools": false
                },
                {
                    "llm_name": "glm-4-plus",
                    "tags": "LLM,CHAT,128K",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "glm-4-0520",
                    "tags": "LLM,CHAT,128K",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "glm-4",
                    "tags": "LLM,CHAT,128K",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "glm-4-airx",
                    "tags": "LLM,CHAT,",
                    "max_tokens": 8000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "glm-4-air",
                    "tags": "LLM,CHAT,",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "glm-4-flash",
                    "tags": "LLM,CHAT,",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "glm-4-flashx",
                    "tags": "LLM,CHAT,",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "glm-4-long",
                    "tags": "LLM,CHAT,",
                    "max_tokens": 1000000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "glm-3-turbo",
                    "tags": "LLM,CHAT,",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "glm-4v",
                    "tags": "LLM,CHAT,IMAGE2TEXT",
                    "max_tokens": 2000,
                    "model_type": "image2text",
                    "is_tools": true
                },
                {
                    "llm_name": "glm-4-9b",
                    "tags": "LLM,CHAT,",
                    "max_tokens": 8192,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "embedding-2",
                    "tags": "TEXT EMBEDDING",
                    "max_tokens": 512,
                    "model_type": "embedding"
                },
                {
                    "llm_name": "embedding-3",
                    "tags": "TEXT EMBEDDING",
                    "max_tokens": 512,
                    "model_type": "embedding"
                },
                {
                    "llm_name": "glm-asr",
                    "tags": "SPEECH2TEXT",
                    "max_tokens": 4096,
                    "model_type": "speech2text"
                }
            ]
        },
        {
            "name": "Ollama",
            "logo": "",
            "tags": "LLM,TEXT EMBEDDING,SPEECH2TEXT,MODERATION",
            "status": "1",
            "rank": "830",
            "llm": []
        },
        {
            "name": "ModelScope",
            "logo": "",
            "tags": "LLM",
            "status": "1",
            "llm": []
        },
        {
            "name": "LocalAI",
            "logo": "",
            "tags": "LLM,TEXT EMBEDDING,SPEECH2TEXT,MODERATION",
            "status": "1",
            "llm": []
        },
        {
            "name": "OpenAI-API-Compatible",
            "logo": "",
            "tags": "LLM,TEXT EMBEDDING,SPEECH2TEXT,MODERATION",
            "status": "1",
            "llm": [],
            "rank": "890"
        },
        {
            "name": "VLLM",
            "logo": "",
            "tags": "LLM,TEXT EMBEDDING,SPEECH2TEXT,MODERATION",
            "status": "1",
            "llm": []
        },
        {
            "name": "Moonshot",
            "logo": "",
            "tags": "LLM,TEXT EMBEDDING,IMAGE2TEXT",
            "status": "1",
            "rank": "960",
            "llm": [
                {
                    "llm_name": "kimi-thinking-preview",
                    "tags": "LLM,CHAT,1M",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "kimi-k2-0711-preview",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "kimi-k2-0905-preview",
                    "tags": "LLM,CHAT,256k",
                    "max_tokens": 262144,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "kimi-k2-thinking",
                    "tags": "LLM,CHAT,256k",
                    "max_tokens": 262144,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "kimi-k2-thinking-turbo",
                    "tags": "LLM,CHAT,256k",
                    "max_tokens": 262144,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "kimi-k2-turbo-preview",
                    "tags": "LLM,CHAT,256k",
                    "max_tokens": 262144,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "kimi-latest",
                    "tags": "LLM,CHAT,8k,32k,128k",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "moonshot-v1-8k",
                    "tags": "LLM,CHAT,8k",
                    "max_tokens": 8192,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "moonshot-v1-32k",
                    "tags": "LLM,CHAT,32k",
                    "max_tokens": 32768,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "moonshot-v1-128k",
                    "tags": "LLM,CHAT,128k",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "moonshot-v1-8k-vision-preview",
                    "tags": "LLM,IMAGE2TEXT,8k",
                    "max_tokens": 8192,
                    "model_type": "image2text",
                    "is_tools": true
                },
                {
                    "llm_name": "moonshot-v1-32k-vision-preview",
                    "tags": "LLM,IMAGE2TEXT,32k",
                    "max_tokens": 32768,
                    "model_type": "image2text",
                    "is_tools": true
                },
                {
                    "llm_name": "moonshot-v1-128k-vision-preview",
                    "tags": "LLM,IMAGE2TEXT,128k",
                    "max_tokens": 131072,
                    "model_type": "image2text",
                    "is_tools": true
                },
                {
                    "llm_name": "moonshot-v1-auto",
                    "tags": "LLM,CHAT,",
                    "max_tokens": 128000,
                    "model_type": "chat",
                    "is_tools": true
                }
            ]
        },
        {
            "name": "FastEmbed",
            "logo": "",
            "tags": "TEXT EMBEDDING",
            "status": "1",
            "llm": []
        },
        {
            "name": "Xinference",
            "logo": "",
            "tags": "LLM,TEXT EMBEDDING,TTS,SPEECH2TEXT,MODERATION,TEXT RE-RANK",
            "status": "1",
            "llm": []
        },
        {
            "name": "DeepSeek",
            "logo": "",
            "tags": "LLM",
            "status": "1",
            "rank": "970",
            "llm": [
                {
                    "llm_name": "deepseek-chat",
                    "tags": "LLM,CHAT,",
                    "max_tokens": 64000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "deepseek-reasoner",
                    "tags": "LLM,CHAT,",
                    "max_tokens": 64000,
                    "model_type": "chat",
                    "is_tools": true
                }
            ]
        },
        {
            "name": "VolcEngine",
            "logo": "",
            "tags": "LLM, TEXT EMBEDDING, IMAGE2TEXT",
            "status": "1",
            "llm": []
        },
        {
            "name": "BaiChuan",
            "logo": "",
            "tags": "LLM,TEXT EMBEDDING",
            "status": "1",
            "llm": [
                {
                    "llm_name": "Baichuan2-Turbo",
                    "tags": "LLM,CHAT,32K",
                    "max_tokens": 32768,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "Baichuan2-Turbo-192k",
                    "tags": "LLM,CHAT,192K",
                    "max_tokens": 196608,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "Baichuan3-Turbo",
                    "tags": "LLM,CHAT,32K",
                    "max_tokens": 32768,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "Baichuan3-Turbo-128k",
                    "tags": "LLM,CHAT,128K",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "Baichuan4",
                    "tags": "LLM,CHAT,128K",
                    "max_tokens": 131072,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "Baichuan-Text-Embedding",
                    "tags": "TEXT EMBEDDING",
                    "max_tokens": 512,
                    "model_type": "embedding"
                }
            ]
        },
        {
            "name": "Jina",
            "logo": "",
            "tags": "TEXT EMBEDDING, TEXT RE-RANK",
            "status": "1",
            "llm": [
                {
                    "llm_name": "jina-reranker-v1-base-en",
                    "tags": "RE-RANK,8k",
                    "max_tokens": 8196,
                    "model_type": "rerank"
                },
                {
                    "llm_name": "jina-reranker-v1-turbo-en",
                    "tags": "RE-RANK,8k",
                    "max_tokens": 8196,
                    "model_type": "rerank"
                },
                {
                    "llm_name": "jina-reranker-v1-tiny-en",
                    "tags": "RE-RANK,8k",
                    "max_tokens": 8196,
                    "model_type": "rerank"
                },
                {
                    "llm_name": "jina-colbert-v1-en",
                    "tags": "RE-RANK,8k",
                    "max_tokens": 8196,
                    "model_type": "rerank"
                },
                {
                    "llm_name": "jina-embeddings-v2-base-en",
                    "tags": "TEXT EMBEDDING",
                    "max_tokens": 8196,
                    "model_type": "embedding"
                },
                {
                    "llm_name": "jina-embeddings-v2-base-de",
                    "tags": "TEXT EMBEDDING",
                    "max_tokens": 8196,
                    "model_type": "embedding"
                },
                {
                    "llm_name": "jina-embeddings-v2-base-es",
                    "tags": "TEXT EMBEDDING",
                    "max_tokens": 8196,
                    "model_type": "embedding"
                },
                {
                    "llm_name": "jina-embeddings-v2-base-code",
                    "tags": "TEXT EMBEDDING",
                    "max_tokens": 8196,
                    "model_type": "embedding"
                },
                {
                    "llm_name": "jina-embeddings-v2-base-zh",
                    "tags": "TEXT EMBEDDING",
                    "max_tokens": 8196,
                    "model_type": "embedding"
                },
                {
                    "llm_name": "jina-reranker-v2-base-multilingual",
                    "tags": "RE-RANK,8k",
                    "max_tokens": 8196,
                    "model_type": "rerank"
                },
                {
                    "llm_name": "jina-embeddings-v3",
                    "tags": "TEXT EMBEDDING",
                    "max_tokens": 8196,
                    "model_type": "embedding"
                }
            ]
        },
        {
            "name": "Builtin",
            "logo": "",
            "tags": "TEXT EMBEDDING",
            "status": "1",
            "llm": [
                {
                    "llm_name": "BAAI/bge-small-en-v1.5",
                    "tags": "TEXT EMBEDDING,512",
                    "max_tokens": 512,
                    "model_type": "embedding"
                },
                {
                    "llm_name": "BAAI/bge-m3",
                    "tags": "TEXT EMBEDDING,8k",
                    "max_tokens": 8192,
                    "model_type": "embedding"
                },
                {
                    "llm_name": "Qwen/Qwen3-Embedding-0.6B",
                    "tags": "TEXT EMBEDDING,32k",
                    "max_tokens": 32768,
                    "model_type": "embedding"
                }
            ]
        },
        {
            "name": "MiniMax",
            "logo": "",
            "tags": "LLM,TEXT EMBEDDING",
            "status": "1",
            "rank": "810",
            "llm": [
                {
                    "llm_name": "abab6.5-chat",
                    "tags": "LLM,CHAT,8k",
                    "max_tokens": 8192,
                    "model_type": "chat"
                },
                {
                    "llm_name": "abab6.5s-chat",
                    "tags": "LLM,CHAT,245k",
                    "max_tokens": 245760,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "abab6.5t-chat",
                    "tags": "LLM,CHAT,8k",
                    "max_tokens": 8192,
                    "model_type": "chat"
                },
                {
                    "llm_name": "abab6.5g-chat",
                    "tags": "LLM,CHAT,8k",
                    "max_tokens": 8192,
                    "model_type": "chat"
                },
                {
                    "llm_name": "abab5.5s-chat",
                    "tags": "LLM,CHAT,8k",
                    "max_tokens": 8192,
                    "model_type": "chat"
                }
            ]
        },
        {
            "name": "Mistral",
            "logo": "",
            "tags": "LLM,TEXT EMBEDDING,MODERATION",
            "status": "1",
            "rank": "910",
            "llm": [
                {
                    "llm_name": "codestral-latest",
                    "tags": "LLM,CHAT,256k",
                    "max_tokens": 256000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "mistral-large-latest",
                    "tags": "LLM,CHAT,131k",
                    "max_tokens": 131000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "mistral-saba-latest",
                    "tags": "LLM,CHAT,32k",
                    "max_tokens": 32000,
                    "model_type": "chat"
                },
                {
                    "llm_name": "pixtral-large-latest",
                    "tags": "LLM,CHAT,IMAGE2TEXT,131k",
                    "max_tokens": 131000,
                    "model_type": "image2text",
                    "is_tools": true
                },
                {
                    "llm_name": "ministral-3b-latest",
                    "tags": "LLM,CHAT,131k",
                    "max_tokens": 131000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "ministral-8b-latest",
                    "tags": "LLM,CHAT,131k",
                    "max_tokens": 131000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "mistral-embed",
                    "tags": "TEXT EMBEDDING,8k",
                    "max_tokens": 8192,
                    "model_type": "embedding"
                },
                {
                    "llm_name": "mistral-moderation-latest",
                    "tags": "LLM,CHAT,8k",
                    "max_tokens": 8192,
                    "model_type": "chat"
                },
                {
                    "llm_name": "mistral-small-latest",
                    "tags": "LLM,CHAT,32k",
                    "max_tokens": 32000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "pixtral-12b-2409",
                    "tags": "LLM,IMAGE2TEXT,131k",
                    "max_tokens": 131000,
                    "model_type": "image2text"
                },
                {
                    "llm_name": "mistral-ocr-latest",
                    "tags": "LLM,IMAGE2TEXT,131k",
                    "max_tokens": 131000,
                    "model_type": "image2text"
                },
                {
                    "llm_name": "open-mistral-nemo",
                    "tags": "LLM,CHAT,131k",
                    "max_tokens": 131000,
                    "model_type": "chat",
                    "is_tools": true
                },
                {
                    "llm_name": "open-codestral-mamba",
                    "tags": "LLM,CHAT,256k",
                    "max_tokens": 256000,
                    "model_type": "chat"
                }
            ]
        },
        {
            "name": "Azure-OpenAI",
            "logo": "",
            "tags": "LLM,TEXT EMBEDDING,SPEECH2TEXT,MODERATION",
            "status": "1",
            "rank": "850",
            "llm": [
                {
                    "llm_name": "gpt-4o-mini",
                    "tags": "LLM,CHAT,128K",
                    "max_tokens": 128000,
                    "model_type": "image2text",
                    "is_tools": true
                },
                {
                    "llm_name": "gpt-4o",
                    "tags": "LLM,CHAT,128K",
                    "max_tokens": 128000,
                    "model_type": "image2text",
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
                    "llm_name": "gpt-3.5-turbo-16k",
                    "tags": "LLM,CHAT,16k",
                    "max_tokens": 16385,
                    "model_type": "chat"
                },
                {
                    "llm_name": "text-embedding-ada-002",
                    "tags": "TEXT EMBEDDING,8K",
                    "max_tokens": 8191,
                    "model_type": "embedding"
                },
                {
                    "llm_name": "text-embedding-3-small",
                    "tags": "TEXT EMBEDDING,8K",
                    "max_tokens": 8191,
                    "model_type": "embedding"
                },
                {
                    "llm_name": "text-embedding-3-large",
                    "tags": "TEXT EMBEDDING,8K",
                    "max_tokens": 8191,
                    "model_type": "embedding"
                },
                {
                    "llm_name": "whisper-1",
                    "tags": "SPEECH2TEXT",
                    "max_tokens": 26214400,
                    "model_type": "speech2text"
                },
                {
                    "llm_name": "gpt-4",
                    "tags": "LLM,CHAT,8K",
                    "max_tokens": 8191,
                    "model_type": "chat"
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
                    "model_type": "chat"
                },
                {
                    "llm_name": "gpt-4-vision-preview",
                    "tags": "LLM,CHAT,IMAGE2TEXT",
                    "max_tokens": 765,
                    "model_type": "image2text"
                }
            ]
        },
        {
            "name": "Bedrock",
            "logo": "",
            "tags": "LLM,TEXT EMBEDDING",
            "status": "1",
            "rank": "860",
 

[... Content truncated for brevity ...]
```

## High-Level Overview

This file is part of the RAGFlow repository located at `conf/llm_factories.json`.

Based on the file structure and naming, it appears to be a data/configuration - json data or configuration file.

The file contains approximately 4844 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

This is a configuration or data file. See the 'Original Source' section for full content.

## Code Structure Analysis

- Total lines: 4844
- Blank lines: 0 (0.0%)
- Comment lines: ~0 (0.0%)
- Code lines: ~4844


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `conf` directory, specifically within `conf`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `conf/` directory
- Potential test file: `test_llm_factories.json`

## Keywords

A22B, A3B, AGENT, API, Align, Anthropic, Audio, Azure, BAAI, BaiChuan, Baichuan, Baichuan2, Baichuan3, Baichuan4, BaiduYiyan, Bedrock, Builtin, CHAT, Chat, Cloud, Coder, Cohere, CometAPI, Compatible, DEEPRESEARCH, DeepInfra, DeepSeek, DeerAPI, Devstral, Distill, EMBEDDING, ERNIE, Embedding, FP8, FastEmbed, Fish, Flash, GLM, GPUStack, Gemini, GiteeAI, Google, Groq, Guard, HuggingFace, Hunyuan, IMAGE2TEXT, Instruct, InternVL2, InternVL3...

---
*Generated by RAGFlow Repository Documentation Generator*
