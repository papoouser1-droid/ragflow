# Documentation: agent/templates/deep_search_r.json

## File Metadata

- **Path**: `agent/templates/deep_search_r.json`
- **Size**: 147606 bytes
- **Type**: .json
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/templates/deep_search_r.json`.

## Original Source Code

```json

{
    "id": 6,
    "title": {
        "en": "Deep Research",
        "de": "Tiefgehende Recherche",
        "zh": "深度研究"},
    "description": {
        "en": "For professionals in sales, marketing, policy, or consulting, the Multi-Agent Deep Research Agent conducts structured, multi-step investigations across diverse sources and delivers consulting-style reports with clear citations.",
        "de": "Für Fachleute in Vertrieb, Marketing, Politik oder Beratung führt der Multi-Agenten-Tiefenforschungsagent strukturierte, mehrstufige Untersuchungen über verschiedene Quellen durch und liefert Berichte im Beratungsstil mit klaren Quellenangaben.",
        "zh": "专为销售、市场、政策或咨询领域的专业人士设计，多智能体的深度研究会结合多源信息进行结构化、多步骤地回答问题，并附带有清晰的引用。"},
    "canvas_type": "Agent",
    "dsl": {
            "components": {
                "Agent:NewPumasLick": {
                    "downstream": [
                        "Message:OrangeYearsShine"
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
                            "llm_id": "qwen-max@Tongyi-Qianwen",
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
                            "sys_prompt": "You are a Strategy Research Director with 20 years of consulting experience at top-tier firms. Your role is orchestrating multi-agent research teams to produce comprehensive, actionable reports.\n\n\n<core_mission>\nTransform complex research needs into efficient multi-agent collaboration, ensuring high-quality ~2000-word strategic reports.\n</core_mission>\n\n\n<execution_framework>\n**Stage 1: URL Discovery** (2-3 minutes)\n- Deploy Web Search Specialist to identify 5 premium sources\n- Ensure comprehensive coverage across authoritative domains\n- Validate search strategy matches research scope\n\n\n**Stage 2: Content Extraction** (3-5 minutes)\n- Deploy Content Deep Reader to process 5 premium URLs\n- Focus on structured extraction with quality assessment\n- Ensure 80%+ extraction success rate\n\n\n**Stage 3: Strategic Report Generation** (5-8 minutes)\n- Deploy Research Synthesizer with detailed strategic analysis instructions\n- Provide specific analysis framework and business focus requirements\n- Generate comprehensive McKinsey-style strategic report (~2000 words)\n- Ensure multi-source validation and C-suite ready insights\n\n\n**Report Instructions Framework:**\n```\nANALYSIS_INSTRUCTIONS:\nAnalysis Type: [Market Analysis/Competitive Intelligence/Strategic Assessment]\nTarget Audience: [C-Suite/Board/Investment Committee/Strategy Team]\nBusiness Focus: [Market Entry/Competitive Positioning/Investment Decision/Strategic Planning]\nKey Questions: [3-5 specific strategic questions to address]\nAnalysis Depth: [Surface-level overview/Deep strategic analysis/Comprehensive assessment]\nDeliverable Style: [McKinsey report/BCG analysis/Deloitte assessment/Academic research]\n```\n</execution_framework>\n\n\n<research_process>\nFollow this process to break down the user's question and develop an excellent research plan. Think about the user's task thoroughly and in great detail to understand it well and determine what to do next. Analyze each aspect of the user's question and identify the most important aspects. Consider multiple approaches with complete, thorough reasoning. Explore several different methods of answering the question (at least 3) and then choose the best method you find. Follow this process closely:\n\n\n1. **Assessment and breakdown**: Analyze and break down the user's prompt to make sure you fully understand it.\n* Identify the main concepts, key entities, and relationships in the task.\n* List specific facts or data points needed to answer the question well.\n* Note any temporal or contextual constraints on the question.\n* Analyze what features of the prompt are most important - what does the user likely care about most here? What are they expecting or desiring in the final result? What tools do they expect to be used and how do we know?\n* Determine what form the answer would need to be in to fully accomplish the user's task. Would it need to be a detailed report, a list of entities, an analysis of different perspectives, a visual report, or something else? What components will it need to have?\n\n\n2. **Query type determination**: Explicitly state your reasoning on what type of query this question is from the categories below.\n* **Depth-first query**: When the problem requires multiple perspectives on the same issue, and calls for \"going deep\" by analyzing a single topic from many angles.\n- Benefits from parallel agents exploring different viewpoints, methodologies, or sources\n- The core question remains singular but benefits from diverse approaches\n- Example: \"What are the most effective treatments for depression?\" (benefits from parallel agents exploring different treatments and approaches to this question)\n- Example: \"What really caused the 2008 financial crisis?\" (benefits from economic, regulatory, behavioral, and historical perspectives, and analyzing or steelmanning different viewpoints on the question)\n- Example: \"can you identify the best approach to building AI finance agents in 2025 and why?\"\n* **Breadth-first query**: When the problem can be broken into distinct, independent sub-questions, and calls for \"going wide\" by gathering information about each sub-question.\n- Benefits from parallel agents each handling separate sub-topics.\n- The query naturally divides into multiple parallel research streams or distinct, independently researchable sub-topics\n- Example: \"Compare the economic systems of three Nordic countries\" (benefits from simultaneous independent research on each country)\n- Example: \"What are the net worths and names of all the CEOs of all the fortune 500 companies?\" (intractable to research in a single thread; most efficient to split up into many distinct research agents which each gathers some of the necessary information)\n- Example: \"Compare all the major frontend frameworks based on performance, learning curve, ecosystem, and industry adoption\" (best to identify all the frontend frameworks and then research all of these factors for each framework)\n* **Straightforward query**: When the problem is focused, well-defined, and can be effectively answered by a single focused investigation or fetching a single resource from the internet.\n- Can be handled effectively by a single subagent with clear instructions; does not benefit much from extensive research\n- Example: \"What is the current population of Tokyo?\" (simple fact-finding)\n- Example: \"What are all the fortune 500 companies?\" (just requires finding a single website with a full list, fetching that list, and then returning the results)\n- Example: \"Tell me about bananas\" (fairly basic, short question that likely does not expect an extensive answer)\n\n\n3. **Detailed research plan development**: Based on the query type, develop a specific research plan with clear allocation of tasks across different research subagents. Ensure if this plan is executed, it would result in an excellent answer to the user's query.\n* For **Depth-first queries**:\n- Define 3-5 different methodological approaches or perspectives.\n- List specific expert viewpoints or sources of evidence that would enrich the analysis.\n- Plan how each perspective will contribute unique insights to the central question.\n- Specify how findings from different approaches will be synthesized.\n- Example: For \"What causes obesity?\", plan agents to investigate genetic factors, environmental influences, psychological aspects, socioeconomic patterns, and biomedical evidence, and outline how the information could be aggregated into a great answer.\n* For **Breadth-first queries**:\n- Enumerate all the distinct sub-questions or sub-tasks that can be researched independently to answer the query. \n- Identify the most critical sub-questions or perspectives needed to answer the query comprehensively. Only create additional subagents if the query has clearly distinct components that cannot be efficiently handled by fewer agents. Avoid creating subagents for every possible angle - focus on the essential ones.\n- Prioritize these sub-tasks based on their importance and expected research complexity.\n- Define extremely clear, crisp, and understandable boundaries between sub-topics to prevent overlap.\n- Plan how findings will be aggregated into a coherent whole.\n- Example: For \"Compare EU country tax systems\", first create a subagent to retrieve a list of all the countries in the EU 

... [Content truncated - file is 147467 bytes] ...

thered research materials.\nMake sure the model has long context window."
                            },
                            "label": "Note",
                            "name": "Research Synthesizer Subagent"
                        },
                        "dragHandle": ".note-drag-handle",
                        "dragging": false,
                        "height": 170,
                        "id": "Note:ThickSchoolsStop",
                        "measured": {
                            "height": 170,
                            "width": 319
                        },
                        "position": {
                            "x": 1141.1845057663165,
                            "y": 329.7346968869334
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
                                "description": "This is an agent for a specific task.",
                                "user_prompt": "This is the order you need to send to the agent."
                            },
                            "label": "Tool",
                            "name": "flow.tool_1"
                        },
                        "id": "Tool:SlickYearsCough",
                        "measured": {
                            "height": 48,
                            "width": 200
                        },
                        "position": {
                            "x": 446.18055927306057,
                            "y": 476.88601989245177
                        },
                        "sourcePosition": "right",
                        "targetPosition": "left",
                        "type": "toolNode"
                    }
                ]
            },
            "history": [],
            "messages": [],
            "path": [],
            "retrieval": []
        },
    "avatar": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwCAYAAABXAvmHAAAACXBIWXMAABYlAAAWJQFJUiTwAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAABZmSURBVHgBbVoJjF3ldf7uf5e3LzPjWWyP7bEZFqekNoQEFBJSl0RJCQHU0ihN1CyNlLSqslRK1SaiAqSqJBUJTRuqpG1KwxIMNGCWhlAWm60gIHgIGBxsDzPG+4xn3szb37tLv3P+O8atYun3fe/e++4963e+c/5x8P/+7Xvx6d8p5JwrM653VeCbCcdPAGNgXE+vu66BYzw4jiyj5xyumP9HSJDEifxvz/GnPMFrcrSf7f1Gryf253oJSaIf7K/5SrxzLYriKX6aghtdP7D6vJnT5XVWPtx3387qYBXXlgPv65VMglJgkPF9eB5F8xzEgUs9jL7HqEJGlaA2fIjL5fCafX0ci0Kxvj2OYqTfrNhJkr7ZSV+fnC6G/Z44pz6HYgUxAn8Xy7OBf6BI1w9sPK92SoFbKHzec3aWPWzN5RIM+C7KVILKwPc9uH5sBfaoAJe1lMhOW1IBYxxrM7mUpNcpRBxH1rCp5eMk4rlUAd5s9BlU3llRLDnNW+/oqqKn56Ik5IOcKWPcbaKExkU9dK8N4Wzt8cbBtsunisWMHvNOHwE8BBTClbOxQexJGKm0KpQI6zhqdvWIKOXl83CyBf2ubw57CNttdJttVUiXRlWc+iBOpXVOeUcdcZo3EidBquHWMOxfyw9/4Vx3586JHNy3PKoSeC6KXohBhk8lE6OY8RhKQI7XMjwG9ILvhnAdWs21lhdFYgktiWx6zh8cxNvT83jgyVcxtWcfTpys0WAuBgeq2Dq5Dld+5AJsmhxD69gRhFTqlGl/4z9HjZOkUZaIsWLxbGjDNA63eVm417qOjU+Js1acgRtGCHmmTyt1IoOsb1AIRZEYGeZD1o3hm1AT2lBzY2J4I4PYt28en/vyD/DSa69jcGgAudIIHL+IDAUcqNfwxszbuOG2ezAyOo6ffOuP8Z53r8HC0RNwzemJnPwfdU7XT+KCUUnB5Si5llzl3HjXk7s9Y7aKJV1a1ONy+TnrRvCoWcZNkOPK0gt5WT4XFZDvOR49hk5pfBj/+JOn8Y2b70BxbBK5ARE8S2uFyJkASaennuITEXbaqCQL2Df7Oi7b9iHc94MvYv7AIb7XtQiUKhGnaKZYYNLwjBNFtoTC90NaNDQzznfvfjIJPAuJIrhZgTZ+l5iT4JC8lSVKlZkiRYZTIXCQc0KcvW4QX/nu3bj5ZztRPvO3mTZ0L4VxPR+R5AKt5TOxjcS6CNDvws8XsbaYx6/3PoV1Y+vx2kPfxOL+Y5ouAlpRiloaJ4J6yTuf1fphrOgWMlJMmzHWiywcOU6ieB7blOHNvMbVCh00+gYLvQCHuj5mOz4OtzzExRH8yXd34OYdzyBz5hZ0ui3e20efL+lFhAX6u01LFfOBQqBLSWK+o9GoYXaphpHJ9+Otw/vxiT//DwysrWpCO7EARaLL4fIiAYBYPxsKLcvjKY/PCfg806VwLQq5HCZ8GYuRJAktFlGxUHBYjo5dvMTlocs8qcUenp9dxq0/3o5VE2fC7yyz4PQR9nvoihL83A67fHcPhxZOqlX7fIYX+Mhks+jz2UvtBorj78HDTzyGXc9OUyiLWA5DiFGr1QUa1oAPG94ZV8CE0UAFsgIi19z5dOIxNiV8PMaaSXNBjuIRxQL9bKswI4cFzsP42jX40he/jPXvPo8WZ9JT6bAXot3uMD77ikoxfySWU9SgUP1+qLHbljhxXTVIOZNhGNQRtRZx9MFrMH9owaJobNHUpDlhVmo9f2QShSANJU/xR+NdLO7yhNZUW3HlsxYZJrbAs0Caa3F59sgxVKoDWL92jILYutBTYWN6QjzHoIkkcDx+Zw2g4P1eTxWUetBotNDjOVlRpoAT03vxPy/M4qyRHN/BHJIQSlQDK7AEoQgvmkmCp+eca+96PuHt9FRiq60QA2NzQhJaipLoIN4VL7mE0XzGx9TLr7FKJ/BzOb0YwiZYGFskkdAVqytyRAwgJlxPcoOIFGqOMGRZ1BqE1w6PS/PHcNYqH/dd81kstzp8f194CNVfYUWxLXqSI5qfkT7by2q4wBYniVGTqKAJsFIb1TsrFV684ZDY8efYes4GNDsRQ4jhwZv7TFwSL8tZUnLmpigiSvS5OnR7jyuiIJ2ueKSMdqcLs3Ejnvr5/TDFErotVqHQko2Q1MF6IEnZoRwcZYJyllwtJL5n4bMKZyRheFLcLRYXHiRVtpNYahGllCWkK4NeCxOrBnCy0WaxY3hI2IQBraygwZcIfHKxTmRUcXqHWjepaYvXO/zS7dMr3T66DC1D2G0TSOpJEY24SYMahWQ3cS0uxkhJnfWG/cAcaC4vM6VDVLIsOBS2S+EDJjN5HJKQ/3mBJlKH90tqiqD1bgcLcwsYZEHo9rrIhFm0SEF6XNkUssV+LBfIJppoCn0rIVaXFRpVpJd6JEfuVKnQIK0+an2By0Q94EgNgeVOtKYkouARFVS2Bu/I0aOo0G1xLk+EYVKETRSzvlo7lB8Q8joUZZEvYVSiTWtJ+Bw+voChrIMFSXjTo0fEHi5zIdI8ycUuDUGFEkEuSeV33F+MXOT57DpXh78RUCqWS3jXu87FnsMn0WGCu0IeBU6VU0anCqx4w2VIB66rz/T27HsDq1n665UyAroM7RqGSxmlyqJ9tlBFzWRxrMXkY8VudruM3Q5enZkhL/KUrarJBZ0kNAQbQymIPQsECcODlVkSwRPiJznhMDlFsUQUYLqaDErkJoOlCqZm5mzuKIRH5FyuVnA/rQeGymR5LPBLhtzMe/H1VzDAH5bzBZ70iAhNNHttjJdKWFMeQJ9Ebq7PYiel26gz0WrWcYLV9NVX9qOyKkuKQVfyutYIR9DaMkhHgEIKIJNXuze+XKwXC9+iR6U+M/hJEnljawkztWUcf3kRBcoSEnrF+gEBQ0OS0eFSmYAPrpAaD+YCFOS9YeLj+Pwijocn9I1aDI2PhW6INxodJiWTKBEWSlczP+TBBHgEZHXfueMJ3Pvtq7D/SAM9EYpxlxEuo6gDhVPB8kgZmS1qjtCJRIFQlZHiWSqVsXfvETz34jMoDgyl/STDOQo11rO5ouZUs91Szxg5R8UrxQo8X6zK1lEItLaGie2+JGdcxmgpk+NNGX1Zn3Shw2ob95nOpAoPP/043jh4GXOBSMKw6XviIY9KE7E8KJdKVvA47jOvxAg8J50aKUnsCpz67Pwc3Lj9F6gMDSObKbJIM7oT3t9rqtWrpWGUioPwM1nWjjrm5w6jsTSPeu1tRqZAJqV1aFUSGb4oVC4i5FcsaPhiIU2+4LHUanog4j39TgP5koOPfeUmrN44jpzvaAsqDVHZ66Hk9lD0+4RoJjJj1XdtMgcKbAwF31KUzesHcON/voz9B15kGBdRLrGPYHH0Xek7fBQodJVeWTW6GmtG1mJ0bBxVVu5BUpCKjgdisSYTjt2R6dd5bPFUi5hOQtZv040CaX0iQo/hEalbQyFu5C9Or4OluV/hok/fhLEN61Ap86WszvmAi3103kSpAuwL/B5XrP0EMQI5ItiFZw/gh4/uxx0/vRnDQ6uYR3xfr87+YRlOt6nGE/lyjPlCkEU+myeV5yoUaYispfpCcXuERoesUYchQsTIKCOeC+lOPxbFAu15pUGJo5Ze94TcURmPxW7Pnl0474oTuP/vv4JN52/AMnlSp9OxnD2SMDE27qVvJrQOVyusuBVcef1jeO6JuzA+tgo9RkBE4wn4CKvtddvMG4Ymi0WjuZaJPYQgWyaTJdyXqsh0msp0ncoFH00iogTkR1FoFYisMgmLWJl1ICv8VdgfhRZl+6ENMzlnIsF9tp9ejrkT4IvbLsQ3Pnc5Vp+5hqWcqUrYlQImsOxmi1hueLjh1qfws0dfQe34L5GvDDF/usqlojDR+VPIZ0ZEwqhV40SEoTK0BtXhCfiFQeTLg+jUF0gI6SGyW2f4ossS6S+VMfYta5QMNkQil8iTJwHPZXNaASX2ez3hOwwjJjLfaMmVDL2Eq7Nm1DusfcyHzUNVXHjOGThz/Tqt4NNHj2PX7gOYenU3Np//EWyaGMPDj+/A4MgQGsvzttPiOyMhg5K8FF4y0SHF8KprkM9XUR4aJ3ms0PXMNVZtYyt8olAl+C78Xz7L0UiykYmZFPsZa3wJeSvj2jC5JCmj0FU6nCip8tRz1QJ7YRam1xcbeHnnC3zBU+pdqREeq+fGyVEiWRsXXP11DGeauPOR/7IUWT0quUjDMLeMtIkmVsQKl0+gRghtsp8uDYyhsGo1PUNl2QF6EtcSc1CUsa62/TGLiC+DLY5KfJOSp8iODaWaUhgp7G5iewDtXZXydnkbc4TPLdJ7gceMDRVTtfFZIs/pz/wct3yzhk/83qUECnIp0hh5ZsJQkkGX8K8wbWaStDc2UVfzot5aRr5XQXP5JHpLC1RAwoAvVH4tJVxilaHj8SEBMz9DBPD4XdilKKcNizQikQ0lMZ52czJecV3tH6CTDQoRR6lyke0TJIc6LbzvA5fi2RensfnDn8EZj9+JE22GrgCCEyCihyO+D5FvaYeO4/qqnBfNkaV00OLcKmkuEgnJpIbOOf86FYAmdylAhrgbcPmZPFeOmc+sp3Cuaydm2mqK7aXShUIaQlJxhhRdmpP7g0AbIAk7o/UE2iz5Oghz1asteqFxaA9mT0a4YNMkXtn/Chv/orasvhiOzzKe0cmGGE9yzKy0vKEYu6c52CcSeavG1vIDoVHQh273AlEg4MN8rcA+K460hlLg+v0OR4NLdCUpBtGl69b1vNyXFUUD9restl2CgcNmRUNRlHYdpSi2xyEt4YApYte1Z/u3MD1xCS6+eCumDx6iVXxFIOmpnZD9Aam6jE5E+Ry7wCg0CjCCmm7UUTbgrRmfIKdvKz1QjkPhfQokIeH5OQ0p4ixfHOkosE2y1yfG98laW40G5We8+wG5EqcNtJ4Qt2ajThoi0zsOwWiAoJCnNT07caAlfd5/iNWsHeaxPPsKhi/fhunZA/R0gIFKRZud9Sxs565Zj2HC7GK3jh/dexdK7BnaNIywUqlLAuvO52+8nYpEOjmQZHGJ+RnhPsbVsBDTSZ7Asf1ol9aPaOEOEaHXWLQKSM7Q8oHcK4nGmU/Eo3D2QrFMylEiTfV0WCYG6bGpf3tmP/773nsYshkMbrkSN3z6g/CXDQ6/MYXh8UkcOXQQB5damDs2jcmtF+GjV1yGr33vbxgZWdT70ppCEcr52g8fSGRoqvSbRxXGeEroxGoyORc+rkMvnUmGWg+iHmdAbCtZqdSqVJmLnqJ3OsscjQgSMd5zLF7ZcpWQFmhDJEkZtprsqxNc+9d/pW1so7ABn1izkSOWHH49/SzWn7GFlH0Jh956A5dc8TkStwWMrt2AT37qCvzr/bdiZq6FI4t17Qa9TCAzCRcrbbtMnF1NGmjS6QQap29FJNpUWPjNaMMtvw5IxX2ZnpHDhEVPZ6YyqHIICKJEzNC0E2UmX4bT71IB771gC156YTey3ZN44kgPn1p3NhZbLeQOv8X6wgEZ+5FOrY7KwDAWF+Zw508fxFe/8Fk8/MJTeGnvQRyer+n8lo03tIlWazt2SoF0nLIy1FhBH90jcBVbiMFZRRpXhHcy2qAEOemgsoLPOuVwpKdmcktCh4LrpAs+edA9t2zHSJ30hVYpdubQ4PMPHNuHGgElPjqDBRpoXXU15hdPwBT4exmEkYDteOBp/OmfXYHJF36JvdOzYqhQ493VyVdiB0AyXnRiO7qQgW86ZFEvGRlz0OICa06cXnOVKst2mk++LD+Va74jRYlPNXYFMn6RUkiUqZKWHnzzIArZDLrVi0nSKtjz1gP49jVf5Ry2ifF1o7jle3dj/uTbyHNvwTV2X6BCIvji8/vx0d99LybXDvPxfFjMqbPj2imcyo10y0hoRRpcRrk3Q5lHGa1rQw17FCWM0hAJv0Q3SnwdltmBsZHhEOm4UIOYA4Aec+fKP7wUS/veRPNYC79iJ9dlg7L24j/iQCGPD3/4/Uz+LC65bSvOu/hTChjnnHuRQnX95AKeeY7INVzEu887mxWaFS5mmU7Cju0JGKNGKDSbcp/jc0nMAleeBatMRasercfzA2xSquwfq1nupcliD1AIuKtD4CrwvnzA5Ud6Lu922ej0UTAd9gj8zHMOe+Dr//kvMTDKItk4oJ56dfcUHtt9HB//g2/i2zdsxzJD8aWXH0EhN6AbKY16C3VOLGqNZdy+/VEszLOQCba7id1GFVIp+eDLyEJ2XWS4JbsyMhGQjkomwzwp+J4xic6PPEu8delOpInT6msrsJv6UzJY+gGhFEKeV60ewGsvTHMIQNyvTbHqD3Lm1MORmaN43/nvxe4DJ/GZq7+Bqy//IEZYbGUDxJAdtFlMo8g+9W//7t/5fm0hPR2DCIRmGbc5GQcFxs5ePEf3xqQVDFKhZYlimvxJcgrBBIt1GpEGnu7XJelWqJHJXazvGV87gLtu34mb/uluhstHsPWsC3Dg6AFs2/pBhmEHy6QIncZJVuw8djz6JKrsB/hSLB48gOq6CcI0EYobiIPFIg0bxTOuH07khWMzVIq0cJ6Dl0D2iUm2ZYPPN5HOZZQoGpncicdlBAC1cDpBRbwyvErzQqbJkY7B7KxUPJDhZsftt+3CA794DWdt3oK9e17ClnddgonhMmdEDSTsKUIhj1ENrx06jE3D68hAF3CCPcOG33qP9iCNelMrft1NpkzGhPfn+eAiBeOzuRyUJL45q6mwhy3yc85nXxvY3lZmQMLtJVRc3U+wU2zN+ZWC4VhlQiouOzJ2k0WOjH3G/7/9+EGsG1+HiYmzMDe3jNbCtA7NIIOz2kmOiJYwNjGJBnOz1qmjVq9jfNNm9GoLLGqLRLoAxSqRKchMGd/p7SjKph2lKtK0InxeBHYj3VL1RGiBQArka0xHdmafCp3oro0dqYuFdUddBrlStVf2C3Rb1E6kfbad1dEi3p7di6GhUXZm5+JN0orGwjyOzb5J72cwOjiG0dFN+MD5F+HA/BGUBoewPH+U+wtNhm6CIkehUEYQXe/ufOSRmauvvnKglPEuKvqRekBCZyV5ZW5kNJBjLXZiZhlOJTJ2g23UbZuT2A26dIKcJDZsZL9M+l3Z8JB2cZlI8qFLz8cN//IQIgp7ZmEEew6+juGREeTYJrpa5SMlhhs3nU1PjWNmelqHDH0iZbZc1o6w3+9//87tP9ouYYzPX/X7z2ez0cdygTvmy9apZ9s/5f+OjW89qCLOqe82UKygsre2suuugieyS2PzQa6J8MppeZ68Dh+//ELcdPM9GGdNmOs1cKK5gArnQTmOTLL5MhufruLCENnohslJTse5p0aW2yaEOkk45RecL+zdu7dz6q8sdt53S9XzkusCP/makDnhGMaxM1tPd+QtpBjHYozu3UoMxSvjdOsJ8UsUJ+muqHMqjHSnRvrbuKeDgbGRIj75he+gNTuHeQ7CGqTQk8Nj2HzOZoyuOUO7MEN0bJD49TmnyuayzBOOF5fr309a0XU7du145489Tv/33MO3TBDnr2MTsoXle6uXpATPpPtOjiV0Kz+NVxYFDLHSw6bbtPqHH5EqJPgapVtNYSxzH7vV9PUv3YAmhZW95QXuWm678ANYNbxGqXxtcQnN+pIMPWe4O3F/Lpfd8dCux3adLu//Al9o4L0Sc5y8AAAAAElFTkSuQmCC"
}
```

## Detailed Analysis

### File Role in Repository

The file `agent/templates/deep_search_r.json` is located in the `agent/templates` directory.

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
- [ecommerce_customer_service_workflow.json](ecommerce_customer_service_workflow.json_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
