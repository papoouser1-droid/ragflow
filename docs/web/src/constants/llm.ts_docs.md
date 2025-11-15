# Documentation: web/src/constants/llm.ts

## File Metadata

- **Path**: `web/src/constants/llm.ts`
- **Size**: 3865 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/constants/llm.ts`.

## Original Source Code

```ts
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

## Detailed Analysis

### File Role in Repository

The file `web/src/constants/llm.ts` is located in the `web/src/constants` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to constants.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

- Watch for XSS vulnerabilities
- Ensure proper input sanitization
- Validate all API calls

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [agent.tsx](agent.tsx_docs.md)
- [authorization.ts](authorization.ts_docs.md)
- [chat.ts](chat.ts_docs.md)
- [common.ts](common.ts_docs.md)
- [file.ts](file.ts_docs.md)
- [form.ts](form.ts_docs.md)
- [knowledge.ts](knowledge.ts_docs.md)
- [permission.ts](permission.ts_docs.md)
- [setting.ts](setting.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
