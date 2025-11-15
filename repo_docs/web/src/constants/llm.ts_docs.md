# File Documentation: web/src/constants/llm.ts

## File Metadata

- **Path**: `web/src/constants/llm.ts`
- **Extension**: `.ts`
- **Lines**: 127
- **Characters**: 3,857
- **Size**: 3,865 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
export enum LLMFactory {
  TongYiQianWen = 'Tongyi-Qianwen',
  Moonshot = 'Moonshot',
  OpenAI = 'OpenAI',
  ZhipuAI = 'ZHIPU-AI',
  WenXinYiYan = '文心一言',
  Ollama = 'Ollama',
  Xinference = 'Xinference',
  ModelScope = 'ModelScope',
  DeepSeek = 'DeepSeek',
  VolcEngine = 'VolcEngine',
  BaiChuan = 'BaiChuan',
  Jina = 'Jina',
  MiniMax = 'MiniMax',
  Mistral = 'Mistral',
  AzureOpenAI = 'Azure-OpenAI',
  Bedrock = 'Bedrock',
  Gemini = 'Gemini',
  Groq = 'Groq',
  OpenRouter = 'OpenRouter',
  LocalAI = 'LocalAI',
  StepFun = 'StepFun',
  NVIDIA = 'NVIDIA',
  LMStudio = 'LM-Studio',
  OpenAiAPICompatible = 'OpenAI-API-Compatible',
  Cohere = 'Cohere',
  LeptonAI = 'LeptonAI',
  TogetherAI = 'TogetherAI',
  PerfXCloud = 'PerfXCloud',
  Upstage = 'Upstage',
  NovitaAI = 'NovitaAI',
  SILICONFLOW = 'SILICONFLOW',
  PPIO = 'PPIO',
  Replicate = 'Replicate',
  TencentHunYuan = 'Tencent Hunyuan',
  XunFeiSpark = 'XunFei Spark',
  BaiduYiYan = 'BaiduYiyan',
  FishAudio = 'Fish Audio',
  TencentCloud = 'Tencent Cloud',
  Anthropic = 'Anthropic',
  VoyageAI = 'Voyage AI',
  GoogleCloud = 'Google Cloud',
  HuggingFace = 'HuggingFace',
  YouDao = 'Youdao',
  BAAI = 'BAAI',
  NomicAI = 'nomic-ai',
  JinaAI = 'jinaai',
  SentenceTransformers = 'sentence-transformers',
  GPUStack = 'GPUStack',
  VLLM = 'VLLM',
  GiteeAI = 'GiteeAI',
  Ai302 = '302.AI',
  DeepInfra = 'DeepInfra',
  Grok = 'Grok',
  XAI = 'xAI',
  TokenPony = 'TokenPony',
  Meituan = 'Meituan',
  Longcat = 'LongCat',
  CometAPI = 'CometAPI',
  DeerAPI = 'DeerAPI',
  Builtin = 'Builtin',
}

// Please lowercase the file name
export const IconMap = {
  [LLMFactory.TongYiQianWen]: 'tongyi-qianwen',
  [LLMFactory.Moonshot]: 'moonshot',
  [LLMFactory.OpenAI]: 'openai',
  [LLMFactory.ZhipuAI]: 'zhipu',
  [LLMFactory.WenXinYiYan]: 'wenxin',
  [LLMFactory.Ollama]: 'ollama',
  [LLMFactory.Xinference]: 'xinference',
  [LLMFactory.ModelScope]: 'modelscope',
  [LLMFactory.DeepSeek]: 'deepseek',
  [LLMFactory.VolcEngine]: 'volcengine',
  [LLMFactory.BaiChuan]: 'baichuan',
  [LLMFactory.Jina]: 'jina',
  [LLMFactory.MiniMax]: 'MiniMax',
  [LLMFactory.Mistral]: 'mistral',
  [LLMFactory.AzureOpenAI]: 'azure',
  [LLMFactory.Bedrock]: 'bedrock',
  [LLMFactory.Gemini]: 'gemini',
  [LLMFactory.Groq]: 'groq-next',
  [LLMFactory.OpenRouter]: 'open-router',
  [LLMFactory.LocalAI]: 'local-ai',
  [LLMFactory.StepFun]: 'stepfun',
  [LLMFactory.NVIDIA]: 'nvidia',
  [LLMFactory.LMStudio]: 'lm-studio',
  [LLMFactory.OpenAiAPICompatible]: 'openai-api',
  [LLMFactory.Cohere]: 'cohere',
  [LLMFactory.LeptonAI]: 'lepton',
  [LLMFactory.TogetherAI]: 'together',
  [LLMFactory.PerfXCloud]: 'perfx-cloud',
  [LLMFactory.Upstage]: 'upstage',
  [LLMFactory.NovitaAI]: 'novita-ai',
  [LLMFactory.SILICONFLOW]: 'siliconflow',
  [LLMFactory.PPIO]: 'ppio',
  [LLMFactory.Replicate]: 'replicate',
  [LLMFactory.TencentHunYuan]: 'hunyuan',
  [LLMFactory.XunFeiSpark]: 'spark',
  [LLMFactory.BaiduYiYan]: 'wenxinyiyan',
  [LLMFactory.FishAudio]: 'fish-audio',
  [LLMFactory.TencentCloud]: 'tencent-cloud',
  [LLMFactory.Anthropic]: 'anthropic',
  [LLMFactory.VoyageAI]: 'voyage',
  [LLMFactory.GoogleCloud]: 'google-cloud',
  [LLMFactory.HuggingFace]: 'huggingface',
  [LLMFactory.YouDao]: 'youdao',
  [LLMFactory.BAAI]: 'baai',
  [LLMFactory.NomicAI]: 'nomic-ai',
  [LLMFactory.JinaAI]: 'jina',
  [LLMFactory.SentenceTransformers]: 'sentence-transformers',
  [LLMFactory.GPUStack]: 'gpustack',
  [LLMFactory.VLLM]: 'vllm',
  [LLMFactory.GiteeAI]: 'gitee-ai',
  [LLMFactory.Ai302]: 'ai302',
  [LLMFactory.DeepInfra]: 'deepinfra',
  [LLMFactory.Grok]: 'grok',
  [LLMFactory.XAI]: 'xai',
  [LLMFactory.TokenPony]: 'tokenpony',
  [LLMFactory.Meituan]: 'longcat',
  [LLMFactory.Longcat]: 'longcat',
  [LLMFactory.CometAPI]: 'cometapi',
  [LLMFactory.DeerAPI]: 'deerapi',
  [LLMFactory.Builtin]: 'builtin',
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/constants/llm.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 127 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `IconMap`: Exported entity

## Code Structure Analysis

- Total lines: 127
- Blank lines: 2 (1.6%)
- Comment lines: ~1 (0.8%)
- Code lines: ~124


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/constants`.

This appears to be a UI component or frontend module.

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

- Other files in `web/src/constants/` directory
- Potential test file: `test_llm.ts`

## Keywords

API, Ai302, Anthropic, Audio, Azure, AzureOpenAI, BAAI, BaiChuan, BaiduYiYan, BaiduYiyan, Bedrock, Builtin, Cloud, Cohere, CometAPI, Compatible, DeepInfra, DeepSeek, DeerAPI, Fish, FishAudio, GPUStack, Gemini, GiteeAI, Google, GoogleCloud, Grok, Groq, HuggingFace, Hunyuan, IconMap, Jina, JinaAI, LLMFactory, LMStudio, LeptonAI, LocalAI, LongCat, Longcat, Meituan, MiniMax, Mistral, ModelScope, Moonshot, NVIDIA, NomicAI, NovitaAI, Ollama, OpenAI, OpenAiAPICompatible...

---
*Generated by RAGFlow Repository Documentation Generator*
