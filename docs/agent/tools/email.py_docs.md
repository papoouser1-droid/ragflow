# Documentation: agent/tools/email.py

## File Metadata

- **Path**: `agent/tools/email.py`
- **Size**: 8841 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/tools/email.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `os`
- `time`
- `abc`
- `json`
- `smtplib`
- `logging`
- `email.mime.text`
- `email.mime.multipart`
- `email.header`
- `email.utils`
- `agent.tools.base`
- `common.connection_utils`

### Classes Defined

This file defines 2 class(es):

#### Class: `EmailParam` (line 31)

**Docstring**: Define the Email component parameters....

**Methods**: __init__, check, get_input_form

#### Class: `Email` (line 99)

**Methods**: _invoke, thoughts

### Functions Defined

This file defines 5 function(s):

#### Function: `__init__` (line 35)

**Parameters**: self

#### Function: `check` (line 74)

**Parameters**: self

#### Function: `get_input_form` (line 81)

**Parameters**: self

#### Function: `_invoke` (line 103)

**Parameters**: self

#### Function: `thoughts` (line 224)

**Parameters**: self

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
import os
import time
from abc import ABC
import json
import smtplib
import logging
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
from email.header import Header
from email.utils import formataddr

from agent.tools.base import ToolParamBase, ToolBase, ToolMeta
from common.connection_utils import timeout


class EmailParam(ToolParamBase):
    """
    Define the Email component parameters.
    """
    def __init__(self):
        self.meta:ToolMeta = {
            "name": "email",
            "description": "The email is a method of electronic communication for sending and receiving information through the Internet. This tool helps users to send emails to one person or to multiple recipients with support for CC, BCC, file attachments, and markdown-to-HTML conversion.",
            "parameters": {
                "to_email": {
                    "type": "string",
                    "description": "The target email address.",
                    "default": "{sys.query}",
                    "required": True
                },
                "cc_email": {
                    "type": "string",
                    "description": "The other email addresses needs to be send to. Comma splited.",
                    "default": "",
                    "required": False
                },
                "content": {
                    "type": "string",
                    "description": "The content of the email.",
                    "default": "",
                    "required": False
                },
                "subject": {
                    "type": "string",
                    "description": "The subject/title of the email.",
                    "default": "",
                    "required": False
                }
            }
        }
        super().__init__()
        # Fixed configuration parameters
        self.smtp_server = ""  # SMTP server address
        self.smtp_port = 465  # SMTP port
        self.email = ""  # Sender email
        self.password = ""  # Email authorization code
        self.sender_name = ""  # Sender name

    def check(self):
        # Check required parameters
        self.check_empty(self.smtp_server, "SMTP Server")
        self.check_empty(self.email, "Email")
        self.check_empty(self.password, "Password")
        self.check_empty(self.sender_name, "Sender Name")

    def get_input_form(self) -> dict[str, dict]:
        return {
            "to_email": {
                "name": "To ",
                "type": "line"
            },
            "subject": {
                "name": "Subject",
                "type": "line",
                "optional": True
            },
            "cc_email": {
                "name": "CC To",
                "type": "line",
                "optional": True
            },
        }

class Email(ToolBase, ABC):
    component_name = "Email"

    @timeout(int(os.environ.get("COMPONENT_EXEC_TIMEOUT", 60)))
    def _invoke(self, **kwargs):
        if self.check_if_canceled("Email processing"):
            return

        if not kwargs.get("to_email"):
            self.set_output("success", False)
            return ""

        last_e = ""
        for _ in range(self._param.max_retries+1):
            if self.check_if_canceled("Email processing"):
                return

            try:
                # Parse JSON string passed from upstream
                email_data = kwargs

                # Validate required fields
                if "to_email" not in email_data:
                    self.set_output("_ERROR", "Missing required field: to_email")
                    self.set_output("success", False)
                    return False

                # Create email object
                msg = MIMEMultipart('alternative')

                # Properly handle sender name encoding
                msg['From'] = formataddr((str(Header(self._param.sender_name,'utf-8')), self._param.email))
                msg['To'] = email_data["to_email"]
                if email_data.get("cc_email"):
                    msg['Cc'] = email_data["cc_email"]
                msg['Subject'] = Header(email_data.get("subject", "No Subject"), 'utf-8').encode()

                # Use content from email_data or default content
                email_content = email_data.get("content", "No content provided")
                # msg.attach(MIMEText(email_content, 'plain', 'utf-8'))
                msg.attach(MIMEText(email_content, 'html', 'utf-8'))

                # Connect to SMTP server and send
                logging.info(f"Connecting to SMTP server {self._param.smtp_server}:{self._param.smtp_port}")

                if self.check_if_canceled("Email processing"):
                    return

                context = smtplib.ssl.create_default_context()
                with smtplib.SMTP(self._param.smtp_server, self._param.smtp_port) as server:
                    server.ehlo()
                    server.starttls(context=context)
                    server.ehlo()
                    # Login
                    logging.info(f"Attempting to login with email: {self._param.email}")
                    server.login(self._param.email, self._param.password)

                    # Get all recipient list
                    recipients = [email_data["to_email"]]
                    if email_data.get("cc_email"):
                        recipients.extend(email_data["cc_email"].split(','))

                    # Send email
                    logging.info(f"Sending email to recipients: {recipients}")

                    if self.check_if_canceled("Email processing"):
                        return

                    try:
                        server.send_message(msg, self._param.email, recipients)
                        success = True
                    except Exception as e:
                        logging.error(f"Error during send_message: {str(e)}")
                        # Try alternative method
                        server.sendmail(self._param.email, recipients, msg.as_string())
                        success = True

                    try:
                        server.quit()
                    except Exception as e:
                        # Ignore errors when closing connection
                        logging.warning(f"Non-fatal error during connection close: {str(e)}")

                self.set_output("success", success)
                return success

            except json.JSONDecodeError:
                error_msg = "Invalid JSON format in input"
                logging.error(error_msg)
                self.set_output("_ERROR", error_msg)
                self.set_output("success", False)
                return False

            except smtplib.SMTPAuthenticationError:
                error_msg = "SMTP Authentication failed. Please check your email and authorization code."
                logging.error(error_msg)
                self.set_output("_ERROR", error_msg)
                self.set_output("success", False)
                return False

            except smtplib.SMTPConnectError:
                error_msg = f"Failed to connect to SMTP server {self._param.smtp_server}:{self._param.smtp_port}"
                logging.error(error_msg)
                last_e = error_msg
                time.sleep(self._param.delay_after_error)

            except smtplib.SMTPException as e:
                error_msg = f"SMTP error occurred: {str(e)}"
                logging.error(error_msg)
                last_e = error_msg
                time.sleep(self._param.delay_after_error)

            except Exception as e:
                error_msg = f"Unexpected error: {str(e)}"
                logging.error(error_msg)
                self.set_output("_ERROR", error_msg)
                self.set_output("success", False)
                return False

        if last_e:
            self.set_output("_ERROR", str(last_e))
            return False

        assert False, self.output()

    def thoughts(self) -> str:
        inputs = self.get_input()
        return """
To: {}
Subject: {}
Your email is on its way—sit tight!
""".format(inputs.get("to_email", "-_-!"), inputs.get("subject", "-_-!"))

```

## Detailed Analysis

### File Role in Repository

The file `agent/tools/email.py` is located in the `agent/tools` directory.

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
- [exesql.py](exesql.py_docs.md)
- [github.py](github.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
