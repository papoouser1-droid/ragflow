# Documentation: rag/llm/chat_model.py

## File Metadata

- **Path**: `rag/llm/chat_model.py`
- **Size**: 77679 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `rag/llm/chat_model.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `asyncio`
- `json`
- `logging`
- `os`
- `random`
- `re`
- `time`
- `abc`
- `copy`
- `typing`
- `urllib.parse`
- `json_repair`
- `litellm`
- `openai`
- `requests`
- `openai`
- `openai.lib.azure`
- `strenum`
- `zhipuai`
- `rag.llm`

### Classes Defined

This file defines 28 class(es):

#### Class: `LLMErrorCode` (line 43)

#### Class: `ReActMode` (line 58)

#### Class: `ToolCallSession` (line 68)

**Methods**: tool_call

#### Class: `Base` (line 72)

**Methods**: __init__, _get_delay, _classify_error, _clean_conf, _chat, _chat_streamly, _length_stop, _retryable_errors, _should_retry, _exceptions, _verbose_tool_use, _append_history, bind_tools, chat_with_tools, chat, _wrap_toolcall_message, chat_streamly_with_tools, chat_streamly, _calculate_dynamic_ctx

#### Class: `GptTurbo` (line 512)

**Methods**: __init__

#### Class: `XinferenceChat` (line 521)

**Methods**: __init__

#### Class: `HuggingFaceChat` (line 531)

**Methods**: __init__

#### Class: `ModelScopeChat` (line 541)

**Methods**: __init__

#### Class: `AzureChat` (line 551)

**Methods**: __init__, _retryable_errors

#### Class: `BaiChuanChat` (line 570)

**Methods**: __init__, _format_params, _clean_conf, _chat, chat_streamly

#### Class: `ZhipuChat` (line 645)

**Methods**: __init__, _clean_conf, _clean_conf_plealty, chat_with_tools, chat_streamly, chat_streamly_with_tools

#### Class: `LocalAIChat` (line 705)

**Methods**: __init__

#### Class: `LocalLLM` (line 718)

**Methods**: __init__, _prepare_prompt, _stream_response, chat, chat_streamly

#### Class: `VolcEngineChat` (line 765)

**Methods**: __init__

#### Class: `MiniMaxChat` (line 780)

**Methods**: __init__, _clean_conf, _chat, chat_streamly

#### Class: `MistralChat` (line 860)

**Methods**: __init__, _clean_conf, _chat, chat_streamly

#### Class: `LmStudioChat` (line 914)

**Methods**: __init__

#### Class: `OpenAI_APIChat` (line 926)

**Methods**: __init__

#### Class: `LeptonAIChat` (line 936)

**Methods**: __init__

#### Class: `ReplicateChat` (line 945)

**Methods**: __init__, _chat, chat_streamly

#### Class: `HunyuanChat` (line 986)

**Methods**: __init__, _clean_conf, _chat, chat_streamly

#### Class: `SparkChat` (line 1064)

**Methods**: __init__

#### Class: `BaiduYiyanChat` (line 1086)

**Methods**: __init__, _clean_conf, _chat, chat_streamly

#### Class: `GoogleChat` (line 1134)

**Methods**: __init__, _clean_conf, _chat, chat_streamly

#### Class: `GPUStackChat` (line 1350)

**Methods**: __init__

#### Class: `TokenPonyChat` (line 1360)

**Methods**: __init__

#### Class: `DeerAPIChat` (line 1369)

**Methods**: __init__

#### Class: `LiteLLMBase` (line 1378)

**Methods**: __init__, _get_delay, _classify_error, _clean_conf, _chat, _chat_streamly, _length_stop, _retryable_errors, _should_retry, _exceptions, _verbose_tool_use, _append_history, bind_tools, _construct_completion_args, chat_with_tools, chat, _wrap_toolcall_message, chat_streamly_with_tools, chat_streamly, _calculate_dynamic_ctx

### Functions Defined

This file defines 97 function(s):

#### Function: `tool_call` (line 69)

**Parameters**: self, name, arguments

#### Function: `__init__` (line 73)

**Parameters**: self, key, model_name, base_url

#### Function: `_get_delay` (line 85)

**Parameters**: self

**Docstring**: Calculate retry delay time...

#### Function: `_classify_error` (line 89)

**Parameters**: self, error

**Docstring**: Classify error based on error message content...

#### Function: `_clean_conf` (line 111)

**Parameters**: self, gen_conf

#### Function: `_chat` (line 142)

**Parameters**: self, history, gen_conf

#### Function: `_chat_streamly` (line 172)

**Parameters**: self, history, gen_conf

#### Function: `_length_stop` (line 207)

**Parameters**: self, ans

#### Function: `_retryable_errors` (line 213)

**Parameters**: self

#### Function: `_should_retry` (line 219)

**Parameters**: self, error_code

#### Function: `_exceptions` (line 222)

**Parameters**: self, e, attempt

#### Function: `_verbose_tool_use` (line 237)

**Parameters**: self, name, args, res

#### Function: `_append_history` (line 240)

**Parameters**: self, hist, tool_call, tool_res

#### Function: `bind_tools` (line 264)

**Parameters**: self, toolcall_session, tools

#### Function: `chat_with_tools` (line 271)

**Parameters**: self, system, history, gen_conf

#### Function: `chat` (line 326)

**Parameters**: self, system, history, gen_conf

#### Function: `_wrap_toolcall_message` (line 341)

**Parameters**: self, stream

#### Function: `chat_streamly_with_tools` (line 355)

**Parameters**: self, system, history, gen_conf

#### Function: `chat_streamly` (line 460)

**Parameters**: self, system, history, gen_conf

#### Function: `_calculate_dynamic_ctx` (line 475)

**Parameters**: self, history

**Docstring**: Calculate dynamic context window size...

#### Function: `__init__` (line 515)

**Parameters**: self, key, model_name, base_url

#### Function: `__init__` (line 524)

**Parameters**: self, key, model_name, base_url

#### Function: `__init__` (line 534)

**Parameters**: self, key, model_name, base_url

#### Function: `__init__` (line 544)

**Parameters**: self, key, model_name, base_url

#### Function: `__init__` (line 554)

**Parameters**: self, key, model_name, base_url

#### Function: `_retryable_errors` (line 562)

**Parameters**: self

#### Function: `__init__` (line 573)

**Parameters**: self, key, model_name, base_url

#### Function: `_format_params` (line 579)

**Parameters**: params

#### Function: `_clean_conf` (line 585)

**Parameters**: self, gen_conf

#### Function: `_chat` (line 591)

**Parameters**: self, history, gen_conf

#### Function: `chat_streamly` (line 606)

**Parameters**: self, system, history, gen_conf

#### Function: `__init__` (line 648)

**Parameters**: self, key, model_name, base_url

#### Function: `_clean_conf` (line 654)

**Parameters**: self, gen_conf

#### Function: `_clean_conf_plealty` (line 660)

**Parameters**: self, gen_conf

#### Function: `chat_with_tools` (line 667)

**Parameters**: self, system, history, gen_conf

#### Function: `chat_streamly` (line 672)

**Parameters**: self, system, history, gen_conf

#### Function: `chat_streamly_with_tools` (line 700)

**Parameters**: self, system, history, gen_conf

#### Function: `__init__` (line 708)

**Parameters**: self, key, model_name, base_url

#### Function: `__init__` (line 719)

**Parameters**: self, key, model_name, base_url

#### Function: `_prepare_prompt` (line 725)

**Parameters**: self, system, history, gen_conf

#### Function: `_stream_response` (line 732)

**Parameters**: self, endpoint, prompt

#### Function: `chat` (line 749)

**Parameters**: self, system, history, gen_conf

#### Function: `chat_streamly` (line 758)

**Parameters**: self, system, history, gen_conf

#### Function: `__init__` (line 768)

**Parameters**: self, key, model_name, base_url

**Docstring**: Since do not want to modify the original database fields, and the VolcEngine authentication method is quite special,
Assemble ark_api_key, ep_id into api_key, store it as a dictionary type, and parse ...

#### Function: `__init__` (line 783)

**Parameters**: self, key, model_name, base_url

#### Function: `_clean_conf` (line 792)

**Parameters**: self, gen_conf

#### Function: `_chat` (line 798)

**Parameters**: self, history, gen_conf

#### Function: `chat_streamly` (line 814)

**Parameters**: self, system, history, gen_conf

#### Function: `__init__` (line 863)

**Parameters**: self, key, model_name, base_url

#### Function: `_clean_conf` (line 871)

**Parameters**: self, gen_conf

#### Function: `_chat` (line 877)

**Parameters**: self, history, gen_conf

#### Function: `chat_streamly` (line 888)

**Parameters**: self, system, history, gen_conf

#### Function: `__init__` (line 917)

**Parameters**: self, key, model_name, base_url

#### Function: `__init__` (line 929)

**Parameters**: self, key, model_name, base_url

#### Function: `__init__` (line 939)

**Parameters**: self, key, model_name, base_url

#### Function: `__init__` (line 948)

**Parameters**: self, key, model_name, base_url

#### Function: `_chat` (line 956)

**Parameters**: self, history, gen_conf

#### Function: `chat_streamly` (line 966)

**Parameters**: self, system, history, gen_conf

#### Function: `__init__` (line 989)

**Parameters**: self, key, model_name, base_url

#### Function: `_clean_conf` (line 1002)

**Parameters**: self, gen_conf

#### Function: `_chat` (line 1010)

**Parameters**: self, history, gen_conf

#### Function: `chat_streamly` (line 1021)

**Parameters**: self, system, history, gen_conf

#### Function: `__init__` (line 1067)

**Parameters**: self, key, model_name, base_url

#### Function: `__init__` (line 1089)

**Parameters**: self, key, model_name, base_url

#### Function: `_clean_conf` (line 1100)

**Parameters**: self, gen_conf

#### Function: `_chat` (line 1106)

**Parameters**: self, history, gen_conf

#### Function: `chat_streamly` (line 1112)

**Parameters**: self, system, history, gen_conf

#### Function: `__init__` (line 1137)

**Parameters**: self, key, model_name, base_url

#### Function: `_clean_conf` (line 1173)

**Parameters**: self, gen_conf

#### Function: `_chat` (line 1186)

**Parameters**: self, history, gen_conf

#### Function: `chat_streamly` (line 1264)

**Parameters**: self, system, history, gen_conf

#### Function: `__init__` (line 1353)

**Parameters**: self, key, model_name, base_url

#### Function: `__init__` (line 1363)

**Parameters**: self, key, model_name, base_url

#### Function: `__init__` (line 1372)

**Parameters**: self, key, model_name, base_url

#### Function: `__init__` (line 1407)

**Parameters**: self, key, model_name, base_url

#### Function: `_get_delay` (line 1431)

**Parameters**: self

**Docstring**: Calculate retry delay time...

#### Function: `_classify_error` (line 1435)

**Parameters**: self, error

**Docstring**: Classify error based on error message content...

#### Function: `_clean_conf` (line 1457)

**Parameters**: self, gen_conf

#### Function: `_chat` (line 1462)

**Parameters**: self, history, gen_conf

#### Function: `_chat_streamly` (line 1482)

**Parameters**: self, history, gen_conf

#### Function: `_length_stop` (line 1527)

**Parameters**: self, ans

#### Function: `_retryable_errors` (line 1533)

**Parameters**: self

#### Function: `_should_retry` (line 1539)

**Parameters**: self, error_code

#### Function: `_exceptions` (line 1542)

**Parameters**: self, e, attempt

#### Function: `_verbose_tool_use` (line 1557)

**Parameters**: self, name, args, res

#### Function: `_append_history` (line 1560)

**Parameters**: self, hist, tool_call, tool_res

#### Function: `bind_tools` (line 1584)

**Parameters**: self, toolcall_session, tools

#### Function: `_construct_completion_args` (line 1591)

**Parameters**: self, history, stream, tools

#### Function: `chat_with_tools` (line 1644)

**Parameters**: self, system, history, gen_conf

#### Function: `chat` (line 1710)

**Parameters**: self, system, history, gen_conf

#### Function: `_wrap_toolcall_message` (line 1726)

**Parameters**: self, stream

#### Function: `chat_streamly_with_tools` (line 1740)

**Parameters**: self, system, history, gen_conf

#### Function: `chat_streamly` (line 1867)

**Parameters**: self, system, history, gen_conf

#### Function: `_calculate_dynamic_ctx` (line 1882)

**Parameters**: self, history

**Docstring**: Calculate dynamic context window size...

#### Function: `count_tokens` (line 478)

**Parameters**: text

**Docstring**: Calculate token count for text...

#### Function: `count_tokens` (line 1885)

**Parameters**: text

**Docstring**: Calculate token count for text...

#### Function: `_to_order_list` (line 1627)

**Parameters**: x

## Original Source Code

```py
#
#  Copyright 2025 The InfiniFlow Authors. All Rights Reserved.
#
#  Licensed under the Apache License, Version 2.0 (the "License");
#  you may not use this file except in compliance with the License.
#  You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
#  Unless required by applicable law or agreed to in writing, software
#  distributed under the License is distributed on an "AS IS" BASIS,
#  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#  See the License for the specific language governing permissions and
#  limitations under the License.
#
import asyncio
import json
import logging
import os
import random
import re
import time
from abc import ABC
from copy import deepcopy
from typing import Any, Protocol
from urllib.parse import urljoin

import json_repair
import litellm
import openai
import requests
from openai import OpenAI
from openai.lib.azure import AzureOpenAI
from strenum import StrEnum
from zhipuai import ZhipuAI

from rag.llm import FACTORY_DEFAULT_BASE_URL, LITELLM_PROVIDER_PREFIX, SupportedLiteLLMProvider
from rag.nlp import is_chinese, is_english
from common.token_utils import num_tokens_from_string, total_token_count_from_response


# Error message constants
class LLMErrorCode(StrEnum):
    ERROR_RATE_LIMIT = "RATE_LIMIT_EXCEEDED"
    ERROR_AUTHENTICATION = "AUTH_ERROR"
    ERROR_INVALID_REQUEST = "INVALID_REQUEST"
    ERROR_SERVER = "SERVER_ERROR"
    ERROR_TIMEOUT = "TIMEOUT"
    ERROR_CONNECTION = "CONNECTION_ERROR"
    ERROR_MODEL = "MODEL_ERROR"
    ERROR_MAX_ROUNDS = "ERROR_MAX_ROUNDS"
    ERROR_CONTENT_FILTER = "CONTENT_FILTERED"
    ERROR_QUOTA = "QUOTA_EXCEEDED"
    ERROR_MAX_RETRIES = "MAX_RETRIES_EXCEEDED"
    ERROR_GENERIC = "GENERIC_ERROR"


class ReActMode(StrEnum):
    FUNCTION_CALL = "function_call"
    REACT = "react"


ERROR_PREFIX = "**ERROR**"
LENGTH_NOTIFICATION_CN = "······\n由于大模型的上下文窗口大小限制，回答已经被大模型截断。"
LENGTH_NOTIFICATION_EN = "...\nThe answer is truncated by your chosen LLM due to its limitation on context length."


class ToolCallSession(Protocol):
    def tool_call(self, name: str, arguments: dict[str, Any]) -> str: ...


class Base(ABC):
    def __init__(self, key, model_name, base_url, **kwargs):
        timeout = int(os.environ.get("LM_TIMEOUT_SECONDS", 600))
        self.client = OpenAI(api_key=key, base_url=base_url, timeout=timeout)
        self.model_name = model_name
        # Configure retry parameters
        self.max_retries = kwargs.get("max_retries", int(os.environ.get("LLM_MAX_RETRIES", 5)))
        self.base_delay = kwargs.get("retry_interval", float(os.environ.get("LLM_BASE_DELAY", 2.0)))
        self.max_rounds = kwargs.get("max_rounds", 5)
        self.is_tools = False
        self.tools = []
        self.toolcall_sessions = {}

    def _get_delay(self):
        """Calculate retry delay time"""
        return self.base_delay * random.uniform(10, 150)

    def _classify_error(self, error):
        """Classify error based on error message content"""
        error_str = str(error).lower()

        keywords_mapping = [
            (["quota", "capacity", "credit", "billing", "balance", "欠费"], LLMErrorCode.ERROR_QUOTA),
            (["rate limit", "429", "tpm limit", "too many requests", "requests per minute"], LLMErrorCode.ERROR_RATE_LIMIT),
            (["auth", "key", "apikey", "401", "forbidden", "permission"], LLMErrorCode.ERROR_AUTHENTICATION),
            (["invalid", "bad request", "400", "format", "malformed", "parameter"], LLMErrorCode.ERROR_INVALID_REQUEST),
            (["server", "503", "502", "504", "500", "unavailable"], LLMErrorCode.ERROR_SERVER),
            (["timeout", "timed out"], LLMErrorCode.ERROR_TIMEOUT),
            (["connect", "network", "unreachable", "dns"], LLMErrorCode.ERROR_CONNECTION),
            (["filter", "content", "policy", "blocked", "safety", "inappropriate"], LLMErrorCode.ERROR_CONTENT_FILTER),
            (["model", "not found", "does not exist", "not available"], LLMErrorCode.ERROR_MODEL),
            (["max rounds"], LLMErrorCode.ERROR_MODEL),
        ]
        for words, code in keywords_mapping:
            if re.search("({})".format("|".join(words)), error_str):
                return code

        return LLMErrorCode.ERROR_GENERIC

    def _clean_conf(self, gen_conf):
        if "max_tokens" in gen_conf:
            del gen_conf["max_tokens"]

        allowed_conf = {
            "temperature",
            "max_completion_tokens",
            "top_p",
            "stream",
            "stream_options",
            "stop",
            "n",
            "presence_penalty",
            "frequency_penalty",
            "functions",
            "function_call",
            "logit_bias",
            "user",
            "response_format",
            "seed",
            "tools",
            "tool_choice",
            "logprobs",
            "top_logprobs",
            "extra_headers"
        }

        gen_conf = {k: v for k, v in gen_conf.items() if k in allowed_conf}

        return gen_conf

    def _chat(self, history, gen_conf, **kwargs):
        logging.info("[HISTORY]" + json.dumps(history, ensure_ascii=False, indent=2))
        if self.model_name.lower().find("qwq") >= 0:
            logging.info(f"[INFO] {self.model_name} detected as reasoning model, using _chat_streamly")

            final_ans = ""
            tol_token = 0
            for delta, tol in self._chat_streamly(history, gen_conf, with_reasoning=False, **kwargs):
                if delta.startswith("<think>") or delta.endswith("</think>"):
                    continue
                final_ans += delta
                tol_token = tol

            if len(final_ans.strip()) == 0:
                final_ans = "**ERROR**: Empty response from reasoning model"

            return final_ans.strip(), tol_token

        if self.model_name.lower().find("qwen3") >= 0:
            kwargs["extra_body"] = {"enable_thinking": False}

        response = self.client.chat.completions.create(model=self.model_name, messages=history, **gen_conf, **kwargs)

        if not response.choices or not response.choices[0].message or not response.choices[0].message.content:
            return "", 0
        ans = response.choices[0].message.content.strip()
        if response.choices[0].finish_reason == "length":
            ans = self._length_stop(ans)
        return ans, total_token_count_from_response(response)

    def _chat_streamly(self, history, gen_conf, **kwargs):
        logging.info("[HISTORY STREAMLY]" + json.dumps(history, ensure_ascii=False, indent=4))
        reasoning_start = False

        if kwargs.get("stop") or "stop" in gen_conf:
            response = self.client.chat.completions.create(model=self.model_name, messages=history, stream=True, **gen_conf, stop=kwargs.get("stop"))
        else:
            response = self.client.chat.completions.create(model=self.model_name, messages=history, stream=True, **gen_conf)

        for resp in response:
            if not resp.choices:
                continue
            if not resp.choices[0].delta.content:
                resp.choices[0].delta.content = ""
            if kwargs.get("with_reasoning", True) and hasattr(resp.choices[0].delta, "reasoning_content") and resp.choices[0].delta.reasoning_content:
                ans = ""
                if not reasoning_start:
                    reasoning_start = True
                    ans = "<think>"
                ans += resp.choices[0].delta.reasoning_content + "</think>"
            else:
                reasoning_start = False
                ans = resp.choices[0].delta.content

            tol = total_token_count_from_response(resp)
            if not tol:
                tol = num_tokens_from_string(resp.choices[0].delta.content)

            if resp.choices[0].finish_reason == "length":
                if is_chinese(ans):
                    ans += LENGTH_NOTIFICATION_CN
                else:
                    ans += LENGTH_NOTIFICATION_EN
            yield ans, tol

    def _length_stop(self, ans):
        if is_chinese([ans]):
            return ans + LENGTH_NOTIFICATION_CN
        return ans + LENGTH_NOTIFICATION_EN

    @property
    def _retryable_errors(self) -> set[str]:
        return {
            LLMErrorCode.ERROR_RATE_LIMIT,
            LLMErrorCode.ERROR_SERVER,
        }

    def _should_retry(self, error_code: str) -> bool:
        return error_code in self._retryable_errors

    def _exceptions(self, e, attempt) -> str | None:
        logging.exception("OpenAI chat_with_tools")
        # Classify the error
        error_code = self._classify_error(e)
        if attempt == self.max_retries:
            error_code = LLMErrorCode.ERROR_MAX_RETRIES

        if self._should_retry(error_code):
            delay = self._get_delay()
            logging.warning(f"Error: {error_code}. Retrying in {delay:.2f} seconds... (Attempt {attempt + 1}/{self.max_retries})")
            time.sleep(delay)
            return None

        return f"{ERROR_PREFIX}: {error_code} - {str(e)}"

    def _verbose_tool_use(self, name, args, res):
        return "<tool_call>" + json.dumps({"name": name, "args": args, "result": res}, ensure_ascii=False, indent=2) + "</tool_call>"

    def _append_history(self, hist, tool_call, tool_res):
        hist.append(
            {
                "role": "assistant",
                "tool_calls": [
                    {
                        "index": tool_call.index,
                        "id": tool_call.id,
                        "function": {
                            "name": tool_call.function.name,
                            "arguments": tool_call.function.arguments,
                        },
                        "type": "function",
                    },
                ],
            }
        )
        try:
            if isinstance(tool_res, dict):
                tool_res = json.dumps(tool_res, ensure_ascii=False)
        finally:
           

... [Content truncated - file is 77565 bytes] ...

 tool_response = self.toolcall_session.tool_call(name, args)
                            history = self._append_history(history, tool_call, tool_response)
                            ans += self._verbose_tool_use(name, args, tool_response)
                        except Exception as e:
                            logging.exception(msg=f"Wrong JSON argument format in LLM tool call response: {tool_call}")
                            history.append({"role": "tool", "tool_call_id": tool_call.id, "content": f"Tool call error: \n{tool_call}\nException:\n" + str(e)})
                            ans += self._verbose_tool_use(name, {}, str(e))

                logging.warning(f"Exceed max rounds: {self.max_rounds}")
                history.append({"role": "user", "content": f"Exceed max rounds: {self.max_rounds}"})

                response, token_count = self._chat(history, gen_conf)
                ans += response
                tk_count += token_count
                return ans, tk_count

            except Exception as e:
                e = self._exceptions(e, attempt)
                if e:
                    return e, tk_count

        assert False, "Shouldn't be here."

    def chat(self, system, history, gen_conf={}, **kwargs):
        if system and history and history[0].get("role") != "system":
            history.insert(0, {"role": "system", "content": system})
        gen_conf = self._clean_conf(gen_conf)

        # Implement exponential backoff retry strategy
        for attempt in range(self.max_retries + 1):
            try:
                response = self._chat(history, gen_conf, **kwargs)
                return response
            except Exception as e:
                e = self._exceptions(e, attempt)
                if e:
                    return e, 0
        assert False, "Shouldn't be here."

    def _wrap_toolcall_message(self, stream):
        final_tool_calls = {}

        for chunk in stream:
            for tool_call in chunk.choices[0].delta.tool_calls or []:
                index = tool_call.index

                if index not in final_tool_calls:
                    final_tool_calls[index] = tool_call

                final_tool_calls[index].function.arguments += tool_call.function.arguments

        return final_tool_calls

    def chat_streamly_with_tools(self, system: str, history: list, gen_conf: dict = {}):
        gen_conf = self._clean_conf(gen_conf)
        tools = self.tools
        if system and history and history[0].get("role") != "system":
            history.insert(0, {"role": "system", "content": system})

        total_tokens = 0
        hist = deepcopy(history)

        # Implement exponential backoff retry strategy
        for attempt in range(self.max_retries + 1):
            history = deepcopy(hist)  # deepcopy is required here
            try:
                for _ in range(self.max_rounds + 1):
                    reasoning_start = False
                    logging.info(f"{tools=}")

                    completion_args = self._construct_completion_args(history=history, stream=True, tools=True, **gen_conf)
                    response = litellm.completion(
                        **completion_args,
                        drop_params=True,
                        timeout=self.timeout,
                    )

                    final_tool_calls = {}
                    answer = ""

                    for resp in response:
                        if not hasattr(resp, "choices") or not resp.choices:
                            continue

                        delta = resp.choices[0].delta

                        if hasattr(delta, "tool_calls") and delta.tool_calls:
                            for tool_call in delta.tool_calls:
                                index = tool_call.index
                                if index not in final_tool_calls:
                                    if not tool_call.function.arguments:
                                        tool_call.function.arguments = ""
                                    final_tool_calls[index] = tool_call
                                else:
                                    final_tool_calls[index].function.arguments += tool_call.function.arguments or ""
                            continue

                        if not hasattr(delta, "content") or delta.content is None:
                            delta.content = ""

                        if hasattr(delta, "reasoning_content") and delta.reasoning_content:
                            ans = ""
                            if not reasoning_start:
                                reasoning_start = True
                                ans = "<think>"
                            ans += delta.reasoning_content + "</think>"
                            yield ans
                        else:
                            reasoning_start = False
                            answer += delta.content
                            yield delta.content

                        tol = total_token_count_from_response(resp)
                        if not tol:
                            total_tokens += num_tokens_from_string(delta.content)
                        else:
                            total_tokens += tol

                        finish_reason = getattr(resp.choices[0], "finish_reason", "")
                        if finish_reason == "length":
                            yield self._length_stop("")

                    if answer:
                        yield total_tokens
                        return

                    for tool_call in final_tool_calls.values():
                        name = tool_call.function.name
                        try:
                            args = json_repair.loads(tool_call.function.arguments)
                            yield self._verbose_tool_use(name, args, "Begin to call...")
                            tool_response = self.toolcall_session.tool_call(name, args)
                            history = self._append_history(history, tool_call, tool_response)
                            yield self._verbose_tool_use(name, args, tool_response)
                        except Exception as e:
                            logging.exception(msg=f"Wrong JSON argument format in LLM tool call response: {tool_call}")
                            history.append(
                                {
                                    "role": "tool",
                                    "tool_call_id": tool_call.id,
                                    "content": f"Tool call error: \n{tool_call}\nException:\n{str(e)}",
                                }
                            )
                            yield self._verbose_tool_use(name, {}, str(e))

                logging.warning(f"Exceed max rounds: {self.max_rounds}")
                history.append({"role": "user", "content": f"Exceed max rounds: {self.max_rounds}"})

                completion_args = self._construct_completion_args(history=history, stream=True, tools=True, **gen_conf)
                response = litellm.completion(
                    **completion_args,
                    drop_params=True,
                    timeout=self.timeout,
                )

                for resp in response:
                    if not hasattr(resp, "choices") or not resp.choices:
                        continue
                    delta = resp.choices[0].delta
                    if not hasattr(delta, "content") or delta.content is None:
                        continue
                    tol = total_token_count_from_response(resp)
                    if not tol:
                        total_tokens += num_tokens_from_string(delta.content)
                    else:
                        total_tokens += tol
                    yield delta.content

                yield total_tokens
                return

            except Exception as e:
                e = self._exceptions(e, attempt)
                if e:
                    yield e
                    yield total_tokens
                    return

        assert False, "Shouldn't be here."

    def chat_streamly(self, system, history, gen_conf: dict = {}, **kwargs):
        if system and history and history[0].get("role") != "system":
            history.insert(0, {"role": "system", "content": system})
        gen_conf = self._clean_conf(gen_conf)
        ans = ""
        total_tokens = 0
        try:
            for delta_ans, tol in self._chat_streamly(history, gen_conf, **kwargs):
                yield delta_ans
                total_tokens += tol
        except openai.APIError as e:
            yield ans + "\n**ERROR**: " + str(e)

        yield total_tokens

    def _calculate_dynamic_ctx(self, history):
        """Calculate dynamic context window size"""

        def count_tokens(text):
            """Calculate token count for text"""
            # Simple calculation: 1 token per ASCII character
            # 2 tokens for non-ASCII characters (Chinese, Japanese, Korean, etc.)
            total = 0
            for char in text:
                if ord(char) < 128:  # ASCII characters
                    total += 1
                else:  # Non-ASCII characters (Chinese, Japanese, Korean, etc.)
                    total += 2
            return total

        # Calculate total tokens for all messages
        total_tokens = 0
        for message in history:
            content = message.get("content", "")
            # Calculate content tokens
            content_tokens = count_tokens(content)
            # Add role marker token overhead
            role_tokens = 4
            total_tokens += content_tokens + role_tokens

        # Apply 1.2x buffer ratio
        total_tokens_with_buffer = int(total_tokens * 1.2)

        if total_tokens_with_buffer <= 8192:
            ctx_size = 8192
        else:
            ctx_multiplier = (total_tokens_with_buffer // 8192) + 1
            ctx_size = ctx_multiplier * 8192

        return ctx_size

```

## Detailed Analysis

### File Role in Repository

The file `rag/llm/chat_model.py` is located in the `rag/llm` directory.

This file is part of the **RAG (Retrieval-Augmented Generation)** core engine.

### Architecture Context

Files in this location typically handle concerns related to llm.

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
- [cv_model.py](cv_model.py_docs.md)
- [embedding_model.py](embedding_model.py_docs.md)
- [rerank_model.py](rerank_model.py_docs.md)
- [sequence2txt_model.py](sequence2txt_model.py_docs.md)
- [tts_model.py](tts_model.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
