# File Documentation: sandbox/Makefile

## File Metadata

- **Path**: `sandbox/Makefile`
- **Extension**: `none`
- **Lines**: 116
- **Characters**: 3,640
- **Size**: 3,718 bytes
- **Purpose**: General file in the repository

## Original Source

```
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

# Force using Bash to ensure the source command is available
SHELL := /bin/bash

# Environment variable definitions
VENV := .venv
PYTHON := $(VENV)/bin/python
UV := uv
ACTIVATE_SCRIPT := $(VENV)/bin/activate
SYS_PYTHON := python3
PYTHONPATH := $(shell pwd)

.PHONY: all setup ensure_env ensure_uv start stop restart build clean test logs

all: setup start

# 🌱 Initialize environment + install dependencies
setup: ensure_env ensure_uv
	@echo "📦 Installing dependencies with uv..."
	@$(UV) sync --python 3.11
	source $(ACTIVATE_SCRIPT) && \
	export PYTHONPATH=$(PYTHONPATH)
	@$(UV) pip install -r executor_manager/requirements.txt
	@echo "✅ Setup complete."

# 🔑 Ensure .env exists (copy from .env.example on first run)
ensure_env:
	@if [ ! -f ".env" ]; then \
		if [ -f ".env.example" ]; then \
			echo "📝 Creating .env from .env.example..."; \
			cp .env.example .env; \
		else \
			echo "⚠️ Warning: .env.example not found, creating empty .env"; \
			touch .env; \
		fi; \
	else \
		echo "✅ .env already exists."; \
	fi

# 🔧 Ensure uv is executable (install using system Python)
ensure_uv:
	@if ! command -v $(UV) >/dev/null 2>&1; then \
		echo "🛠️ Installing uv using system Python..."; \
		$(SYS_PYTHON) -m pip install -q --upgrade pip; \
		$(SYS_PYTHON) -m pip install -q uv || (echo "⚠️ uv install failed, check manually" && exit 1); \
	fi

# 🐳 Service control (using safer variable loading)
start:
	@echo "🚀 Starting services..."
	source $(ACTIVATE_SCRIPT) && \
	export PYTHONPATH=$(PYTHONPATH) && \
	[ -f .env ] && source .env || true && \
	bash scripts/start.sh

stop:
	@echo "🛑 Stopping services..."
	source $(ACTIVATE_SCRIPT) && \
	bash scripts/stop.sh

restart: stop start
	@echo "🔁 Restarting services..."

build:
	@echo "🔧 Building base sandbox images..."
	@if [ -f .env ]; then \
		source .env && \
		echo "🐍 Building base sandbox image for Python ($$SANDBOX_BASE_PYTHON_IMAGE)..." && \
		docker build -t "$$SANDBOX_BASE_PYTHON_IMAGE" ./sandbox_base_image/python && \
		echo "⬢ Building base sandbox image for Nodejs ($$SANDBOX_BASE_NODEJS_IMAGE)..." && \
		docker build -t "$$SANDBOX_BASE_NODEJS_IMAGE" ./sandbox_base_image/nodejs; \
	else \
		echo "⚠️ .env file not found, skipping build."; \
	fi

test:
	@echo "🧪 Running sandbox security tests..."
	source $(ACTIVATE_SCRIPT) && \
	export PYTHONPATH=$(PYTHONPATH) && \
	$(PYTHON) tests/sandbox_security_tests_full.py

logs:
	@echo "📋 Showing logs from api-server and executor-manager..."
	docker compose logs -f

# 🧹 Clean all containers and volumes
clean:
	@echo "🧹 Cleaning all containers and volumes..."
	@docker compose down -v || true
	@if [ -f .env ]; then \
		source .env && \
		for i in $$(seq 0 $$((SANDBOX_EXECUTOR_MANAGER_POOL_SIZE - 1))); do \
			echo "🧹 Deleting sandbox_python_$$i..." && \
			docker rm -f sandbox_python_$$i 2>/dev/null || true && \
			echo "🧹 Deleting sandbox_nodejs_$$i..." && \
			docker rm -f sandbox_nodejs_$$i 2>/dev/null || true; \
		done; \
	else \
		echo "⚠️ .env not found, skipping container cleanup"; \
	fi

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

This file's structure is not automatically analyzed. See the 'Original Source' section for content.

## Code Structure Analysis

- Total lines: 116
- Blank lines: 15 (12.9%)
- Comment lines: ~22 (19.0%)
- Code lines: ~79


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `sandbox` directory, specifically within `sandbox`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 4 loop(s) - consider algorithmic complexity

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

- Other files in `sandbox/` directory
- Potential test file: `test_Makefile`

## Keywords

ACTIVATE_SCRIPT, ANY, All, Apache, Authors, BASIS, Bash, Building, CONDITIONS, Clean, Cleaning, Copyright, Creating, Deleting, Ensure, Environment, Force, InfiniFlow, Initialize, Installing, KIND, LICENSE, License, Licensed, Nodejs, PHONY, PYTHON, PYTHONPATH, Python, Reserved, Restarting, Rights, Running, SANDBOX_BASE_NODEJS_IMAGE, SANDBOX_BASE_PYTHON_IMAGE, SANDBOX_EXECUTOR_MANAGER_POOL_SIZE, SHELL, SYS_PYTHON, See, Service, Setup, Showing, Starting, Stopping, The, Unless, VENV, Version, WARRANTIES, WITHOUT...

---
*Generated by RAGFlow Repository Documentation Generator*
