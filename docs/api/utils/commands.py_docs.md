# Documentation: api/utils/commands.py

## File Metadata

- **Path**: `api/utils/commands.py`
- **Size**: 3363 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `api/utils/commands.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `base64`
- `click`
- `re`
- `flask`
- `werkzeug.security`
- `api.db.services`

### Functions Defined

This file defines 3 function(s):

#### Function: `reset_password` (line 31)

**Parameters**: email, new_password, password_confirm

#### Function: `reset_email` (line 52)

**Parameters**: email, new_email, email_confirm

#### Function: `register_commands` (line 76)

**Parameters**: app

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

import base64
import click
import re

from flask import Flask
from werkzeug.security import generate_password_hash

from api.db.services import UserService


@click.command('reset-password', help='Reset the account password.')
@click.option('--email', prompt=True, help='The email address of the account whose password you need to reset')
@click.option('--new-password', prompt=True, help='the new password.')
@click.option('--password-confirm', prompt=True, help='the new password confirm.')
def reset_password(email, new_password, password_confirm):
    if str(new_password).strip() != str(password_confirm).strip():
        click.echo(click.style('sorry. The two passwords do not match.', fg='red'))
        return
    user = UserService.query(email=email)
    if not user:
        click.echo(click.style('sorry. The Email is not registered!.', fg='red'))
        return
    encode_password = base64.b64encode(new_password.encode('utf-8')).decode('utf-8')
    password_hash = generate_password_hash(encode_password)
    user_dict = {
        'password': password_hash
    }
    UserService.update_user(user[0].id,user_dict)
    click.echo(click.style('Congratulations! Password has been reset.', fg='green'))


@click.command('reset-email', help='Reset the account email.')
@click.option('--email', prompt=True, help='The old email address of the account whose email you need to reset')
@click.option('--new-email', prompt=True, help='the new email.')
@click.option('--email-confirm', prompt=True, help='the new email confirm.')
def reset_email(email, new_email, email_confirm):
    if str(new_email).strip() != str(email_confirm).strip():
        click.echo(click.style('Sorry, new email and confirm email do not match.', fg='red'))
        return
    if str(new_email).strip() == str(email).strip():
        click.echo(click.style('Sorry, new email and old email are the same.', fg='red'))
        return
    user = UserService.query(email=email)
    if not user:
        click.echo(click.style('sorry. the account: [{}] not exist .'.format(email), fg='red'))
        return
    if not re.match(r"^[\w\._-]+@([\w_-]+\.)+[\w-]{2,4}$", new_email):
        click.echo(click.style('sorry. {} is not a valid email. '.format(new_email), fg='red'))
        return
    new_user = UserService.query(email=new_email)
    if new_user:
        click.echo(click.style('sorry. the account: [{}] is exist .'.format(new_email), fg='red'))
        return
    user_dict = {
        'email': new_email
    }
    UserService.update_user(user[0].id,user_dict)
    click.echo(click.style('Congratulations!, email has been reset.', fg='green'))

def register_commands(app: Flask):
    app.cli.add_command(reset_password)
    app.cli.add_command(reset_email)

```

## Detailed Analysis

### File Role in Repository

The file `api/utils/commands.py` is located in the `api/utils` directory.

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
- [common.py](common.py_docs.md)
- [configs.py](configs.py_docs.md)
- [crypt.py](crypt.py_docs.md)
- [email_templates.py](email_templates.py_docs.md)
- [file_utils.py](file_utils.py_docs.md)
- [health_utils.py](health_utils.py_docs.md)
- [json_encode.py](json_encode.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
