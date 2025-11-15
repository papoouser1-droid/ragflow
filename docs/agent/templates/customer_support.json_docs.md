# Documentation: agent/templates/customer_support.json

## File Metadata

- **Path**: `agent/templates/customer_support.json`
- **Size**: 55059 bytes
- **Type**: .json
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/templates/customer_support.json`.

## Original Source Code

```json

{
    "id": 10,
    "title": {
        "en":"Customer Support",
        "de": "Kundensupport",
        "zh": "客户支持"},
    "description": {
        "en": "This is an intelligent customer service processing system workflow based on user intent classification. It uses LLM to identify user demand types and transfers them to the corresponding professional agent for processing.",
        "de": "Dies ist ein intelligentes Kundenservice-Verarbeitungssystem-Workflow basierend auf Benutzerabsichtsklassifizierung. Es verwendet LLM zur Identifizierung von Benutzeranforderungstypen und überträgt diese zur Verarbeitung an den entsprechenden professionellen Agenten.",
        "zh": "工作流系统，用于智能客服场景。基于用户意图分类。使用大模型识别用户需求类型，并将需求转移给相应的智能体进行处理。"},
    "canvas_type": "Customer Support",
    "dsl": {
            "components": {
                "Agent:DullTownsHope": {
                    "downstream": [
                        "Message:GreatDucksArgue"
                    ],
                    "obj": {
                        "component_name": "Agent",
                        "params": {
                            "delay_after_error": 1,
                            "description": "",
                            "exception_comment": "",
                            "exception_default_value": "",
                            "exception_goto": [],
                            "exception_method": null,
                            "frequencyPenaltyEnabled": false,
                            "frequency_penalty": 0.3,
                            "llm_id": "deepseek-chat@DeepSeek",
                            "maxTokensEnabled": false,
                            "max_retries": 3,
                            "max_rounds": 5,
                            "max_tokens": 4096,
                            "mcp": [],
                            "message_history_window_size": 12,
                            "outputs": {
                                "content": {
                                    "type": "string",
                                    "value": ""
                                }
                            },
                            "parameter": "Balance",
                            "presencePenaltyEnabled": false,
                            "presence_penalty": 0.2,
                            "prompts": [
                                {
                                    "content": "The user query is {sys.query}",
                                    "role": "user"
                                }
                            ],
                            "sys_prompt": "You are an empathetic mood-soothing assistant.  \n\nYour role is to comfort and encourage users when they feel upset or frustrated.  \n\n- Use a warm, kind, and understanding tone.  \n\n- Focus on showing empathy and emotional support rather than solving the problem directly.  \n\n- Always encourage users with positive and reassuring statements.  ",
                            "temperature": 0.5,
                            "temperatureEnabled": true,
                            "tools": [],
                            "topPEnabled": false,
                            "top_p": 0.85,
                            "user_prompt": "",
                            "visual_files_var": ""
                        }
                    },
                    "upstream": [
                        "Categorize:DullFriendsThank"
                    ]
                },
                "Agent:KhakiSunsJudge": {
                    "downstream": [
                        "Message:GreatDucksArgue"
                    ],
                    "obj": {
                        "component_name": "Agent",
                        "params": {
                            "delay_after_error": 1,
                            "description": "",
                            "exception_comment": "",
                            "exception_default_value": "",
                            "exception_goto": [],
                            "exception_method": null,
                            "frequencyPenaltyEnabled": false,
                            "frequency_penalty": 0.7,
                            "llm_id": "deepseek-chat@DeepSeek",
                            "maxTokensEnabled": false,
                            "max_retries": 3,
                            "max_rounds": 5,
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
                                    "content": "The user query is {sys.query}\n\nThe relevant document are {Retrieval:ShyPumasJoke@formalized_content}",
                                    "role": "user"
                                }
                            ],
                            "sys_prompt": "You are a highly professional product information advisor.  \n\nYour only mission is to provide accurate, factual, and structured answers to all product-related queries.\n\nAbsolutely no assumptions, guesses, or fabricated content are allowed. \n\n**Key Principles:**\n\n1. **Strict Database Reliance:**  \n\n   - Every answer must be based solely on the verified product information stored in the relevant documen.\n\n   - You are NOT allowed to invent, speculate, or infer details beyond what is retrieved.  \n\n   - If you cannot find relevant data, respond with: *\"I cannot find this information in our official product database. Please check back later or provide more details for further search.\"*\n\n2. **Information Accuracy and Structure:**  \n\n   - Provide information in a clear, concise, and professional way.  \n\n   - Use bullet points or numbered lists if there are multiple key points (e.g., features, price, warranty, technical specifications).  \n\n   - Always specify the version or model number when applicable to avoid confusion.\n\n3. **Tone and Style:**  \n\n   - Maintain a polite, professional, and helpful tone at all times.  \n\n   - Avoid marketing exaggeration or promotional language; stay strictly factual.  \n\n   - Do not express personal opinions; only cite official product data.\n\n4.  **User Guidance:**  \n\n   - If the user\u2019s query is unclear or too broad, politely request clarification or guide them to provide more specific product details (e.g., product name, model, version).  \n\n   - Example: *\"Could you please specify the product model or category so I can retrieve the most relevant information for you?\"*\n\n5. **Response Length and Formatting:**  \n\n   - Keep each answer within 100\u2013150 words for general queries.  \n\n   - For complex or multi-step explanations, you may extend to 200\u2013250 words, but always remain clear and well-structured.\n\n6. **Critical Reminder:**  \n\nYour authority and reliability depend entirely on the relevant document responses. Any fabricated, speculative, or unverified content will be considered a critical failure of your role.\n\n\n",
                            "temperature": 0.1,
                            "temperatureEnabled": true,
                            "tools": [],
                            "topPEnabled": false,
                            "top_p": 0.3,
                            "user_prompt": "",
                            "visual_files_var": ""
                        }
                    },
                    "upstream": [
                        "Retrieval:ShyPumasJoke"
                    ]
                },
                "Agent:TwelveOwlsWatch": {
                    "downstream": [
                        "Message:GreatDucksArgue"
                    ],
                    "obj": {
                        "component_name": "Agent",
                        "params": {
                            "delay_after_error": 1,
                            "description": "",
                            "exception_comment": "",
                            "exception_default_value": "",
                            "exception_goto": [],
                            "exception_method": null,
                            "frequencyPenaltyEnabled": false,
                            "frequency_penalty": 0.3,
                            "llm_id": "deepseek-chat@DeepSeek",
                            "maxTokensEnabled": false,
                            "max_retries": 3,
                            "max_rounds": 5,
                            "max_tokens": 4096,
                            "mcp": [],
                            "message_history_window_size": 12,
                            "outputs": {
                                "content": {
                                    "type": "string",
                                    "value": ""
                                }
                            },
                            "parameter": "Balance",
                            "presencePenaltyEnabled": false,
                            "presence_penalty": 0.2,
                            "prompts": [
                                {
                                    "content": "The user query is {sys.query}",
                                    "role": "user"
                                }
                            ],
                            "sys_prompt": "You are a friendly and casual conversational assistant.  \n\nYour primary goal is to engage users in light and enjoyable daily conversation.  \n\n- Keep a natural, relaxed, and positive tone.  \n\n- Avoid sensitive, controversial, or negative topics.  \n\n- You may gently 

... [Content truncated - file is 54937 bytes] ...

. It uses LLM to identify user demand types and transfers them to the corresponding professional agent for processing."
                            },
                            "label": "Note",
                            "name": "Workflow Overall Description"
                        },
                        "dragHandle": ".note-drag-handle",
                        "dragging": false,
                        "height": 171,
                        "id": "Note:AllGuestsShow",
                        "measured": {
                            "height": 171,
                            "width": 380
                        },
                        "position": {
                            "x": -283.6407251474677,
                            "y": 157.2943019466498
                        },
                        "resizing": false,
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "noteNode",
                        "width": 380
                    },
                    {
                        "data": {
                            "form": {
                                "text": "Here, product document snippets related to the user's question will be retrieved from the knowledge base first, and the relevant document snippets will be passed to the LLM together with the user's question."
                            },
                            "label": "Note",
                            "name": "Product info Agent"
                        },
                        "dragHandle": ".note-drag-handle",
                        "dragging": false,
                        "height": 154,
                        "id": "Note:IcyBooksCough",
                        "measured": {
                            "height": 154,
                            "width": 370
                        },
                        "position": {
                            "x": 1014.0959071234828,
                            "y": 492.830874176321
                        },
                        "resizing": false,
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "noteNode",
                        "width": 370
                    },
                    {
                        "data": {
                            "form": {
                                "text": "Here, a text will be randomly selected for answering"
                            },
                            "label": "Note",
                            "name": "What else\uff1f"
                        },
                        "dragHandle": ".note-drag-handle",
                        "dragging": false,
                        "id": "Note:AllThingsHide",
                        "measured": {
                            "height": 136,
                            "width": 249
                        },
                        "position": {
                            "x": 770.7060131788647,
                            "y": -123.23496705283817
                        },
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "noteNode"
                    }
                ]
            },
            "history": [],
            "messages": [],
            "path": [],
            "retrieval": []
        },
    "avatar": "data:image/png;base64,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"
}
```

## Detailed Analysis

### File Role in Repository

The file `agent/templates/customer_support.json` is located in the `agent/templates` directory.

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
- [cv_analysis_and_candidate_evaluation.json](cv_analysis_and_candidate_evaluation.json_docs.md)
- [deep_research.json](deep_research.json_docs.md)
- [deep_search_r.json](deep_search_r.json_docs.md)
- [ecommerce_customer_service_workflow.json](ecommerce_customer_service_workflow.json_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
