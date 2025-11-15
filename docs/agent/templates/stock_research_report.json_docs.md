# Documentation: agent/templates/stock_research_report.json

## File Metadata

- **Path**: `agent/templates/stock_research_report.json`
- **Size**: 88467 bytes
- **Type**: .json
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/templates/stock_research_report.json`.

## Original Source Code

```json
{
    "id": 26,
    "title": {
        "en": "Stock Research Report Agent",
        "de": "Aktienanalyse Agent",
        "zh": "股票研究报告智能体"
    },
    "description": {
        "en": "This template helps financial analysts quickly organize information — it can automatically retrieve company data, consolidate financial metrics, and integrate research report insights.",
        "de": "Diese Vorlage hilft Finanzanalysten, Informationen schnell zu organisieren – der Agent kann automatisch Unternehmensdaten abrufen, Finanzkennzahlen konsolidieren und Forschungsberichte integrieren.",
        "zh": "这个模板可以帮助金融分析师快速整理信息——它能够自动获取公司数据、整合财务指标，并汇总研报观点。"
    },
    "canvas_type": "Recommended",
    "dsl": {
            "components": {
                "Agent:ManyToesBrush": {
                    "downstream": [
                        "Switch:FluffyCoinsSell"
                    ],
                    "obj": {
                        "component_name": "Agent",
                        "params": {
                            "cite": true,
                            "delay_after_error": 1,
                            "description": "",
                            "exception_default_value": "",
                            "exception_goto": [],
                            "exception_method": "",
                            "frequencyPenaltyEnabled": false,
                            "frequency_penalty": 0.7,
                            "llm_id": "qwen-turbo-latest@Tongyi-Qianwen",
                            "maxTokensEnabled": false,
                            "max_retries": 3,
                            "max_rounds": 1,
                            "max_tokens": 256,
                            "mcp": [],
                            "message_history_window_size": 12,
                            "outputs": {
                                "content": {
                                    "type": "string",
                                    "value": ""
                                }
                            },
                            "presencePenaltyEnabled": false,
                            "presence_penalty": 0.4,
                            "prompts": [
                                {
                                    "content": "{sys.query}",
                                    "role": "user"
                                }
                            ],
                            "sys_prompt": "<role> \n\nYour responsibility is to identify and extract the stock name or abbreviation from the user's natural language query and return the corresponding unique stock code.\n\n </role> \n\n\n\n<rules> \n\n1. Only one result is allowed: - If a stock is identified \u2192 only return the corresponding stock code; - If no stock is identified \u2192 only return \u201cNot Found\u201d. 2. **Do not** output any additional text, punctuation, explanation, prefixes, or line breaks. 3. The output must strictly adhere to the <response_format>. \n\n</rules>\n\n\n<response_format>\nOnly output the stock code (e.g., AAPL or 600519)\nOr only output \u201cNot Found\u201d\n</response_format>\n\n\n<response_examples>\nUser input: \u201cHelp me check the research report of Apple\u201d \u2192 Output: AAPL\nUser input: \u201cHow is Maotai\u2019s financial performance\u201d \u2192 Output: 600519\nUser input: \u201cHow is the Shanghai Composite Index doing today\u201d \u2192 Output: Not Found\n</response_examples>\n\n\n<tools> - Tavily Search: Use this tool when you are unsure of the stock code. - If you are confident, you do not need to use the tool. \n\n</tools>\n\n\n<strict_output_requirements>\nOnly output the result, no explanations, hints, or notes allowed.\nThe output can only be the stock code or \u201cNot Found\u201d, otherwise, it is considered an incorrect answer.\n</strict_output_requirements>",
                            "temperature": 0.1,
                            "temperatureEnabled": false,
                            "tools": [
                                {
                                    "component_name": "TavilySearch",
                                    "name": "TavilySearch",
                                    "params": {
                                        "api_key": "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",
                                        "days": 7,
                                        "exclude_domains": [],
                                        "include_answer": false,
                                        "include_domains": [],
                                        "include_image_descriptions": false,
                                        "include_images": false,
                                        "include_raw_content": true,
                                        "max_results": 5,
                                        "outputs": {
                                            "formalized_content": {
                                                "type": "string",
                                                "value": ""
                                            },
                                            "json": {
                                                "type": "Array<Object>",
                                                "value": []
                                            }
                                        },
                                        "query": "sys.query",
                                        "search_depth": "basic",
                                        "topic": "general"
                                    }
                                }
                            ],
                            "topPEnabled": false,
                            "top_p": 0.3,
                            "user_prompt": "",
                            "visual_files_var": ""
                        }
                    },
                    "upstream": [
                        "begin"
                    ]
                },
                "Agent:SadDodosRescue": {
                    "downstream": [
                        "Agent:SharpSlothsSlide"
                    ],
                    "obj": {
                        "component_name": "Agent",
                        "params": {
                            "cite": true,
                            "delay_after_error": 1,
                            "description": "",
                            "exception_default_value": "",
                            "exception_goto": [],
                            "exception_method": "",
                            "frequencyPenaltyEnabled": false,
                            "frequency_penalty": 0.7,
                            "llm_id": "kimi-k2-turbo-preview@Moonshot",
                            "maxTokensEnabled": false,
                            "max_retries": 3,
                            "max_rounds": 1,
                            "max_tokens": 256,
                            "mcp": [
                                {
                                    "mcp_id": "30d6ef8ea8d511f0828382e3548809fa",
                                    "tools": {}
                                }
                            ],
                            "message_history_window_size": 12,
                            "outputs": {
                                "content": {
                                    "type": "string",
                                    "value": ""
                                }
                            },
                            "presencePenaltyEnabled": false,
                            "presence_penalty": 0.4,
                            "prompts": [
                                {
                                    "content": "user's query is {sys.query}\n\n\n{Agent:ManyToesBrush@content}\n",
                                    "role": "user"
                                }
                            ],
                            "sys_prompt": "<role> \n\nYou are the information extraction agent. You understand the user\u2019s query and delegate tasks to investoday and the internal research report retrieval agent. \n\n</role> \n\n<requirements>\n\n 1. Based on the stock code output by the \"Extract Stock Code\" agent, call investoday's list_news to retrieve the latest authoritative research reports and views, and save all publicly available key information. \n\n2. Call the \"Internal Research Report Retrieval Agent\" and save the full text of the research report output. \n\n3. Output the content retrieved from investoday and the Internal Research Report Retrieval Agent in full. \n\n</requirements>\n\n\n<report_structure_requirements>\nThe output must be divided into two sections:\n#1. Title: \u201cinvestoday\u201d\nDirectly output the content collected from investoday without any additional processing.\n#2. Title: \"Internal Research Report Retrieval Agent\"\nDirectly output the content provided by the Internal Research Report Retrieval Agent.\n</report_structure_requirements>",
                            "temperature": 0.1,
                            "temperatureEnabled": false,
                            "tools": [
                                {
                                    "component_name": "Agent",
                                    "id": "Agent:MightyIdeasGlow",
                                    "name": "Internal Research Report Retrieval Agent",
                                    "params": {
                                        "cite": true,
                                        "delay_after_error": 1,
                                        "description": "You are a senior financial content analyst who can accurately identify the companies, stock codes, industries or topics mentioned in user questions, and completely extract relevant research content from the knowledge base to ensure that data, opinions and conclusions are not lost.",
                          

... [Content truncated - file is 87177 bytes] ...

                 "dragHandle": ".note-drag-handle",
                        "dragging": false,
                        "height": 185,
                        "id": "Note:SadWallsSniff",
                        "measured": {
                            "height": 185,
                            "width": 328
                        },
                        "position": {
                            "x": 527.9711365245946,
                            "y": 448.2236919343899
                        },
                        "resizing": false,
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "noteNode",
                        "width": 328
                    }
                ]
            },
            "history": [],
            "messages": [],
            "path": [],
            "retrieval": []
        },
        "avatar":
    "data:image/png;base64,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"
}
```

## Detailed Analysis

### File Role in Repository

The file `agent/templates/stock_research_report.json` is located in the `agent/templates` directory.

This file is part of the **Agent System** for workflow management.

### Architecture Context

Files in this location typically handle concerns related to templates.

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

- [advanced_ingestion_pipeline.json](advanced_ingestion_pipeline.json_docs.md)
- [choose_your_knowledge_base_agent.json](choose_your_knowledge_base_agent.json_docs.md)
- [choose_your_knowledge_base_workflow.json](choose_your_knowledge_base_workflow.json_docs.md)
- [chunk_summary.json](chunk_summary.json_docs.md)
- [customer_review_analysis.json](customer_review_analysis.json_docs.md)
- [customer_service.json](customer_service.json_docs.md)
- [customer_support.json](customer_support.json_docs.md)
- [cv_analysis_and_candidate_evaluation.json](cv_analysis_and_candidate_evaluation.json_docs.md)
- [deep_research.json](deep_research.json_docs.md)
- [deep_search_r.json](deep_search_r.json_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
