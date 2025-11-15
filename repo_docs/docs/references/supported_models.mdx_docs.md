# File Documentation: docs/references/supported_models.mdx

## File Metadata

- **Path**: `docs/references/supported_models.mdx`
- **Extension**: `.mdx`
- **Lines**: 82
- **Characters**: 8,924
- **Size**: 8,924 bytes
- **Purpose**: General file in the repository

## Original Source

```
---
sidebar_position: 1
slug: /supported_models
---

# Supported models

import APITable from '@site/src/components/APITable';

A complete list of models supported by RAGFlow, which will continue to expand.

```mdx-code-block
<APITable>
```

| Provider              | Chat               | Embedding          | Rerank             | Img2txt            | Speech2txt         | TTS                |
| --------------------- | ------------------ | ------------------ | ------------------ | ------------------ | ------------------ | ------------------ |
| Anthropic             | :heavy_check_mark: |                    |                    |                    |                    |                    |
| Azure-OpenAI          | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: | :heavy_check_mark: |                    |
| BAAI                  |                    | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |
| BaiChuan              | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |                    |
| BaiduYiyan            | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |
| Bedrock               | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |                    |
| Cohere                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |
| DeepSeek              | :heavy_check_mark: |                    |                    |                    |                    |                    |
| FastEmbed             |                    | :heavy_check_mark: |                    |                    |                    |                    |
| Fish Audio            |                    |                    |                    |                    |                    | :heavy_check_mark: |
| Gemini                | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |
| Google Cloud          | :heavy_check_mark: |                    |                    |                    |                    |                    |
| GPUStack              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: | :heavy_check_mark: |
| Groq                  | :heavy_check_mark: |                    |                    |                    |                    |                    |
| HuggingFace           | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |                    |
| Jina                  |                    | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |
| LeptonAI              | :heavy_check_mark: |                    |                    |                    |                    |                    |
| LocalAI               | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |
| LM-Studio             | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |
| MiniMax               | :heavy_check_mark: |                    |                    |                    |                    |                    |
| Mistral               | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |                    |
| ModelScope            | :heavy_check_mark: |                    |                    |                    |                    |                    |
| Moonshot              | :heavy_check_mark: |                    |                    | :heavy_check_mark: |                    |                    |
| Novita AI             | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |                    |
| NVIDIA                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |
| Ollama                | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |
| OpenAI                | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| OpenAI-API-Compatible | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |
| OpenRouter            | :heavy_check_mark: |                    |                    | :heavy_check_mark: |                    |                    |
| PerfXCloud            | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |                    |
| Replicate             | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |                    |
| PPIO                  | :heavy_check_mark: |                    |                    |                    |                    |                    |
| SILICONFLOW           | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |
| StepFun               | :heavy_check_mark: |                    |                    |                    |                    |                    |
| Tencent Hunyuan       | :heavy_check_mark: |                    |                    |                    |                    |                    |
| Tencent Cloud         |                    |                    |                    |                    | :heavy_check_mark: |                    |
| TogetherAI            | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |
| Tongyi-Qianwen        | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Upstage               | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |                    |
| VLLM                  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |
| VolcEngine            | :heavy_check_mark: |                    |                    |                    |                    |                    |
| Voyage AI             |                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |
| Xinference            | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| XunFei Spark          | :heavy_check_mark: |                    |                    |                    |                    | :heavy_check_mark: |
| xAI                   | :heavy_check_mark: |                    |                    | :heavy_check_mark: |                    |                    |
| Youdao                |                    | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |
| ZHIPU-AI              | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |
| 01.AI                 | :heavy_check_mark: |                    |                    |                    |                    |                    |
| DeepInfra             | :heavy_check_mark: | :heavy_check_mark: |                    |                    | :heavy_check_mark: | :heavy_check_mark: |
| 302.AI                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |
| CometAPI              | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |                    |
| DeerAPI               | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |

```mdx-code-block
</APITable>
```

:::danger IMPORTANT
If your model is not listed here but has APIs compatible with those of OpenAI, click **OpenAI-API-Compatible** on the **Model providers** page to configure your model.
:::

:::note
The list of supported models is extracted from [this source](https://github.com/infiniflow/ragflow/blob/main/rag/llm/__init__.py) and may not be the most current. For the latest supported model list, please refer to the Python file.
:::

```

## High-Level Overview

# Supported models

## Detailed Walkthrough

This file's structure is not automatically analyzed. See the 'Original Source' section for content.

## Code Structure Analysis

- Total lines: 82
- Blank lines: 9 (11.0%)
- Comment lines: ~1 (1.2%)
- Code lines: ~72


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `docs` directory, specifically within `docs/references`.

This file likely defines data models or schemas used throughout the application.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `docs/references/` directory
- Potential test file: `test_supported_models.mdx`

## Keywords

@site/src/components/APITable, API, APITable, APIs, Anthropic, Audio, Azure, BAAI, BaiChuan, BaiduYiyan, Bedrock, Chat, Cloud, Cohere, CometAPI, Compatible, DeepInfra, DeepSeek, DeerAPI, Embedding, FastEmbed, Fish, For, GPUStack, Gemini, Google, Groq, HuggingFace, Hunyuan, IMPORTANT, Img2txt, Jina, LeptonAI, LocalAI, MiniMax, Mistral, Model, ModelScope, Moonshot, NVIDIA, Novita, Ollama, OpenAI, OpenRouter, PPIO, PerfXCloud, Provider, Python, Qianwen, RAGFlow...

---
*Generated by RAGFlow Repository Documentation Generator*
