# File Documentation: agent/templates/ecommerce_customer_service_workflow.json

## File Metadata

- **Path**: `agent/templates/ecommerce_customer_service_workflow.json`
- **Extension**: `.json`
- **Lines**: 1,056
- **Characters**: 56,362
- **Size**: 56,483 bytes
- **Purpose**: Data/Configuration - JSON data or configuration file

## Original Source

```json
{
    "id": 22,
    "title": {
        "en": "Ecommerce Customer Service Workflow",
        "de": "Ecommerce Kundenservice Workflow",
        "zh": "电子商务客户服务工作流程"
    },
    "description": {
        "en": "This template helps e-commerce platforms address complex customer needs, such as comparing product features, providing usage support, and coordinating home installation services.",
        "de": "Diese Vorlage hilft E-Commerce-Plattformen, komplexe Kundenbedürfnisse zu erfüllen, wie z.B. den Vergleich von Produktmerkmalen, die Bereitstellung von Nutzungsunterstützung und die Koordination von Hausinstallationsdiensten.",
        "zh": "该模板可帮助电子商务平台解决复杂的客户需求，例如比较产品功能、提供使用支持和协调家庭安装服务。"
    },
    "canvas_type": "Customer Support",
    "dsl": {
        "components": {
            "Agent:DeepCoatsDress": {
                "downstream": [
                    "Message:KhakiSymbolsMarry"
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
                        "llm_id": "deepseek-v3@Tongyi-Qianwen",
                        "maxTokensEnabled": false,
                        "max_retries": 3,
                        "max_rounds": 1,
                        "max_tokens": 256,
                        "mcp": [],
                        "message_history_window_size": 6,
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
                                "content": "User's query is {sys.query}\n\n\n",
                                "role": "user"
                            }
                        ],
                        "sys_prompt": "# Role\nYou are an Installation Booking Assistant.\n## Goal\nCollect the following three pieces of information from the user \n1. Contact Number  \n2. Preferred Installation Time  \n3. Installation Address  \nOnce all three are collected, confirm the information and inform the user that a technician will contact them later by phone.\n## Instructions\n1. **Check if all three details** (Contact Number, Preferred Installation Time, Installation Address) have been provided.\n2. **If some details are missing**, acknowledge the ones provided and only ask for the missing information.\n3. Do **not repeat** the full request once some details are already known.\n4. Once all three details are collected, summarize and confirm them with the user.",
                        "temperature": 0.1,
                        "temperatureEnabled": false,
                        "tools": [],
                        "topPEnabled": false,
                        "top_p": 0.3,
                        "user_prompt": "",
                        "visual_files_var": ""
                    }
                },
                "upstream": [
                    "Categorize:NewDonkeysShare"
                ]
            },
            "Agent:PlentyCandiesRefuse": {
                "downstream": [
                    "Message:KhakiSymbolsMarry"
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
                        "llm_id": "deepseek-v3@Tongyi-Qianwen",
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
                                "content": "User's query is  {sys.query}\n\n\nSchema is {Retrieval:EightyDaysHappen@formalized_content}",
                                "role": "user"
                            }
                        ],
                        "sys_prompt": "# Specification Comparison Agent Prompt\n## Role\nYou are a product specification comparison assistant.\n## Goal\nHelp the user compare two or more products based on their features and specifications. Provide clear, accurate, and concise comparisons to assist the user in making an informed decision.\n---\n## Instructions\n- Start by confirming the product models or options the user wants to compare.\n- If the user has not specified the models, politely ask for them.\n- Present the comparison in a structured way (e.g., bullet points or a table format if supported).\n- Highlight key differences such as size, capacity, performance, energy efficiency, and price if available.\n- Maintain a neutral and professional tone without suggesting unnecessary upselling.\n---",
                        "temperature": 0.1,
                        "temperatureEnabled": false,
                        "tools": [],
                        "topPEnabled": false,
                        "top_p": 0.3,
                        "user_prompt": "",
                        "visual_files_var": ""
                    }
                },
                "upstream": [
                    "Retrieval:EightyDaysHappen"
                ]
            },
            "Agent:ShinyCooksCall": {
                "downstream": [
                    "Message:KhakiSymbolsMarry"
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
                        "llm_id": "deepseek-v3@Tongyi-Qianwen",
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
                                "content": "User\u2018s query is {sys.query}\n\nSchema is {Retrieval:EagerTipsFeel@formalized_content}\n\n",
                                "role": "user"
                            }
                        ],
                        "sys_prompt": "# Usage Guide Agent Prompt\n## Role\nYou are a product usage guide assistant.\n## Goal\nProvide clear, step-by-step instructions to help the user set up, operate, and maintain their product. Answer questions about functions, settings, and troubleshooting.\n---\n## Instructions\n- If the user asks about setup, provide easy-to-follow installation or configuration steps.\n- If the user asks about a feature, explain its purpose and how to activate it.\n- For troubleshooting, suggest common solutions first, then guide through advanced checks if needed.\n- Keep the response simple, clear, and actionable for a non-technical user.\n---",
                        "temperature": 0.1,
                        "temperatureEnabled": false,
                        "tools": [],
                        "topPEnabled": false,
                        "top_p": 0.3,
                        "user_prompt": "",
                        "visual_files_var": ""
                    }
                },
                "upstream": [
                    "Retrieval:EagerTipsFeel"
                ]
            },
            "Categorize:NewDonkeysShare": {
                "downstream": [
                    "Retrieval:EightyDaysHappen",
                    "Retrieval:EagerTipsFeel",
                    "Agent:DeepCoatsDress"
                ],
                "obj": {
                    "component_name": "Categorize",
                    "params": {
                        "category_description": {
                            "Book Installation": {
                                "description": "Handles the user\u2019s request to schedule, reschedule, or confirm an appointment for professional installation services at the customer\u2019s location.",
                                "examples": [
                                    "\u201cI\u2019d like to schedule installation for my product.\u201d\n\n\u201cCan I reschedule my installation appointment for next week?\u201d\n"
                                ],
                                "to": [
                                    "Agent:DeepCoatsDress"
                                ]
                            },
                            "Product Feature Comparison": {
                                "description": "Helps the user compare the features, technical specifications, and characteristics of different products to assist them in making an informed purchase decision.",
                                "examples": [
                                    "\u201cCan you compare the features of Model X and Model Y?\u201d\n\n\u201cWhat\u2019s the difference between Option A and Option B?\u201d\n\n\u201cWhich model, X100 or X200, has better performance and more features?\u201d"
                                ],
                                "to": [
                                    "Retrieval:EightyDaysHappen"
                                ]
                            },
                            "Product Usage Guide": {
                                "description": "Provides the user with detailed instructions, guides, or troubleshooting tips for using, maintaining, or optimizing the performance of their purchased product.",
                                "examples": [
                                    "\u201cHow do I set up this product?\u201d\n\n\u201cHow do I clean and maintain this product?\u201d\n\n\u201cCan you guide me on how to use the advanced features?\u201d"
                                ],
                                "to": [
                                    "Retrieval:EagerTipsFeel"
                                ]
                            }
                        },
                        "llm_id": "deepseek-v3@Tongyi-Qianwen",
                        "message_history_window_size": 1,
                        "outputs": {
                            "category_name": {
                                "type": "string"
                            }
                        },
                        "query": "sys.query"
                    }
                },
                "upstream": [
                    "begin"
                ]
            },
            "Message:KhakiSymbolsMarry": {
                "downstream": [],
                "obj": {
                    "component_name": "Message",
                    "params": {
                        "content": [
                            "{Agent:PlentyCandiesRefuse@content}{Agent:ShinyCooksCall@content}{Agent:DeepCoatsDress@content}"
                        ]
                    }
                },
                "upstream": [
                    "Agent:PlentyCandiesRefuse",
                    "Agent:ShinyCooksCall",
                    "Agent:DeepCoatsDress"
                ]
            },
            "Retrieval:EagerTipsFeel": {
                "downstream": [
                    "Agent:ShinyCooksCall"
                ],
                "obj": {
                    "component_name": "Retrieval",
                    "params": {
                        "cross_languages": [],
                        "empty_response": "",
                        "kb_ids": [
                            "a5aaec4a819b11f095f1047c16ec874f"
                        ],
                        "keywords_similarity_weight": 0.7,
                        "outputs": {
                            "formalized_content": {
                                "type": "string",
                                "value": ""
                            }
                        },
                        "query": "{sys.query}",
                        "rerank_id": "",
                        "similarity_threshold": 0.2,
                        "top_k": 1024,
                        "top_n": 8,
                        "use_kg": false
                    }
                },
                "upstream": [
                    "Categorize:NewDonkeysShare"
                ]
            },
            "Retrieval:EightyDaysHappen": {
                "downstream": [
                    "Agent:PlentyCandiesRefuse"
                ],
                "obj": {
                    "component_name": "Retrieval",
                    "params": {
                        "cross_languages": [],
                        "empty_response": "",
                        "kb_ids": [
                            "65cb5150819b11f08347047c16ec874f"
                        ],
                        "keywords_similarity_weight": 0.7,
                        "outputs": {
                            "formalized_content": {
                                "type": "string",
                                "value": ""
                            }
                        },
                        "query": "{sys.query}",
                        "rerank_id": "",
                        "similarity_threshold": 0.2,
                        "top_k": 1024,
                        "top_n": 8,
                        "use_kg": false
                    }
                },
                "upstream": [
                    "Categorize:NewDonkeysShare"
                ]
            },
            "begin": {
                "downstream": [
                    "Categorize:NewDonkeysShare"
                ],
                "obj": {
                    "component_name": "Begin",
                    "params": {
                        "enablePrologue": true,
                        "inputs": {},
                        "mode": "conversational",
                        "prologue": "Hi! I'm your assistant. "
                    }
                },
                "upstream": []
            }
        },
        "globals": {
            "sys.conversation_turns": 0,
            "sys.files": [],
            "sys.query": "",
            "sys.user_id": ""
        },
        "graph": {
            "edges": [
                {
                    "data": {
                        "isHovered": false
                    },
                    "id": "xy-edge__beginstart-Categorize:NewDonkeysShareend",
                    "source": "begin",
                    "sourceHandle": "start",
                    "target": "Categorize:NewDonkeysShare",
                    "targetHandle": "end"
                },
                {
                    "data": {
                        "isHovered": false
                    },
                    "id": "xy-edge__Retrieval:EightyDaysHappenstart-Agent:PlentyCandiesRefuseend",
                    "source": "Retrieval:EightyDaysHappen",
                    "sourceHandle": "start",
                    "target": "Agent:PlentyCandiesRefuse",
                    "targetHandle": "end"
                },
                {
                    "data": {
                        "isHovered": false
                    },
                    "id": "xy-edge__Retrieval:EagerTipsFeelstart-Agent:ShinyCooksCallend",
                    "source": "Retrieval:EagerTipsFeel",
                    "sourceHandle": "start",
                    "target": "Agent:ShinyCooksCall",
                    "targetHandle": "end"
                },
                {
                    "data": {
                        "isHovered": false
                    },
                    "id": "xy-edge__Categorize:NewDonkeysShare3f76ad5d-1562-4323-bd13-474a80938db0-Retrieval:EightyDaysHappenend",
                    "markerEnd": "logo",
                    "source": "Categorize:NewDonkeysShare",
                    "sourceHandle": "3f76ad5d-1562-4323-bd13-474a80938db0",
                    "style": {
                        "stroke": "rgba(91, 93, 106, 1)",
                        "strokeWidth": 1
                    },
                    "target": "Retrieval:EightyDaysHappen",
                    "targetHandle": "end",
                    "type": "buttonEdge",
                    "zIndex": 1001
                },
                {
                    "data": {
                        "isHovered": false
                    },
                    "id": "xy-edge__Categorize:NewDonkeysShare442ea422-de6a-4c78-afcd-ac4a41daa4ca-Retrieval:EagerTipsFeelend",
                    "markerEnd": "logo",
                    "source": "Categorize:NewDonkeysShare",
                    "sourceHandle": "442ea422-de6a-4c78-afcd-ac4a41daa4ca",
                    "style": {
                        "stroke": "rgba(91, 93, 106, 1)",
                        "strokeWidth": 1
                    },
                    "target": "Retrieval:EagerTipsFeel",
                    "targetHandle": "end",
                    "type": "buttonEdge",
                    "zIndex": 1001
                },
                {
                    "data": {
                        "isHovered": false
                    },
                    "id": "xy-edge__Agent:PlentyCandiesRefusestart-Message:KhakiSymbolsMarryend",
                    "markerEnd": "logo",
                    "source": "Agent:PlentyCandiesRefuse",
                    "sourceHandle": "start",
                    "style": {
                        "stroke": "rgba(91, 93, 106, 1)",
                        "strokeWidth": 1
                    },
                    "target": "Message:KhakiSymbolsMarry",
                    "targetHandle": "end",
                    "type": "buttonEdge",
                    "zIndex": 1001
                },
                {
                    "data": {
                        "isHovered": false
                    },
                    "id": "xy-edge__Agent:ShinyCooksCallstart-Message:KhakiSymbolsMarryend",
                    "markerEnd": "logo",
                    "source": "Agent:ShinyCooksCall",
                    "sourceHandle": "start",
                    "style": {
                        "stroke": "rgba(91, 93, 106, 1)",
                        "strokeWidth": 1
                    },
                    "target": "Message:KhakiSymbolsMarry",
                    "targetHandle": "end",
                    "type": "buttonEdge",
                    "zIndex": 1001
                },
                {
                    "data": {
                        "isHovered": false
                    },
                    "id": "xy-edge__Categorize:NewDonkeysShare81a65fca-a460-4a3b-a4d5-50e76da760bb-Agent:DeepCoatsDressend",
                    "markerEnd": "logo",
                    "source": "Categorize:NewDonkeysShare",
                    "sourceHandle": "81a65fca-a460-4a3b-a4d5-50e76da760bb",
                    "style": {
                        "stroke": "rgba(91, 93, 106, 1)",
                        "strokeWidth": 1
                    },
                    "target": "Agent:DeepCoatsDress",
                    "targetHandle": "end",
                    "type": "buttonEdge",
                    "zIndex": 1001
                },
                {
                    "data": {
                        "isHovered": false
                    },
                    "id": "xy-edge__Agent:DeepCoatsDressstart-Message:KhakiSymbolsMarryend",
                    "markerEnd": "logo",
                    "source": "Agent:DeepCoatsDress",
                    "sourceHandle": "start",
                    "style": {
                        "stroke": "rgba(91, 93, 106, 1)",
                        "strokeWidth": 1
                    },
                    "target": "Message:KhakiSymbolsMarry",
                    "targetHandle": "end",
                    "type": "buttonEdge",
                    "zIndex": 1001
                }
            ],
            "nodes": [
                {
                    "data": {
                        "form": {
                            "enablePrologue": true,
                            "inputs": {},
                            "mode": "conversational",
                            "prologue": "Hi! I'm your assistant. "
                        },
                        "label": "Begin",
                        "name": "begin"
                    },
                    "dragging": false,
                    "id": "begin",
                    "measured": {
                        "height": 48,
                        "width": 200
                    },
                    "position": {
                        "x": 16.54401073812774,
                        "y": 204.87390426641298
                    },
                    "selected": false,
                    "sourcePosition": "left",
                    "targetPosition": "right",
                    "type": "beginNode"
                },
                {
                    "data": {
                        "form": {
                            "frequencyPenaltyEnabled": false,
                            "frequency_penalty": 0.7,
                            "items": [
                                {
                                    "description": "Helps the user compare the features, technical specifications, and characteristics of different products to assist them in making an informed purchase decision.",
                                    "examples": [
                                        {
                                            "value": "\u201cCan you compare the features of Model X and Model Y?\u201d\n\n\u201cWhat\u2019s the difference between Option A and Option B?\u201d\n\n\u201cWhich model, X100 or X200, has better performance and more features?\u201d"
                                        }
                                    ],
                                    "name": "Product Feature Comparison",
                                    "uuid": "3f76ad5d-1562-4323-bd13-474a80938db0"
                                },
                                {
                                    "description": "Provides the user with detailed instructions, guides, or troubleshooting tips for using, maintaining, or optimizing the performance of their purchased product.",
                                    "examples": [
                                        {
                                            "value": "\u201cHow do I set up this product?\u201d\n\n\u201cHow do I clean and maintain this product?\u201d\n\n\u201cCan you guide me on how to use the advanced features?\u201d"
                                        }
                                    ],
                                    "name": "Product Usage Guide",
                                    "uuid": "442ea422-de6a-4c78-afcd-ac4a41daa4ca"
                                },
                                {
                                    "description": "Handles the user\u2019s request to schedule, reschedule, or confirm an appointment for professional installation services at the customer\u2019s location.",
                                    "examples": [
                                        {
                                            "value": "\u201cI\u2019d like to schedule installation for my product.\u201d\n\n\u201cCan I reschedule my installation appointment for next week?\u201d\n"
                                        }
                                    ],
                                    "name": "Book Installation",
                                    "uuid": "81a65fca-a460-4a3b-a4d5-50e76da760bb"
                                }
                            ],
                            "llm_id": "deepseek-v3@Tongyi-Qianwen",
                            "maxTokensEnabled": false,
                            "max_tokens": 256,
                            "message_history_window_size": 1,
                            "outputs": {
                                "category_name": {
                                    "type": "string"
                                }
                            },
                            "parameter": "Precise",
                            "presencePenaltyEnabled": false,
                            "presence_penalty": 0.4,
                            "query": "sys.query",
                            "temperature": 0.1,
                            "temperatureEnabled": false,
                            "topPEnabled": false,
                            "top_p": 0.3
                        },
                        "label": "Categorize",
                        "name": "Event Classification"
                    },
                    "dragging": false,
                    "id": "Categorize:NewDonkeysShare",
                    "measured": {
                        "height": 172,
                        "width": 200
                    },
                    "position": {
                        "x": 291.21809479853766,
                        "y": 142.25418266609364
                    },
                    "selected": false,
                    "sourcePosition": "right",
                    "targetPosition": "left",
                    "type": "categorizeNode"
                },
                {
                    "data": {
                        "form": {
                            "cross_languages": [],
                            "empty_response": "",
                            "kb_ids": [
                                "65cb5150819b11f08347047c16ec874f"
                            ],
                            "keywords_similarity_weight": 0.7,
                            "outputs": {
                                "formalized_content": {
                                    "type": "string",
                                    "value": ""
                                }
                            },
                            "query": "{sys.query}",
                            "rerank_id": "",
                            "similarity_threshold": 0.2,
                            "top_k": 1024,
                            "top_n": 8,
                            "use_kg": false
                        },
                        "label": "Retrieval",
                        "name": "Feature Comparison Knowledge Base"
                    },
                    "dragging": false,
                    "id": "Retrieval:EightyDaysHappen",
                    "measured": {
                        "height": 96,
                        "width": 200
                    },
                    "position": {
                        "x": 566.9838181044963,
                        "y": 60.56474206037082
                    },
                    "selected": false,
                    "sourcePosition": "right",
                    "targetPosition": "left",
                    "type": "retrievalNode"
                },
                {
                    "data": {
                        "form": {
                            "cite": true,
                            "delay_after_error": 1,
                            "description": "",
                            "exception_default_value": "",
                            "exception_goto": [],
                            "exception_method": "",
                            "frequencyPenaltyEnabled": false,
                            "frequency_penalty": 0.7,
                            "llm_id": "deepseek-v3@Tongyi-Qianwen",
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
                                    "content": "User's query is  {sys.query}\n\n\nSchema is {Retrieval:EightyDaysHappen@formalized_content}",
                                    "role": "user"
                                }
                            ],
                            "sys_prompt": "# Specification Comparison Agent Prompt\n## Role\nYou are a product specification comparison assistant.\n## Goal\nHelp the user compare two or more products based on their features and specifications. Provide clear, accurate, and concise comparisons to assist the user in making an informed decision.\n---\n## Instructions\n- Start by confirming the product models or options the user wants to compare.\n- If the user has not specified the models, politely ask for them.\n- Present the comparison in a structured way (e.g., bullet points or a table format if supported).\n- Highlight key differences such as size, capacity, performance, energy efficiency, and price if available.\n- Maintain a neutral and professional tone without suggesting unnecessary upselling.\n---",
                            "temperature": 0.1,
                            "temperatureEnabled": false,
                            "tools": [],
                            "topPEnabled": false,
                            "top_p": 0.3,
                            "user_prompt": "",
                            "visual_files_var": ""
                        },
                        "label": "Agent",
                        "name": "Feature Comparison Agent"
                    },
                    "dragging": false,
                    "id": "Agent:PlentyCandiesRefuse",
                    "measured": {
                        "height": 84,
                        "width": 200
                    },
                    "position": {
                        "x": 857.7148601953267,
                        "y": 66.63237329303199
                    },
                    "selected": false,
                    "sourcePosition": "right",
                    "targetPosition": "left",
                    "type": "agentNode"
                },
                {
                    "data": {
                        "form": {
                            "cross_languages": [],
                            "empty_response": "",
                            "kb_ids": [
                                "a5aaec4a819b11f095f1047c16ec874f"
                            ],
                            "keywords_similarity_weight": 0.7,
                            "outputs": {
                                "formalized_content": {
                                    "type": "string",
                                    "value": ""
                                }
                            },
                            "query": "{sys.query}",
                            "rerank_id": "",
                            "similarity_threshold": 0.2,
                            "top_k": 1024,
                            "top_n": 8,
                            "use_kg": false
                        },
                        "label": "Retrieval",
                        "name": "Usage Guide Knowledge Base"
                    },
                    "dragging": false,
                    "id": "Retrieval:EagerTipsFeel",
                    "measured": {
                        "height": 96,
                        "width": 200
                    },
                    "position": {
                        "x": 567.0647658166523,
                        "y": 213.38735509338153
                    },
                    "selected": false,
                    "sourcePosition": "right",
                    "targetPosition": "left",
                    "type": "retrievalNode"
                },
                {
                    "data": {
                        "form": {
                            "cite": true,
                            "delay_after_error": 1,
                            "description": "",
                            "exception_default_value": "",
                            "exception_goto": [],
                            "exception_method": "",
                            "frequencyPenaltyEnabled": false,
                            "frequency_penalty": 0.7,
                            "llm_id": "deepseek-v3@Tongyi-Qianwen",
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
                                    "content": "User\u2018s query is {sys.query}\n\nSchema is {Retrieval:EagerTipsFeel@formalized_content}\n\n",
                                    "role": "user"
                                }
                            ],
                            "sys_prompt": "# Usage Guide Agent Prompt\n## Role\nYou are a product usage guide assistant.\n## Goal\nProvide clear, step-by-step instructions to help the user set up, operate, and maintain their product. Answer questions about functions, settings, and troubleshooting.\n---\n## Instructions\n- If the user asks about setup, provide easy-to-follow installation or configuration steps.\n- If the user asks about a feature, explain its purpose and how to activate it.\n- For troubleshooting, suggest common solutions first, then guide through advanced checks if needed.\n- Keep the response simple, clear, and actionable for a non-technical user.\n---",
                            "temperature": 0.1,
                            "temperatureEnabled": false,
                            "tools": [],
                            "topPEnabled": false,
                            "top_p": 0.3,
                            "user_prompt": "",
                            "visual_files_var": ""
                        },
                        "label": "Agent",
                        "name": "Usage Guide Agent"
                    },
                    "dragging": false,
                    "id": "Agent:ShinyCooksCall",
                    "measured": {
                        "height": 84,
                        "width": 200
                    },
                    "position": {
                        "x": 861.0463780800472,
                        "y": 218.84239036799477
                    },
                    "selected": false,
                    "sourcePosition": "right",
                    "targetPosition": "left",
                    "type": "agentNode"
                },
                {
                    "data": {
                        "form": {
                            "cite": true,
                            "delay_after_error": 1,
                            "description": "",
                            "exception_default_value": "",
                            "exception_goto": [],
                            "exception_method": "",
                            "frequencyPenaltyEnabled": false,
                            "frequency_penalty": 0.7,
                            "llm_id": "deepseek-v3@Tongyi-Qianwen",
                            "maxTokensEnabled": false,
                            "max_retries": 3,
                            "max_rounds": 1,
                            "max_tokens": 256,
                            "mcp": [],
                            "message_history_window_size": 6,
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
                                    "content": "User's query is {sys.query}\n\n\n",
                                    "role": "user"
                                }
                            ],
                            "sys_prompt": "# Role\nYou are an Installation Booking Assistant.\n## Goal\nCollect the following three pieces of information from the user \n1. Contact Number  \n2. Preferred Installation Time  \n3. Installation Address  \nOnce all three are collected, confirm the information and inform the user that a technician will contact them later by phone.\n## Instructions\n1. **Check if all three details** (Contact Number, Preferred Installation Time, Installation Address) have been provided.\n2. **If some details are missing**, acknowledge the ones provided and only ask for the missing information.\n3. Do **not repeat** the full request once some details are already known.\n4. Once all three details are collected, summarize and confirm them with the user.",
                            "temperature": 0.1,
                            "temperatureEnabled": false,
                            "tools": [],
                            "topPEnabled": false,
                            "top_p": 0.3,
                            "user_prompt": "",
                            "visual_files_var": ""
                        },
                        "label": "Agent",
                        "name": " Installation Booking Agent"
                    },
                    "dragging": false,
                    "id": "Agent:DeepCoatsDress",
                    "measured": {
                        "height": 84,
                        "width": 200
                    },
                    "position": {
                        "x": 645.7337493836382,
                        "y": 409.2170327976632
                    },
                    "selected": false,
                    "sourcePosition": "right",
                    "targetPosition": "left",
                    "type": "agentNode"
                },
                {
                    "data": {
                        "form": {
                            "content": [
                                "{Agent:PlentyCandiesRefuse@content}{Agent:ShinyCooksCall@content}{Agent:DeepCoatsDress@content}"
                            ]
                        },
                        "label": "Message",
                        "name": "Message"
                    },
                    "dragging": false,
                    "id": "Message:KhakiSymbolsMarry",
                    "measured": {
                        "height": 56,
                        "width": 200
                    },
                    "position": {
                        "x": 1241.2275787739002,
                        "y": 238.1004882989556
                    },
                    "selected": false,
                    "sourcePosition": "right",
                    "targetPosition": "left",
                    "type": "messageNode"
                },
                {
                    "data": {
                        "form": {
                            "text": "This workflow automatically replies to user inquiries about multi-product feature comparisons, single product features based on the knowledge base,and automatically records users' appointment installation information."
                        },
                        "label": "Note",
                        "name": "Workflow Overall Description"
                    },
                    "dragHandle": ".note-drag-handle",
                    "dragging": false,
                    "height": 177,
                    "id": "Note:TamePlacesStay",
                    "measured": {
                        "height": 177,
                        "width": 352
                    },
                    "position": {
                        "x": -74.64018485740644,
                        "y": -278.09128552569814
                    },
                    "resizing": false,
                    "selected": false,
                    "sourcePosition": "right",
                    "targetPosition": "left",
                    "type": "noteNode",
                    "width": 352
                },
                {
                    "data": {
                        "form": {
                            "text": "The **Categorize** node can direct users to different handling workflows.\n"
                        },
                        "label": "Note",
                        "name": "Note\uff1aEvent Classification"
                    },
                    "dragHandle": ".note-drag-handle",
                    "dragging": false,
                    "id": "Note:FuzzyOttersTake",
                    "measured": {
                        "height": 136,
                        "width": 255
                    },
                    "position": {
                        "x": 265.4869944440654,
                        "y": 356.5967388588942
                    },
                    "selected": false,
                    "sourcePosition": "right",
                    "targetPosition": "left",
                    "type": "noteNode"
                },
                {
                    "data": {
                        "form": {
                            "text": "Use this node to set up the product information knowledge base."
                        },
                        "label": "Note",
                        "name": "Note\uff1aFeature Comparison Knowledge Base"
                    },
                    "dragHandle": ".note-drag-handle",
                    "dragging": false,
                    "height": 128,
                    "id": "Note:FloppyKingsRead",
                    "measured": {
                        "height": 128,
                        "width": 486
                    },
                    "position": {
                        "x": 559.4676154300545,
                        "y": -272.09359584713263
                    },
                    "resizing": false,
                    "selected": false,
                    "sourcePosition": "right",
                    "targetPosition": "left",
                    "type": "noteNode",
                    "width": 486
                },
                {
                    "data": {
                        "form": {
                            "text": "Use this node to set up the user guide knowledge base."
                        },
                        "label": "Note",
                        "name": "Note\uff1aUsage Guide Knowledge Base"
                    },
                    "dragHandle": ".note-drag-handle",
                    "dragging": false,
                    "height": 131,
                    "id": "Note:PlainTrainsTickle",
                    "measured": {
                        "height": 131,
                        "width": 492
                    },
                    "position": {
                        "x": 562.2432036803011,
                        "y": -115.13957305647553
                    },
                    "resizing": false,
                    "selected": false,
                    "sourcePosition": "right",
                    "targetPosition": "left",
                    "type": "noteNode",
                    "width": 492
                },
                {
                    "data": {
                        "form": {
                            "text": "This Agent looks up and summarizes the differences between different product models."
                        },
                        "label": "Note",
                        "name": "Note\uff1aFeature Comparison Agent"
                    },
                    "dragHandle": ".note-drag-handle",
                    "dragging": false,
                    "height": 130,
                    "id": "Note:NinetyClubsAct",
                    "measured": {
                        "height": 130,
                        "width": 314
                    },
                    "position": {
                        "x": 1242.1094687263958,
                        "y": -101.26619228497279
                    },
                    "resizing": false,
                    "selected": false,
                    "sourcePosition": "right",
                    "targetPosition": "left",
                    "type": "noteNode",
                    "width": 314
                },
                {
                    "data": {
                        "form": {
                            "text": "This Agent queries the user guide knowledge base to get usage help."
                        },
                        "label": "Note",
                        "name": "Note\uff1aUsage Guide Agent"
                    },
                    "dragHandle": ".note-drag-handle",
                    "dragging": false,
                    "height": 138,
                    "id": "Note:CleverViewsLearn",
                    "measured": {
                        "height": 138,
                        "width": 309
                    },
                    "position": {
                        "x": 1242.0223497932525,
                        "y": 71.55537317461697
                    },
                    "resizing": false,
                    "selected": false,
                    "sourcePosition": "right",
                    "targetPosition": "left",
                    "type": "noteNode",
                    "width": 309
                },
                {
                    "data": {
                        "form": {
                            "text": "This Agent collects the user\u2019s installation details through a multi-turn conversation."
                        },
                        "label": "Note",
                        "name": "Note\uff1a Installation Booking Agent"
                    },
                    "dragHandle": ".note-drag-handle",
                    "dragging": false,
                    "height": 150,
                    "id": "Note:SoftFoxesTan",
                    "measured": {
                        "height": 150,
                        "width": 338
                    },
                    "position": {
                        "x": 976.444626825383,
                        "y": 504.7856230269402
                    },
                    "resizing": false,
                    "selected": false,
                    "sourcePosition": "right",
                    "targetPosition": "left",
                    "type": "noteNode",
                    "width": 338
                },
                {
                    "data": {
                        "form": {
                            "text": "https://huggingface.co/datasets/InfiniFlow/Ecommerce-Customer-Service-Workflow"
                        },
                        "label": "Note",
                        "name": "Dataset"
                    },
                    "dragHandle": ".note-drag-handle",
                    "dragging": false,
                    "height": 157,
                    "id": "Note:CyanLandsStudy",
                    "measured": {
                        "height": 157,
                        "width": 356
                    },
                    "position": {
                        "x": -74.238694872689,
                        "y": -77.31812780982554
                    },
                    "resizing": false,
                    "selected": false,
                    "sourcePosition": "right",
                    "targetPosition": "left",
            

[... Content truncated for brevity ...]
```

## High-Level Overview

This file is part of the RAGFlow repository located at `agent/templates/ecommerce_customer_service_workflow.json`.

Based on the file structure and naming, it appears to be a data/configuration - json data or configuration file.

The file contains approximately 1056 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

This is a configuration or data file. See the 'Original Source' section for full content.

## Code Structure Analysis

- Total lines: 1056
- Blank lines: 0 (0.0%)
- Comment lines: ~0 (0.0%)
- Code lines: ~1056


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `agent` directory, specifically within `agent/templates`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 14 loop(s) - consider algorithmic complexity
- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `agent/templates/` directory
- Potential test file: `test_ecommerce_customer_service_workflow.json`

## Keywords

ANEOwL7ql9Gx9Mj2hvh, AWCyqzSCNs1HxWf, Address, Agent, Answer, Assistant, BBAWusr8sVypsQgLNm8wrK1hky2m, Base, BdJ5kL6xrCCSg2DrByZ1SKpImLBj4kjG, Begin, Bereitstellung, Book, Booking, C8FV1Zi6w44powUGnT8bq6Sqo6Iek8YNci, CJ5ubuHH1h7jwlZfw, CN16A71GDd, CVq2OQj6PYRzht776VZw8fQYvvXwexUJhBGmyse22I96jEMrlwMg1Fsl8J3ziIox6Ihtzbj, Categorize, Check, CiZmZ0kjBKr5LqJhD5t7Icq5PhXI6dqgPmEk8NHVK3j4, Classification, CleverViewsLearn, Commerce, Comparison, Contact, CrjigiPDfwzqU30bjx99i, Customer, CyanLandsStudy, D48VMoFUoQnrBW9FKlPM, Dataset, DeepCoatsDress, DeepCoatsDressend, DeepCoatsDressstart, Description, Diese, Dn773Pbx09os4, EagerTipsFeel, EagerTipsFeelend, EagerTipsFeelstart, Ecommerce, EiABAidBaCFBAsNBJMQSpIg0TdmmxE3ioiFnOFvvS3VXvefztqrqGdo9anatr, EightyDaysHappen, EightyDaysHappenend, EightyDaysHappenstart, Event, FJChKljWbEZVcSk0ndATRN7T6fb5DmLSTCqAfwgaO7vfbTYbF0tED4XtKgRyfIB5j5vTyeJpJuom8eY9WeReOrNwfcIXeddv3NCxq8JATbc7nQ5m52YZBnMaTaYmqhScMUs6ECheGORMHPvK6zmNUuGQ1bo90OGhg1c9h8Ir8FDJXVaJzftMQ8ManY, FTiKsfvs, Feature, FloppyKingsRead, For...

---
*Generated by RAGFlow Repository Documentation Generator*
