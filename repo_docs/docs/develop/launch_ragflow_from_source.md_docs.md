# File Documentation: docs/develop/launch_ragflow_from_source.md

## File Metadata

- **Path**: `docs/develop/launch_ragflow_from_source.md`
- **Extension**: `.md`
- **Lines**: 142
- **Characters**: 3,613
- **Size**: 3,613 bytes
- **Purpose**: Documentation - Markdown documentation file

## Original Source

```markdown
---
sidebar_position: 2
slug: /launch_ragflow_from_source
---

# Launch service from source

A guide explaining how to set up a RAGFlow service from its source code. By following this guide, you'll be able to debug using the source code.

## Target audience

Developers who have added new features or modified existing code and wish to debug using the source code, *provided that* their machine has the target deployment environment set up.

## Prerequisites

- CPU &ge; 4 cores
- RAM &ge; 16 GB
- Disk &ge; 50 GB
- Docker &ge; 24.0.0 & Docker Compose &ge; v2.26.1

:::tip NOTE
If you have not installed Docker on your local machine (Windows, Mac, or Linux), see the [Install Docker Engine](https://docs.docker.com/engine/install/) guide.
:::

## Launch a service from source

To launch a RAGFlow service from source code:

### Clone the RAGFlow repository

```bash
git clone https://github.com/infiniflow/ragflow.git
cd ragflow/
```

### Install Python dependencies

1. Install uv:
   
   ```bash
   pipx install uv
   ```

2. Install Python dependencies:

   ```bash
   uv sync --python 3.10 # install RAGFlow dependent python modules
   ```
   *A virtual environment named `.venv` is created, and all Python dependencies are installed into the new environment.*

### Launch third-party services

The following command launches the 'base' services (MinIO, Elasticsearch, Redis, and MySQL) using Docker Compose:

```bash
docker compose -f docker/docker-compose-base.yml up -d
```

### Update `host` and `port` Settings for Third-party Services

1. Add the following line to `/etc/hosts` to resolve all hosts specified in **docker/service_conf.yaml.template** to `127.0.0.1`:

   ```
   127.0.0.1       es01 infinity mysql minio redis
   ```

2. In **docker/service_conf.yaml.template**, update mysql port to `5455` and es port to `1200`, as specified in **docker/.env**.

### Launch the RAGFlow backend service

1. Comment out the `nginx` line in **docker/entrypoint.sh**.

   ```
   # /usr/sbin/nginx
   ```

2. Activate the Python virtual environment:

   ```bash
   source .venv/bin/activate
   export PYTHONPATH=$(pwd)
   ```

3. **Optional:** If you cannot access HuggingFace, set the HF_ENDPOINT environment variable to use a mirror site:
 
   ```bash
   export HF_ENDPOINT=https://hf-mirror.com
   ```

4. Check the configuration in **conf/service_conf.yaml**, ensuring all hosts and ports are correctly set.
   
5. Run the **entrypoint.sh** script to launch the backend service:

   ```shell
   JEMALLOC_PATH=$(pkg-config --variable=libdir jemalloc)/libjemalloc.so;
   LD_PRELOAD=$JEMALLOC_PATH python rag/svr/task_executor.py 1;
   ```
   ```shell
   python api/ragflow_server.py;
   ```

### Launch the RAGFlow frontend service

1. Navigate to the `web` directory and install the frontend dependencies:

   ```bash
   cd web
   npm install
   ```

2. Update `proxy.target` in **.umirc.ts** to `http://127.0.0.1:9380`:

   ```bash
   vim .umirc.ts
   ```

3. Start up the RAGFlow frontend service:

   ```bash
   npm run dev 
   ```

   *The following message appears, showing the IP address and port number of your frontend service:*  

   ![](https://github.com/user-attachments/assets/0daf462c-a24d-4496-a66f-92533534e187)

### Access the RAGFlow service

In your web browser, enter `http://127.0.0.1:<PORT>/`, ensuring the port number matches that shown in the screenshot above.

### Stop the RAGFlow service when the development is done

1. Stop the RAGFlow frontend service:
   ```bash
   pkill npm
   ```

2. Stop the RAGFlow backend service:
   ```bash
   pkill -f "docker/entrypoint.sh"
   ```

```

## High-Level Overview

# Launch service from source

## Detailed Walkthrough

This is a documentation file. See the 'Original Source' section for full content.

## Code Structure Analysis

- Total lines: 142
- Blank lines: 48 (33.8%)
- Comment lines: ~15 (10.6%)
- Code lines: ~79


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `docs` directory, specifically within `docs/develop`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `docs/develop/` directory
- Potential test file: `test_launch_ragflow_from_source.md`

## Keywords

Access, Activate, Add, CPU, Check, Clone, Comment, Compose, Developers, Disk, Docker, Documentation, Elasticsearch, Engine, HF_ENDPOINT, HuggingFace, Install, JEMALLOC_PATH, LD_PRELOAD, Launch, Linux, Mac, MinIO, MySQL, NOTE, Navigate, Optional, PORT, PYTHONPATH, Prerequisites, Python, RAGFlow, RAM, Redis, Run, Services, Settings, Start, Stop, Target, The, Third, Update, Windows

---
*Generated by RAGFlow Repository Documentation Generator*
