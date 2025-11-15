# File Documentation: agent/templates/customer_support.json

## File Metadata

- **Path**: `agent/templates/customer_support.json`
- **Extension**: `.json`
- **Lines**: 886
- **Characters**: 54,937
- **Size**: 55,059 bytes
- **Purpose**: Data/Configuration - JSON data or configuration file

## Original Source

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
                            "sys_prompt": "You are a friendly and casual conversational assistant.  \n\nYour primary goal is to engage users in light and enjoyable daily conversation.  \n\n- Keep a natural, relaxed, and positive tone.  \n\n- Avoid sensitive, controversial, or negative topics.  \n\n- You may gently guide the conversation by introducing related casual topics if the user shows interest.  \n\n",
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
                "Categorize:DullFriendsThank": {
                    "downstream": [
                        "Message:BreezyDonutsHeal",
                        "Agent:TwelveOwlsWatch",
                        "Agent:DullTownsHope",
                        "Retrieval:ShyPumasJoke"
                    ],
                    "obj": {
                        "component_name": "Categorize",
                        "params": {
                            "category_description": {
                                "1. contact": {
                                    "description": "This answer provide a specific contact information, like e-mail, phone number, wechat number, line number, twitter, discord, etc,.",
                                    "examples": [
                                        "My phone number is 203921\nkevinhu.hk@gmail.com\nThis is my discord number: johndowson_29384\n13212123432\n8379829"
                                    ],
                                    "to": [
                                        "Message:BreezyDonutsHeal"
                                    ]
                                },
                                "2. casual": {
                                    "description": "The question is not about the product usage, appearance and how it works. Just casual chat.",
                                    "examples": [
                                        "How are you doing?\nWhat is your name?\nAre you a robot?\nWhat's the weather?\nWill it rain?"
                                    ],
                                    "to": [
                                        "Agent:TwelveOwlsWatch"
                                    ]
                                },
                                "3. complain": {
                                    "description": "Complain even curse about the product or service you provide. But the comment is not specific enough.",
                                    "examples": [
                                        "How bad is it.\nIt's really sucks.\nDamn, for God's sake, can it be more steady?\nShit, I just can't use this shit.\nI can't stand it anymore."
                                    ],
                                    "to": [
                                        "Agent:DullTownsHope"
                                    ]
                                },
                                "4. product related": {
                                    "description": "The question is about the product usage, appearance and how it works.",
                                    "examples": [
                                        "Why it always beaming?\nHow to install it onto the wall?\nIt leaks, what to do?\nException: Can't connect to ES cluster\nHow to build the RAGFlow image from scratch"
                                    ],
                                    "to": [
                                        "Retrieval:ShyPumasJoke"
                                    ]
                                }
                            },
                            "llm_id": "deepseek-chat@DeepSeek",
                            "message_history_window_size": 1,
                            "outputs": {
                                "category_name": {
                                    "type": "string"
                                }
                            },
                            "query": "sys.query",
                            "temperature": "0.1"
                        }
                    },
                    "upstream": [
                        "begin"
                    ]
                },
                "Message:BreezyDonutsHeal": {
                    "downstream": [],
                    "obj": {
                        "component_name": "Message",
                        "params": {
                            "content": [
                                "Okay, I've already write this down. What else I can do for you?",
                                "Get it. What else I can do for you?",
                                "Thanks for your trust! Our expert will contact ASAP. So, anything else I can do for you?",
                                "Thanks! So, anything else I can do for you?"
                            ]
                        }
                    },
                    "upstream": [
                        "Categorize:DullFriendsThank"
                    ]
                },
                "Message:GreatDucksArgue": {
                    "downstream": [],
                    "obj": {
                        "component_name": "Message",
                        "params": {
                            "content": [
                                "{Agent:TwelveOwlsWatch@content}{Agent:DullTownsHope@content}{Agent:KhakiSunsJudge@content}"
                            ]
                        }
                    },
                    "upstream": [
                        "Agent:TwelveOwlsWatch",
                        "Agent:DullTownsHope",
                        "Agent:KhakiSunsJudge"
                    ]
                },
                "Retrieval:ShyPumasJoke": {
                    "downstream": [
                        "Agent:KhakiSunsJudge"
                    ],
                    "obj": {
                        "component_name": "Retrieval",
                        "params": {
                            "cross_languages": [],
                            "empty_response": "",
                            "kb_ids": [],
                            "keywords_similarity_weight": 0.7,
                            "outputs": {
                                "formalized_content": {
                                    "type": "string",
                                    "value": ""
                                }
                            },
                            "query": "sys.query",
                            "rerank_id": "",
                            "similarity_threshold": 0.2,
                            "top_k": 1024,
                            "top_n": 8,
                            "use_kg": false
                        }
                    },
                    "upstream": [
                        "Categorize:DullFriendsThank"
                    ]
                },
                "begin": {
                    "downstream": [
                        "Categorize:DullFriendsThank"
                    ],
                    "obj": {
                        "component_name": "Begin",
                        "params": {
                            "enablePrologue": true,
                            "inputs": {},
                            "mode": "conversational",
                            "prologue": "Hi! I'm an official AI customer service representative. How can I help you?"
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
                        "id": "xy-edge__beginstart-Categorize:DullFriendsThankend",
                        "source": "begin",
                        "sourceHandle": "start",
                        "target": "Categorize:DullFriendsThank",
                        "targetHandle": "end"
                    },
                    {
                        "data": {
                            "isHovered": false
                        },
                        "id": "xy-edge__Categorize:DullFriendsThanke4d754a5-a33e-4096-8648-8688e5474a15-Message:BreezyDonutsHealend",
                        "source": "Categorize:DullFriendsThank",
                        "sourceHandle": "e4d754a5-a33e-4096-8648-8688e5474a15",
                        "target": "Message:BreezyDonutsHeal",
                        "targetHandle": "end"
                    },
                    {
                        "data": {
                            "isHovered": false
                        },
                        "id": "xy-edge__Categorize:DullFriendsThank8cbf6ea3-a176-490d-9f8c-86373c932583-Agent:TwelveOwlsWatchend",
                        "source": "Categorize:DullFriendsThank",
                        "sourceHandle": "8cbf6ea3-a176-490d-9f8c-86373c932583",
                        "target": "Agent:TwelveOwlsWatch",
                        "targetHandle": "end"
                    },
                    {
                        "data": {
                            "isHovered": false
                        },
                        "id": "xy-edge__Categorize:DullFriendsThankacc40a78-1b9e-4d2f-b5d6-64e01ab69269-Agent:DullTownsHopeend",
                        "source": "Categorize:DullFriendsThank",
                        "sourceHandle": "acc40a78-1b9e-4d2f-b5d6-64e01ab69269",
                        "target": "Agent:DullTownsHope",
                        "targetHandle": "end"
                    },
                    {
                        "data": {
                            "isHovered": false
                        },
                        "id": "xy-edge__Categorize:DullFriendsThankdfa5eead-9341-4f22-9236-068dbfb745e8-Retrieval:ShyPumasJokeend",
                        "source": "Categorize:DullFriendsThank",
                        "sourceHandle": "dfa5eead-9341-4f22-9236-068dbfb745e8",
                        "target": "Retrieval:ShyPumasJoke",
                        "targetHandle": "end"
                    },
                    {
                        "data": {
                            "isHovered": false
                        },
                        "id": "xy-edge__Retrieval:ShyPumasJokestart-Agent:KhakiSunsJudgeend",
                        "source": "Retrieval:ShyPumasJoke",
                        "sourceHandle": "start",
                        "target": "Agent:KhakiSunsJudge",
                        "targetHandle": "end"
                    },
                    {
                        "data": {
                            "isHovered": false
                        },
                        "id": "xy-edge__Agent:TwelveOwlsWatchstart-Message:GreatDucksArgueend",
                        "source": "Agent:TwelveOwlsWatch",
                        "sourceHandle": "start",
                        "target": "Message:GreatDucksArgue",
                        "targetHandle": "end"
                    },
                    {
                        "data": {
                            "isHovered": false
                        },
                        "id": "xy-edge__Agent:DullTownsHopestart-Message:GreatDucksArgueend",
                        "markerEnd": "logo",
                        "source": "Agent:DullTownsHope",
                        "sourceHandle": "start",
                        "style": {
                            "stroke": "rgba(91, 93, 106, 1)",
                            "strokeWidth": 1
                        },
                        "target": "Message:GreatDucksArgue",
                        "targetHandle": "end",
                        "type": "buttonEdge",
                        "zIndex": 1001
                    },
                    {
                        "data": {
                            "isHovered": false
                        },
                        "id": "xy-edge__Agent:KhakiSunsJudgestart-Message:GreatDucksArgueend",
                        "markerEnd": "logo",
                        "source": "Agent:KhakiSunsJudge",
                        "sourceHandle": "start",
                        "style": {
                            "stroke": "rgba(91, 93, 106, 1)",
                            "strokeWidth": 1
                        },
                        "target": "Message:GreatDucksArgue",
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
                                "prologue": "Hi! I'm an official AI customer service representative. How can I help you?"
                            },
                            "label": "Begin",
                            "name": "begin"
                        },
                        "id": "begin",
                        "measured": {
                            "height": 48,
                            "width": 200
                        },
                        "position": {
                            "x": 50,
                            "y": 200
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
                                "frequency_penalty": 0.5,
                                "items": [
                                    {
                                        "description": "This answer provide a specific contact information, like e-mail, phone number, wechat number, line number, twitter, discord, etc,.",
                                        "examples": [
                                            {
                                                "value": "My phone number is 203921\nkevinhu.hk@gmail.com\nThis is my discord number: johndowson_29384\n13212123432\n8379829"
                                            }
                                        ],
                                        "name": "1. contact",
                                        "uuid": "e4d754a5-a33e-4096-8648-8688e5474a15"
                                    },
                                    {
                                        "description": "The question is not about the product usage, appearance and how it works. Just casual chat.",
                                        "examples": [
                                            {
                                                "value": "How are you doing?\nWhat is your name?\nAre you a robot?\nWhat's the weather?\nWill it rain?"
                                            }
                                        ],
                                        "name": "2. casual",
                                        "uuid": "8cbf6ea3-a176-490d-9f8c-86373c932583"
                                    },
                                    {
                                        "description": "Complain even curse about the product or service you provide. But the comment is not specific enough.",
                                        "examples": [
                                            {
                                                "value": "How bad is it.\nIt's really sucks.\nDamn, for God's sake, can it be more steady?\nShit, I just can't use this shit.\nI can't stand it anymore."
                                            }
                                        ],
                                        "name": "3. complain",
                                        "uuid": "acc40a78-1b9e-4d2f-b5d6-64e01ab69269"
                                    },
                                    {
                                        "description": "The question is about the product usage, appearance and how it works.",
                                        "examples": [
                                            {
                                                "value": "Why it always beaming?\nHow to install it onto the wall?\nIt leaks, what to do?\nException: Can't connect to ES cluster\nHow to build the RAGFlow image from scratch"
                                            }
                                        ],
                                        "name": "4. product related",
                                        "uuid": "dfa5eead-9341-4f22-9236-068dbfb745e8"
                                    }
                                ],
                                "llm_id": "deepseek-chat@DeepSeek",
                                "maxTokensEnabled": false,
                                "max_tokens": 4096,
                                "message_history_window_size": 1,
                                "outputs": {
                                    "category_name": {
                                        "type": "string"
                                    }
                                },
                                "parameter": "Precise",
                                "presencePenaltyEnabled": false,
                                "presence_penalty": 0.5,
                                "query": "sys.query",
                                "temperature": "0.1",
                                "temperatureEnabled": true,
                                "topPEnabled": false,
                                "top_p": 0.75
                            },
                            "label": "Categorize",
                            "name": "Categorize"
                        },
                        "dragging": false,
                        "id": "Categorize:DullFriendsThank",
                        "measured": {
                            "height": 204,
                            "width": 200
                        },
                        "position": {
                            "x": 377.1140727959881,
                            "y": 138.1799140251472
                        },
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "categorizeNode"
                    },
                    {
                        "data": {
                            "form": {
                                "content": [
                                    "Okay, I've already write this down. What else I can do for you?",
                                    "Get it. What else I can do for you?",
                                    "Thanks for your trust! Our expert will contact ASAP. So, anything else I can do for you?",
                                    "Thanks! So, anything else I can do for you?"
                                ]
                            },
                            "label": "Message",
                            "name": "What else?"
                        },
                        "dragging": false,
                        "id": "Message:BreezyDonutsHeal",
                        "measured": {
                            "height": 56,
                            "width": 200
                        },
                        "position": {
                            "x": 724.8348409169271,
                            "y": 60.09138437270154
                        },
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "messageNode"
                    },
                    {
                        "data": {
                            "form": {
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
                                "sys_prompt": "You are a friendly and casual conversational assistant.  \n\nYour primary goal is to engage users in light and enjoyable daily conversation.  \n\n- Keep a natural, relaxed, and positive tone.  \n\n- Avoid sensitive, controversial, or negative topics.  \n\n- You may gently guide the conversation by introducing related casual topics if the user shows interest.  \n\n",
                                "temperature": 0.5,
                                "temperatureEnabled": true,
                                "tools": [],
                                "topPEnabled": false,
                                "top_p": 0.85,
                                "user_prompt": "",
                                "visual_files_var": ""
                            },
                            "label": "Agent",
                            "name": "Causal chat"
                        },
                        "dragging": false,
                        "id": "Agent:TwelveOwlsWatch",
                        "measured": {
                            "height": 84,
                            "width": 200
                        },
                        "position": {
                            "x": 720.4965892695689,
                            "y": 167.46311264481432
                        },
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "agentNode"
                    },
                    {
                        "data": {
                            "form": {
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
                            },
                            "label": "Agent",
                            "name": "Soothe mood"
                        },
                        "dragging": false,
                        "id": "Agent:DullTownsHope",
                        "measured": {
                            "height": 84,
                            "width": 200
                        },
                        "position": {
                            "x": 722.665715093248,
                            "y": 281.3422183879642
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
                                "kb_ids": [],
                                "keywords_similarity_weight": 0.7,
                                "outputs": {
                                    "formalized_content": {
                                        "type": "string",
                                        "value": ""
                                    }
                                },
                                "query": "sys.query",
                                "rerank_id": "",
                                "similarity_threshold": 0.2,
                                "top_k": 1024,
                                "top_n": 8,
                                "use_kg": false
                            },
                            "label": "Retrieval",
                            "name": "Search product info"
                        },
                        "dragging": false,
                        "id": "Retrieval:ShyPumasJoke",
                        "measured": {
                            "height": 56,
                            "width": 200
                        },
                        "position": {
                            "x": 645.6873721057459,
                            "y": 516.6923702571407
                        },
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "retrievalNode"
                    },
                    {
                        "data": {
                            "form": {
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
                            },
                            "label": "Agent",
                            "name": "Product info"
                        },
                        "dragging": false,
                        "id": "Agent:KhakiSunsJudge",
                        "measured": {
                            "height": 84,
                            "width": 200
                        },
                        "position": {
                            "x": 726.580040161058,
                            "y": 386.5448208363979
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
                                    "{Agent:TwelveOwlsWatch@content}{Agent:DullTownsHope@content}{Agent:KhakiSunsJudge@content}"
                                ]
                            },
                            "label": "Message",
                            "name": "Response"
                        },
                        "dragging": false,
                        "id": "Message:GreatDucksArgue",
                        "measured": {
                            "height": 56,
                            "width": 200
                        },
                        "position": {
                            "x": 1073.6401719497055,
                            "y": 279.1730925642852
                        },
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "messageNode"
                    },
                    {
                        "data": {
                            "form": {
                                "text": "This is an intelligent customer service processing system workflow based on user intent classification. It uses LLM to identify user demand types and transfers them to the corresponding professional agent for processing."
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
    "avatar": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwCAYAAABXAvmHAAAACXBIWXMAABYlAAAWJQFJUiTwAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAABJJSURBVHgBjVoJdB3Vef7mzrxNy9NiS9Zi2RLyJss2Yik2ZrWNwW5YXdo6JKWOk0M5OW3tUjg9Dm3Z2iTHNA0uxOUQSmPHAZuGUDaDIQESQoyJF4HxItnW4k2StUtPT9JbZvL/996ZN0+CJMN5fpqZN3f+9fu//78YmHCcaO243hHGbTac2wVENV8zDAPqW/4L/zU+NdQ/cOg/07RRWlyE1HgSz+/aido5c3H+/HkYQqBiWjlmzapBZVkZUskUhmJxuQQ/5x5yXXdN93CcRjhGY1qkHykvKmrzy+v9qrW1tTBpRh5yHGejuiHk0krQLxDavUPXHdtGVfkU7GtsxJbvPwErnIPainIcbz6Orp4LuGrJVWg7cxptbe3o7evDtddehwf/6QFMr6hA78AghBDZSvjfJ3VQSgpHPBETqUdqiooGvPez8OMi+B6dNRiOqR50bDo1PRX9i2UUoUVtIBwOwbYTWHvXWgz2DWHj/f+IRYsa8NJPtqFqTh2KSkpRO6MKixfVwaZHkmkbjYcP49HH/g2mZeKF7dsxOp5AKpWaJLz/vT5FGuOGvYyVkGonzcBDZNEGYVvKsg4/SLfERD9NOBwD4ZwIWk6fwlwKjfVf/xvs3fch7v7LO8nSrcjJz6d1LCTGxnDkeBN+/PLrGOXQGY7hopqL8PKLu/DE5s1YSt44eeoEcnLCnqW/6NAKNeTa4iF5fqy1o9qw7FbDNtk/MGzh09rR4SJ8K/jik77SyVEsmDcbjUebUVpaguHhIRQXFmLbT3ZhbKgfucWliETCXqiNJxO4409vQkBY5IkUecRGWXEhvvr1e3DnX9yJG0iZkdEx/D4vuJ6IpxPLhAjYD0mBpPCGsrrhTBbWk9/wFqiumIKrr1yMfYc+QUF+EQaH4hTLBniZ/p5umIGgjO10Oi2f5GciwRB+9tpb6OztRk4kQq8T6Ojpx7YfPo3XX3sDnx09Css0vXf4vycpkxYENI7RoEFECm/o0GAlDEdgcvyoxfLzc/Hvj38fa79yN2qqZmIskdBpD8QpnuMjIxq1HJaAckT9zbrk5uTig737sf+zI3IdVrq7bxA/fGoLvvXP/4q8nNDnC8+xYPqTHbcZTWc6HCW78ARXMhpSW/eSuq6usUWLCnJw2eWX4eCBAxgYGpOWZoHJo4jFxrHjuWdRUlaBSH4BWVRIGBUECo6RhilCdM7K2CgsimL5NVdiPD5OSqYRi49gx4934L6NGymxx7MRj17ASW/Tcyn67VgirYPbxXd/6PiAxhXetUhJyVR8+/HvYfnyZQhQSLAFleMcWJS0XRcuIBgMwDADMsZZePaCIxcjpztpBj

[... Content truncated for brevity ...]
```

## High-Level Overview

This file is part of the RAGFlow repository located at `agent/templates/customer_support.json`.

Based on the file structure and naming, it appears to be a data/configuration - json data or configuration file.

The file contains approximately 886 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

This is a configuration or data file. See the 'Original Source' section for full content.

## Code Structure Analysis

- Total lines: 886
- Blank lines: 1 (0.1%)
- Comment lines: ~0 (0.0%)
- Code lines: ~885


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `agent` directory, specifically within `agent/templates`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 21 loop(s) - consider algorithmic complexity
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

- Other files in `agent/templates/` directory
- Potential test file: `test_customer_support.json`

## Keywords

A0gLPej8hyFRAAAAAElFTkSuQmCC, A33DsKkJKdrTKE, ASAP, Accuracy, Agent, Agenten, AllGuestsShow, AllThingsHide, Always, Any, Avoid, B6dNRiOqR50bDo1PRX9i2UUoUVtIBwOwbYTWHvXWgz2DWHj, BCkZknAfCowS1Od6MiUMynU6ivLIc3fS7GFGRrU9, BL5kHEN7QEaKkPivZLF1BDneuesYVipIQu, BXbetoiJnIS, BaTlhSuooYSZGP7sGdLdMNWLowVRwnlLC, Balance, Begin, Benutzerabsichtsklassifizierung, Benutzeranforderungstypen, BreezyDonutsHeal, BreezyDonutsHealend, But, BxG2GbPrMTI0iDyi0ZKUGMITypkgeoZ, CJMN5fpqZN3f, CNF1EqvM1qtkxYv, Can, Categorize, Causal, Complain, Could, Critical, Customer, D4vuJ6IpxPLhAjYD0mBpPCGsrrhTBbWk9, DBxnYuHYyMNSUowhrVZc2qJvyRkbrB38goK0EzbTMuvX4a6, DE5s1YSt44eeoEcnLCnqW, DFW37QKjZ98imFKytNnzmCUIHfDA, DS9u245p1PfGic3, Database, DeepSeek, Description, Dies, DullFriendsThank, DullFriendsThank8cbf6ea3, DullFriendsThankacc40a78, DullFriendsThankdfa5eead, DullFriendsThanke4d754a5, DullFriendsThankend, DullTownsHope, DullTownsHopeend...

---
*Generated by RAGFlow Repository Documentation Generator*
