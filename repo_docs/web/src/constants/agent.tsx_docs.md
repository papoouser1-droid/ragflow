# File Documentation: web/src/constants/agent.tsx

## File Metadata

- **Path**: `web/src/constants/agent.tsx`
- **Extension**: `.tsx`
- **Lines**: 172
- **Characters**: 4,511
- **Size**: 4,517 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/constants/agent.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 172 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (5)

- `CodeTemplateStrMap`: Exported entity
- `AgentGlobalsSysQueryWithBrace`: Exported entity
- `variableCheckBoxFieldMap`: Exported entity
- `initialLlmBaseValues`: Exported entity
- `SwitchOperatorOptions`: Exported entity

### Functions (2)

- `main()`: Function definition
- `variableCheckBoxFieldMap()`: Function definition

### Imports (3)

- `import { setInitialChatVariableEnabledFieldValue } from '@/utils/chat';`
- `import { Circle, CircleSlash2 } from 'lucide-react';`
- `import { ChatVariableEnabledField, variableEnabledFieldMap } from './chat';`

## Code Structure Analysis

- Total lines: 172
- Blank lines: 13 (7.6%)
- Comment lines: ~0 (0.0%)
- Code lines: ~159


## Dependencies and Imports

- `@/utils/chat`
- `lucide-react`
- `./chat`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/constants`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization
- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/constants/` directory
- Potential test file: `test_agent.tsx`

## Keywords

./chat, @/utils/chat, Agent, AgentCanvas, AgentCategory, AgentGlobals, AgentGlobalsSysQueryWithBrace, AgentQuery, AkShare, ArXiv, Begin, Bing, Body, Categorize, Category, ChatVariableEnabledField, Circle, CircleSlash2, Code, CodeExec, CodeTemplateStrMap, ComparisonOperator, Contains, Crawler, DataOperations, DataflowCanvas, DataflowOperator, DuckDuckGo, Email, Empty, EndWith, Equal, Error, ExeSQL, Extractor, File, GitHub, Google, GoogleScholar, GreatEqual, GreatThan, Greater, HierarchicalMerger, Invoke, Iteration, IterationItem, IterationStart, Javascript, Jin10, KeywordExtract...

---
*Generated by RAGFlow Repository Documentation Generator*
