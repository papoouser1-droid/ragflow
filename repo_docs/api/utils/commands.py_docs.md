# File Documentation: api/utils/commands.py

## File Metadata

- **Path**: `api/utils/commands.py`
- **Extension**: `.py`
- **Lines**: 79
- **Characters**: 3,363
- **Size**: 3,363 bytes
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


### Functions (3)

- `reset_password()`: Function definition
- `reset_email()`: Function definition
- `register_commands()`: Function definition

### Imports (6)

- `import base64`
- `import click`
- `import re`
- `from flask import Flask`
- `from werkzeug.security import generate_password_hash`
- `from api.db.services import UserService`

## Code Structure Analysis

- Total lines: 79
- Blank lines: 9 (11.4%)
- Comment lines: ~15 (19.0%)
- Code lines: ~55


## Dependencies and Imports

- `import base64`
- `import click`
- `import re`
- `from flask import Flask`
- `from werkzeug.security import generate_password_hash`
- `from api.db.services import UserService`

## Design & Architecture

This file is located in the `api` directory, specifically within `api/utils`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity
- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `api/utils/` directory
- Imports from `flask`
- Imports from `werkzeug.security`
- Imports from `api.db.services`
- Potential test file: `test_commands.py`

## Keywords

ANY, All, Apache, Authors, BASIS, CONDITIONS, Congratulations, Copyright, Email, Flask, InfiniFlow, KIND, LICENSE, License, Licensed, Password, Python, Reserved, Reset, Rights, See, Sorry, The, True, Unless, UserService, Version, WARRANTIES, WITHOUT, You, click, register_commands, reset_email, reset_password

---
*Generated by RAGFlow Repository Documentation Generator*
