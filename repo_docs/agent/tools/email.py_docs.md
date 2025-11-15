# File Documentation: agent/tools/email.py

## File Metadata

- **Path**: `agent/tools/email.py`
- **Extension**: `.py`
- **Lines**: 231
- **Characters**: 8,839
- **Size**: 8,841 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

## Original Source

```python
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

## High-Level Overview

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

## Detailed Walkthrough

### Classes (2)

- `EmailParam`: Class definition
- `Email`: Class definition

### Imports (12)

- `import os`
- `import time`
- `from abc import ABC`
- `import json`
- `import smtplib`
- `import logging`
- `from email.mime.text import MIMEText`
- `from email.mime.multipart import MIMEMultipart`
- `from email.header import Header`
- `from email.utils import formataddr`

## Code Structure Analysis

- Total lines: 231
- Blank lines: 32 (13.9%)
- Comment lines: ~32 (13.9%)
- Code lines: ~167


## Dependencies and Imports

- `import os`
- `import time`
- `from abc import ABC`
- `import json`
- `import smtplib`
- `import logging`
- `from email.mime.text import MIMEText`
- `from email.mime.multipart import MIMEMultipart`
- `from email.header import Header`
- `from email.utils import formataddr`
- `from agent.tools.base import ToolParamBase, ToolBase, ToolMeta`
- `from common.connection_utils import timeout`

## Design & Architecture

This file is located in the `agent` directory, specifically within `agent/tools`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 4 loop(s) - consider algorithmic complexity
- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `agent/tools/` directory
- Imports from `abc`
- Imports from `email.mime.text`
- Imports from `email.mime.multipart`
- Imports from `email.header`
- Imports from `email.utils`
- Potential test file: `test_email.py`

## Keywords

ABC, ANY, All, Apache, Attempting, Authentication, Authors, BASIS, BCC, COMPONENT_EXEC_TIMEOUT, CONDITIONS, Check, Comma, Connect, Connecting, Copyright, Create, Define, Email, EmailParam, Error, Exception, Failed, False, Fixed, From, Get, HTML, Header, Ignore, InfiniFlow, Internet, Invalid, JSON, JSONDecodeError, KIND, LICENSE, License, Licensed, Login, MIMEMultipart, MIMEText, Missing, Name, Non, Parse, Password, Please, Properly, Python...

---
*Generated by RAGFlow Repository Documentation Generator*
