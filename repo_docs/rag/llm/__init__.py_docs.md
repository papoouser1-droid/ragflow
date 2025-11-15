# File Documentation: rag/llm/__init__.py

## File Metadata

- **Path**: `rag/llm/__init__.py`
- **Extension**: `.py`
- **Lines**: 161
- **Characters**: 6,111
- **Size**: 6,111 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

## Original Source

```python
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
#  AFTER UPDATING THIS FILE, PLEASE ENSURE THAT docs/references/supported_models.mdx IS ALSO UPDATED for consistency!
#

import importlib
import inspect

from strenum import StrEnum


class SupportedLiteLLMProvider(StrEnum):
    Tongyi_Qianwen = "Tongyi-Qianwen"
    Dashscope = "Dashscope"
    Bedrock = "Bedrock"
    Moonshot = "Moonshot"
    xAI = "xAI"
    DeepInfra = "DeepInfra"
    Groq = "Groq"
    Cohere = "Cohere"
    Gemini = "Gemini"
    DeepSeek = "DeepSeek"
    Nvidia = "NVIDIA"
    TogetherAI = "TogetherAI"
    Anthropic = "Anthropic"
    Ollama = "Ollama"
    LongCat = "LongCat"
    CometAPI = "CometAPI"
    SILICONFLOW = "SILICONFLOW"
    OpenRouter = "OpenRouter"
    StepFun = "StepFun"
    PPIO = "PPIO"
    PerfXCloud = "PerfXCloud"
    Upstage = "Upstage"
    NovitaAI = "NovitaAI"
    Lingyi_AI = "01.AI"
    GiteeAI = "GiteeAI"
    AI_302 = "302.AI"


FACTORY_DEFAULT_BASE_URL = {
    SupportedLiteLLMProvider.Tongyi_Qianwen: "https://dashscope.aliyuncs.com/compatible-mode/v1",
    SupportedLiteLLMProvider.Dashscope: "https://dashscope.aliyuncs.com/compatible-mode/v1",
    SupportedLiteLLMProvider.Moonshot: "https://api.moonshot.cn/v1",
    SupportedLiteLLMProvider.Ollama: "",
    SupportedLiteLLMProvider.LongCat: "https://api.longcat.chat/openai",
    SupportedLiteLLMProvider.CometAPI: "https://api.cometapi.com/v1",
    SupportedLiteLLMProvider.SILICONFLOW: "https://api.siliconflow.cn/v1",
    SupportedLiteLLMProvider.OpenRouter: "https://openrouter.ai/api/v1",
    SupportedLiteLLMProvider.StepFun: "https://api.stepfun.com/v1",
    SupportedLiteLLMProvider.PPIO: "https://api.ppinfra.com/v3/openai",
    SupportedLiteLLMProvider.PerfXCloud: "https://cloud.perfxlab.cn/v1",
    SupportedLiteLLMProvider.Upstage: "https://api.upstage.ai/v1/solar",
    SupportedLiteLLMProvider.NovitaAI: "https://api.novita.ai/v3/openai",
    SupportedLiteLLMProvider.Lingyi_AI: "https://api.lingyiwanwu.com/v1",
    SupportedLiteLLMProvider.GiteeAI: "https://ai.gitee.com/v1/",
    SupportedLiteLLMProvider.AI_302: "https://api.302.ai/v1",
    SupportedLiteLLMProvider.Anthropic: "https://api.anthropic.com/",
}


LITELLM_PROVIDER_PREFIX = {
    SupportedLiteLLMProvider.Tongyi_Qianwen: "dashscope/",
    SupportedLiteLLMProvider.Dashscope: "dashscope/",
    SupportedLiteLLMProvider.Bedrock: "bedrock/",
    SupportedLiteLLMProvider.Moonshot: "moonshot/",
    SupportedLiteLLMProvider.xAI: "xai/",
    SupportedLiteLLMProvider.DeepInfra: "deepinfra/",
    SupportedLiteLLMProvider.Groq: "groq/",
    SupportedLiteLLMProvider.Cohere: "",  # don't need a prefix
    SupportedLiteLLMProvider.Gemini: "gemini/",
    SupportedLiteLLMProvider.DeepSeek: "deepseek/",
    SupportedLiteLLMProvider.Nvidia: "nvidia_nim/",
    SupportedLiteLLMProvider.TogetherAI: "together_ai/",
    SupportedLiteLLMProvider.Anthropic: "",  # don't need a prefix
    SupportedLiteLLMProvider.Ollama: "ollama_chat/",
    SupportedLiteLLMProvider.LongCat: "openai/",
    SupportedLiteLLMProvider.CometAPI: "openai/",
    SupportedLiteLLMProvider.SILICONFLOW: "openai/",
    SupportedLiteLLMProvider.OpenRouter: "openai/",
    SupportedLiteLLMProvider.StepFun: "openai/",
    SupportedLiteLLMProvider.PPIO: "openai/",
    SupportedLiteLLMProvider.PerfXCloud: "openai/",
    SupportedLiteLLMProvider.Upstage: "openai/",
    SupportedLiteLLMProvider.NovitaAI: "openai/",
    SupportedLiteLLMProvider.Lingyi_AI: "openai/",
    SupportedLiteLLMProvider.GiteeAI: "openai/",
    SupportedLiteLLMProvider.AI_302: "openai/",
}

ChatModel = globals().get("ChatModel", {})
CvModel = globals().get("CvModel", {})
EmbeddingModel = globals().get("EmbeddingModel", {})
RerankModel = globals().get("RerankModel", {})
Seq2txtModel = globals().get("Seq2txtModel", {})
TTSModel = globals().get("TTSModel", {})


MODULE_MAPPING = {
    "chat_model": ChatModel,
    "cv_model": CvModel,
    "embedding_model": EmbeddingModel,
    "rerank_model": RerankModel,
    "sequence2txt_model": Seq2txtModel,
    "tts_model": TTSModel,
}

package_name = __name__

for module_name, mapping_dict in MODULE_MAPPING.items():
    full_module_name = f"{package_name}.{module_name}"
    module = importlib.import_module(full_module_name)

    base_class = None
    lite_llm_base_class = None
    for name, obj in inspect.getmembers(module):
        if inspect.isclass(obj):
            if name == "Base":
                base_class = obj
            elif name == "LiteLLMBase":
                lite_llm_base_class = obj
                assert hasattr(obj, "_FACTORY_NAME"), "LiteLLMbase should have _FACTORY_NAME field."
                if hasattr(obj, "_FACTORY_NAME"):
                    if isinstance(obj._FACTORY_NAME, list):
                        for factory_name in obj._FACTORY_NAME:
                            mapping_dict[factory_name] = obj
                    else:
                        mapping_dict[obj._FACTORY_NAME] = obj

    if base_class is not None:
        for _, obj in inspect.getmembers(module):
            if inspect.isclass(obj) and issubclass(obj, base_class) and obj is not base_class and hasattr(obj, "_FACTORY_NAME"):
                if isinstance(obj._FACTORY_NAME, list):
                    for factory_name in obj._FACTORY_NAME:
                        mapping_dict[factory_name] = obj
                else:
                    mapping_dict[obj._FACTORY_NAME] = obj


__all__ = [
    "ChatModel",
    "CvModel",
    "EmbeddingModel",
    "RerankModel",
    "Seq2txtModel",
    "TTSModel",
]

```

## High-Level Overview

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
#  AFTER UPDATING THIS FILE, PLEASE ENSURE THAT docs/references/supported_models.mdx IS ALSO UPDATED for consistency!
#

## Detailed Walkthrough

### Classes (1)

- `SupportedLiteLLMProvider`: Class definition

### Imports (3)

- `import importlib`
- `import inspect`
- `from strenum import StrEnum`

## Code Structure Analysis

- Total lines: 161
- Blank lines: 18 (11.2%)
- Comment lines: ~17 (10.6%)
- Code lines: ~126


## Dependencies and Imports

- `import importlib`
- `import inspect`
- `from strenum import StrEnum`

## Design & Architecture

This file is located in the `rag` directory, specifically within `rag/llm`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 7 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **SQL Operations**: Ensure parameterized queries to prevent SQL injection
- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `rag/llm/` directory
- Imports from `strenum`
- Potential test file: `test___init__.py`

## Keywords

AFTER, AI_302, ALSO, ANY, All, Anthropic, Apache, Authors, BASIS, Base, Bedrock, CONDITIONS, ChatModel, Cohere, CometAPI, Copyright, CvModel, Dashscope, DeepInfra, DeepSeek, ENSURE, EmbeddingModel, FACTORY_DEFAULT_BASE_URL, FILE, Gemini, GiteeAI, Groq, InfiniFlow, KIND, LICENSE, LITELLM_PROVIDER_PREFIX, License, Licensed, Lingyi_AI, LiteLLMBase, LiteLLMbase, LongCat, MODULE_MAPPING, Moonshot, NVIDIA, None, NovitaAI, Nvidia, Ollama, OpenRouter, PLEASE, PPIO, PerfXCloud, Python, Qianwen...

---
*Generated by RAGFlow Repository Documentation Generator*
