# Documentation: agent/templates/market_generate_seo_blog.json

## File Metadata

- **Path**: `agent/templates/market_generate_seo_blog.json`
- **Size**: 63665 bytes
- **Type**: .json
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/templates/market_generate_seo_blog.json`.

## Original Source Code

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
                            "sys_prompt": "# Role\n\nYou are the **Parse_And_Keyword_Agent**, responsible for interpreting a user's blog writing request and generating a structured writing intent summary and keyword strategy for SEO-optimized content generation.\n\n# Goals\n\n1. Extract and infer the user's true writing intent, even if the input is informal or vague.\n\n2. Identify the writing type, target audience, and implied goal.\n\n3. Suggest 3\u20135 long-tail keywords based on the input and context.\n\n4. Output all data in a Markdown format for downstream agents.\n\n# Operating Guidelines\n\n\n- If the user's input lacks clarity, make reasonable and **conservative** assumptions based on SEO best practices.\n\n- Always choose one clear \"Writing Type\" from the list below.\n\n- Your job is not to write the blog \u2014 only to structure the brief.\n\n# Output Format\n\n```markdown\n## Writing Type\n\n[Choose one: Tutorial / Informative Guide / Marketing Content / Case Study / Opinion Piece / How-to / Comparison Article]\n\n## Target Audience\n\n[Try to be specific based on clues in the input: e.g., marketing managers, junior developers, SEO beginners]\n\n## User Intent Summary\n\n[A 1\u20132 sentence summary of what the user wants to achieve with the blo

... [Content truncated - file is 63532 bytes] ...

t does**:\n- Understands the main topic you want to write about  \n- Identifies who the blog is for (e.g., beginners, marketers, developers)  \n- Determines the writing purpose (e.g., SEO traffic, product promotion, education)  \n- Suggests 3\u20135 long-tail SEO keywords related to the topic"
                            },
                            "label": "Note",
                            "name": "Parse And Keyword Agent"
                        },
                        "dragHandle": ".note-drag-handle",
                        "dragging": false,
                        "height": 152,
                        "id": "Note:EmptyChairsShake",
                        "measured": {
                            "height": 152,
                            "width": 340
                        },
                        "position": {
                            "x": 295.04147626768133,
                            "y": 372.2755718118446
                        },
                        "resizing": false,
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "noteNode",
                        "width": 340
                    },
                    {
                        "data": {
                            "form": {
                                "text": "**Purpose**:  \nThis agent builds the blog structure \u2014 just like writing a table of contents before you start writing the full article.\n\n**What it does**:\n- Suggests a clear blog title that includes important keywords  \n- Breaks the article into sections using H2 and H3 headings (like a professional blog layout)  \n- Assigns 1\u20132 recommended keywords to each section to help with SEO  \n- Follows the writing goal and target audience set in the previous step"
                            },
                            "label": "Note",
                            "name": "Outline Agent"
                        },
                        "dragHandle": ".note-drag-handle",
                        "dragging": false,
                        "height": 146,
                        "id": "Note:TallMelonsNotice",
                        "measured": {
                            "height": 146,
                            "width": 343
                        },
                        "position": {
                            "x": 598.5644991893463,
                            "y": 5.801054564756448
                        },
                        "resizing": false,
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "noteNode",
                        "width": 343
                    },
                    {
                        "data": {
                            "form": {
                                "text": "**Purpose**:  \nThis agent is responsible for writing the actual content of the blog \u2014 paragraph by paragraph \u2014 based on the outline created earlier.\n\n**What it does**:\n- Looks at each H2/H3 section in the outline  \n- Writes 150\u2013220 words of clear, helpful, and well-structured content per section  \n- Includes the suggested SEO keywords naturally (not keyword stuffing)  \n- Uses real examples or facts if needed (by calling a web search tool like Tavily)"
                            },
                            "label": "Note",
                            "name": "Body Agent"
                        },
                        "dragHandle": ".note-drag-handle",
                        "dragging": false,
                        "height": 137,
                        "id": "Note:RipeCougarsBuild",
                        "measured": {
                            "height": 137,
                            "width": 319
                        },
                        "position": {
                            "x": 860.4854129814981,
                            "y": 427.2196835690842
                        },
                        "resizing": false,
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "noteNode",
                        "width": 319
                    },
                    {
                        "data": {
                            "form": {
                                "text": "**Purpose**:  \nThis agent reviews the entire blog draft to make sure it is smooth, professional, and SEO-friendly. It acts like a human editor before publishing.\n\n**What it does**:\n- Polishes the writing: improves sentence clarity, fixes awkward phrasing  \n- Makes sure the content flows well from one section to the next  \n- Double-checks keyword usage: are they present, natural, and not overused?  \n- Verifies the blog structure (H1, H2, H3 headings) is correct  \n- Adds two key SEO elements:\n  - **Meta Title** (shows up in search results)\n  - **Meta Description** (summary for Google and social sharing)"
                            },
                            "label": "Note",
                            "name": "Editor Agent"
                        },
                        "dragHandle": ".note-drag-handle",
                        "height": 146,
                        "id": "Note:OpenTurkeysSell",
                        "measured": {
                            "height": 146,
                            "width": 320
                        },
                        "position": {
                            "x": 1129,
                            "y": -30
                        },
                        "resizing": false,
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "noteNode",
                        "width": 320
                    }
                ]
            },
            "history": [],
            "messages": [],
            "path": [],
            "retrieval": []
        },
    "avatar": "data:image/jpeg;base64,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"
}
```

## Detailed Analysis

### File Role in Repository

The file `agent/templates/market_generate_seo_blog.json` is located in the `agent/templates` directory.

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
