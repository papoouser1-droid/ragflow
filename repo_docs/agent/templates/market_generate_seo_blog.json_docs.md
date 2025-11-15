# File Documentation: agent/templates/market_generate_seo_blog.json

## File Metadata

- **Path**: `agent/templates/market_generate_seo_blog.json`
- **Extension**: `.json`
- **Lines**: 921
- **Characters**: 63,532
- **Size**: 63,665 bytes
- **Purpose**: Data/Configuration - JSON data or configuration file

## Original Source

```json
{
    "id": 12,
    "title": {
        "en": "Generate SEO Blog",
        "de": "SEO Blog generieren",
        "zh": "生成SEO博客"},
    "description": {
        "en": "This workflow automatically generates a complete SEO-optimized blog article based on a simple user input. You don't need any writing experience. Just provide a topic or short request — the system will handle the rest.",
        "de": "Dieser Workflow generiert automatisch einen vollständigen SEO-optimierten Blogartikel basierend auf einer einfachen Benutzereingabe. Sie benötigen keine Schreiberfahrung. Geben Sie einfach ein Thema oder eine kurze Anfrage ein – das System übernimmt den Rest.",
        "zh": "此工作流根据简单的用户输入自动生成完整的SEO博客文章。你无需任何写作经验，只需提供一个主题或简短请求，系统将处理其余部分。"},
    "canvas_type": "Marketing",
    "dsl": {
            "components": {
                "Agent:BetterSitesSend": {
                    "downstream": [
                        "Agent:EagerNailsRemain"
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
                            "max_rounds": 3,
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
                                    "content": "The parse and keyword agent output is {Agent:ClearRabbitsScream@content}",
                                    "role": "user"
                                }
                            ],
                            "sys_prompt": "# Role\n\nYou are the **Outline_Agent**, responsible for generating a clear and SEO-optimized blog outline based on the user's parsed writing intent and keyword strategy.\n\n# Tool Access:\n\n- You have access to a search tool called `Tavily Search`.\n\n- If you are unsure how to structure a section, you may call this tool to search for related blog outlines or content from Google.\n\n- Do not overuse it. Your job is to extract **structure**, not to write paragraphs.\n\n\n# Goals\n\n1. Create a well-structured outline with appropriate H2 and H3 headings.\n\n2. Ensure logical flow from introduction to conclusion.\n\n3. Assign 1\u20132 suggested long-tail keywords to each major section for SEO alignment.\n\n4. Make the structure suitable for downstream paragraph writing.\n\n\n\n\n#Note\n\n- Use concise, scannable section titles.\n\n- Do not write full paragraphs.\n\n- Prioritize clarity, logical progression, and SEO alignment.\n\n\n\n- If the blog type is \u201cTutorial\u201d or \u201cHow-to\u201d, include step-based sections.\n\n\n# Input\n\nYou will receive:\n\n- Writing Type (e.g., Tutorial, Informative Guide)\n\n- Target Audience\n\n- User Intent Summary\n\n- 3\u20135 long-tail keywords\n\n\nUse this information to design a structure that both informs readers and maximizes search engine visibility.\n\n# Output Format\n\n```markdown\n\n## Blog Title (suggested)\n\n[Give a short, SEO-friendly title suggestion]\n\n## Outline\n\n### Introduction\n\n- Purpose of the article\n\n- Brief context\n\n- **Suggested keywords**: [keyword1, keyword2]\n\n### H2: [Section Title 1]\n\n- [Short description of what this section will cover]\n\n- **Suggested keywords**: [keyword1, keyword2]\n\n### H2: [Section Title 2]\n\n- [Short description of what this section will cover]\n\n- **Suggested keywords**: [keyword1, keyword2]\n\n### H2: [Section Title 3]\n\n- [Optional H3 Subsection Title A]\n\n  - [Explanation of sub-point]\n\n- [Optional H3 Subsection Title B]\n\n  - [Explanation of sub-point]\n\n- **Suggested keywords**: [keyword1]\n\n### Conclusion\n\n- Recap key takeaways\n\n- Optional CTA (Call to Action)\n\n- **Suggested keywords**: [keyword3]\n\n",
                            "temperature": 0.5,
                            "temperatureEnabled": true,
                            "tools": [
                                {
                                    "component_name": "TavilySearch",
                                    "name": "TavilySearch",
                                    "params": {
                                        "api_key": "",
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
                            "top_p": 0.85,
                            "user_prompt": "",
                            "visual_files_var": ""
                        }
                    },
                    "upstream": [
                        "Agent:ClearRabbitsScream"
                    ]
                },
                "Agent:ClearRabbitsScream": {
                    "downstream": [
                        "Agent:BetterSitesSend"
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
                            "frequency_penalty": 0.5,
                            "llm_id": "deepseek-chat@DeepSeek",
                            "maxTokensEnabled": false,
                            "max_retries": 3,
                            "max_rounds": 1,
                            "max_tokens": 4096,
                            "mcp": [],
                            "message_history_window_size": 12,
                            "outputs": {
                                "content": {
                                    "type": "string",
                                    "value": ""
                                }
                            },
                            "parameter": "Precise",
                            "presencePenaltyEnabled": false,
                            "presence_penalty": 0.5,
                            "prompts": [
                                {
                                    "content": "The user query is {sys.query}",
                                    "role": "user"
                                }
                            ],
                            "sys_prompt": "# Role\n\nYou are the **Parse_And_Keyword_Agent**, responsible for interpreting a user's blog writing request and generating a structured writing intent summary and keyword strategy for SEO-optimized content generation.\n\n# Goals\n\n1. Extract and infer the user's true writing intent, even if the input is informal or vague.\n\n2. Identify the writing type, target audience, and implied goal.\n\n3. Suggest 3\u20135 long-tail keywords based on the input and context.\n\n4. Output all data in a Markdown format for downstream agents.\n\n# Operating Guidelines\n\n\n- If the user's input lacks clarity, make reasonable and **conservative** assumptions based on SEO best practices.\n\n- Always choose one clear \"Writing Type\" from the list below.\n\n- Your job is not to write the blog \u2014 only to structure the brief.\n\n# Output Format\n\n```markdown\n## Writing Type\n\n[Choose one: Tutorial / Informative Guide / Marketing Content / Case Study / Opinion Piece / How-to / Comparison Article]\n\n## Target Audience\n\n[Try to be specific based on clues in the input: e.g., marketing managers, junior developers, SEO beginners]\n\n## User Intent Summary\n\n[A 1\u20132 sentence summary of what the user wants to achieve with the blog post]\n\n## Suggested Long-tail Keywords\n\n- keyword 1\n\n- keyword 2\n\n- keyword 3\n\n- keyword 4 (optional)\n\n- keyword 5 (optional)\n\n\n\n\n## Input Examples (and how to handle them)\n\nInput: \"I want to write about RAGFlow.\"\n\u2192 Output: Informative Guide, Audience: AI developers, Intent: explain what RAGFlow is and its use cases\n\nInput: \"Need a blog to promote our prompt design tool.\"\n\u2192 Output: Marketing Content, Audience: product managers or tool adopters, Intent: raise awareness and interest in the product\n\n\n\nInput: \"How to get more Google traffic using AI\"\n\u2192 Output: How-to, Audience: SEO marketers, Intent: guide readers on applying AI for SEO growth",
                            "temperature": 0.2,
                            "temperatureEnabled": true,
                            "tools": [],
                            "topPEnabled": false,
                            "top_p": 0.75,
                            "user_prompt": "",
                            "visual_files_var": ""
                        }
                    },
                    "upstream": [
                        "begin"
                    ]
                },
                "Agent:EagerNailsRemain": {
                    "downstream": [
                        "Agent:LovelyHeadsOwn"
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
                            "frequency_penalty": 0.5,
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
                            "parameter": "Precise",
                            "presencePenaltyEnabled": false,
                            "presence_penalty": 0.5,
                            "prompts": [
                                {
                                    "content": "The parse and keyword agent output is {Agent:ClearRabbitsScream@content}\n\n\n\nThe Outline agent output is {Agent:BetterSitesSend@content}",
                                    "role": "user"
                                }
                            ],
                            "sys_prompt": "# Role\n\nYou are the **Body_Agent**, responsible for generating the full content of each section of an SEO-optimized blog based on the provided outline and keyword strategy.\n\n# Tool Access:\n\nYou can use the `Tavily Search` tool to retrieve relevant content, statistics, or examples to support each section you're writing.\n\nUse it **only** when the provided outline lacks enough information, or if the section requires factual grounding.\n\nAlways cite the original link or indicate source where possible.\n\n\n# Goals\n\n1. Write each section (based on H2/H3 structure) as a complete and natural blog paragraph.\n\n2. Integrate the suggested long-tail keywords naturally into each section.\n\n3. When appropriate, use the `Tavily Search` tool to enrich your writing with relevant facts, examples, or quotes.\n\n4. Ensure each section is clear, engaging, and informative, suitable for both human readers and search engines.\n\n\n# Style Guidelines\n\n- Write in a tone appropriate to the audience. Be explanatory, not promotional, unless it's a marketing blog.\n\n- Avoid generic filler content. Prioritize clarity, structure, and value.\n\n- Ensure SEO keywords are embedded seamlessly, not forcefully.\n\n\n\n- Maintain writing rhythm. Vary sentence lengths. Use transitions between ideas.\n\n\n# Input\n\n\nYou will receive:\n\n- Blog title\n\n- Structured outline (including section titles, keywords, and descriptions)\n\n- Target audience\n\n- Blog type and user intent\n\nYou must **follow the outline strictly**. Write content **section-by-section**, based on the structure.\n\n\n# Output Format\n\n```markdown\n\n## H2: [Section Title]\n\n[Your generated content for this section \u2014 500-600 words, using keywords naturally.]\n\n",
                            "temperature": 0.2,
                            "temperatureEnabled": true,
                            "tools": [
                                {
                                    "component_name": "TavilySearch",
                                    "name": "TavilySearch",
                                    "params": {
                                        "api_key": "",
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
                            "top_p": 0.75,
                            "user_prompt": "",
                            "visual_files_var": ""
                        }
                    },
                    "upstream": [
                        "Agent:BetterSitesSend"
                    ]
                },
                "Agent:LovelyHeadsOwn": {
                    "downstream": [
                        "Message:LegalBeansBet"
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
                            "frequency_penalty": 0.5,
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
                            "parameter": "Precise",
                            "presencePenaltyEnabled": false,
                            "presence_penalty": 0.5,
                            "prompts": [
                                {
                                    "content": "The parse and keyword agent output is {Agent:ClearRabbitsScream@content}\n\nThe Outline agent output is {Agent:BetterSitesSend@content}\n\nThe Body agent output is {Agent:EagerNailsRemain@content}",
                                    "role": "user"
                                }
                            ],
                            "sys_prompt": "# Role\n\nYou are the **Editor_Agent**, responsible for finalizing the blog post for both human readability and SEO effectiveness.\n\n# Goals\n\n1. Polish the entire blog content for clarity, coherence, and style.\n\n2. Improve transitions between sections, ensure logical flow.\n\n3. Verify that keywords are used appropriately and effectively.\n\n4. Conduct a lightweight SEO audit \u2014 checking keyword density, structure (H1/H2/H3), and overall searchability.\n\n\n\n# Style Guidelines\n\n- Be precise. Avoid bloated or vague language.\n\n- Maintain an informative and engaging tone, suitable to the target audience.\n\n- Do not remove keywords unless absolutely necessary for clarity.\n\n- Ensure paragraph flow and section continuity.\n\n\n# Input\n\nYou will receive:\n\n- Full blog content, written section-by-section\n\n- Original outline with suggested keywords\n\n- Target audience and writing type\n\n# Output Format\n\n```markdown\n\n[The revised, fully polished blog post content goes here.]\n\n",
                            "temperature": 0.2,
                            "temperatureEnabled": true,
                            "tools": [],
                            "topPEnabled": false,
                            "top_p": 0.75,
                            "user_prompt": "",
                            "visual_files_var": ""
                        }
                    },
                    "upstream": [
                        "Agent:EagerNailsRemain"
                    ]
                },
                "Message:LegalBeansBet": {
                    "downstream": [],
                    "obj": {
                        "component_name": "Message",
                        "params": {
                            "content": [
                                "{Agent:LovelyHeadsOwn@content}"
                            ]
                        }
                    },
                    "upstream": [
                        "Agent:LovelyHeadsOwn"
                    ]
                },
                "begin": {
                    "downstream": [
                        "Agent:ClearRabbitsScream"
                    ],
                    "obj": {
                        "component_name": "Begin",
                        "params": {
                            "enablePrologue": true,
                            "inputs": {},
                            "mode": "conversational",
                            "prologue": "Hi! I'm your SEO blog assistant.\n\nTo get started, please tell me:\n1. What topic you want the blog to cover\n2. Who is the target audience\n3. What you hope to achieve with this blog (e.g., SEO traffic, teaching beginners, promoting a product)\n"
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
                        "id": "xy-edge__beginstart-Agent:ClearRabbitsScreamend",
                        "source": "begin",
                        "sourceHandle": "start",
                        "target": "Agent:ClearRabbitsScream",
                        "targetHandle": "end"
                    },
                    {
                        "data": {
                            "isHovered": false
                        },
                        "id": "xy-edge__Agent:ClearRabbitsScreamstart-Agent:BetterSitesSendend",
                        "source": "Agent:ClearRabbitsScream",
                        "sourceHandle": "start",
                        "target": "Agent:BetterSitesSend",
                        "targetHandle": "end"
                    },
                    {
                        "data": {
                            "isHovered": false
                        },
                        "id": "xy-edge__Agent:BetterSitesSendtool-Tool:SharpPensBurnend",
                        "source": "Agent:BetterSitesSend",
                        "sourceHandle": "tool",
                        "target": "Tool:SharpPensBurn",
                        "targetHandle": "end"
                    },
                    {
                        "data": {
                            "isHovered": false
                        },
                        "id": "xy-edge__Agent:BetterSitesSendstart-Agent:EagerNailsRemainend",
                        "source": "Agent:BetterSitesSend",
                        "sourceHandle": "start",
                        "target": "Agent:EagerNailsRemain",
                        "targetHandle": "end"
                    },
                    {
                        "id": "xy-edge__Agent:EagerNailsRemaintool-Tool:WickedDeerHealend",
                        "source": "Agent:EagerNailsRemain",
                        "sourceHandle": "tool",
                        "target": "Tool:WickedDeerHeal",
                        "targetHandle": "end"
                    },
                    {
                        "data": {
                            "isHovered": false
                        },
                        "id": "xy-edge__Agent:EagerNailsRemainstart-Agent:LovelyHeadsOwnend",
                        "source": "Agent:EagerNailsRemain",
                        "sourceHandle": "start",
                        "target": "Agent:LovelyHeadsOwn",
                        "targetHandle": "end"
                    },
                    {
                        "data": {
                            "isHovered": false
                        },
                        "id": "xy-edge__Agent:LovelyHeadsOwnstart-Message:LegalBeansBetend",
                        "source": "Agent:LovelyHeadsOwn",
                        "sourceHandle": "start",
                        "target": "Message:LegalBeansBet",
                        "targetHandle": "end"
                    }
                ],
                "nodes": [
                    {
                        "data": {
                            "form": {
                                "enablePrologue": true,
                                "inputs": {},
                                "mode": "conversational",
                                "prologue": "Hi! I'm your SEO blog assistant.\n\nTo get started, please tell me:\n1. What topic you want the blog to cover\n2. Who is the target audience\n3. What you hope to achieve with this blog (e.g., SEO traffic, teaching beginners, promoting a product)\n"
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
                                "delay_after_error": 1,
                                "description": "",
                                "exception_comment": "",
                                "exception_default_value": "",
                                "exception_goto": [],
                                "exception_method": null,
                                "frequencyPenaltyEnabled": false,
                                "frequency_penalty": 0.5,
                                "llm_id": "deepseek-chat@DeepSeek",
                                "maxTokensEnabled": false,
                                "max_retries": 3,
                                "max_rounds": 1,
                                "max_tokens": 4096,
                                "mcp": [],
                                "message_history_window_size": 12,
                                "outputs": {
                                    "content": {
                                        "type": "string",
                                        "value": ""
                                    }
                                },
                                "parameter": "Precise",
                                "presencePenaltyEnabled": false,
                                "presence_penalty": 0.5,
                                "prompts": [
                                    {
                                        "content": "The user query is {sys.query}",
                                        "role": "user"
                                    }
                                ],
                                "sys_prompt": "# Role\n\nYou are the **Parse_And_Keyword_Agent**, responsible for interpreting a user's blog writing request and generating a structured writing intent summary and keyword strategy for SEO-optimized content generation.\n\n# Goals\n\n1. Extract and infer the user's true writing intent, even if the input is informal or vague.\n\n2. Identify the writing type, target audience, and implied goal.\n\n3. Suggest 3\u20135 long-tail keywords based on the input and context.\n\n4. Output all data in a Markdown format for downstream agents.\n\n# Operating Guidelines\n\n\n- If the user's input lacks clarity, make reasonable and **conservative** assumptions based on SEO best practices.\n\n- Always choose one clear \"Writing Type\" from the list below.\n\n- Your job is not to write the blog \u2014 only to structure the brief.\n\n# Output Format\n\n```markdown\n## Writing Type\n\n[Choose one: Tutorial / Informative Guide / Marketing Content / Case Study / Opinion Piece / How-to / Comparison Article]\n\n## Target Audience\n\n[Try to be specific based on clues in the input: e.g., marketing managers, junior developers, SEO beginners]\n\n## User Intent Summary\n\n[A 1\u20132 sentence summary of what the user wants to achieve with the blog post]\n\n## Suggested Long-tail Keywords\n\n- keyword 1\n\n- keyword 2\n\n- keyword 3\n\n- keyword 4 (optional)\n\n- keyword 5 (optional)\n\n\n\n\n## Input Examples (and how to handle them)\n\nInput: \"I want to write about RAGFlow.\"\n\u2192 Output: Informative Guide, Audience: AI developers, Intent: explain what RAGFlow is and its use cases\n\nInput: \"Need a blog to promote our prompt design tool.\"\n\u2192 Output: Marketing Content, Audience: product managers or tool adopters, Intent: raise awareness and interest in the product\n\n\n\nInput: \"How to get more Google traffic using AI\"\n\u2192 Output: How-to, Audience: SEO marketers, Intent: guide readers on applying AI for SEO growth",
                                "temperature": 0.2,
                                "temperatureEnabled": true,
                                "tools": [],
                                "topPEnabled": false,
                                "top_p": 0.75,
                                "user_prompt": "",
                                "visual_files_var": ""
                            },
                            "label": "Agent",
                            "name": "Parse And Keyword Agent"
                        },
                        "dragging": false,
                        "id": "Agent:ClearRabbitsScream",
                        "measured": {
                            "height": 84,
                            "width": 200
                        },
                        "position": {
                            "x": 344.7766966202233,
                            "y": 234.82202253184496
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
                                "max_rounds": 3,
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
                                        "content": "The parse and keyword agent output is {Agent:ClearRabbitsScream@content}",
                                        "role": "user"
                                    }
                                ],
                                "sys_prompt": "# Role\n\nYou are the **Outline_Agent**, responsible for generating a clear and SEO-optimized blog outline based on the user's parsed writing intent and keyword strategy.\n\n# Tool Access:\n\n- You have access to a search tool called `Tavily Search`.\n\n- If you are unsure how to structure a section, you may call this tool to search for related blog outlines or content from Google.\n\n- Do not overuse it. Your job is to extract **structure**, not to write paragraphs.\n\n\n# Goals\n\n1. Create a well-structured outline with appropriate H2 and H3 headings.\n\n2. Ensure logical flow from introduction to conclusion.\n\n3. Assign 1\u20132 suggested long-tail keywords to each major section for SEO alignment.\n\n4. Make the structure suitable for downstream paragraph writing.\n\n\n\n\n#Note\n\n- Use concise, scannable section titles.\n\n- Do not write full paragraphs.\n\n- Prioritize clarity, logical progression, and SEO alignment.\n\n\n\n- If the blog type is \u201cTutorial\u201d or \u201cHow-to\u201d, include step-based sections.\n\n\n# Input\n\nYou will receive:\n\n- Writing Type (e.g., Tutorial, Informative Guide)\n\n- Target Audience\n\n- User Intent Summary\n\n- 3\u20135 long-tail keywords\n\n\nUse this information to design a structure that both informs readers and maximizes search engine visibility.\n\n# Output Format\n\n```markdown\n\n## Blog Title (suggested)\n\n[Give a short, SEO-friendly title suggestion]\n\n## Outline\n\n### Introduction\n\n- Purpose of the article\n\n- Brief context\n\n- **Suggested keywords**: [keyword1, keyword2]\n\n### H2: [Section Title 1]\n\n- [Short description of what this section will cover]\n\n- **Suggested keywords**: [keyword1, keyword2]\n\n### H2: [Section Title 2]\n\n- [Short description of what this section will cover]\n\n- **Suggested keywords**: [keyword1, keyword2]\n\n### H2: [Section Title 3]\n\n- [Optional H3 Subsection Title A]\n\n  - [Explanation of sub-point]\n\n- [Optional H3 Subsection Title B]\n\n  - [Explanation of sub-point]\n\n- **Suggested keywords**: [keyword1]\n\n### Conclusion\n\n- Recap key takeaways\n\n- Optional CTA (Call to Action)\n\n- **Suggested keywords**: [keyword3]\n\n",
                                "temperature": 0.5,
                                "temperatureEnabled": true,
                                "tools": [
                                    {
                                        "component_name": "TavilySearch",
                                        "name": "TavilySearch",
                                        "params": {
                                            "api_key": "",
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
                                "top_p": 0.85,
                                "user_prompt": "",
                                "visual_files_var": ""
                            },
                            "label": "Agent",
                            "name": "Outline Agent"
                        },
                        "dragging": false,
                        "id": "Agent:BetterSitesSend",
                        "measured": {
                            "height": 84,
                            "width": 200
                        },
                        "position": {
                            "x": 613.4368763415628,
                            "y": 164.3074269048589
                        },
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "agentNode"
                    },
                    {
                        "data": {
                            "form": {
                                "description": "This is an agent for a specific task.",
                                "user_prompt": "This is the order you need to send to the agent."
                            },
                            "label": "Tool",
                            "name": "flow.tool_0"
                        },
                        "dragging": false,
                        "id": "Tool:SharpPensBurn",
                        "measured": {
                            "height": 44,
                            "width": 200
                        },
                        "position": {
                            "x": 580.1877078861457,
                            "y": 287.7669662022325
                        },
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "toolNode"
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
                                "frequency_penalty": 0.5,
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
                                "parameter": "Precise",
                                "presencePenaltyEnabled": false,
                                "presence_penalty": 0.5,
                                "prompts": [
                                    {
                                        "content": "The parse and keyword agent output is {Agent:ClearRabbitsScream@content}\n\n\n\nThe Outline agent output is {Agent:BetterSitesSend@content}",
                                        "role": "user"
                                    }
                                ],
                                "sys_prompt": "# Role\n\nYou are the **Body_Agent**, responsible for generating the full content of each section of an SEO-optimized blog based on the provided outline and keyword strategy.\n\n# Tool Access:\n\nYou can use the `Tavily Search` tool to retrieve relevant content, statistics, or examples to support each section you're writing.\n\nUse it **only** when the provided outline lacks enough information, or if the section requires factual grounding.\n\nAlways cite the original link or indicate source where possible.\n\n\n# Goals\n\n1. Write each section (based on H2/H3 structure) as a complete and natural blog paragraph.\n\n2. Integrate the suggested long-tail keywords naturally into each section.\n\n3. When appropriate, use the `Tavily Search` tool to enrich your writing with relevant facts, examples, or quotes.\n\n4. Ensure each section is clear, engaging, and informative, suitable for both human readers and search engines.\n\n\n# Style Guidelines\n\n- Write in a tone appropriate to the audience. Be explanatory, not promotional, unless it's a marketing blog.\n\n- Avoid generic filler content. Prioritize clarity, structure, and value.\n\n- Ensure SEO keywords are embedded seamlessly, not forcefully.\n\n\n\n- Maintain writing rhythm. Vary sentence lengths. Use transitions between ideas.\n\n\n# Input\n\n\nYou will receive:\n\n- Blog title\n\n- Structured outline (including section titles, keywords, and descriptions)\n\n- Target audience\n\n- Blog type and user intent\n\nYou must **follow the outline strictly**. Write content **section-by-section**, based on the structure.\n\n\n# Output Format\n\n```markdown\n\n## H2: [Section Title]\n\n[Your generated content for this section \u2014 500-600 words, using keywords naturally.]\n\n",
                                "temperature": 0.2,
                                "temperatureEnabled": true,
                                "tools": [
                                    {
                                        "component_name": "TavilySearch",
                                        "name": "TavilySearch",
                                        "params": {
                                            "api_key": "",
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
                                "top_p": 0.75,
                                "user_prompt": "",
                                "visual_files_var": ""
                            },
                            "label": "Agent",
                            "name": "Body Agent"
                        },
                        "dragging": false,
                        "id": "Agent:EagerNailsRemain",
                        "measured": {
                            "height": 84,
                            "width": 200
                        },
                        "position": {
                            "x": 889.0614605692713,
                            "y": 247.00973041799065
                        },
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "agentNode"
                    },
                    {
                        "data": {
                            "form": {
                                "description": "This is an agent for a specific task.",
                                "user_prompt": "This is the order you need to send to the agent."
                            },
                            "label": "Tool",
                            "name": "flow.tool_1"
                        },
                        "dragging": false,
                        "id": "Tool:WickedDeerHeal",
                        "measured": {
                            "height": 44,
                            "width": 200
                        },
                        "position": {
                            "x": 853.2006404239659,
                            "y": 364.37541577229143
                        },
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "toolNode"
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
                                "frequency_penalty": 0.5,
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
                                "parameter": "Precise",
                                "presencePenaltyEnabled": false,
                                "presence_penalty": 0.5,
                                "prompts": [
                                    {
                                        "content": "The parse and keyword agent output is {Agent:ClearRabbitsScream@content}\n\nThe Outline agent output is {Agent:BetterSitesSend@content}\n\nThe Body agent output is {Agent:EagerNailsRemain@content}",
                                        "role": "user"
                                    }
                                ],
                                "sys_prompt": "# Role\n\nYou are the **Editor_Agent**, responsible for finalizing the blog post for both human readability and SEO effectiveness.\n\n# Goals\n\n1. Polish the entire blog content for clarity, coherence, and style.\n\n2. Improve transitions between sections, ensure logical flow.\n\n3. Verify that keywords are used appropriately and effectively.\n\n4. Conduct a lightweight SEO audit \u2014 checking keyword density, structure (H1/H2/H3), and overall searchability.\n\n\n\n# Style Guidelines\n\n- Be precise. Avoid bloated or vague language.\n\n- Maintain an informative and engaging tone, suitable to the target audience.\n\n- Do not remove keywords unless absolutely necessary for clarity.\n\n- Ensure paragraph flow and section continuity.\n\n\n# Input\n\nYou will receive:\n\n- Full blog content, written section-by-section\n\n- Original outline with suggested keywords\n\n- Target audience and writing type\n\n# Output Format\n\n```markdown\n\n[The revised, fully polished blog post content goes here.]\n\n",
                                "temperature": 0.2,
                                "temperatureEnabled": true,
                                "tools": [],
                                "topPEnabled": false,
                                "top_p": 0.75,
                                "user_prompt": "",
                                "visual_files_var": ""
                            },
           

[... Content truncated for brevity ...]
```

## High-Level Overview

This file is part of the RAGFlow repository located at `agent/templates/market_generate_seo_blog.json`.

Based on the file structure and naming, it appears to be a data/configuration - json data or configuration file.

The file contains approximately 921 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

This is a configuration or data file. See the 'Original Source' section for full content.

## Code Structure Analysis

- Total lines: 921
- Blank lines: 0 (0.0%)
- Comment lines: ~0 (0.0%)
- Code lines: ~921


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `agent` directory, specifically within `agent/templates`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 35 loop(s) - consider algorithmic complexity
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
- Potential test file: `test_market_generate_seo_blog.json`

## Keywords

A8eOIsnsTBrkBHXA7, AE1SnRXBNdNRMlefT6g7aY6giK, ALrPa, Access, Action, Adds, Agent, Always, And, Anfrage, Array, Article, Assign, Assigns, Audience, Avoid, B40O9a16h, Balance, Begin, Benutzereingabe, BetterSitesSend, BetterSitesSendend, BetterSitesSendstart, BetterSitesSendtool, Blog, Blogartikel, Body, Body_Agent, Breaks, Brief, CTA, Call, Case, Choose, ClearRabbitsScream, ClearRabbitsScreamend, ClearRabbitsScreamstart, Comparison, Conclusion, Conduct, Content, Create, DD6E9gHXIjrYjwUPQTCXYgHPhIV974, DeepSeek, Description, Designing, Determines, Dieser, Double, DtRiu...

---
*Generated by RAGFlow Repository Documentation Generator*
