# Documentation: web/src/interfaces/database/flow.ts

## File Metadata

- **Path**: `web/src/interfaces/database/flow.ts`
- **Size**: 4044 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/interfaces/database/flow.ts`.

## Original Source Code

```ts
import { Edge, Node } from '@xyflow/react';
import { IReference, Message } from './chat';

export type DSLComponents = Record<string, IOperator>;

export interface DSL {
  components: DSLComponents;
  history: any[];
  path?: string[][];
  answer?: any[];
  graph?: IGraph;
  messages: Message[];
  reference: IReference[];
  globals: Record<string, any>;
  retrieval: IReference[];
}

export interface IOperator {
  obj: IOperatorNode;
  downstream: string[];
  upstream: string[];
  parent_id?: string;
}

export interface IOperatorNode {
  component_name: string;
  params: Record<string, unknown>;
}

export declare interface IFlow {
  avatar?: string;
  canvas_type: null;
  create_date: string;
  create_time: number;
  description: string;
  dsl: DSL;
  id: string;
  title: string;
  update_date: string;
  update_time: number;
  user_id: string;
  permission: string;
  nickname: string;
}

export interface IFlowTemplate {
  avatar: string;
  canvas_type: string;
  canvas_category?: string;
  create_date: string;
  create_time: number;
  description: {
    en: string;
    zh: string;
    de: string;
  };
  dsl: DSL;
  id: string;
  title: {
    en: string;
    zh: string;
    de: string;
  };
  update_date: string;
  update_time: number;
}

export type ICategorizeItemResult = Record<
  string,
  Omit<ICategorizeItem, 'name'>
>;

export interface IGenerateForm {
  max_tokens?: number;
  temperature?: number;
  top_p?: number;
  presence_penalty?: number;
  frequency_penalty?: number;
  cite?: boolean;
  prompt: number;
  llm_id: string;
  parameters: { key: string; component_id: string };
}
export interface ICategorizeItem {
  name: string;
  description?: string;
  examples?: string;
  to?: string;
  index: number;
}

export interface ICategorizeForm extends IGenerateForm {
  category_description: ICategorizeItemResult;
}

export interface IRelevantForm extends IGenerateForm {
  yes: string;
  no: string;
}

export interface ISwitchCondition {
  items: ISwitchItem[];
  logical_operator: string;
  to: string[] | string;
}

export interface ISwitchItem {
  cpn_id: string;
  operator: string;
  value: string;
}

export interface ISwitchForm {
  conditions: ISwitchCondition[];
  end_cpn_id: string;
  no: string;
}

export interface IBeginForm {
  prologue?: string;
}

export interface IRetrievalForm {
  similarity_threshold?: number;
  keywords_similarity_weight?: number;
  top_n?: number;
  top_k?: number;
  rerank_id?: string;
  empty_response?: string;
  kb_ids: string[];
}

export interface ICodeForm {
  inputs?: Array<{ name?: string; component_id?: string }>;
  lang: string;
  script?: string;
}

export type BaseNodeData<TForm extends any> = {
  label: string; // operator type
  name: string; // operator name
  color?: string;
  form?: TForm;
};

export type BaseNode<T = any> = Node<BaseNodeData<T>>;

export type IBeginNode = BaseNode<IBeginForm>;
export type IRetrievalNode = BaseNode<IRetrievalForm>;
export type IGenerateNode = BaseNode<IGenerateForm>;
export type ICategorizeNode = BaseNode<ICategorizeForm>;
export type ISwitchNode = BaseNode<ISwitchForm>;
export type IRagNode = BaseNode;
export type IRelevantNode = BaseNode;
export type ILogicNode = BaseNode;
export type INoteNode = BaseNode;
export type IMessageNode = BaseNode;
export type IRewriteNode = BaseNode;
export type IInvokeNode = BaseNode;
export type ITemplateNode = BaseNode;
export type IEmailNode = BaseNode;
export type IIterationNode = BaseNode;
export type IIterationStartNode = BaseNode;
export type IKeywordNode = BaseNode;
export type ICodeNode = BaseNode<ICodeForm>;
export type IAgentNode<T = any> = BaseNode<T>;

export type RAGFlowNodeType =
  | IBeginNode
  | IRetrievalNode
  | IGenerateNode
  | ICategorizeNode
  | ISwitchNode
  | IRagNode
  | IRelevantNode
  | ILogicNode
  | INoteNode
  | IMessageNode
  | IRewriteNode
  | IInvokeNode
  | ITemplateNode
  | IEmailNode
  | IIterationNode
  | IIterationStartNode
  | IKeywordNode;

export interface IGraph {
  nodes: RAGFlowNodeType[];
  edges: Edge[];
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/interfaces/database/flow.ts` is located in the `web/src/interfaces/database` directory.

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
- [chat.ts](chat.ts_docs.md)
- [document.ts](document.ts_docs.md)
- [file-manager.ts](file-manager.ts_docs.md)
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
