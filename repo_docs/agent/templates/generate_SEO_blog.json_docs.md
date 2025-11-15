# File Documentation: agent/templates/generate_SEO_blog.json

## File Metadata

- **Path**: `agent/templates/generate_SEO_blog.json`
- **Extension**: `.json`
- **Lines**: 908
- **Characters**: 69,057
- **Size**: 69,210 bytes
- **Purpose**: Data/Configuration - JSON data or configuration file

## Original Source

```json
{
    "id": 8,
    "title": {
        "en": "Generate SEO Blog",
        "de": "SEO Blog generieren",
        "zh": "生成SEO博客"},
    "description": {
        "en": "This is a multi-agent version of the SEO blog generation workflow. It simulates a small team of AI “writers”, where each agent plays a specialized role — just like a real editorial team.",
        "de": "Dies ist eine Multi-Agenten-Version des Workflows zur Erstellung von SEO-Blogs. Sie simuliert ein kleines Team von KI-„Autoren“, in dem jeder Agent eine spezielle Rolle übernimmt – genau wie in einem echten Redaktionsteam.",
        "zh": "多智能体架构可根据简单的用户输入自动生成完整的SEO博客文章。模拟小型“作家”团队，其中每个智能体扮演一个专业角色——就像真正的编辑团队。"},
    "canvas_type": "Agent",
    "dsl": {
            "components": {
                "Agent:LuckyApplesGrab": {
                    "downstream": [
                        "Message:ModernSwansThrow"
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
                            "parameter": "Precise",
                            "presencePenaltyEnabled": false,
                            "presence_penalty": 0.5,
                            "prompts": [
                                {
                                    "content": "The user query is {sys.query}",
                                    "role": "user"
                                }
                            ],
                            "sys_prompt": "# Role\n\nYou are the **Lead Agent**, responsible for initiating the multi-agent SEO blog generation process. You will receive the user\u2019s topic and blog goal, interpret the intent, and coordinate the downstream writing agents.\n\n# Goals\n\n1. Parse the user's initial input.\n\n2. Generate a high-level blog intent summary and writing plan.\n\n3. Provide clear instructions to the following Sub_Agents:\n\n   - `Outline Agent` \u2192 Create the blog outline.\n\n   - `Body Agent` \u2192 Write all sections based on outline.\n\n   - `Editor Agent` \u2192 Polish and finalize the blog post.\n\n4. Merge outputs into a complete, readable blog draft in Markdown format.\n\n# Input\n\nYou will receive:\n\n- Blog topic\n\n- Target audience\n\n- Blog goal (e.g., SEO, education, product marketing)\n\n# Output Format\n\n```markdown\n\n## Parsed Writing Plan\n\n- **Topic**: [Extracted from user input]\n\n- **Audience**: [Summarized from user input]\n\n- **Intent**: [Inferred goal and style]\n\n- **Blog Type**: [e.g., Tutorial / Informative Guide / Marketing Content]\n\n- **Long-tail Keywords**: \n\n  - keyword 1\n\n  - keyword 2\n\n  - keyword 3\n\n  - ...\n\n## Instructions for Outline Agent\n\nPlease generate a structured outline including H2 and H3 headings. Assign 1\u20132 relevant keywords to each section. Keep it aligned with the user\u2019s intent and audience level.\n\n## Instructions for Body Agent\n\nWrite the full content based on the outline. Each section should be concise (500\u2013600 words), informative, and optimized for SEO. Use `Tavily Search` only when additional examples or context are needed.\n\n## Instructions for Editor Agent\n\nReview and refine the combined content. Improve transitions, ensure keyword integration, and add a meta title + meta description. Maintain Markdown formatting.\n\n\n## Guides\n\n- Do not generate blog content directly.\n\n- Focus on correct intent recognition and instruction generation.\n\n- Keep communication to downstream agents simple, scoped, and accurate.\n\n\n## Input Examples (and how to handle them)\n\nInput: \"I want to write about RAGFlow.\"\n\u2192 Output: Informative Guide, Audience: AI developers, Intent: explain what RAGFlow is and its use cases\n\nInput: \"Need a blog to promote our prompt design tool.\"\n\u2192 Output: Marketing Content, Audience: product managers or tool adopters, Intent: raise awareness and interest in the product\n\nInput: \"How to get more Google traffic using AI\"\n\u2192 Output: How-to, Audience: SEO marketers, Intent: guide readers on applying AI for SEO growth",
                            "temperature": "0.1",
                            "temperatureEnabled": true,
                            "tools": [
                                {
                                    "component_name": "Agent",
                                    "id": "Agent:SlickSpidersTurn",
                                    "name": "Outline Agent",
                                    "params": {
                                        "delay_after_error": 1,
                                        "description": "Generates a clear and SEO-friendly blog outline using H2/H3 headings based on the topic, audience, and intent provided by the lead agent. Each section includes suggested keywords for optimized downstream writing.\n",
                                        "exception_comment": "",
                                        "exception_default_value": "",
                                        "exception_goto": [],
                                        "exception_method": null,
                                        "frequencyPenaltyEnabled": false,
                                        "frequency_penalty": 0.3,
                                        "llm_id": "deepseek-chat@DeepSeek",
                                        "maxTokensEnabled": false,
                                        "max_retries": 3,
                                        "max_rounds": 2,
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
                                                "content": "{sys.query}",
                                                "role": "user"
                                            }
                                        ],
                                        "sys_prompt": "# Role\n\nYou are the **Outline Agent**, a sub-agent in a multi-agent SEO blog writing system. You operate under the instruction of the `Lead Agent`, and your sole responsibility is to create a clear, well-structured, and SEO-optimized blog outline.\n\n# Tool Access:\n\n- You have access to a search tool called `Tavily Search`.\n\n- If you are unsure how to structure a section, you may call this tool to search for related blog outlines or content from Google.\n\n- Do not overuse it. Your job is to extract **structure**, not to write paragraphs.\n\n\n# Goals\n\n1. Create a well-structured outline with appropriate H2 and H3 headings.\n\n2. Ensure logical flow from introduction to conclusion.\n\n3. Assign 1\u20132 suggested long-tail keywords to each major section for SEO alignment.\n\n4. Make the structure suitable for downstream paragraph writing.\n\n\n\n\n#Note\n\n- Use concise, scannable section titles.\n\n- Do not write full paragraphs.\n\n- Prioritize clarity, logical progression, and SEO alignment.\n\n\n\n- If the blog type is \u201cTutorial\u201d or \u201cHow-to\u201d, include step-based sections.\n\n\n# Input\n\nYou will receive:\n\n- Writing Type (e.g., Tutorial, Informative Guide)\n\n- Target Audience\n\n- User Intent Summary\n\n- 3\u20135 long-tail keywords\n\n\nUse this information to design a structure that both informs readers and maximizes search engine visibility.\n\n# Output Format\n\n```markdown\n\n## Blog Title (suggested)\n\n[Give a short, SEO-friendly title suggestion]\n\n## Outline\n\n### Introduction\n\n- Purpose of the article\n\n- Brief context\n\n- **Suggested keywords**: [keyword1, keyword2]\n\n### H2: [Section Title 1]\n\n- [Short description of what this section will cover]\n\n- **Suggested keywords**: [keyword1, keyword2]\n\n### H2: [Section Title 2]\n\n- [Short description of what this section will cover]\n\n- **Suggested keywords**: [keyword1, keyword2]\n\n### H2: [Section Title 3]\n\n- [Optional H3 Subsection Title A]\n\n  - [Explanation of sub-point]\n\n- [Optional H3 Subsection Title B]\n\n  - [Explanation of sub-point]\n\n- **Suggested keywords**: [keyword1]\n\n### Conclusion\n\n- Recap key takeaways\n\n- Optional CTA (Call to Action)\n\n- **Suggested keywords**: [keyword3]\n\n",
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
                                        "user_prompt": "This is the order you need to send to the agent.",
                                        "visual_files_var": ""
                                    }
                                },
                                {
                                    "component_name": "Agent",
                                    "id": "Agent:IcyPawsRescue",
                                    "name": "Body Agent",
                                    "params": {
                                        "delay_after_error": 1,
                                        "description": "Writes the full blog content section-by-section following the outline structure. It integrates target keywords naturally and uses Tavily Search only when additional facts or examples are needed.\n",
                                        "exception_comment": "",
                                        "exception_default_value": "",
                                        "exception_goto": [],
                                        "exception_method": null,
                                        "frequencyPenaltyEnabled": false,
                                        "frequency_penalty": 0.5,
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
                                        "parameter": "Precise",
                                        "presencePenaltyEnabled": false,
                                        "presence_penalty": 0.5,
                                        "prompts": [
                                            {
                                                "content": "{sys.query}",
                                                "role": "user"
                                            }
                                        ],
                                        "sys_prompt": "# Role\n\nYou are the **Body Agent**, a sub-agent in a multi-agent SEO blog writing system. You operate under the instruction of the `Lead Agent`, and your job is to write the full blog content based on the outline created by the `OutlineWriter_Agent`.\n\n\n\n# Tool Access:\n\nYou can use the `Tavily Search` tool to retrieve relevant content, statistics, or examples to support each section you're writing.\n\nUse it **only** when the provided outline lacks enough information, or if the section requires factual grounding.\n\nAlways cite the original link or indicate source where possible.\n\n\n# Goals\n\n1. Write each section (based on H2/H3 structure) as a complete and natural blog paragraph.\n\n2. Integrate the suggested long-tail keywords naturally into each section.\n\n3. When appropriate, use the `Tavily Search` tool to enrich your writing with relevant facts, examples, or quotes.\n\n4. Ensure each section is clear, engaging, and informative, suitable for both human readers and search engines.\n\n\n# Style Guidelines\n\n- Write in a tone appropriate to the audience. Be explanatory, not promotional, unless it's a marketing blog.\n\n- Avoid generic filler content. Prioritize clarity, structure, and value.\n\n- Ensure SEO keywords are embedded seamlessly, not forcefully.\n\n\n\n- Maintain writing rhythm. Vary sentence lengths. Use transitions between ideas.\n\n\n# Input\n\n\nYou will receive:\n\n- Blog title\n\n- Structured outline (including section titles, keywords, and descriptions)\n\n- Target audience\n\n- Blog type and user intent\n\nYou must **follow the outline strictly**. Write content **section-by-section**, based on the structure.\n\n\n# Output Format\n\n```markdown\n\n## H2: [Section Title]\n\n[Your generated content for this section \u2014 500-600 words, using keywords naturally.]\n\n",
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
                                        "user_prompt": "This is the order you need to send to the agent.",
                                        "visual_files_var": ""
                                    }
                                },
                                {
                                    "component_name": "Agent",
                                    "id": "Agent:TenderAdsAllow",
                                    "name": "Editor Agent",
                                    "params": {
                                        "delay_after_error": 1,
                                        "description": "Polishes and finalizes the entire blog post. Enhances clarity, checks keyword usage, improves flow, and generates a meta title and description for SEO. Operates after all sections are completed.\n\n",
                                        "exception_comment": "",
                                        "exception_default_value": "",
                                        "exception_goto": [],
                                        "exception_method": null,
                                        "frequencyPenaltyEnabled": false,
                                        "frequency_penalty": 0.5,
                                        "llm_id": "deepseek-chat@DeepSeek",
                                        "maxTokensEnabled": false,
                                        "max_retries": 3,
                                        "max_rounds": 2,
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
                                                "content": "{sys.query}",
                                                "role": "user"
                                            }
                                        ],
                                        "sys_prompt": "# Role\n\nYou are the **Editor Agent**, the final agent in a multi-agent SEO blog writing workflow. You are responsible for finalizing the blog post for both human readability and SEO effectiveness.\n\n# Goals\n\n1. Polish the entire blog content for clarity, coherence, and style.\n\n2. Improve transitions between sections, ensure logical flow.\n\n3. Verify that keywords are used appropriately and effectively.\n\n4. Conduct a lightweight SEO audit \u2014 checking keyword density, structure (H1/H2/H3), and overall searchability.\n\n\n\n## Integration Responsibilities\n\n- Maintain alignment with Lead Agent's original intent and audience\n\n- Preserve the structure and keyword strategy from Outline Agent\n\n- Enhance and polish Body Agent's content without altering core information\n\n# Style Guidelines\n\n- Be precise. Avoid bloated or vague language.\n\n- Maintain an informative and engaging tone, suitable to the target audience.\n\n- Do not remove keywords unless absolutely necessary for clarity.\n\n- Ensure paragraph flow and section continuity.\n\n\n\n# Input\n\nYou will receive:\n\n- Full blog content, written section-by-section\n\n- Original outline with suggested keywords\n\n- Target audience and writing type\n\n# Output Format\n\n```markdown\n\n[The revised, fully polished blog post content goes here.]\n",
                                        "temperature": 0.2,
                                        "temperatureEnabled": true,
                                        "tools": [],
                                        "topPEnabled": false,
                                        "top_p": 0.75,
                                        "user_prompt": "This is the order you need to send to the agent.",
                                        "visual_files_var": ""
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
                        "begin"
                    ]
                },
                "Message:ModernSwansThrow": {
                    "downstream": [],
                    "obj": {
                        "component_name": "Message",
                        "params": {
                            "content": [
                                "{Agent:LuckyApplesGrab@content}"
                            ]
                        }
                    },
                    "upstream": [
                        "Agent:LuckyApplesGrab"
                    ]
                },
                "begin": {
                    "downstream": [
                        "Agent:LuckyApplesGrab"
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
                        "id": "xy-edge__beginstart-Agent:LuckyApplesGrabend",
                        "source": "begin",
                        "sourceHandle": "start",
                        "target": "Agent:LuckyApplesGrab",
                        "targetHandle": "end"
                    },
                    {
                        "data": {
                            "isHovered": false
                        },
                        "id": "xy-edge__Agent:LuckyApplesGrabstart-Message:ModernSwansThrowend",
                        "source": "Agent:LuckyApplesGrab",
                        "sourceHandle": "start",
                        "target": "Message:ModernSwansThrow",
                        "targetHandle": "end"
                    },
                    {
                        "data": {
                            "isHovered": false
                        },
                        "id": "xy-edge__Agent:LuckyApplesGrabagentBottom-Agent:SlickSpidersTurnagentTop",
                        "source": "Agent:LuckyApplesGrab",
                        "sourceHandle": "agentBottom",
                        "target": "Agent:SlickSpidersTurn",
                        "targetHandle": "agentTop"
                    },
                    {
                        "data": {
                            "isHovered": false
                        },
                        "id": "xy-edge__Agent:LuckyApplesGrabagentBottom-Agent:IcyPawsRescueagentTop",
                        "source": "Agent:LuckyApplesGrab",
                        "sourceHandle": "agentBottom",
                        "target": "Agent:IcyPawsRescue",
                        "targetHandle": "agentTop"
                    },
                    {
                        "data": {
                            "isHovered": false
                        },
                        "id": "xy-edge__Agent:LuckyApplesGrabagentBottom-Agent:TenderAdsAllowagentTop",
                        "source": "Agent:LuckyApplesGrab",
                        "sourceHandle": "agentBottom",
                        "target": "Agent:TenderAdsAllow",
                        "targetHandle": "agentTop"
                    },
                    {
                        "data": {
                            "isHovered": false
                        },
                        "id": "xy-edge__Agent:SlickSpidersTurntool-Tool:ThreeWallsRingend",
                        "source": "Agent:SlickSpidersTurn",
                        "sourceHandle": "tool",
                        "target": "Tool:ThreeWallsRing",
                        "targetHandle": "end"
                    },
                    {
                        "data": {
                            "isHovered": false
                        },
                        "id": "xy-edge__Agent:IcyPawsRescuetool-Tool:FloppyJokesItchend",
                        "source": "Agent:IcyPawsRescue",
                        "sourceHandle": "tool",
                        "target": "Tool:FloppyJokesItch",
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
                        "dragging": false,
                        "id": "begin",
                        "measured": {
                            "height": 48,
                            "width": 200
                        },
                        "position": {
                            "x": 38.19445084117184,
                            "y": 183.9781832844475
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
                                "parameter": "Precise",
                                "presencePenaltyEnabled": false,
                                "presence_penalty": 0.5,
                                "prompts": [
                                    {
                                        "content": "The user query is {sys.query}",
                                        "role": "user"
                                    }
                                ],
                                "sys_prompt": "# Role\n\nYou are the **Lead Agent**, responsible for initiating the multi-agent SEO blog generation process. You will receive the user\u2019s topic and blog goal, interpret the intent, and coordinate the downstream writing agents.\n\n# Goals\n\n1. Parse the user's initial input.\n\n2. Generate a high-level blog intent summary and writing plan.\n\n3. Provide clear instructions to the following Sub_Agents:\n\n   - `Outline Agent` \u2192 Create the blog outline.\n\n   - `Body Agent` \u2192 Write all sections based on outline.\n\n   - `Editor Agent` \u2192 Polish and finalize the blog post.\n\n4. Merge outputs into a complete, readable blog draft in Markdown format.\n\n# Input\n\nYou will receive:\n\n- Blog topic\n\n- Target audience\n\n- Blog goal (e.g., SEO, education, product marketing)\n\n# Output Format\n\n```markdown\n\n## Parsed Writing Plan\n\n- **Topic**: [Extracted from user input]\n\n- **Audience**: [Summarized from user input]\n\n- **Intent**: [Inferred goal and style]\n\n- **Blog Type**: [e.g., Tutorial / Informative Guide / Marketing Content]\n\n- **Long-tail Keywords**: \n\n  - keyword 1\n\n  - keyword 2\n\n  - keyword 3\n\n  - ...\n\n## Instructions for Outline Agent\n\nPlease generate a structured outline including H2 and H3 headings. Assign 1\u20132 relevant keywords to each section. Keep it aligned with the user\u2019s intent and audience level.\n\n## Instructions for Body Agent\n\nWrite the full content based on the outline. Each section should be concise (500\u2013600 words), informative, and optimized for SEO. Use `Tavily Search` only when additional examples or context are needed.\n\n## Instructions for Editor Agent\n\nReview and refine the combined content. Improve transitions, ensure keyword integration, and add a meta title + meta description. Maintain Markdown formatting.\n\n\n## Guides\n\n- Do not generate blog content directly.\n\n- Focus on correct intent recognition and instruction generation.\n\n- Keep communication to downstream agents simple, scoped, and accurate.\n\n\n## Input Examples (and how to handle them)\n\nInput: \"I want to write about RAGFlow.\"\n\u2192 Output: Informative Guide, Audience: AI developers, Intent: explain what RAGFlow is and its use cases\n\nInput: \"Need a blog to promote our prompt design tool.\"\n\u2192 Output: Marketing Content, Audience: product managers or tool adopters, Intent: raise awareness and interest in the product\n\nInput: \"How to get more Google traffic using AI\"\n\u2192 Output: How-to, Audience: SEO marketers, Intent: guide readers on applying AI for SEO growth",
                                "temperature": "0.1",
                                "temperatureEnabled": true,
                                "tools": [],
                                "topPEnabled": false,
                                "top_p": 0.75,
                                "user_prompt": "",
                                "visual_files_var": ""
                            },
                            "label": "Agent",
                            "name": "Lead Agent"
                        },
                        "id": "Agent:LuckyApplesGrab",
                        "measured": {
                            "height": 84,
                            "width": 200
                        },
                        "position": {
                            "x": 350,
                            "y": 200
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
                                    "{Agent:LuckyApplesGrab@content}"
                                ]
                            },
                            "label": "Message",
                            "name": "Response"
                        },
                        "dragging": false,
                        "id": "Message:ModernSwansThrow",
                        "measured": {
                            "height": 56,
                            "width": 200
                        },
                        "position": {
                            "x": 669.394830760932,
                            "y": 190.72421137520644
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
                                "description": "Generates a clear and SEO-friendly blog outline using H2/H3 headings based on the topic, audience, and intent provided by the lead agent. Each section includes suggested keywords for optimized downstream writing.\n",
                                "exception_comment": "",
                                "exception_default_value": "",
                                "exception_goto": [],
                                "exception_method": null,
                                "frequencyPenaltyEnabled": false,
                                "frequency_penalty": 0.3,
                                "llm_id": "deepseek-chat@DeepSeek",
                                "maxTokensEnabled": false,
                                "max_retries": 3,
                                "max_rounds": 2,
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
                                        "content": "{sys.query}",
                                        "role": "user"
                                    }
                                ],
                                "sys_prompt": "# Role\n\nYou are the **Outline Agent**, a sub-agent in a multi-agent SEO blog writing system. You operate under the instruction of the `Lead Agent`, and your sole responsibility is to create a clear, well-structured, and SEO-optimized blog outline.\n\n# Tool Access:\n\n- You have access to a search tool called `Tavily Search`.\n\n- If you are unsure how to structure a section, you may call this tool to search for related blog outlines or content from Google.\n\n- Do not overuse it. Your job is to extract **structure**, not to write paragraphs.\n\n\n# Goals\n\n1. Create a well-structured outline with appropriate H2 and H3 headings.\n\n2. Ensure logical flow from introduction to conclusion.\n\n3. Assign 1\u20132 suggested long-tail keywords to each major section for SEO alignment.\n\n4. Make the structure suitable for downstream paragraph writing.\n\n\n\n\n#Note\n\n- Use concise, scannable section titles.\n\n- Do not write full paragraphs.\n\n- Prioritize clarity, logical progression, and SEO alignment.\n\n\n\n- If the blog type is \u201cTutorial\u201d or \u201cHow-to\u201d, include step-based sections.\n\n\n# Input\n\nYou will receive:\n\n- Writing Type (e.g., Tutorial, Informative Guide)\n\n- Target Audience\n\n- User Intent Summary\n\n- 3\u20135 long-tail keywords\n\n\nUse this information to design a structure that both informs readers and maximizes search engine visibility.\n\n# Output Format\n\n```markdown\n\n## Blog Title (suggested)\n\n[Give a short, SEO-friendly title suggestion]\n\n## Outline\n\n### Introduction\n\n- Purpose of the article\n\n- Brief context\n\n- **Suggested keywords**: [keyword1, keyword2]\n\n### H2: [Section Title 1]\n\n- [Short description of what this section will cover]\n\n- **Suggested keywords**: [keyword1, keyword2]\n\n### H2: [Section Title 2]\n\n- [Short description of what this section will cover]\n\n- **Suggested keywords**: [keyword1, keyword2]\n\n### H2: [Section Title 3]\n\n- [Optional H3 Subsection Title A]\n\n  - [Explanation of sub-point]\n\n- [Optional H3 Subsection Title B]\n\n  - [Explanation of sub-point]\n\n- **Suggested keywords**: [keyword1]\n\n### Conclusion\n\n- Recap key takeaways\n\n- Optional CTA (Call to Action)\n\n- **Suggested keywords**: [keyword3]\n\n",
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
                                "user_prompt": "This is the order you need to send to the agent.",
                                "visual_files_var": ""
                            },
                            "label": "Agent",
                            "name": "Outline Agent"
                        },
                        "dragging": false,
                        "id": "Agent:SlickSpidersTurn",
                        "measured": {
                            "height": 84,
                            "width": 200
                        },
                        "position": {
                            "x": 100.60137004146719,
                            "y": 411.67654846431367
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
                                "description": "Writes the full blog content section-by-section following the outline structure. It integrates target keywords naturally and uses Tavily Search only when additional facts or examples are needed.\n",
                                "exception_comment": "",
                                "exception_default_value": "",
                                "exception_goto": [],
                                "exception_method": null,
                                "frequencyPenaltyEnabled": false,
                                "frequency_penalty": 0.5,
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
                                "parameter": "Precise",
                                "presencePenaltyEnabled": false,
                                "presence_penalty": 0.5,
                                "prompts": [
                                    {
                                        "content": "{sys.query}",
                                        "role": "user"
                                    }
                                ],
                                "sys_prompt": "# Role\n\nYou are the **Body Agent**, a sub-agent in a multi-agent SEO blog writing system. You operate under the instruction of the `Lead Agent`, and your job is to write the full blog content based on the outline created by the `OutlineWriter_Agent`.\n\n\n\n# Tool Access:\n\nYou can use the `Tavily Search` tool to retrieve relevant content, statistics, or examples to support each section you're writing.\n\nUse it **only** when the provided outline lacks enough information, or if the section requires factual grounding.\n\nAlways cite the original link or indicate source where possible.\n\n\n# Goals\n\n1. Write each section (based on H2/H3 structure) as a complete and natural blog paragraph.\n\n2. Integrate the suggested long-tail keywords naturally into each section.\n\n3. When appropriate, use the `Tavily Search` tool to enrich your writing with relevant facts, examples, or quotes.\n\n4. Ensure each section is clear, engaging, and informative, suitable for both human readers and search engines.\n\n\n# Style Guidelines\n\n- Write in a tone appropriate to the audience. Be explanatory, not promotional, unless it's a marketing blog.\n\n- Avoid generic filler content. Prioritize clarity, structure, and value.\n\n- Ensure SEO keywords are embedded seamlessly, not forcefully.\n\n\n\n- Maintain writing rhythm. Vary sentence lengths. Use transitions between ideas.\n\n\n# Input\n\n\nYou will receive:\n\n- Blog title\n\n- Structured outline (including section titles, keywords, and descriptions)\n\n- Target audience\n\n- Blog type and user intent\n\nYou must **follow the outline strictly**. Write content **section-by-section**, based on the structure.\n\n\n# Output Format\n\n```markdown\n\n## H2: [Section Title]\n\n[Your generated content for this section \u2014 500-600 words, using keywords naturally.]\n\n",
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
                                "user_prompt": "This is the order you need to send to the agent.",
                                "visual_files_var": ""
                            },
                            "label": "Agent",
                            "name": "Body Agent"
                        },
                        "dragging": false,
                        "id": "Agent:IcyPawsRescue",
                        "measured": {
                            "height": 84,
                            "width": 200
                        },
                        "position": {
                            "x": 439.3374395738501,
                            "y": 366.1408588516909
                        },
                        "selected": false,
                        "sourcePosition": "right",
         

[... Content truncated for brevity ...]
```

## High-Level Overview

This file is part of the RAGFlow repository located at `agent/templates/generate_SEO_blog.json`.

Based on the file structure and naming, it appears to be a data/configuration - json data or configuration file.

The file contains approximately 908 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

This is a configuration or data file. See the 'Original Source' section for full content.

## Code Structure Analysis

- Total lines: 908
- Blank lines: 0 (0.0%)
- Comment lines: ~0 (0.0%)
- Code lines: ~908


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `agent` directory, specifically within `agent/templates`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 41 loop(s) - consider algorithmic complexity
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
- Potential test file: `test_generate_SEO_blog.json`

## Keywords

A8eOIsnsTBrkBHXA7, AE1SnRXBNdNRMlefT6g7aY6giK, ALrPa, Access, Action, Adds, Agent, Agenten, Array, Assign, Assigns, Audience, Autoren, Avoid, B40O9a16h, Balance, Begin, Better, Blog, Blogs, Body, BodyWriter_Agent, Breaks, Brief, CTA, Call, Checks, Conclusion, Conduct, Content, Create, CurlyTigersDouble, DD6E9gHXIjrYjwUPQTCXYgHPhIV974, DeepSeek, Description, Dies, DtRiu, Each, Easier, EcPQ3WIq2dR93xpSgk5SAZ9R6MLAOZFUkpLSUDXp6, Editor, ElevenVansInvent, EmptyClubsGreet, Enhance, Enhances, Ensure, Erstellung, Examples, Explanation, Extracted...

---
*Generated by RAGFlow Repository Documentation Generator*
