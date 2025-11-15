# Documentation: agent/tools/qweather.py

## File Metadata

- **Path**: `agent/tools/qweather.py`
- **Size**: 6524 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/tools/qweather.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `abc`
- `pandas`
- `requests`
- `agent.component.base`

### Classes Defined

This file defines 2 class(es):

#### Class: `QWeatherParam` (line 22)

**Docstring**: Define the QWeather component parameters....

**Methods**: __init__, check

#### Class: `QWeather` (line 57)

**Methods**: _run

### Functions Defined

This file defines 3 function(s):

#### Function: `__init__` (line 27)

**Parameters**: self

#### Function: `check` (line 46)

**Parameters**: self

#### Function: `_run` (line 60)

**Parameters**: self, history

## Original Source Code

```py
#
#  Copyright 2024 The InfiniFlow Authors. All Rights Reserved.
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
from abc import ABC
import pandas as pd
import requests
from agent.component.base import ComponentBase, ComponentParamBase


class QWeatherParam(ComponentParamBase):
    """
    Define the QWeather component parameters.
    """

    def __init__(self):
        super().__init__()
        self.web_apikey = "xxx"
        self.lang = "zh"
        self.type = "weather"
        self.user_type = 'free'
        self.error_code = {
            "204": "The request was successful, but the region you are querying does not have the data you need at this time.",
            "400": "Request error, may contain incorrect request parameters or missing mandatory request parameters.",
            "401": "Authentication fails, possibly using the wrong KEY, wrong digital signature, wrong type of KEY (e.g. using the SDK's KEY to access the Web API).",
            "402": "Exceeded the number of accesses or the balance is not enough to support continued access to the service, you can recharge, upgrade the accesses or wait for the accesses to be reset.",
            "403": "No access, may be the binding PackageName, BundleID, domain IP address is inconsistent, or the data that requires additional payment.",
            "404": "The queried data or region does not exist.",
            "429": "Exceeded the limited QPM (number of accesses per minute), please refer to the QPM description",
            "500": "No response or timeout, interface service abnormality please contact us"
            }
        # Weather
        self.time_period = 'now'

    def check(self):
        self.check_empty(self.web_apikey, "BaiduFanyi APPID")
        self.check_valid_value(self.type, "Type", ["weather", "indices", "airquality"])
        self.check_valid_value(self.user_type, "Free subscription or paid subscription", ["free", "paid"])
        self.check_valid_value(self.lang, "Use language",
                               ['zh', 'zh-hant', 'en', 'de', 'es', 'fr', 'it', 'ja', 'ko', 'ru', 'hi', 'th', 'ar', 'pt',
                                'bn', 'ms', 'nl', 'el', 'la', 'sv', 'id', 'pl', 'tr', 'cs', 'et', 'vi', 'fil', 'fi',
                                'he', 'is', 'nb'])
        self.check_valid_value(self.time_period, "Time period", ['now', '3d', '7d', '10d', '15d', '30d'])


class QWeather(ComponentBase, ABC):
    component_name = "QWeather"

    def _run(self, history, **kwargs):
        if self.check_if_canceled("Qweather processing"):
            return

        ans = self.get_input()
        ans = "".join(ans["content"]) if "content" in ans else ""
        if not ans:
            return QWeather.be_output("")

        try:
            if self.check_if_canceled("Qweather processing"):
                return

            response = requests.get(
                url="https://geoapi.qweather.com/v2/city/lookup?location=" + ans + "&key=" + self._param.web_apikey).json()
            if response["code"] == "200":
                location_id = response["location"][0]["id"]
            else:
                return QWeather.be_output("**Error**" + self._param.error_code[response["code"]])

            if self.check_if_canceled("Qweather processing"):
                return

            base_url = "https://api.qweather.com/v7/" if self._param.user_type == 'paid' else "https://devapi.qweather.com/v7/"

            if self._param.type == "weather":
                url = base_url + "weather/" + self._param.time_period + "?location=" + location_id + "&key=" + self._param.web_apikey + "&lang=" + self._param.lang
                response = requests.get(url=url).json()
                if self.check_if_canceled("Qweather processing"):
                    return
                if response["code"] == "200":
                    if self._param.time_period == "now":
                        return QWeather.be_output(str(response["now"]))
                    else:
                        qweather_res = [{"content": str(i) + "\n"} for i in response["daily"]]
                        if self.check_if_canceled("Qweather processing"):
                            return
                        if not qweather_res:
                            return QWeather.be_output("")

                        df = pd.DataFrame(qweather_res)
                        return df
                else:
                    return QWeather.be_output("**Error**" + self._param.error_code[response["code"]])

            elif self._param.type == "indices":
                url = base_url + "indices/1d?type=0&location=" + location_id + "&key=" + self._param.web_apikey + "&lang=" + self._param.lang
                response = requests.get(url=url).json()
                if self.check_if_canceled("Qweather processing"):
                    return
                if response["code"] == "200":
                    indices_res = response["daily"][0]["date"] + "\n" + "\n".join(
                        [i["name"] + ": " + i["category"] + ", " + i["text"] for i in response["daily"]])
                    return QWeather.be_output(indices_res)

                else:
                    return QWeather.be_output("**Error**" + self._param.error_code[response["code"]])

            elif self._param.type == "airquality":
                url = base_url + "air/now?location=" + location_id + "&key=" + self._param.web_apikey + "&lang=" + self._param.lang
                response = requests.get(url=url).json()
                if self.check_if_canceled("Qweather processing"):
                    return
                if response["code"] == "200":
                    return QWeather.be_output(str(response["now"]))
                else:
                    return QWeather.be_output("**Error**" + self._param.error_code[response["code"]])
        except Exception as e:
            if self.check_if_canceled("Qweather processing"):
                return
            return QWeather.be_output("**Error**" + str(e))

```

## Detailed Analysis

### File Role in Repository

The file `agent/tools/qweather.py` is located in the `agent/tools` directory.

This file is part of the **Agent System** for workflow management.

### Architecture Context

Files in this location typically handle concerns related to tools.

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
- [akshare.py](akshare.py_docs.md)
- [arxiv.py](arxiv.py_docs.md)
- [base.py](base.py_docs.md)
- [code_exec.py](code_exec.py_docs.md)
- [crawler.py](crawler.py_docs.md)
- [deepl.py](deepl.py_docs.md)
- [duckduckgo.py](duckduckgo.py_docs.md)
- [email.py](email.py_docs.md)
- [exesql.py](exesql.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
