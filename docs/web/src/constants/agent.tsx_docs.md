# Documentation: web/src/constants/agent.tsx

## File Metadata

- **Path**: `web/src/constants/agent.tsx`
- **Size**: 4517 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/constants/agent.tsx`.

## Original Source Code

```tsx
import { setInitialChatVariableEnabledFieldValue } from '@/utils/chat';
import { Circle, CircleSlash2 } from 'lucide-react';
import { ChatVariableEnabledField, variableEnabledFieldMap } from './chat';

export enum ProgrammingLanguage {
  Python = 'python',
  Javascript = 'javascript',
}

export const CodeTemplateStrMap = {
  [ProgrammingLanguage.Python]: `def main(arg1: str, arg2: str) -> str:
    return f"result: {arg1 + arg2}"
`,
  [ProgrammingLanguage.Javascript]: `const axios = require('axios');
async function main({}) {
  try {
    const response = await axios.get('https://github.com/infiniflow/ragflow');
    return 'Body:' + response.data;
  } catch (error) {
    return 'Error:' + error.message;
  }
}`,
};

export enum AgentGlobals {
  SysQuery = 'sys.query',
  SysUserId = 'sys.user_id',
  SysConversationTurns = 'sys.conversation_turns',
  SysFiles = 'sys.files',
}

export const AgentGlobalsSysQueryWithBrace = `{${AgentGlobals.SysQuery}}`;

export const variableCheckBoxFieldMap = Object.keys(
  variableEnabledFieldMap,
).reduce<Record<string, boolean>>((pre, cur) => {
  pre[cur] = setInitialChatVariableEnabledFieldValue(
    cur as ChatVariableEnabledField,
  );
  return pre;
}, {});

export const initialLlmBaseValues = {
  ...variableCheckBoxFieldMap,
  temperature: 0.1,
  top_p: 0.3,
  frequency_penalty: 0.7,
  presence_penalty: 0.4,
  max_tokens: 256,
};

export enum AgentCategory {
  AgentCanvas = 'agent_canvas',
  DataflowCanvas = 'dataflow_canvas',
}

export enum AgentQuery {
  Category = 'category',
}

export enum DataflowOperator {
  Begin = 'File',
  Note = 'Note',
  Parser = 'Parser',
  Tokenizer = 'Tokenizer',
  Splitter = 'Splitter',
  HierarchicalMerger = 'HierarchicalMerger',
  Extractor = 'Extractor',
}

export enum Operator {
  Begin = 'Begin',
  Retrieval = 'Retrieval',
  Categorize = 'Categorize',
  Message = 'Message',
  Relevant = 'Relevant',
  RewriteQuestion = 'RewriteQuestion',
  KeywordExtract = 'KeywordExtract',
  DuckDuckGo = 'DuckDuckGo',
  Wikipedia = 'Wikipedia',
  PubMed = 'PubMed',
  ArXiv = 'ArXiv',
  Google = 'Google',
  Bing = 'Bing',
  GoogleScholar = 'GoogleScholar',
  GitHub = 'GitHub',
  QWeather = 'QWeather',
  ExeSQL = 'ExeSQL',
  Switch = 'Switch',
  WenCai = 'WenCai',
  AkShare = 'AkShare',
  YahooFinance = 'YahooFinance',
  Jin10 = 'Jin10',
  TuShare = 'TuShare',
  Note = 'Note',
  Crawler = 'Crawler',
  Invoke = 'Invoke',
  Email = 'Email',
  Iteration = 'Iteration',
  IterationStart = 'IterationItem',
  Code = 'CodeExec',
  WaitingDialogue = 'WaitingDialogue',
  Agent = 'Agent',
  Tool = 'Tool',
  TavilySearch = 'TavilySearch',
  TavilyExtract = 'TavilyExtract',
  UserFillUp = 'UserFillUp',
  StringTransform = 'StringTransform',
  SearXNG = 'SearXNG',
  Placeholder = 'Placeholder',
  DataOperations = 'DataOperations',
  ListOperations = 'ListOperations',
  VariableAssigner = 'VariableAssigner',
  VariableAggregator = 'VariableAggregator',
  File = 'File', // pipeline
  Parser = 'Parser',
  Tokenizer = 'Tokenizer',
  Splitter = 'Splitter',
  HierarchicalMerger = 'HierarchicalMerger',
  Extractor = 'Extractor',
}

export enum ComparisonOperator {
  Equal = '=',
  NotEqual = '≠',
  GreatThan = '>',
  GreatEqual = '≥',
  LessThan = '<',
  LessEqual = '≤',
  Contains = 'contains',
  NotContains = 'not contains',
  StartWith = 'start with',
  EndWith = 'end with',
  Empty = 'empty',
  NotEmpty = 'not empty',
}

export const SwitchOperatorOptions = [
  { value: ComparisonOperator.Equal, label: 'equal', icon: 'equal' },
  { value: ComparisonOperator.NotEqual, label: 'notEqual', icon: 'not-equals' },
  { value: ComparisonOperator.GreatThan, label: 'gt', icon: 'Less' },
  {
    value: ComparisonOperator.GreatEqual,
    label: 'ge',
    icon: 'Greater-or-equal',
  },
  { value: ComparisonOperator.LessThan, label: 'lt', icon: 'Less' },
  { value: ComparisonOperator.LessEqual, label: 'le', icon: 'less-or-equal' },
  { value: ComparisonOperator.Contains, label: 'contains', icon: 'Contains' },
  {
    value: ComparisonOperator.NotContains,
    label: 'notContains',
    icon: 'not-contains',
  },
  {
    value: ComparisonOperator.StartWith,
    label: 'startWith',
    icon: 'list-start',
  },
  { value: ComparisonOperator.EndWith, label: 'endWith', icon: 'list-end' },
  {
    value: ComparisonOperator.Empty,
    label: 'empty',
    icon: <Circle className="size-4" />,
  },
  {
    value: ComparisonOperator.NotEmpty,
    label: 'notEmpty',
    icon: <CircleSlash2 className="size-4" />,
  },
];

```

## Detailed Analysis

### File Role in Repository

The file `web/src/constants/agent.tsx` is located in the `web/src/constants` directory.

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

- [authorization.ts](authorization.ts_docs.md)
- [chat.ts](chat.ts_docs.md)
- [common.ts](common.ts_docs.md)
- [file.ts](file.ts_docs.md)
- [form.ts](form.ts_docs.md)
- [knowledge.ts](knowledge.ts_docs.md)
- [llm.ts](llm.ts_docs.md)
- [permission.ts](permission.ts_docs.md)
- [setting.ts](setting.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
