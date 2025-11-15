# Documentation: docker/docker-compose.yml

## File Metadata

- **Path**: `docker/docker-compose.yml`
- **Size**: 5733 bytes
- **Type**: .yml
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `docker/docker-compose.yml`.

## Original Source Code

```yml
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

## Detailed Analysis

### File Role in Repository

The file `docker/docker-compose.yml` is located in the `docker` directory.

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
- [entrypoint.sh](entrypoint.sh_docs.md)
- [infinity_conf.toml](infinity_conf.toml_docs.md)
- [init.sql](init.sql_docs.md)
- [launch_backend_service.sh](launch_backend_service.sh_docs.md)
- [migration.sh](migration.sh_docs.md)
- [service_conf.yaml.template](service_conf.yaml.template_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
