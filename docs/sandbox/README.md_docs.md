# Documentation: sandbox/README.md

## File Metadata

- **Path**: `sandbox/README.md`
- **Size**: 8556 bytes
- **Type**: .md
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `sandbox/README.md`.

## Original Source Code

```md
# RAGFlow Sandbox

A secure, pluggable code execution backend for RAGFlow and beyond.

## 🔧 Features

- ✅ **Seamless RAGFlow Integration** — Out-of-the-box compatibility with the `code` component.
- 🔐 **High Security** — Leverages [gVisor](https://gvisor.dev/) for syscall-level sandboxing.
- 🔧 **Customizable Sandboxing** — Easily modify `seccomp` settings as needed.
- 🧩 **Pluggable Runtime Support** — Easily extend to support any programming language.
- ⚙️ **Developer Friendly** — Get started with a single command using `Makefile`.

## 🏗 Architecture

<p align="center">
  <img src="asserts/code_executor_manager.svg" width="520" alt="Architecture Diagram">
</p>

## 🚀 Quick Start

### 📋 Prerequisites

#### Required

- Linux distro compatible with gVisor
- [gVisor](https://gvisor.dev/docs/user_guide/install/)
- Docker >= `24.0.0`
- Docker Compose >= `v2.26.1` like [RAGFlow](https://github.com/infiniflow/ragflow)
- [uv](https://docs.astral.sh/uv/) as package and project manager

#### Optional (Recommended)

- [GNU Make](https://www.gnu.org/software/make/) for simplified CLI management

---

### 🐳 Build Docker Base Images

We use isolated base images for secure containerized execution:

```bash
# Build base images manually
docker build -t sandbox-base-python:latest ./sandbox_base_image/python
docker build -t sandbox-base-nodejs:latest ./sandbox_base_image/nodejs

# OR use Makefile
make build
```

Then, build the executor manager image:

```bash
docker build -t sandbox-executor-manager:latest ./executor_manager
```

---

### 📦 Running with RAGFlow

1. Ensure gVisor is correctly installed.
2. Configure your `.env` in `docker/.env`:

   - Uncomment sandbox-related variables.
   - Enable sandbox profile at the bottom.
3. Add the following line to `/etc/hosts` as recommended:

   ```text
   127.0.0.1 sandbox-executor-manager
   ```

4. Start RAGFlow service.

---

### 🧭 Running Standalone

#### Manual Setup

1. Initialize environment:

   ```bash
   cp .env.example .env
   ```

2. Launch:

   ```bash
   docker compose -f docker-compose.yml up
   ```

3. Test:

   ```bash
   source .venv/bin/activate
   export PYTHONPATH=$(pwd)
   uv pip install -r executor_manager/requirements.txt
   uv run tests/sandbox_security_tests_full.py
   ```

#### With Make

```bash
make          # setup + build + launch + test
```

---

### 📈 Monitoring

```bash
docker logs -f sandbox-executor-manager  # Manual
make logs                                 # With Make
```

---

### 🧰 Makefile Toolbox

| Command           | Description                                      |
| ----------------- | ------------------------------------------------ |
| `make`            | Setup, build, launch and test all at once        |
| `make setup`      | Initialize environment and install uv            |
| `make ensure_env` | Auto-create `.env` if missing                    |
| `make ensure_uv`  | Install `uv` package manager if missing          |
| `make build`      | Build all Docker base images                     |
| `make start`      | Start services with safe env loading and testing |
| `make stop`       | Gracefully stop all services                     |
| `make restart`    | Shortcut for `stop` + `start`                    |
| `make test`       | Run full test suite                              |
| `make logs`       | Stream container logs                            |
| `make clean`      | Stop and remove orphan containers and volumes    |

---

## 🔐 Security

The RAGFlow sandbox is designed to balance security and usability, offering solid protection without compromising developer experience.

### ✅ gVisor Isolation

At its core, we use [gVisor](https://gvisor.dev/docs/architecture_guide/security/), a user-space kernel, to isolate code execution from the host system. gVisor intercepts and restricts syscalls, offering robust protection against container escapes and privilege escalations.

### 🔒 Optional seccomp Support (Advanced)

For users who need **zero-trust-level syscall control**, we support an additional `seccomp` profile. This feature restricts containers to only a predefined set of system calls, as specified in `executor_manager/seccomp-profile-default.json`.

> ⚠️ This feature is **disabled by default** to maintain compatibility and usability. Enabling it may cause compatibility issues with some dependencies.

#### To enable seccomp

1. Edit your `.env` file:

   ```dotenv
   SANDBOX_ENABLE_SECCOMP=true
   ```

2. Customize allowed syscalls in:

   ```
   executor_manager/seccomp-profile-default.json
   ```

   This profile is passed to the container with:

   ```bash
   --security-opt seccomp=/app/seccomp-profile-default.json
   ```

### 🧠 Python Code AST Inspection

In addition to sandboxing, Python code is **statically analyzed via AST (Abstract Syntax Tree)** before execution. Potentially malicious code (e.g. file operations, subprocess calls, etc.) is rejected early, providing an extra layer of protection.

---

This security model strikes a balance between **robust isolation** and **developer usability**. While `seccomp` can be highly restrictive, our default setup aims to keep things usable for most developers — no obscure crashes or cryptic setup required.

## 📦 Add Extra Dependencies for Supported Languages

Currently, the following languages are officially supported:

| Language | Priority |
| -------- | -------- |
| Python   | High     |
| Node.js  | Medium   |

### 🐍 Python

To add Python dependencies, simply edit the following file:

```bash
sandbox_base_image/python/requirements.txt
```

Add any additional packages you need, one per line (just like a normal pip requirements file).

### 🟨 Node.js

To add Node.js dependencies:

1. Navigate to the Node.js base image directory:

   ```bash
   cd sandbox_base_image/nodejs
   ```

2. Use `npm` to install the desired packages. For example:

   ```bash
   npm install lodash
   ```

3. The dependencies will be saved to `package.json` and `package-lock.json`, and included in the Docker image when rebuilt.

---


## Usage

### 🐍 A Python example

```python
def main(arg1: str, arg2: str) -> str:
    return f"result: {arg1 + arg2}"
```

### 🟨 JavaScript examples

A simple sync function

```javascript
function main({arg1, arg2}) {
  return arg1+arg2
}
```

Async funcion with aioxs

```javascript
const axios = require('axios');
async function main() {
  try {
    const response = await axios.get('https://github.com/infiniflow/ragflow');
    return 'Body:' + response.data;
  } catch (error) {
    return 'Error:' + error.message;
  }
}
```

---

## 📋 FAQ

### ❓Sandbox Not Working?

Follow this checklist to troubleshoot:

- [ ] **Is your machine compatible with gVisor?**

  Ensure that your system supports gVisor. Refer to the [gVisor installation guide](https://gvisor.dev/docs/user_guide/install/).

- [ ] **Is gVisor properly installed?**

  **Common error:**

  `HTTPConnectionPool(host='sandbox-executor-manager', port=9385): Read timed out.`

  Cause: `runsc` is an unknown or invalid Docker runtime.
  **Fix:**

  - Install gVisor

  - Restart Docker

  - Test with:

    ```bash
    docker run --rm --runtime=runsc hello-world
    ```

- [ ] **Is `sandbox-executor-manager` mapped in `/etc/hosts`?**

  **Common error:**

  `HTTPConnectionPool(host='none', port=9385): Max retries exceeded.`

  **Fix:**

  Add the following entry to `/etc/hosts`:

  ```text
  127.0.0.1 es01 infinity mysql minio redis sandbox-executor-manager
  ```

- [ ] **Have you enabled sandbox-related configurations in RAGFlow?**

  Double-check that all sandbox settings are correctly enabled in your RAGFlow configuration.

- [ ] **Have you pulled the required base images for the runners?**

  **Common error:**

  `HTTPConnectionPool(host='sandbox-executor-manager', port=9385): Read timed out.`

  Cause: no runner was started.

  **Fix:**

  Pull the necessary base images:

  ```bash
  docker pull infiniflow/sandbox-base-nodejs:latest
  docker pull infiniflow/sandbox-base-python:latest
  ```

- [ ] **Did you restart the service after making changes?**

  Any changes to configuration or environment require a full service restart to take effect.


### ❓Container pool is busy?

All available runners are currently in use, executing tasks/running code. Please try again shortly, or consider increasing the pool size in the configuration to improve availability and reduce wait times.

## 🤝 Contribution

Contributions are welcome!

```

## Detailed Analysis

### File Role in Repository

The file `sandbox/README.md` is located in the `sandbox` directory.

### Architecture Context

Files in this location typically handle concerns related to sandbox.

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

- [Makefile](Makefile_docs.md)
- [docker-compose.yml](docker-compose.yml_docs.md)
- [pyproject.toml](pyproject.toml_docs.md)
- [uv.lock](uv.lock_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
