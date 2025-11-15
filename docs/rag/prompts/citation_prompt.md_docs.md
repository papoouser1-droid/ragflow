# Documentation: rag/prompts/citation_prompt.md

## File Metadata

- **Path**: `rag/prompts/citation_prompt.md`
- **Size**: 5436 bytes
- **Type**: .md
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `rag/prompts/citation_prompt.md`.

## Original Source Code

```md
Based on the provided document or chat history, add citations to the input text using the format specified later. 

# Citation Requirements:

## Technical Rules:
- Use format: [ID:i] or [ID:i] [ID:j] for multiple sources
- Place citations at the end of sentences, before punctuation
- Maximum 4 citations per sentence
- DO NOT cite content not from <context></context>
- DO NOT modify whitespace or original text
- STRICTLY prohibit non-standard formatting (~~, etc.)

## What MUST Be Cited:
1. **Quantitative data**: Numbers, percentages, statistics, measurements
2. **Temporal claims**: Dates, timeframes, sequences of events  
3. **Causal relationships**: Claims about cause and effect
4. **Comparative statements**: Rankings, comparisons, superlatives
5. **Technical definitions**: Specialized terms, concepts, methodologies
6. **Direct attributions**: What someone said, did, or believes
7. **Predictions/forecasts**: Future projections, trend analyses
8. **Controversial claims**: Disputed facts, minority opinions

## What Should NOT Be Cited:
- Common knowledge (e.g., "The sun rises in the east")
- Transitional phrases
- General introductions
- Your own analysis or synthesis (unless directly from source)

# Comprehensive Examples:

## Example 1: Data and Statistics
<context>
ID: 45
└── Content: The global smartphone market grew by 7.8% in Q3 2024, with Samsung holding 20.6% market share and Apple at 15.8%.

ID: 46  
└── Content: 5G adoption reached 1.5 billion users worldwide by October 2024.
</context>

USER: How is the smartphone market performing?

ASSISTANT: 
The smartphone industry is showing strong recovery. The global smartphone market grew by 7.8% in Q3 2024 [ID:45]. Samsung leads with 20.6% market share, followed by Apple at 15.8% [ID:45]. This growth is partly driven by 5G technology, which reached 1.5 billion users worldwide by October 2024 [ID:46].

## Example 2: Causal Relationships and Predictions
<context>
ID: 78
└── Content: Rising interest rates have led to a 15% decline in housing starts. Economists predict this trend will continue through 2025.

ID: 79
└── Content: The Federal Reserve's aggressive monetary policy is the primary driver behind the real estate slowdown.
</context>

USER: What's happening in the real estate market?

ASSISTANT:
The real estate market is experiencing significant challenges. Rising interest rates have led to a 15% decline in housing starts [ID:78]. The Federal Reserve's aggressive monetary policy is the primary driver behind this real estate slowdown [ID:79]. Economists predict this trend will continue through 2025 [ID:78]. These factors suggest a prolonged period of market adjustment ahead.

## Example 3: Technical Concepts and Comparisons
<context>
ID: 101
└── Content: Quantum computing uses quantum bits (qubits) that can exist in superposition, unlike classical bits which are either 0 or 1.

ID: 102
└── Content: Current quantum computers can solve certain problems 158 million times faster than classical supercomputers.
</context>

USER: Explain quantum computing advantages.

ASSISTANT:
Quantum computing represents a fundamental shift in computational capability. Unlike classical computers, quantum computing uses quantum bits (qubits) that can exist in superposition, rather than being limited to just 0 or 1 [ID:101]. This unique property enables extraordinary performance gains. Current quantum computers can solve certain problems 158 million times faster than classical supercomputers [ID:102]. This technology promises to revolutionize fields requiring complex calculations.

## Example 4: Mixed Citation Needs
<context>
ID: 201
└── Content: CEO Jane Smith announced: "We're pivoting to AI-first strategy, expecting 40% revenue growth by 2026."

ID: 202
└── Content: Industry analysts remain skeptical, with Morgan Stanley rating the stock as "underperform."

ID: 203
└── Content: The company's AI division currently represents only 5% of total revenue.
</context>

USER: What's the company's new strategy?

ASSISTANT:
The company is undergoing a major strategic transformation. CEO Jane Smith announced: "We're pivoting to AI-first strategy, expecting 40% revenue growth by 2026" [ID:201]. However, this ambitious goal faces skepticism, as industry analysts remain doubtful, with Morgan Stanley rating the stock as "underperform" [ID:202]. The challenge is significant given that the company's AI division currently represents only 5% of total revenue [ID:203]. Success will depend on execution of this bold vision.

## Example 5: When NOT to Cite
<context>
ID: 301
└── Content: Paris is the capital of France. The Eiffel Tower was completed in 1889.
</context>

USER: Tell me about Paris.

ASSISTANT:
Paris is the capital of France. It's known for its rich history, culture, and architecture. The Eiffel Tower was completed in 1889 [ID:301]. The city attracts millions of tourists annually. Paris remains one of the world's most visited destinations.
(Note: Only the specific date needs citation, not common knowledge about Paris)

--- Examples END ---

REMEMBER: 
- Cite FACTS, not opinions or transitions
- Each citation supports the ENTIRE sentence
- When in doubt, ask: "Would a fact-checker need to verify this?"
- Place citations at sentence end, before punctuation
- Format likes this is FORBIDDEN: [ID:0, ID:5, ID:...]. It MUST be seperated like, [ID:0][ID:5]...

```

## Detailed Analysis

### File Role in Repository

The file `rag/prompts/citation_prompt.md` is located in the `rag/prompts` directory.

This file is part of the **RAG (Retrieval-Augmented Generation)** core engine.

### Architecture Context

Files in this location typically handle concerns related to prompts.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [__init__.py](__init__.py_docs.md)
- [analyze_task_system.md](analyze_task_system.md_docs.md)
- [analyze_task_user.md](analyze_task_user.md_docs.md)
- [ask_summary.md](ask_summary.md_docs.md)
- [assign_toc_levels.md](assign_toc_levels.md_docs.md)
- [citation_plus.md](citation_plus.md_docs.md)
- [content_tagging_prompt.md](content_tagging_prompt.md_docs.md)
- [cross_languages_sys_prompt.md](cross_languages_sys_prompt.md_docs.md)
- [cross_languages_user_prompt.md](cross_languages_user_prompt.md_docs.md)
- [full_question_prompt.md](full_question_prompt.md_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
