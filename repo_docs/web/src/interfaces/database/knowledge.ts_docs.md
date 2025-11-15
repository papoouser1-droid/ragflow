# File Documentation: web/src/interfaces/database/knowledge.ts

## File Metadata

- **Path**: `web/src/interfaces/database/knowledge.ts`
- **Extension**: `.ts`
- **Lines**: 180
- **Characters**: 4,016
- **Size**: 4,016 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { RunningStatus } from '@/constants/knowledge';
import { DataSourceKey } from '@/pages/user-setting/data-source/contant';
import { TreeData } from '@antv/g6/lib/types';
export interface IConnector {
  id: string;
  name: string;
  status: RunningStatus;
  source: DataSourceKey;
  auto_parse?: '0' | '1';
}
// knowledge base
export interface IKnowledge {
  avatar?: any;
  chunk_num: number;
  create_date: string;
  create_time: number;
  created_by: string;
  description: string;
  doc_num: number;
  id: string;
  name: string;
  parser_config: ParserConfig;
  parser_id: string;
  pipeline_id: string;
  pipeline_name: string;
  pipeline_avatar: string;
  permission: string;
  similarity_threshold: number;
  status: string;
  tenant_id: string;
  token_num: number;
  update_date: string;
  update_time: number;
  vector_similarity_weight: number;
  embd_id: string;
  nickname: string;
  operator_permission: number;
  size: number;
  raptor_task_finish_at?: string;
  raptor_task_id?: string;
  mindmap_task_finish_at?: string;
  mindmap_task_id?: string;
  graphrag_task_finish_at: string;
  graphrag_task_id: string;
  connectors: IConnector[];
}

export interface IKnowledgeResult {
  kbs: IKnowledge[];
  total: number;
}

export interface Raptor {
  use_raptor: boolean;
}

export interface ParserConfig {
  from_page?: number;
  to_page?: number;
  auto_keywords?: number;
  auto_questions?: number;
  chunk_token_num?: number;
  delimiter?: string;
  html4excel?: boolean;
  layout_recognize?: boolean;
  raptor?: Raptor;
  tag_kb_ids?: string[];
  topn_tags?: number;
  graphrag?: { use_graphrag?: boolean };
}

export interface IKnowledgeFileParserConfig {
  chunk_token_num: number;
  layout_recognize: boolean;
  pages: number[][];
  task_page_size: number;
}
export interface IKnowledgeFile {
  chunk_num: number;
  create_date: string;
  create_time: number;
  created_by: string;
  id: string;
  kb_id: string;
  location: string;
  name: string;
  parser_id: string;
  process_begin_at?: any;
  process_duration: number;
  progress: number; // parsing process
  progress_msg: string; // parsing log
  run: RunningStatus; // parsing status
  size: number;
  source_type: string;
  status: string; // enabled
  thumbnail?: any; // base64
  token_num: number;
  type: string;
  update_date: string;
  update_time: number;
  parser_config: IKnowledgeFileParserConfig;
}

export interface ITenantInfo {
  asr_id: string;
  embd_id: string;
  img2txt_id: string;
  llm_id: string;
  name: string;
  parser_ids: string;
  role: string;
  tenant_id: string;
  chat_id: string;
  speech2text_id: string;
  rerank_id?: string;
  tts_id: string;
}

export interface IChunk {
  available_int: number; // Whether to enable, 0: not enabled, 1: enabled
  chunk_id: string;
  content_with_weight: string;
  doc_id: string;
  doc_name: string;
  image_id: string;
  important_kwd?: string[];
  question_kwd?: string[]; // keywords
  tag_kwd?: string[];
  positions: number[][];
  tag_feas?: Record<string, number>;
}

export interface ITestingChunk {
  chunk_id: string;
  content_ltks: string;
  content_with_weight: string;
  doc_id: string;
  doc_name: string;
  img_id: string;
  image_id: string;
  important_kwd: any[];
  kb_id: string;
  similarity: number;
  term_similarity: number;
  vector: number[];
  vector_similarity: number;
  highlight: string;
  positions: number[][];
  docnm_kwd: string;
  doc_type_kwd: string;
}

export interface ITestingDocument {
  count: number;
  doc_id: string;
  doc_name: string;
}

export interface ITestingResult {
  chunks: ITestingChunk[];
  documents: ITestingDocument[];
  total: number;
  labels?: Record<string, number>;
}

export interface INextTestingResult {
  chunks: ITestingChunk[];
  doc_aggs: ITestingDocument[];
  total: number;
  labels?: Record<string, number>;
  isRuned?: boolean;
}

export type IRenameTag = { fromTag: string; toTag: string };

export interface IKnowledgeGraph {
  graph: Record<string, any>;
  mind_map: TreeData;
}

```

## High-Level Overview

// knowledge base

## Detailed Walkthrough


### Imports (3)

- `import { RunningStatus } from '@/constants/knowledge';`
- `import { DataSourceKey } from '@/pages/user-setting/data-source/contant';`
- `import { TreeData } from '@antv/g6/lib/types';`

## Code Structure Analysis

- Total lines: 180
- Blank lines: 13 (7.2%)
- Comment lines: ~1 (0.6%)
- Code lines: ~166


## Dependencies and Imports

- `@/constants/knowledge`
- `@/pages/user-setting/data-source/contant`
- `@antv/g6/lib/types`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/interfaces/database`.

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

- Other files in `web/src/interfaces/database/` directory
- Potential test file: `test_knowledge.ts`

## Keywords

@/constants/knowledge, @/pages/user-setting/data-source/contant, @antv/g6/lib/types, DataSourceKey, IChunk, IConnector, IKnowledge, IKnowledgeFile, IKnowledgeFileParserConfig, IKnowledgeGraph, IKnowledgeResult, INextTestingResult, IRenameTag, ITenantInfo, ITestingChunk, ITestingDocument, ITestingResult, ParserConfig, Raptor, Record, RunningStatus, TreeData, TypeScript, Whether, antv

---
*Generated by RAGFlow Repository Documentation Generator*
