# File Documentation: admin/server/admin_server.py

## File Metadata

- **Path**: `admin/server/admin_server.py`
- **Extension**: `.py`
- **Lines**: 80
- **Characters**: 2,708
- **Size**: 2,708 bytes
- **Purpose**: Python Script - Executable Python code

## Original Source

```python
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

import os
import signal
import logging
import time
import threading
import traceback
from werkzeug.serving import run_simple
from flask import Flask
from routes import admin_bp
from common.log_utils import init_root_logger
from common.constants import SERVICE_CONF
from common.config_utils import show_configs
from common import settings
from config import load_configurations, SERVICE_CONFIGS
from auth import init_default_admin, setup_auth
from flask_session import Session
from flask_login import LoginManager
from common.versions import get_ragflow_version

stop_event = threading.Event()

if __name__ == '__main__':
    init_root_logger("admin_service")
    logging.info(r"""
        ____  ___   ______________                 ___       __          _     
       / __ \/   | / ____/ ____/ /___ _      __   /   | ____/ /___ ___  (_)___ 
      / /_/ / /| |/ / __/ /_  / / __ \ | /| / /  / /| |/ __  / __ `__ \/ / __ \
     / _, _/ ___ / /_/ / __/ / / /_/ / |/ |/ /  / ___ / /_/ / / / / / / / / / /
    /_/ |_/_/  |_\____/_/   /_/\____/|__/|__/  /_/  |_\__,_/_/ /_/ /_/_/_/ /_/ 
    """)

    app = Flask(__name__)
    app.register_blueprint(admin_bp)
    app.config["SESSION_PERMANENT"] = False
    app.config["SESSION_TYPE"] = "filesystem"
    app.config["MAX_CONTENT_LENGTH"] = int(
        os.environ.get("MAX_CONTENT_LENGTH", 1024 * 1024 * 1024)
    )
    Session(app)
    logging.info(f'RAGFlow version: {get_ragflow_version()}')
    show_configs()
    login_manager = LoginManager()
    login_manager.init_app(app)
    settings.init_settings()
    setup_auth(login_manager)
    init_default_admin()
    SERVICE_CONFIGS.configs = load_configurations(SERVICE_CONF)

    try:
        logging.info("RAGFlow Admin service start...")
        run_simple(
            hostname="0.0.0.0",
            port=9381,
            application=app,
            threaded=True,
            use_reloader=False,
            use_debugger=True,
        )
    except Exception:
        traceback.print_exc()
        stop_event.set()
        time.sleep(1)
        os.kill(os.getpid(), signal.SIGKILL)

```

## High-Level Overview

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

## Detailed Walkthrough


### Imports (18)

- `import os`
- `import signal`
- `import logging`
- `import time`
- `import threading`
- `import traceback`
- `from werkzeug.serving import run_simple`
- `from flask import Flask`
- `from routes import admin_bp`
- `from common.log_utils import init_root_logger`

## Code Structure Analysis

- Total lines: 80
- Blank lines: 6 (7.5%)
- Comment lines: ~16 (20.0%)
- Code lines: ~58


## Dependencies and Imports

- `import os`
- `import signal`
- `import logging`
- `import time`
- `import threading`
- `import traceback`
- `from werkzeug.serving import run_simple`
- `from flask import Flask`
- `from routes import admin_bp`
- `from common.log_utils import init_root_logger`
- `from common.constants import SERVICE_CONF`
- `from common.config_utils import show_configs`
- `from common import settings`
- `from config import load_configurations, SERVICE_CONFIGS`
- `from auth import init_default_admin, setup_auth`
- `from flask_session import Session`
- `from flask_login import LoginManager`
- `from common.versions import get_ragflow_version`

## Design & Architecture

This file is located in the `admin` directory, specifically within `admin/server`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `admin/server/` directory
- Imports from `werkzeug.serving`
- Imports from `flask`
- Imports from `routes`
- Imports from `common.log_utils`
- Imports from `common.constants`
- Potential test file: `test_admin_server.py`

## Keywords

ANY, Admin, All, Apache, Authors, BASIS, CONDITIONS, Copyright, Event, Exception, False, Flask, InfiniFlow, KIND, LICENSE, License, Licensed, LoginManager, MAX_CONTENT_LENGTH, Python, RAGFlow, Reserved, Rights, SERVICE_CONF, SERVICE_CONFIGS, SESSION_PERMANENT, SESSION_TYPE, SIGKILL, See, Session, The, True, Unless, Version, WARRANTIES, WITHOUT, You

---
*Generated by RAGFlow Repository Documentation Generator*
