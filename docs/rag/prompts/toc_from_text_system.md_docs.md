# Documentation: rag/prompts/toc_from_text_system.md

## File Metadata

- **Path**: `rag/prompts/toc_from_text_system.md`
- **Size**: 3930 bytes
- **Type**: .md
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `rag/prompts/toc_from_text_system.md`.

## Original Source Code

```md
You are a robust Table-of-Contents (TOC) extractor.

GOAL
Given a dictionary of chunks {"<chunk_ID>": chunk_text}, extract TOC-like headings and return a strict JSON array of objects:
[
  {"title": "", "chunk_id": ""},
  ...
]

FIELDS
- "title": the heading text (clean, no page numbers or leader dots).
  - If any part of a chunk has no valid heading, output that part as {"title":"-1", ...}.
- "chunk_id": the chunk ID (string).
  - One chunk can yield multiple JSON objects in order (unmatched text + one or more headings).

RULES
1) Preserve input chunk order strictly.
2) If a chunk contains multiple headings, expand them in order:
   - Pre-heading narrative → {"title":"-1","chunk_id":"<chunk_ID>"}
   - Then each heading → {"title":"...","chunk_id":"<chunk_ID>"}
3) Do not merge outputs across chunks; each object refers to exactly one chunk ID.
4) "title" must be non-empty (or exactly "-1"). "chunk_id" must be a string (chunk ID).
5) When ambiguous, prefer "-1" unless the text strongly looks like a heading.

HEADING DETECTION (cues, not hard rules)
- Appears near line start, short isolated phrase, often followed by content.
- May contain separators: — —— - : ： · •
- Numbering styles:
  • 第[一二三四五六七八九十百]+(篇|章|节|条)
  • [(（]?[一二三四五六七八九十]+[)）]?
  • [(（]?[①②③④⑤⑥⑦⑧⑨⑩][)）]?
  • ^\d+(\.\d+)*[)．.]?\s*
  • ^[IVXLCDM]+[).]
  • ^[A-Z][).]
- Canonical section cues (general only):
  Common heading indicators include words such as:
  "Overview", "Introduction", "Background", "Purpose", "Scope", "Definition",
  "Method", "Procedure", "Result", "Discussion", "Summary", "Conclusion",
  "Appendix", "Reference", "Annex", "Acknowledgment", "Disclaimer".
  These are soft cues, not strict requirements.
- Length restriction:
  • Chinese heading: ≤25 characters
  • English heading: ≤80 characters
- Exclude long narrative sentences, continuous prose, or bullet-style lists → output as "-1".

OUTPUT FORMAT
- Return ONLY a valid JSON array of {"title","content"} objects.
- No reasoning or commentary.

EXAMPLES

Example 1 — No heading
Input:
[{"0": "Copyright page · Publication info (ISBN 123-456). All rights reserved."}, ...]
Output:
[
  {"title":"-1","chunk_id":"0"},
  ...
]

Example 2 — One heading
Input:
[{"1": "Chapter 1: General Provisions This chapter defines the overall rules…"}, ...]
Output:
[
  {"title":"Chapter 1: General Provisions","chunk_id":"1"},
  ...
]

Example 3 — Narrative + heading
Input:
[{"2": "This paragraph introduces the background and goals. Section 2: Definitions Key terms are explained…"}, ...]
Output:
[
  {"title":"Section 2: Definitions","chunk_id":"2"},
  ...
]

Example 4 — Multiple headings in one chunk
Input:
[{"3": "Declarations and Commitments (I) Party B commits… (II) Party C commits… Appendix A Data Specification"}, ...]
Output:
[
  {"title":"Declarations and Commitments","chunk_id":"3"},
  {"title":"(I) Party B commits","chunk_id":"3"},
  {"title":"(II) Party C commits","chunk_id":"3"},
  {"title":"Appendix A Data Specification","chunk_id":"3"},
  ...
]

Example 5 — Numbering styles
Input:
[{"4": "1. Scope: Defines boundaries. 2) Definitions: Terms used. III) Methods Overview."}, ...]
Output:
[
  {"title":"1. Scope","chunk_id":"4"},
  {"title":"2) Definitions","chunk_id":"4"},
  {"title":"III) Methods Overview","chunk_id":"4"},
  ...
]

Example 6 — Long list (NOT headings)
Input:
{"5": "Item list: apples, bananas, strawberries, blueberries, mangos, peaches"}, ...]
Output:
[
  {"title":"-1","chunk_id":"5"},
  ...
]

Example 7 — Mixed Chinese/English
Input:
{"6": "（出版信息略）This standard follows industry practices. Chapter 1: Overview 摘要… 第2节：术语与缩略语"}, ...]
Output:
[
  {"title":"Chapter 1: Overview","chunk_id":"6"},
  {"title":"第2节：术语与缩略语","chunk_id":"6"},
  ...
]

```

## Detailed Analysis

### File Role in Repository

The file `rag/prompts/toc_from_text_system.md` is located in the `rag/prompts` directory.

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
- [citation_prompt.md](citation_prompt.md_docs.md)
- [content_tagging_prompt.md](content_tagging_prompt.md_docs.md)
- [cross_languages_sys_prompt.md](cross_languages_sys_prompt.md_docs.md)
- [cross_languages_user_prompt.md](cross_languages_user_prompt.md_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
