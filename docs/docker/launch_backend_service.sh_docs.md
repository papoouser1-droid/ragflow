# Documentation: docker/launch_backend_service.sh

## File Metadata

- **Path**: `docker/launch_backend_service.sh`
- **Size**: 3492 bytes
- **Type**: .sh
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `docker/launch_backend_service.sh`.

## Original Source Code

```sh
#!/bin/bash

# Exit immediately if a command exits with a non-zero status
set -e

# Function to load environment variables from .env file
load_env_file() {
    # Get the directory of the current script
    local script_dir="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
    local env_file="$script_dir/.env"

    # Check if .env file exists
    if [ -f "$env_file" ]; then
        echo "Loading environment variables from: $env_file"
        # Source the .env file
        set -a
        source "$env_file" 
        set +a
    else
        echo "Warning: .env file not found at: $env_file"
    fi
}

# Load environment variables
load_env_file

# Unset HTTP proxies that might be set by Docker daemon
export http_proxy=""; export https_proxy=""; export no_proxy=""; export HTTP_PROXY=""; export HTTPS_PROXY=""; export NO_PROXY=""
export PYTHONPATH=$(pwd)

export LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu/
JEMALLOC_PATH=$(pkg-config --variable=libdir jemalloc)/libjemalloc.so

PY=python3

# Set default number of workers if WS is not set or less than 1
if [[ -z "$WS" || $WS -lt 1 ]]; then
  WS=1
fi

# Maximum number of retries for each task executor and server
MAX_RETRIES=5

# Flag to control termination
STOP=false

# Array to keep track of child PIDs
PIDS=()

# Set the path to the NLTK data directory
export NLTK_DATA="./nltk_data"

# Function to handle termination signals
cleanup() {
  echo "Termination signal received. Shutting down..."
  STOP=true
  # Terminate all child processes
  for pid in "${PIDS[@]}"; do
    if kill -0 "$pid" 2>/dev/null; then
      echo "Killing process $pid"
      kill "$pid"
    fi
  done
  exit 0
}

# Trap SIGINT and SIGTERM to invoke cleanup
trap cleanup SIGINT SIGTERM

# Function to execute task_executor with retry logic
task_exe(){
    local task_id=$1
    local retry_count=0
    while ! $STOP && [ $retry_count -lt $MAX_RETRIES ]; do
        echo "Starting task_executor.py for task $task_id (Attempt $((retry_count+1)))"
        LD_PRELOAD=$JEMALLOC_PATH $PY rag/svr/task_executor.py "$task_id"
        EXIT_CODE=$?
        if [ $EXIT_CODE -eq 0 ]; then
            echo "task_executor.py for task $task_id exited successfully."
            break
        else
            echo "task_executor.py for task $task_id failed with exit code $EXIT_CODE. Retrying..." >&2
            retry_count=$((retry_count + 1))
            sleep 2
        fi
    done

    if [ $retry_count -ge $MAX_RETRIES ]; then
        echo "task_executor.py for task $task_id failed after $MAX_RETRIES attempts. Exiting..." >&2
        cleanup
    fi
}

# Function to execute ragflow_server with retry logic
run_server(){
    local retry_count=0
    while ! $STOP && [ $retry_count -lt $MAX_RETRIES ]; do
        echo "Starting ragflow_server.py (Attempt $((retry_count+1)))"
        $PY api/ragflow_server.py
        EXIT_CODE=$?
        if [ $EXIT_CODE -eq 0 ]; then
            echo "ragflow_server.py exited successfully."
            break
        else
            echo "ragflow_server.py failed with exit code $EXIT_CODE. Retrying..." >&2
            retry_count=$((retry_count + 1))
            sleep 2
        fi
    done

    if [ $retry_count -ge $MAX_RETRIES ]; then
        echo "ragflow_server.py failed after $MAX_RETRIES attempts. Exiting..." >&2
        cleanup
    fi
}

# Start task executors
for ((i=0;i<WS;i++))
do
  task_exe "$i" &
  PIDS+=($!)
done

# Start the main server
run_server &
PIDS+=($!)

# Wait for all background processes to finish
wait

```

## Detailed Analysis

### File Role in Repository

The file `docker/launch_backend_service.sh` is located in the `docker` directory.

### Architecture Context

Files in this location typically handle concerns related to docker.

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

- [README.md](README.md_docs.md)
- [docker-compose-CN-oc9.yml](docker-compose-CN-oc9.yml_docs.md)
- [docker-compose-base.yml](docker-compose-base.yml_docs.md)
- [docker-compose-macos.yml](docker-compose-macos.yml_docs.md)
- [docker-compose.yml](docker-compose.yml_docs.md)
- [entrypoint.sh](entrypoint.sh_docs.md)
- [infinity_conf.toml](infinity_conf.toml_docs.md)
- [init.sql](init.sql_docs.md)
- [migration.sh](migration.sh_docs.md)
- [service_conf.yaml.template](service_conf.yaml.template_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
