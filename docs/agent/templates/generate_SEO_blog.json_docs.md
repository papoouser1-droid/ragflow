# Documentation: agent/templates/generate_SEO_blog.json

## File Metadata

- **Path**: `agent/templates/generate_SEO_blog.json`
- **Size**: 69210 bytes
- **Type**: .json
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/templates/generate_SEO_blog.json`.

## Original Source Code

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
                                        "temperatu

... [Content truncated - file is 69057 bytes] ...

ads your blog topic and intent\n- Generates a clear writing plan (topic, audience, goal, keywords)\n- Sends instructions to all sub-agents\n- Waits for their responses and checks quality\n- If any section is missing or weak, it can request a rewrite\n- Finally, it assembles all parts into a complete blog and sends it back to you\n"
                            },
                            "label": "Note",
                            "name": "Lead Agent"
                        },
                        "dragHandle": ".note-drag-handle",
                        "dragging": false,
                        "height": 146,
                        "id": "Note:EmptyClubsGreet",
                        "measured": {
                            "height": 146,
                            "width": 334
                        },
                        "position": {
                            "x": 390.1408623279084,
                            "y": 2.6521144030202493
                        },
                        "resizing": false,
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "noteNode",
                        "width": 334
                    },
                    {
                        "data": {
                            "form": {
                                "text": "**Purpose**:  \nThis agent is responsible for building the blog's structure. It creates an outline that shows what the article will cover and how it's organized.\n\n**What it does**:\n- Suggests a blog title that matches the topic and keywords  \n- Breaks the article into sections using H2 and H3 headers  \n- Adds a short description of what each section should include  \n- Assigns SEO keywords to each section for better search visibility  \n- Uses search data (via Tavily Search) to find how similar blogs are structured"
                            },
                            "label": "Note",
                            "name": "Outline Agent"
                        },
                        "dragHandle": ".note-drag-handle",
                        "dragging": false,
                        "height": 157,
                        "id": "Note:CurlyTigersDouble",
                        "measured": {
                            "height": 157,
                            "width": 394
                        },
                        "position": {
                            "x": -60.03139680691618,
                            "y": 595.8208080534818
                        },
                        "resizing": false,
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "noteNode",
                        "width": 394
                    },
                    {
                        "data": {
                            "form": {
                                "text": "**Purpose**:  \nThis agent is in charge of writing the full blog content, section by section, based on the outline it receives.\n\n**What it does**:\n- Takes each section heading from the outline (H2 / H3)\n- Writes a complete paragraph (150\u2013220 words) under each section\n- Naturally includes the keywords provided for that section\n- Uses the Tavily Search tool to add real-world examples, definitions, or facts if needed\n- Makes sure each section is clear, useful, and easy to read\n"
                            },
                            "label": "Note",
                            "name": "Body Agent"
                        },
                        "dragHandle": ".note-drag-handle",
                        "dragging": false,
                        "height": 164,
                        "id": "Note:StrongKingsCamp",
                        "measured": {
                            "height": 164,
                            "width": 408
                        },
                        "position": {
                            "x": 446.54943226110845,
                            "y": 590.9443887062529
                        },
                        "resizing": false,
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "noteNode",
                        "width": 408
                    },
                    {
                        "data": {
                            "form": {
                                "text": "**Purpose**:  \nThis agent reviews, polishes, and finalizes the blog post written by the BodyWriter_Agent. It ensures everything is clean, smooth, and SEO-compliant.\n\n**What it does**:\n- Improves grammar, sentence flow, and transitions  \n- Makes sure the content reads naturally and professionally  \n- Checks whether keywords are present and well integrated (but not overused)  \n- Verifies that the structure follows the correct H1/H2/H3 format  \n"
                            },
                            "label": "Note",
                            "name": "Editor Agent"
                        },
                        "dragHandle": ".note-drag-handle",
                        "dragging": false,
                        "height": 147,
                        "id": "Note:OpenOttersShow",
                        "measured": {
                            "height": 147,
                            "width": 357
                        },
                        "position": {
                            "x": 976.6858726228803,
                            "y": 422.7404806291804
                        },
                        "resizing": false,
                        "selected": false,
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "noteNode",
                        "width": 357
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

The file `agent/templates/generate_SEO_blog.json` is located in the `agent/templates` directory.

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
