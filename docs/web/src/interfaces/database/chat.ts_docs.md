# Documentation: web/src/interfaces/database/chat.ts

## File Metadata

- **Path**: `web/src/interfaces/database/chat.ts`
- **Size**: 3560 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/interfaces/database/chat.ts`.

## Original Source Code

```ts
import { MessageType } from '@/constants/chat';
import { IAttachment } from '@/hooks/use-send-message';

export interface PromptConfig {
  empty_response: string;
  parameters: Parameter[];
  prologue: string;
  system: string;
  tts?: boolean;
  quote: boolean;
  keyword: boolean;
  refine_multiturn: boolean;
  use_kg: boolean;
  reasoning?: boolean;
  cross_languages?: Array<string>;
}

export interface Parameter {
  key: string;
  optional: boolean;
}

export interface LlmSetting {
  Creative: Variable;
  Custom: Variable;
  Evenly: Variable;
  Precise: Variable;
}

export interface Variable {
  frequency_penalty?: number;
  max_tokens?: number;
  presence_penalty?: number;
  temperature?: number;
  top_p?: number;
  llm_id?: string;
}

export interface IDialog {
  create_date: string;
  create_time: number;
  description: string;
  icon: string;
  id: string;
  dialog_id: string;
  kb_ids: string[];
  kb_names: string[];
  language: string;
  llm_id: string;
  llm_setting: Variable;
  llm_setting_type: string;
  name: string;
  prompt_config: PromptConfig;
  prompt_type: string;
  status: string;
  tenant_id: string;
  update_date: string;
  update_time: number;
  vector_similarity_weight: number;
  similarity_threshold: number;
  top_k: number;
  top_n: number;
  meta_data_filter: MetaDataFilter;
}

interface MetaDataFilter {
  manual: Manual[];
  method: string;
}

interface Manual {
  key: string;
  op: string;
  value: string;
}

export interface IConversation {
  create_date: string;
  create_time: number;
  dialog_id: string;
  id: string;
  avatar: string;
  message: Message[];
  reference: IReference[];
  name: string;
  update_date: string;
  update_time: number;
  is_new: true;
}

export interface Message {
  content: string;
  role: MessageType;
  doc_ids?: string[];
  prompt?: string;
  id?: string;
  audio_binary?: string;
  data?: any;
  files?: File[];
  chatBoxId?: string;
  attachment?: IAttachment;
}

export interface IReferenceChunk {
  id: string;
  content: null;
  document_id: string;
  document_name: string;
  dataset_id: string;
  image_id: string;
  similarity: number;
  vector_similarity: number;
  term_similarity: number;
  positions: number[];
  doc_type?: string;
}

export interface IReference {
  chunks: IReferenceChunk[];
  doc_aggs: Docagg[];
  total: number;
}

export interface IReferenceObject {
  chunks: Record<string, IReferenceChunk>;
  doc_aggs: Record<string, Docagg>;
}

export interface IAnswer {
  answer: string;
  attachment?: IAttachment;
  reference?: IReference;
  conversationId?: string;
  prompt?: string;
  id?: string;
  audio_binary?: string;
  data?: any;
  chatBoxId?: string;
}

export interface Docagg {
  count: number;
  doc_id: string;
  doc_name: string;
  url?: string;
}

// interface Chunk {
//   chunk_id: string;
//   content_ltks: string;
//   content_with_weight: string;
//   doc_id: string;
//   docnm_kwd: string;
//   img_id: string;
//   important_kwd: any[];
//   kb_id: string;
//   similarity: number;
//   term_similarity: number;
//   vector_similarity: number;
// }

export interface IToken {
  create_date: string;
  create_time: number;
  tenant_id: string;
  token: string;
  update_date?: any;
  update_time?: any;
  beta: string;
}

export interface IStats {
  pv: [string, number][];
  uv: [string, number][];
  speed: [string, number][];
  tokens: [string, number][];
  round: [string, number][];
  thumb_up: [string, number][];
}

export interface IExternalChatInfo {
  avatar?: string;
  title: string;
  prologue?: string;
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/interfaces/database/chat.ts` is located in the `web/src/interfaces/database` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to database.

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

- [agent.ts](agent.ts_docs.md)
- [base.ts](base.ts_docs.md)
- [document.ts](document.ts_docs.md)
- [file-manager.ts](file-manager.ts_docs.md)
- [flow.ts](flow.ts_docs.md)
- [knowledge.ts](knowledge.ts_docs.md)
- [llm.ts](llm.ts_docs.md)
- [mcp-server.ts](mcp-server.ts_docs.md)
- [mcp.ts](mcp.ts_docs.md)
- [plugin.ts](plugin.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
