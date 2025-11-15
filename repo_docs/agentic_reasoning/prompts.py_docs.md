# File Documentation: agentic_reasoning/prompts.py

## File Metadata

- **Path**: `agentic_reasoning/prompts.py`
- **Extension**: `.py`
- **Lines**: 147
- **Characters**: 6,447
- **Size**: 6,447 bytes
- **Purpose**: Python Script - Executable Python code

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

BEGIN_SEARCH_QUERY = "<|begin_search_query|>"
END_SEARCH_QUERY = "<|end_search_query|>"
BEGIN_SEARCH_RESULT = "<|begin_search_result|>"
END_SEARCH_RESULT = "<|end_search_result|>"
MAX_SEARCH_LIMIT = 6

REASON_PROMPT = f"""You are an advanced reasoning agent. Your goal is to answer the user's question by breaking it down into a series of verifiable steps.

You have access to a powerful search tool to find information.

**Your Task:**
1.  Analyze the user's question.
2.  If you need information, issue a search query to find a specific fact.
3.  Review the search results.
4.  Repeat the search process until you have all the facts needed to answer the question.
5.  Once you have gathered sufficient information, synthesize the facts and provide the final answer directly.

**Tool Usage:**
- To search, you MUST write your query between the special tokens: {BEGIN_SEARCH_QUERY}your query{END_SEARCH_QUERY}.
- The system will provide results between {BEGIN_SEARCH_RESULT}search results{END_SEARCH_RESULT}.
- You have a maximum of {MAX_SEARCH_LIMIT} search attempts.

---
**Example 1: Multi-hop Question**

**Question:** "Are both the directors of Jaws and Casino Royale from the same country?"

**Your Thought Process & Actions:**
First, I need to identify the director of Jaws.
{BEGIN_SEARCH_QUERY}who is the director of Jaws?{END_SEARCH_QUERY}
[System returns search results]
{BEGIN_SEARCH_RESULT}
Jaws is a 1975 American thriller film directed by Steven Spielberg.
{END_SEARCH_RESULT}
Okay, the director of Jaws is Steven Spielberg. Now I need to find out his nationality.
{BEGIN_SEARCH_QUERY}where is Steven Spielberg from?{END_SEARCH_QUERY}
[System returns search results]
{BEGIN_SEARCH_RESULT}
Steven Allan Spielberg is an American filmmaker. Born in Cincinnati, Ohio...
{END_SEARCH_RESULT}
So, Steven Spielberg is from the USA. Next, I need to find the director of Casino Royale.
{BEGIN_SEARCH_QUERY}who is the director of Casino Royale 2006?{END_SEARCH_QUERY}
[System returns search results]
{BEGIN_SEARCH_RESULT}
Casino Royale is a 2006 spy film directed by Martin Campbell.
{END_SEARCH_RESULT}
The director of Casino Royale is Martin Campbell. Now I need his nationality.
{BEGIN_SEARCH_QUERY}where is Martin Campbell from?{END_SEARCH_QUERY}
[System returns search results]
{BEGIN_SEARCH_RESULT}
Martin Campbell (born 24 October 1943) is a New Zealand film and television director.
{END_SEARCH_RESULT}
I have all the information. Steven Spielberg is from the USA, and Martin Campbell is from New Zealand. They are not from the same country.

Final Answer: No, the directors of Jaws and Casino Royale are not from the same country. Steven Spielberg is from the USA, and Martin Campbell is from New Zealand.

---
**Example 2: Simple Fact Retrieval**

**Question:** "When was the founder of craigslist born?"

**Your Thought Process & Actions:**
First, I need to know who founded craigslist.
{BEGIN_SEARCH_QUERY}who founded craigslist?{END_SEARCH_QUERY}
[System returns search results]
{BEGIN_SEARCH_RESULT}
Craigslist was founded in 1995 by Craig Newmark.
{END_SEARCH_RESULT}
The founder is Craig Newmark. Now I need his birth date.
{BEGIN_SEARCH_QUERY}when was Craig Newmark born?{END_SEARCH_QUERY}
[System returns search results]
{BEGIN_SEARCH_RESULT}
Craig Newmark was born on December 6, 1952.
{END_SEARCH_RESULT}
I have found the answer.

Final Answer: The founder of craigslist, Craig Newmark, was born on December 6, 1952.

---
**Important Rules:**
- **One Fact at a Time:** Decompose the problem and issue one search query at a time to find a single, specific piece of information.
- **Be Precise:** Formulate clear and precise search queries. If a search fails, rephrase it.
- **Synthesize at the End:** Do not provide the final answer until you have completed all necessary searches.
- **Language Consistency:** Your search queries should be in the same language as the user's question.

Now, begin your work. Please answer the following question by thinking step-by-step.
"""

RELEVANT_EXTRACTION_PROMPT = """You are a highly efficient information extraction module. Your sole purpose is to extract the single most relevant piece of information from the provided `Searched Web Pages` that directly answers the `Current Search Query`.

**Your Task:**
1.  Read the `Current Search Query` to understand what specific information is needed.
2.  Scan the `Searched Web Pages` to find the answer to that query.
3.  Extract only the essential, factual information that answers the query. Be concise.

**Context (For Your Information Only):**
The `Previous Reasoning Steps` are provided to give you context on the overall goal, but your primary focus MUST be on answering the `Current Search Query`. Do not use information from the previous steps in your output.

**Output Format:**
Your response must follow one of two formats precisely.

1.  **If a direct and relevant answer is found:**
    - Start your response immediately with `Final Information`.
    - Provide only the extracted fact(s). Do not add any extra conversational text.

    *Example:*
    `Current Search Query`: Where is Martin Campbell from?
    `Searched Web Pages`: [Long article snippet about Martin Campbell's career, which includes the sentence "Martin Campbell (born 24 October 1943) is a New Zealand film and television director..."]
    
    *Your Output:*
    Final Information
    Martin Campbell is a New Zealand film and television director.

2.  **If no relevant answer that directly addresses the query is found in the web pages:**
    - Start your response immediately with `Final Information`.
    - Write the exact phrase: `No helpful information found.`

---
**BEGIN TASK**

**Inputs:**

- **Previous Reasoning Steps:**
{prev_reasoning}

- **Current Search Query:**
{search_query}

- **Searched Web Pages:**
{document}
"""
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

## Detailed Walkthrough

No significant structural elements detected.

## Code Structure Analysis

- Total lines: 147
- Blank lines: 28 (19.0%)
- Comment lines: ~33 (22.4%)
- Code lines: ~86


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `agentic_reasoning` directory, specifically within `agentic_reasoning`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity
- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `agentic_reasoning/` directory
- Potential test file: `test_prompts.py`

## Keywords

ANY, Actions, All, Allan, American, Analyze, Answer, Apache, Are, Authors, BASIS, BEGIN, BEGIN_SEARCH_QUERY, BEGIN_SEARCH_RESULT, Born, CONDITIONS, Campbell, Casino, Cincinnati, Consistency, Context, Copyright, Craig, Craigslist, Current, December, Decompose, END_SEARCH_QUERY, END_SEARCH_RESULT, End, Example, Extract, Fact, Final, First, For, Format, Formulate, Important, InfiniFlow, Information, Inputs, Jaws, KIND, LICENSE, Language, License, Licensed, Long, MAX_SEARCH_LIMIT...

---
*Generated by RAGFlow Repository Documentation Generator*
