# File Documentation: sandbox/executor_manager/core/container.py

## File Metadata

- **Path**: `sandbox/executor_manager/core/container.py`
- **Extension**: `.py`
- **Lines**: 192
- **Characters**: 7,571
- **Size**: 7,603 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

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
import asyncio
import contextlib
import os
from queue import Empty, Queue

from models.enums import SupportLanguage
from util import env_setting_enabled, is_valid_memory_limit
from utils.common import async_run_command

from core.logger import logger

_CONTAINER_QUEUES: dict[SupportLanguage, Queue] = {}
_CONTAINER_LOCK: asyncio.Lock = asyncio.Lock()
_CONTAINER_EXECUTION_SEMAPHORES: dict[SupportLanguage, asyncio.Semaphore] = {}


async def init_containers(size: int) -> tuple[int, int]:
    global _CONTAINER_QUEUES
    _CONTAINER_QUEUES = {SupportLanguage.PYTHON: Queue(), SupportLanguage.NODEJS: Queue()}

    async with _CONTAINER_LOCK:
        while not _CONTAINER_QUEUES[SupportLanguage.PYTHON].empty():
            _CONTAINER_QUEUES[SupportLanguage.PYTHON].get_nowait()
        while not _CONTAINER_QUEUES[SupportLanguage.NODEJS].empty():
            _CONTAINER_QUEUES[SupportLanguage.NODEJS].get_nowait()

    for language in SupportLanguage:
        _CONTAINER_EXECUTION_SEMAPHORES[language] = asyncio.Semaphore(size)

    create_tasks = []
    for i in range(size):
        name = f"sandbox_python_{i}"
        logger.info(f"🛠️ Creating Python container {i + 1}/{size}")
        create_tasks.append(_prepare_container(name, SupportLanguage.PYTHON))

        name = f"sandbox_nodejs_{i}"
        logger.info(f"🛠️ Creating Node.js container {i + 1}/{size}")
        create_tasks.append(_prepare_container(name, SupportLanguage.NODEJS))

    results = await asyncio.gather(*create_tasks, return_exceptions=True)
    success_count = sum(1 for r in results if r is True)
    total_task_count = len(create_tasks)
    return success_count, total_task_count


async def teardown_containers():
    async with _CONTAINER_LOCK:
        while not _CONTAINER_QUEUES[SupportLanguage.PYTHON].empty():
            name = _CONTAINER_QUEUES[SupportLanguage.PYTHON].get_nowait()
            await async_run_command("docker", "rm", "-f", name, timeout=5)
        while not _CONTAINER_QUEUES[SupportLanguage.NODEJS].empty():
            name = _CONTAINER_QUEUES[SupportLanguage.NODEJS].get_nowait()
            await async_run_command("docker", "rm", "-f", name, timeout=5)


async def _prepare_container(name: str, language: SupportLanguage) -> bool:
    """Prepare a single container"""
    with contextlib.suppress(Exception):
        await async_run_command("docker", "rm", "-f", name, timeout=5)

    if await create_container(name, language):
        _CONTAINER_QUEUES[language].put(name)
        return True
    return False


async def create_container(name: str, language: SupportLanguage) -> bool:
    """Asynchronously create a container"""
    create_args = [
        "docker",
        "run",
        "-d",
        "--runtime=runsc",
        "--name",
        name,
        "--read-only",
        "--tmpfs",
        "/workspace:rw,exec,size=100M,uid=65534,gid=65534",
        "--tmpfs",
        "/tmp:rw,exec,size=50M",
        "--user",
        "nobody",
        "--workdir",
        "/workspace",
    ]
    if os.getenv("SANDBOX_MAX_MEMORY"):
        memory_limit = os.getenv("SANDBOX_MAX_MEMORY") or "256m"
        if is_valid_memory_limit(memory_limit):
            logger.info(f"SANDBOX_MAX_MEMORY: {os.getenv('SANDBOX_MAX_MEMORY')}")
        else:
            logger.info("Invalid SANDBOX_MAX_MEMORY, using default value: 256m")
            memory_limit = "256m"
        create_args.extend(["--memory", memory_limit])
    else:
        logger.info("Set default SANDBOX_MAX_MEMORY: 256m")
        create_args.extend(["--memory", "256m"])

    if env_setting_enabled("SANDBOX_ENABLE_SECCOMP", "false"):
        logger.info(f"SANDBOX_ENABLE_SECCOMP: {os.getenv('SANDBOX_ENABLE_SECCOMP')}")
        create_args.extend(["--security-opt", "seccomp=/app/seccomp-profile-default.json"])

    if language == SupportLanguage.PYTHON:
        create_args.append(os.getenv("SANDBOX_BASE_PYTHON_IMAGE", "sandbox-base-python:latest"))
    elif language == SupportLanguage.NODEJS:
        create_args.append(os.getenv("SANDBOX_BASE_NODEJS_IMAGE", "sandbox-base-nodejs:latest"))

    logger.info(f"Sandbox config:\n\t {create_args}")

    try:
        returncode, _, stderr = await async_run_command(*create_args, timeout=10)
        if returncode != 0:
            logger.error(f"❌ Container creation failed {name}: {stderr}")
            return False

        if language == SupportLanguage.NODEJS:
            copy_cmd = ["docker", "exec", name, "bash", "-c", "cp -a /app/node_modules /workspace/"]
            returncode, _, stderr = await async_run_command(*copy_cmd, timeout=10)
            if returncode != 0:
                logger.error(f"❌ Failed to prepare dependencies for {name}: {stderr}")
                return False

        return await container_is_running(name)
    except Exception as e:
        logger.error(f"❌ Container creation exception {name}: {str(e)}")
        return False


async def recreate_container(name: str, language: SupportLanguage) -> bool:
    """Asynchronously recreate a container"""
    logger.info(f"🛠️ Recreating container: {name}")
    try:
        await async_run_command("docker", "rm", "-f", name, timeout=5)

        return await create_container(name, language)
    except Exception as e:
        logger.error(f"❌ Container {name} recreation failed: {str(e)}")
        return False


async def release_container(name: str, language: SupportLanguage):
    """Asynchronously release a container"""
    async with _CONTAINER_LOCK:
        if await container_is_running(name):
            _CONTAINER_QUEUES[language].put(name)
            logger.info(f"🟢 Released container: {name} (remaining available: {_CONTAINER_QUEUES[language].qsize()})")
        else:
            logger.warning(f"⚠️ Container {name} has crashed, attempting to recreate...")
            if await recreate_container(name, language):
                _CONTAINER_QUEUES[language].put(name)
                logger.info(f"✅ Container {name} successfully recreated and returned to queue")


async def allocate_container_blocking(language: SupportLanguage, timeout=10) -> str:
    """Asynchronously allocate an available container"""
    start_time = asyncio.get_running_loop().time()
    while asyncio.get_running_loop().time() - start_time < timeout:
        try:
            name = _CONTAINER_QUEUES[language].get_nowait()
            async with _CONTAINER_LOCK:
                if not await container_is_running(name) and not await recreate_container(name, language):
                    continue

                return name
        except Empty:
            await asyncio.sleep(0.1)

    return ""


async def container_is_running(name: str) -> bool:
    """Asynchronously check the container status"""
    try:
        returncode, stdout, _ = await async_run_command("docker", "inspect", "-f", "{{.State.Running}}", name, timeout=2)
        return returncode == 0 and stdout.strip() == "true"
    except Exception:
        return False

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


### Imports (8)

- `import asyncio`
- `import contextlib`
- `import os`
- `from queue import Empty, Queue`
- `from models.enums import SupportLanguage`
- `from util import env_setting_enabled, is_valid_memory_limit`
- `from utils.common import async_run_command`
- `from core.logger import logger`

## Code Structure Analysis

- Total lines: 192
- Blank lines: 35 (18.2%)
- Comment lines: ~21 (10.9%)
- Code lines: ~136


## Dependencies and Imports

- `import asyncio`
- `import contextlib`
- `import os`
- `from queue import Empty, Queue`
- `from models.enums import SupportLanguage`
- `from util import env_setting_enabled, is_valid_memory_limit`
- `from utils.common import async_run_command`
- `from core.logger import logger`

## Design & Architecture

This file is located in the `sandbox` directory, specifically within `sandbox/executor_manager/core`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 10 loop(s) - consider algorithmic complexity
- Uses asynchronous patterns for better performance

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

- Other files in `sandbox/executor_manager/core/` directory
- Imports from `queue`
- Imports from `models.enums`
- Imports from `util`
- Imports from `utils.common`
- Imports from `core.logger`
- Potential test file: `test_container.py`

## Keywords

ANY, All, Apache, Asynchronously, Authors, BASIS, CONDITIONS, Container, Copyright, Creating, Empty, Exception, Failed, False, InfiniFlow, Invalid, KIND, LICENSE, License, Licensed, Lock, NODEJS, Node, PYTHON, Prepare, Python, Queue, Recreating, Released, Reserved, Rights, Running, SANDBOX_BASE_NODEJS_IMAGE, SANDBOX_BASE_PYTHON_IMAGE, SANDBOX_ENABLE_SECCOMP, SANDBOX_MAX_MEMORY, Sandbox, See, Semaphore, Set, State, SupportLanguage, The, True, Unless, Version, WARRANTIES, WITHOUT, You, _prepare_container...

---
*Generated by RAGFlow Repository Documentation Generator*
