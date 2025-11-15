# File Documentation: docker/docker-compose.yml

## File Metadata

- **Path**: `docker/docker-compose.yml`
- **Extension**: `.yml`
- **Lines**: 135
- **Characters**: 5,733
- **Size**: 5,733 bytes
- **Purpose**: Infrastructure - Docker container configuration

## Original Source

```yaml
include:
  - ./docker-compose-base.yml
# To ensure that the container processes the locally modified `service_conf.yaml.template` instead of the one included in its image, you need to mount the local `service_conf.yaml.template` to the container.
services:
  ragflow-cpu:
    depends_on:
      mysql:
        condition: service_healthy
    profiles:
      - cpu
    image: ${RAGFLOW_IMAGE}
    # Example configuration to set up an MCP server:
    # command:
    #   - --enable-mcpserver
    #   - --mcp-host=0.0.0.0
    #   - --mcp-port=9382
    #   - --mcp-base-url=http://127.0.0.1:9380
    #   - --mcp-script-path=/ragflow/mcp/server/server.py
    #   - --mcp-mode=self-host
    #   - --mcp-host-api-key=ragflow-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    # Optional transport flags for MCP (customize if needed).
    # Host mode need to combined with --no-transport-streamable-http-enabled flag, namely, host+streamable-http is not supported yet.
    # The following are enabled by default unless explicitly disabled with --no-<flag>.
    #   - --no-transport-sse-enabled # Disable legacy SSE endpoints (/sse and /messages/)
    #   - --no-transport-streamable-http-enabled #  Disable Streamable HTTP transport (/mcp endpoint)
    #   - --no-json-response # Disable JSON response mode in Streamable HTTP transport (instead of SSE over HTTP)

    # Example configration to start Admin server:
    # command:
    #   - --enable-adminserver
    ports:
      - ${SVR_WEB_HTTP_PORT}:80
      - ${SVR_WEB_HTTPS_PORT}:443
      - ${SVR_HTTP_PORT}:9380
      - ${ADMIN_SVR_HTTP_PORT}:9381
      - ${SVR_MCP_PORT}:9382 # entry for MCP (host_port:docker_port). The docker_port must match the value you set for `mcp-port` above.
    volumes:
      - ./ragflow-logs:/ragflow/logs
      - ./nginx/ragflow.conf:/etc/nginx/conf.d/ragflow.conf
      - ./nginx/proxy.conf:/etc/nginx/proxy.conf
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
      - ../history_data_agent:/ragflow/history_data_agent
      - ./service_conf.yaml.template:/ragflow/conf/service_conf.yaml.template
      - ./entrypoint.sh:/ragflow/entrypoint.sh
    env_file: .env
    networks:
      - ragflow
    restart: on-failure
    # https://docs.docker.com/engine/daemon/prometheus/#create-a-prometheus-configuration
    # If you use Docker Desktop, the --add-host flag is optional. This flag ensures that the host's internal IP is exposed to the Prometheus container.
    extra_hosts:
      - "host.docker.internal:host-gateway"

  ragflow-gpu:
    depends_on:
      mysql:
        condition: service_healthy
    profiles:
      - gpu
    image: ${RAGFLOW_IMAGE}
    # Example configuration to set up an MCP server:
    # command:
    #   - --enable-mcpserver
    #   - --mcp-host=0.0.0.0
    #   - --mcp-port=9382
    #   - --mcp-base-url=http://127.0.0.1:9380
    #   - --mcp-script-path=/ragflow/mcp/server/server.py
    #   - --mcp-mode=self-host
    #   - --mcp-host-api-key=ragflow-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    # Optional transport flags for MCP (customize if needed).
    # Host mode need to combined with --no-transport-streamable-http-enabled flag, namely, host+streamable-http is not supported yet.
    # The following are enabled by default unless explicitly disabled with --no-<flag>.
    #   - --no-transport-sse-enabled # Disable legacy SSE endpoints (/sse and /messages/)
    #   - --no-transport-streamable-http-enabled #  Disable Streamable HTTP transport (/mcp endpoint)
    #   - --no-json-response # Disable JSON response mode in Streamable HTTP transport (instead of SSE over HTTP)

    # Example configration to start Admin server:
    # command:
    #   - --enable-adminserver
    ports:
      - ${SVR_WEB_HTTP_PORT}:80
      - ${SVR_WEB_HTTPS_PORT}:443
      - ${SVR_HTTP_PORT}:9380
      - ${ADMIN_SVR_HTTP_PORT}:9381
      - ${SVR_MCP_PORT}:9382 # entry for MCP (host_port:docker_port). The docker_port must match the value you set for `mcp-port` above.
    volumes:
      - ./ragflow-logs:/ragflow/logs
      - ./nginx/ragflow.conf:/etc/nginx/conf.d/ragflow.conf
      - ./nginx/proxy.conf:/etc/nginx/proxy.conf
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
      - ../history_data_agent:/ragflow/history_data_agent
      - ./service_conf.yaml.template:/ragflow/conf/service_conf.yaml.template
      - ./entrypoint.sh:/ragflow/entrypoint.sh
    env_file: .env
    networks:
      - ragflow
    restart: on-failure
    # https://docs.docker.com/engine/daemon/prometheus/#create-a-prometheus-configuration
    # If you use Docker Desktop, the --add-host flag is optional. This flag ensures that the host's internal IP is exposed to the Prometheus container.
    extra_hosts:
      - "host.docker.internal:host-gateway"
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: all
              capabilities: [gpu]


  # executor:
  #   depends_on:
  #     mysql:
  #       condition: service_healthy
  #   image: ${RAGFLOW_IMAGE}
  #   volumes:
  #     - ./ragflow-logs:/ragflow/logs
  #     - ./nginx/ragflow.conf:/etc/nginx/conf.d/ragflow.conf
  #   env_file: .env
  #   entrypoint: "/ragflow/entrypoint_task_executor.sh 1 3"
  #   networks:
  #     - ragflow
  #   restart: on-failure
  #   # https://docs.docker.com/engine/daemon/prometheus/#create-a-prometheus-configuration
  #   # If you're using Docker Desktop, the --add-host flag is optional. This flag makes sure that the host's internal IP gets exposed to the Prometheus container.
  #   extra_hosts:
  #     - "host.docker.internal:host-gateway"
  #   deploy:
  #     resources:
  #       reservations:
  #         devices:
  #           - driver: nvidia
  #             count: all
  #             capabilities: [gpu]

```

## High-Level Overview

# To ensure that the container processes the locally modified `service_conf.yaml.template` instead of the one included in its image, you need to mount the local `service_conf.yaml.template` to the container.
    # Example configuration to set up an MCP server:
    # command:
    #   - --enable-mcpserver
    #   - --mcp-host=0.0.0.0
    #   - --mcp-port=9382
    #   - --mcp-base-url=http://127.0.0.1:9380
    #   - --mcp-script-path=/ragflow/mcp/server/server.py
    #   - --mcp-mode=self-host
    #   - --mcp-host-api-key=ragflow-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    # Optional transport flags for MCP (customize if needed).
    # Host mode need to combined with --no-transport-streamable-http-enabled flag, namely, host+streamable-http is not supported yet.
    # The following are enabled by default unless explicitly disabled with --no-<flag>.
    #   - --no-transport-sse-enabled # Disable legacy SSE endpoints (/sse and /messages/)
    #   - --no-transport-streamable-http-enabled #  Disable Streamable HTTP transport (/mcp endpoint)
    #   - --no-json-response # Disable JSON response mode in Streamable HTTP transport (instead of SSE over HTTP)

## Detailed Walkthrough

This is a configuration or data file. See the 'Original Source' section for full content.

## Code Structure Analysis

- Total lines: 135
- Blank lines: 6 (4.4%)
- Comment lines: ~65 (48.1%)
- Code lines: ~64


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `docker` directory, specifically within `docker`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 6 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `docker/` directory
- Potential test file: `test_docker-compose.yml`

## Keywords

ADMIN_SVR_HTTP_PORT, Admin, Desktop, Disable, Docker, Example, HTTP, Host, JSON, MCP, Optional, Prometheus, RAGFLOW_IMAGE, SSE, SVR_HTTP_PORT, SVR_MCP_PORT, SVR_WEB_HTTPS_PORT, SVR_WEB_HTTP_PORT, Streamable, The, This

---
*Generated by RAGFlow Repository Documentation Generator*
