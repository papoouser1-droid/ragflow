# Documentation: graphrag/query_analyze_prompt.py

## File Metadata

- **Path**: `graphrag/query_analyze_prompt.py`
- **Size**: 7933 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `graphrag/query_analyze_prompt.py`.

## Python Module Overview

### Module Docstring

```
Reference:
 - [LightRag](https://github.com/HKUDS/LightRAG)
 - [MiniRAG](https://github.com/HKUDS/MiniRAG)
```

## Original Source Code

```py
# Licensed under the MIT License
"""
Reference:
 - [LightRag](https://github.com/HKUDS/LightRAG)
 - [MiniRAG](https://github.com/HKUDS/MiniRAG)
"""
PROMPTS = {}

PROMPTS["minirag_query2kwd"] = """---Role---

You are a helpful assistant tasked with identifying both answer-type and low-level keywords in the user's query.

---Goal---

Given the query, list both answer-type and low-level keywords.
answer_type_keywords focus on the type of the answer to the certain query, while low-level keywords focus on specific entities, details, or concrete terms.
The answer_type_keywords must be selected from Answer type pool. 
This pool is in the form of a dictionary, where the key represents the Type you should choose from and the value represents the example samples.

---Instructions---

- Output the keywords in JSON format.
- The JSON should have three keys:
  - "answer_type_keywords" for the types of the answer. In this list, the types with the highest likelihood should be placed at the forefront. No more than 3.
  - "entities_from_query" for specific entities or details. It must be extracted from the query.
######################
-Examples-
######################
Example 1:

Query: "How does international trade influence global economic stability?"
Answer type pool: {{
 'PERSONAL LIFE': ['FAMILY TIME', 'HOME MAINTENANCE'],
 'STRATEGY': ['MARKETING PLAN', 'BUSINESS EXPANSION'],
 'SERVICE FACILITATION': ['ONLINE SUPPORT', 'CUSTOMER SERVICE TRAINING'],
 'PERSON': ['JANE DOE', 'JOHN SMITH'],
 'FOOD': ['PASTA', 'SUSHI'],
 'EMOTION': ['HAPPINESS', 'ANGER'],
 'PERSONAL EXPERIENCE': ['TRAVEL ABROAD', 'STUDYING ABROAD'],
 'INTERACTION': ['TEAM MEETING', 'NETWORKING EVENT'],
 'BEVERAGE': ['COFFEE', 'TEA'],
 'PLAN': ['ANNUAL BUDGET', 'PROJECT TIMELINE'],
 'GEO': ['NEW YORK CITY', 'SOUTH AFRICA'],
 'GEAR': ['CAMPING TENT', 'CYCLING HELMET'],
 'EMOJI': ['🎉', '🚀'],
 'BEHAVIOR': ['POSITIVE FEEDBACK', 'NEGATIVE CRITICISM'],
 'TONE': ['FORMAL', 'INFORMAL'],
 'LOCATION': ['DOWNTOWN', 'SUBURBS']
}}
################
Output:
{{
  "answer_type_keywords": ["STRATEGY","PERSONAL LIFE"],
  "entities_from_query": ["Trade agreements", "Tariffs", "Currency exchange", "Imports", "Exports"]
}}
#############################
Example 2:

Query: "When was SpaceX's first rocket launch?"
Answer type pool: {{
 'DATE AND TIME': ['2023-10-10 10:00', 'THIS AFTERNOON'],
 'ORGANIZATION': ['GLOBAL INITIATIVES CORPORATION', 'LOCAL COMMUNITY CENTER'],
 'PERSONAL LIFE': ['DAILY EXERCISE ROUTINE', 'FAMILY VACATION PLANNING'],
 'STRATEGY': ['NEW PRODUCT LAUNCH', 'YEAR-END SALES BOOST'],
 'SERVICE FACILITATION': ['REMOTE IT SUPPORT', 'ON-SITE TRAINING SESSIONS'],
 'PERSON': ['ALEXANDER HAMILTON', 'MARIA CURIE'],
 'FOOD': ['GRILLED SALMON', 'VEGETARIAN BURRITO'],
 'EMOTION': ['EXCITEMENT', 'DISAPPOINTMENT'],
 'PERSONAL EXPERIENCE': ['BIRTHDAY CELEBRATION', 'FIRST MARATHON'],
 'INTERACTION': ['OFFICE WATER COOLER CHAT', 'ONLINE FORUM DEBATE'],
 'BEVERAGE': ['ICED COFFEE', 'GREEN SMOOTHIE'],
 'PLAN': ['WEEKLY MEETING SCHEDULE', 'MONTHLY BUDGET OVERVIEW'],
 'GEO': ['MOUNT EVEREST BASE CAMP', 'THE GREAT BARRIER REEF'],
 'GEAR': ['PROFESSIONAL CAMERA EQUIPMENT', 'OUTDOOR HIKING GEAR'],
 'EMOJI': ['📅', '⏰'],
 'BEHAVIOR': ['PUNCTUALITY', 'HONESTY'],
 'TONE': ['CONFIDENTIAL', 'SATIRICAL'],
 'LOCATION': ['CENTRAL PARK', 'DOWNTOWN LIBRARY']
}}

################
Output:
{{
  "answer_type_keywords": ["DATE AND TIME", "ORGANIZATION", "PLAN"],
  "entities_from_query": ["SpaceX", "Rocket launch", "Aerospace", "Power Recovery"]

}}
#############################
Example 3:

Query: "What is the role of education in reducing poverty?"
Answer type pool: {{
 'PERSONAL LIFE': ['MANAGING WORK-LIFE BALANCE', 'HOME IMPROVEMENT PROJECTS'],
 'STRATEGY': ['MARKETING STRATEGIES FOR Q4', 'EXPANDING INTO NEW MARKETS'],
 'SERVICE FACILITATION': ['CUSTOMER SATISFACTION SURVEYS', 'STAFF RETENTION PROGRAMS'],
 'PERSON': ['ALBERT EINSTEIN', 'MARIA CALLAS'],
 'FOOD': ['PAN-FRIED STEAK', 'POACHED EGGS'],
 'EMOTION': ['OVERWHELM', 'CONTENTMENT'],
 'PERSONAL EXPERIENCE': ['LIVING ABROAD', 'STARTING A NEW JOB'],
 'INTERACTION': ['SOCIAL MEDIA ENGAGEMENT', 'PUBLIC SPEAKING'],
 'BEVERAGE': ['CAPPUCCINO', 'MATCHA LATTE'],
 'PLAN': ['ANNUAL FITNESS GOALS', 'QUARTERLY BUSINESS REVIEW'],
 'GEO': ['THE AMAZON RAINFOREST', 'THE GRAND CANYON'],
 'GEAR': ['SURFING ESSENTIALS', 'CYCLING ACCESSORIES'],
 'EMOJI': ['💻', '📱'],
 'BEHAVIOR': ['TEAMWORK', 'LEADERSHIP'],
 'TONE': ['FORMAL MEETING', 'CASUAL CONVERSATION'],
 'LOCATION': ['URBAN CITY CENTER', 'RURAL COUNTRYSIDE']
}}

################
Output:
{{
  "answer_type_keywords": ["STRATEGY", "PERSON"],
  "entities_from_query": ["School access", "Literacy rates", "Job training", "Income inequality"]
}}
#############################
Example 4:

Query: "Where is the capital of the United States?"
Answer type pool: {{
 'ORGANIZATION': ['GREENPEACE', 'RED CROSS'],
 'PERSONAL LIFE': ['DAILY WORKOUT', 'HOME COOKING'],
 'STRATEGY': ['FINANCIAL INVESTMENT', 'BUSINESS EXPANSION'],
 'SERVICE FACILITATION': ['ONLINE SUPPORT', 'CUSTOMER SERVICE TRAINING'],
 'PERSON': ['ALBERTA SMITH', 'BENJAMIN JONES'],
 'FOOD': ['PASTA CARBONARA', 'SUSHI PLATTER'],
 'EMOTION': ['HAPPINESS', 'SADNESS'],
 'PERSONAL EXPERIENCE': ['TRAVEL ADVENTURE', 'BOOK CLUB'],
 'INTERACTION': ['TEAM BUILDING', 'NETWORKING MEETUP'],
 'BEVERAGE': ['LATTE', 'GREEN TEA'],
 'PLAN': ['WEIGHT LOSS', 'CAREER DEVELOPMENT'],
 'GEO': ['PARIS', 'NEW YORK'],
 'GEAR': ['CAMERA', 'HEADPHONES'],
 'EMOJI': ['🏢', '🌍'],
 'BEHAVIOR': ['POSITIVE THINKING', 'STRESS MANAGEMENT'],
 'TONE': ['FRIENDLY', 'PROFESSIONAL'],
 'LOCATION': ['DOWNTOWN', 'SUBURBS']
}}
################
Output:
{{
  "answer_type_keywords": ["LOCATION"],
  "entities_from_query": ["capital of the United States", "Washington", "New York"]
}}
#############################

-Real Data-
######################
Query: {query}
Answer type pool:{TYPE_POOL}
######################
Output:

"""

PROMPTS["keywords_extraction"] = """---Role---

You are a helpful assistant tasked with identifying both high-level and low-level keywords in the user's query.

---Goal---

Given the query, list both high-level and low-level keywords. High-level keywords focus on overarching concepts or themes, while low-level keywords focus on specific entities, details, or concrete terms.

---Instructions---

- Output the keywords in JSON format.
- The JSON should have two keys:
  - "high_level_keywords" for overarching concepts or themes.
  - "low_level_keywords" for specific entities or details.

######################
-Examples-
######################
{examples}

#############################
-Real Data-
######################
Query: {query}
######################
The `Output` should be human text, not unicode characters. Keep the same language as `Query`.
Output:

"""

PROMPTS["keywords_extraction_examples"] = [
    """Example 1:

Query: "How does international trade influence global economic stability?"
################
Output:
{
  "high_level_keywords": ["International trade", "Global economic stability", "Economic impact"],
  "low_level_keywords": ["Trade agreements", "Tariffs", "Currency exchange", "Imports", "Exports"]
}
#############################""",
    """Example 2:

Query: "What are the environmental consequences of deforestation on biodiversity?"
################
Output:
{
  "high_level_keywords": ["Environmental consequences", "Deforestation", "Biodiversity loss"],
  "low_level_keywords": ["Species extinction", "Habitat destruction", "Carbon emissions", "Rainforest", "Ecosystem"]
}
#############################""",
    """Example 3:

Query: "What is the role of education in reducing poverty?"
################
Output:
{
  "high_level_keywords": ["Education", "Poverty reduction", "Socioeconomic development"],
  "low_level_keywords": ["School access", "Literacy rates", "Job training", "Income inequality"]
}
#############################""",
]

```

## Detailed Analysis

### File Role in Repository

The file `graphrag/query_analyze_prompt.py` is located in the `graphrag` directory.

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
- [entity_resolution_prompt.py](entity_resolution_prompt.py_docs.md)
- [search.py](search.py_docs.md)
- [utils.py](utils.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
