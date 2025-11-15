# File Documentation: helm/values.yaml

## File Metadata

- **Path**: `helm/values.yaml`
- **Extension**: `.yaml`
- **Lines**: 235
- **Characters**: 5,481
- **Size**: 5,481 bytes
- **Purpose**: Configuration - YAML configuration file

## Original Source

```yaml
# Based on docker compose .env file

# Global image pull secrets configuration
imagePullSecrets: []

env:
  # The type of doc engine to use.
  # Available options:
  # - `elasticsearch` (default)
  # - `infinity` (https://github.com/infiniflow/infinity)
  # - `opensearch` (https://github.com/opensearch-project/OpenSearch)
  # DOC_ENGINE: elasticsearch
  DOC_ENGINE: infinity
  # DOC_ENGINE: opensearch

  # The version of Elasticsearch.
  STACK_VERSION: "8.11.3"

  # The password for Elasticsearch
  ELASTIC_PASSWORD: infini_rag_flow_helm

  # The password for OpenSearch.
  # At least one uppercase letter, one lowercase letter, one digit, and one special character
  OPENSEARCH_PASSWORD: infini_rag_flow_OS_01

  # The password for MySQL
  MYSQL_PASSWORD: infini_rag_flow_helm
  # The database of the MySQL service to use
  MYSQL_DBNAME: rag_flow

  # The username for MinIO.
  MINIO_ROOT_USER: rag_flow
  # The password for MinIO
  MINIO_PASSWORD: infini_rag_flow_helm

  # The password for Redis
  REDIS_PASSWORD: infini_rag_flow_helm

  # The local time zone.
  TZ: "Asia/Shanghai"

  # Uncomment the following line if you have limited access to huggingface.co:
  # HF_ENDPOINT: https://hf-mirror.com

  # The maximum file size for each uploaded file, in bytes.
  # You can uncomment this line and update the value if you wish to change 128M file size limit
  # MAX_CONTENT_LENGTH: "134217728"
  # After making the change, ensure you update `client_max_body_size` in nginx/nginx.conf correspondingly.

  # The number of document chunks processed in a single batch during document parsing.
  DOC_BULK_SIZE: 4

  # The number of text chunks processed in a single batch during embedding vectorization.
  EMBEDDING_BATCH_SIZE: 16

ragflow:
  image:
    repository: infiniflow/ragflow
    tag: v0.22.0
    pullPolicy: IfNotPresent
    pullSecrets: []
  # Optional service configuration overrides
  # to be written to local.service_conf.yaml
  # inside the RAGFlow container
  # https://ragflow.io/docs/dev/configurations#service-configuration
  service_conf:

  # Optional yaml formatted override for the
  # llm_factories.json file inside the RAGFlow
  # container.
  llm_factories:
    # factory_llm_infos:
    # - name: OpenAI-API-Compatible
    #   logo: ""
    #   tags: "LLM,TEXT EMBEDDING,SPEECH2TEXT,MODERATION"
    #   status: "1"
    #   llm:
    #     - llm_name: my-custom-llm
    #       tags: "LLM,CHAT,"
    #       max_tokens: 100000
    #       model_type: chat
    #       is_tools: false

  # Kubernetes configuration
  deployment:
    strategy:
    resources:
  service:
    # Use LoadBalancer to expose the web interface externally
    type: ClusterIP
  api:
    service:
      enabled: true
      type: ClusterIP

infinity:
  image:
    repository: infiniflow/infinity
    tag: v0.6.5
    pullPolicy: IfNotPresent
    pullSecrets: []
  storage:
    className:
    capacity: 5Gi
  deployment:
    strategy:
    resources:
  service:
    type: ClusterIP

elasticsearch:
  image:
    repository: elasticsearch
    tag: "8.11.3"
    pullPolicy: IfNotPresent
    pullSecrets: []
  initContainers:
    alpine:
      repository: alpine
      tag: latest
      pullPolicy: IfNotPresent
    busybox:
      repository: busybox
      tag: latest
      pullPolicy: IfNotPresent
  storage:
    className:
    capacity: 20Gi
  deployment:
    strategy:
    resources:
      requests:
        cpu: "4"
        memory: "16Gi"
  service:
    type: ClusterIP

opensearch:
  image:
    repository: opensearchproject/opensearch
    tag: 2.19.1
    pullPolicy: IfNotPresent
    pullSecrets: []
  initContainers:
    alpine:
      repository: alpine
      tag: latest
      pullPolicy: IfNotPresent
    busybox:
      repository: busybox
      tag: latest
      pullPolicy: IfNotPresent
  storage:
    className:
    capacity: 20Gi
  deployment:
    strategy:
    resources:
      requests:
        cpu: "4"
        memory: "16Gi"
  service:
    type: ClusterIP

minio:
  image:
    repository: quay.io/minio/minio
    tag: RELEASE.2023-12-20T01-00-02Z
    pullPolicy: IfNotPresent
    pullSecrets: []
  storage:
    className:
    capacity: 5Gi
  deployment:
    strategy:
    resources:
  service:
    type: ClusterIP

mysql:
  image:
    repository: mysql
    tag: 8.0.39
    pullPolicy: IfNotPresent
    pullSecrets: []
  storage:
    className:
    capacity: 5Gi
  deployment:
    strategy:
    resources:
  service:
    type: ClusterIP

redis:
  image:
    repository: valkey/valkey
    tag: 8
    pullPolicy: IfNotPresent
    pullSecrets: []
  storage:
    className:
    capacity: 5Gi
  persistence:
    enabled: true
    # Set's the retention policy for the persistent storage (only available in k8s 1.32 or later)
    # https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/#persistentvolumeclaim-retention
    # retentionPolicy:
      # whenDeleted: Delete
      # whenScaled: Delete
  deployment:
    strategy:
    resources:
  service:
    type: ClusterIP


# This block is for setting up web service ingress. For more information, see:
# https://kubernetes.io/docs/concepts/services-networking/ingress/
ingress:
  enabled: false
  className: ""
  annotations: {}
    # kubernetes.io/ingress.class: nginx
    # kubernetes.io/tls-acme: "true"
  hosts:
    - host: chart-example.local
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls: []
  #  - secretName: chart-example-tls
  #    hosts:
  #      - chart-example.local

```

## High-Level Overview

# Based on docker compose .env file

## Detailed Walkthrough

This is a configuration or data file. See the 'Original Source' section for full content.

## Code Structure Analysis

- Total lines: 235
- Blank lines: 25 (10.6%)
- Comment lines: ~59 (25.1%)
- Code lines: ~151


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `helm` directory, specifically within `helm`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 10 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `helm/` directory
- Potential test file: `test_values.yaml`

## Keywords

API, After, Asia, Available, Based, CHAT, ClusterIP, Compatible, DOC_BULK_SIZE, DOC_ENGINE, Delete, ELASTIC_PASSWORD, EMBEDDING, EMBEDDING_BATCH_SIZE, Elasticsearch, For, Global, HF_ENDPOINT, IfNotPresent, ImplementationSpecific, Kubernetes, LLM, LoadBalancer, MAX_CONTENT_LENGTH, MINIO_PASSWORD, MINIO_ROOT_USER, MODERATION, MYSQL_DBNAME, MYSQL_PASSWORD, MinIO, MySQL, OPENSEARCH_PASSWORD, OpenAI, OpenSearch, Optional, RAGFlow, REDIS_PASSWORD, RELEASE, Redis, SPEECH2TEXT, STACK_VERSION, Set, Shanghai, TEXT, The, This, Uncomment, Use, You, externally...

---
*Generated by RAGFlow Repository Documentation Generator*
