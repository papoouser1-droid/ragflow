# Documentation: graphrag/entity_resolution_prompt.py

## File Metadata

- **Path**: `graphrag/entity_resolution_prompt.py`
- **Size**: 4459 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `graphrag/entity_resolution_prompt.py`.

## Python Module Overview

## Original Source Code

```py
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

ENTITY_RESOLUTION_PROMPT = """
-Goal-
Please answer the following Question as required

-Steps-
1. Identify each line of questioning as required

2. Return output in English as a single list of each line answer in steps 1. Use **{record_delimiter}** as the list delimiter.

######################
-Examples-
######################
Example 1:

Question:
When determining whether two Products are the same, you should only focus on critical properties and overlook noisy factors. 

Demonstration 1: name of Product A is : "computer", name of Product B is :"phone"  No, Product A and Product B are different products.
Question 1: name of Product A is : "television", name of Product B is :"TV"  
Question 2: name of Product A is : "cup", name of Product B is :"mug"  
Question 3: name of Product A is : "soccer", name of Product B is :"football"  
Question 4: name of Product A is : "pen", name of Product B is  :"eraser"  

Use domain knowledge of Products to help understand the text and answer the above 4 questions in the format: For Question i, Yes, Product A and Product B are the same product. or  No, Product A and Product B are different products. For Question i+1, (repeat the above procedures)
################
Output:
(For question {entity_index_delimiter}1{entity_index_delimiter}, {resolution_result_delimiter}no{resolution_result_delimiter}, Product A and Product B are different products.){record_delimiter}
(For question {entity_index_delimiter}2{entity_index_delimiter}, {resolution_result_delimiter}no{resolution_result_delimiter}, Product A and Product B are different products.){record_delimiter}
(For question {entity_index_delimiter}3{entity_index_delimiter}, {resolution_result_delimiter}yes{resolution_result_delimiter}, Product A and Product B are the same product.){record_delimiter}
(For question {entity_index_delimiter}4{entity_index_delimiter}, {resolution_result_delimiter}no{resolution_result_delimiter}, Product A and Product B are different products.){record_delimiter}
#############################

Example 2:

Question:
When determining whether two toponym are the same, you should only focus on critical properties and overlook noisy factors. 

Demonstration 1: name of toponym A is : "nanjing", name of toponym B is :"nanjing city"  No, toponym A and toponym B are same toponym.
Question 1: name of toponym A is : "Chicago", name of toponym B is :"ChiTown"  
Question 2: name of toponym A is : "Shanghai", name of toponym B is :"Zhengzhou"  
Question 3: name of toponym A is : "Beijing", name of toponym B is :"Peking"
Question 4: name of toponym A is : "Los Angeles", name of toponym B is :"Cleveland" 

Use domain knowledge of toponym to help understand the text and answer the above 4 questions in the format: For Question i, Yes, toponym A and toponym B are the same toponym. or  No, toponym A and toponym B are different toponym. For Question i+1, (repeat the above procedures)
################
Output:
(For question {entity_index_delimiter}1{entity_index_delimiter}, {resolution_result_delimiter}yes{resolution_result_delimiter}, toponym A and toponym B are same toponym.){record_delimiter}
(For question {entity_index_delimiter}2{entity_index_delimiter}, {resolution_result_delimiter}no{resolution_result_delimiter}, toponym A and toponym B are different toponym.){record_delimiter}
(For question {entity_index_delimiter}3{entity_index_delimiter}, {resolution_result_delimiter}yes{resolution_result_delimiter}, toponym A and toponym B are the same toponym.){record_delimiter}
(For question {entity_index_delimiter}4{entity_index_delimiter}, {resolution_result_delimiter}no{resolution_result_delimiter}, toponym A and toponym B are different toponym.){record_delimiter}
#############################

-Real Data-
######################
Question:{input_text}
######################
Output:
"""

```

## Detailed Analysis

### File Role in Repository

The file `graphrag/entity_resolution_prompt.py` is located in the `graphrag` directory.

This file is part of the **Graph RAG** knowledge graph system.

### Architecture Context

Files in this location typically handle concerns related to graphrag.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

- Ensure all user inputs are validated
- Check for SQL injection vulnerabilities
- Verify authentication and authorization

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [__init__.py](__init__.py_docs.md)
- [entity_resolution.py](entity_resolution.py_docs.md)
- [query_analyze_prompt.py](query_analyze_prompt.py_docs.md)
- [search.py](search.py_docs.md)
- [utils.py](utils.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
