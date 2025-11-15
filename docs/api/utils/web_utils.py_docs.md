# Documentation: api/utils/web_utils.py

## File Metadata

- **Path**: `api/utils/web_utils.py`
- **Size**: 6957 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `api/utils/web_utils.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `base64`
- `ipaddress`
- `json`
- `re`
- `socket`
- `urllib.parse`
- `api.apps`
- `flask_mail`
- `flask`
- `api.utils.email_templates`
- `selenium`
- `selenium.common.exceptions`
- `selenium.webdriver.chrome.options`
- `selenium.webdriver.chrome.service`
- `selenium.webdriver.common.by`
- `selenium.webdriver.support.expected_conditions`
- `selenium.webdriver.support.ui`
- `webdriver_manager.chrome`
- `api.apps`
- `hashlib`

### Functions Defined

This file defines 12 function(s):

#### Function: `html2pdf` (line 90)

**Parameters**: source, timeout, install_driver, print_options

#### Function: `__send_devtools` (line 100)

**Parameters**: driver, cmd, params

#### Function: `__get_pdf_from_html` (line 112)

**Parameters**: path, timeout, install_driver, print_options

#### Function: `is_private_ip` (line 146)

**Parameters**: ip

#### Function: `is_valid_url` (line 154)

**Parameters**: url

#### Function: `safe_json_parse` (line 171)

**Parameters**: data

#### Function: `get_float` (line 180)

**Parameters**: req, key, default

#### Function: `send_email_html` (line 188)

**Parameters**: subject, to_email, template_key

**Docstring**: Generic HTML email sender using shared templates.
template_key must exist in EMAIL_TEMPLATES....

#### Function: `send_invite_email` (line 202)

**Parameters**: to_email, invite_url, tenant_id, inviter

#### Function: `otp_keys` (line 215)

**Parameters**: email

#### Function: `hash_code` (line 225)

**Parameters**: code, salt

#### Function: `captcha_key` (line 231)

**Parameters**: email

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

import base64
import ipaddress
import json
import re
import socket
from urllib.parse import urlparse

from api.apps import smtp_mail_server
from flask_mail import Message
from flask import render_template_string
from api.utils.email_templates import EMAIL_TEMPLATES
from selenium import webdriver
from selenium.common.exceptions import TimeoutException
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
from selenium.webdriver.support.expected_conditions import staleness_of
from selenium.webdriver.support.ui import WebDriverWait
from webdriver_manager.chrome import ChromeDriverManager


OTP_LENGTH = 8
OTP_TTL_SECONDS = 5 * 60
ATTEMPT_LIMIT = 5
ATTEMPT_LOCK_SECONDS = 30 * 60
RESEND_COOLDOWN_SECONDS = 60


CONTENT_TYPE_MAP = {
    # Office
    "docx": "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
    "doc": "application/msword",
    "pdf": "application/pdf",
    "csv": "text/csv",
    "xls": "application/vnd.ms-excel",
    "xlsx": "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet",
    # Text/code
    "txt": "text/plain",
    "py": "text/plain",
    "js": "text/plain",
    "java": "text/plain",
    "c": "text/plain",
    "cpp": "text/plain",
    "h": "text/plain",
    "php": "text/plain",
    "go": "text/plain",
    "ts": "text/plain",
    "sh": "text/plain",
    "cs": "text/plain",
    "kt": "text/plain",
    "sql": "text/plain",
    # Web
    "md": "text/markdown",
    "markdown": "text/markdown",
    "htm": "text/html",
    "html": "text/html",
    "json": "application/json",
    # Image formats
    "png": "image/png",
    "jpg": "image/jpeg",
    "jpeg": "image/jpeg",
    "gif": "image/gif",
    "bmp": "image/bmp",
    "tiff": "image/tiff",
    "tif": "image/tiff",
    "webp": "image/webp",
    "svg": "image/svg+xml",
    "ico": "image/x-icon",
    "avif": "image/avif",
    "heic": "image/heic",
}


def html2pdf(
    source: str,
    timeout: int = 2,
    install_driver: bool = True,
    print_options: dict = {},
):
    result = __get_pdf_from_html(source, timeout, install_driver, print_options)
    return result


def __send_devtools(driver, cmd, params={}):
    resource = "/session/%s/chromium/send_command_and_get_result" % driver.session_id
    url = driver.command_executor._url + resource
    body = json.dumps({"cmd": cmd, "params": params})
    response = driver.command_executor._request("POST", url, body)

    if not response:
        raise Exception(response.get("value"))

    return response.get("value")


def __get_pdf_from_html(path: str, timeout: int, install_driver: bool, print_options: dict):
    webdriver_options = Options()
    webdriver_prefs = {}
    webdriver_options.add_argument("--headless")
    webdriver_options.add_argument("--disable-gpu")
    webdriver_options.add_argument("--no-sandbox")
    webdriver_options.add_argument("--disable-dev-shm-usage")
    webdriver_options.experimental_options["prefs"] = webdriver_prefs

    webdriver_prefs["profile.default_content_settings"] = {"images": 2}

    if install_driver:
        service = Service(ChromeDriverManager().install())
        driver = webdriver.Chrome(service=service, options=webdriver_options)
    else:
        driver = webdriver.Chrome(options=webdriver_options)

    driver.get(path)

    try:
        WebDriverWait(driver, timeout).until(staleness_of(driver.find_element(by=By.TAG_NAME, value="html")))
    except TimeoutException:
        calculated_print_options = {
            "landscape": False,
            "displayHeaderFooter": False,
            "printBackground": True,
            "preferCSSPageSize": True,
        }
        calculated_print_options.update(print_options)
        result = __send_devtools(driver, "Page.printToPDF", calculated_print_options)
        driver.quit()
        return base64.b64decode(result["data"])


def is_private_ip(ip: str) -> bool:
    try:
        ip_obj = ipaddress.ip_address(ip)
        return ip_obj.is_private
    except ValueError:
        return False


def is_valid_url(url: str) -> bool:
    if not re.match(r"(https?)://[-A-Za-z0-9+&@#/%?=~_|!:,.;]+[-A-Za-z0-9+&@#/%=~_|]", url):
        return False
    parsed_url = urlparse(url)
    hostname = parsed_url.hostname

    if not hostname:
        return False
    try:
        ip = socket.gethostbyname(hostname)
        if is_private_ip(ip):
            return False
    except socket.gaierror:
        return False
    return True


def safe_json_parse(data: str | dict) -> dict:
    if isinstance(data, dict):
        return data
    try:
        return json.loads(data) if data else {}
    except (json.JSONDecodeError, TypeError):
        return {}


def get_float(req: dict, key: str, default: float | int = 10.0) -> float:
    try:
        parsed = float(req.get(key, default))
        return parsed if parsed > 0 else default
    except (TypeError, ValueError):
        return default


def send_email_html(subject: str, to_email: str, template_key: str, **context):
    """Generic HTML email sender using shared templates.
    template_key must exist in EMAIL_TEMPLATES.
    """
    from api.apps import app
    tmpl = EMAIL_TEMPLATES.get(template_key)
    if not tmpl:
        raise ValueError(f"Unknown email template: {template_key}")
    with app.app_context():
        msg = Message(subject=subject, recipients=[to_email])
        msg.html = render_template_string(tmpl, **context)
        smtp_mail_server.send(msg)


def send_invite_email(to_email, invite_url, tenant_id, inviter):
    # Reuse the generic HTML sender with 'invite' template
    send_email_html(
        subject="RAGFlow Invitation",
        to_email=to_email,
        template_key="invite",
        email=to_email,
        invite_url=invite_url,
        tenant_id=tenant_id,
        inviter=inviter,
    )


def otp_keys(email: str):
    email = (email or "").strip().lower()
    return (
        f"otp:{email}",
        f"otp_attempts:{email}",
        f"otp_last_sent:{email}",
        f"otp_lock:{email}",
    )


def hash_code(code: str, salt: bytes) -> str:
    import hashlib
    import hmac 
    return hmac.new(salt, (code or "").encode("utf-8"), hashlib.sha256).hexdigest()
    

def captcha_key(email: str) -> str:
    return f"captcha:{email}"
    

```

## Detailed Analysis

### File Role in Repository

The file `api/utils/web_utils.py` is located in the `api/utils` directory.

This file is part of the **API/Backend** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to utils.

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
- [api_utils.py](api_utils.py_docs.md)
- [base64_image.py](base64_image.py_docs.md)
- [commands.py](commands.py_docs.md)
- [common.py](common.py_docs.md)
- [configs.py](configs.py_docs.md)
- [crypt.py](crypt.py_docs.md)
- [email_templates.py](email_templates.py_docs.md)
- [file_utils.py](file_utils.py_docs.md)
- [health_utils.py](health_utils.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
