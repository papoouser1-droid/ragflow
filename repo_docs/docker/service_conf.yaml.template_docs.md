# File Documentation: docker/service_conf.yaml.template

## File Metadata

- **Path**: `docker/service_conf.yaml.template`
- **Extension**: `.template`
- **Lines**: 147
- **Characters**: 4,332
- **Size**: 4,332 bytes
- **Purpose**: General file in the repository

## Original Source

```
ragflow:
  host: ${RAGFLOW_HOST:-0.0.0.0}
  http_port: 9380
admin:
  host: ${RAGFLOW_HOST:-0.0.0.0}
  http_port: 9381
mysql:
  name: '${MYSQL_DBNAME:-rag_flow}'
  user: '${MYSQL_USER:-root}'
  password: '${MYSQL_PASSWORD:-infini_rag_flow}'
  host: '${MYSQL_HOST:-mysql}'
  port: 3306
  max_connections: 900
  stale_timeout: 300
  max_allowed_packet: ${MYSQL_MAX_PACKET:-1073741824}
minio:
  user: '${MINIO_USER:-rag_flow}'
  password: '${MINIO_PASSWORD:-infini_rag_flow}'
  host: '${MINIO_HOST:-minio}:9000'
es:
  hosts: 'http://${ES_HOST:-es01}:9200'
  username: '${ES_USER:-elastic}'
  password: '${ELASTIC_PASSWORD:-infini_rag_flow}'
os:
  hosts: 'http://${OS_HOST:-opensearch01}:9201'
  username: '${OS_USER:-admin}'
  password: '${OPENSEARCH_PASSWORD:-infini_rag_flow_OS_01}'
infinity:
  uri: '${INFINITY_HOST:-infinity}:23817'
  db_name: 'default_db'
redis:
  db: 1
  password: '${REDIS_PASSWORD:-infini_rag_flow}'
  host: '${REDIS_HOST:-redis}:6379'
user_default_llm:
  default_models:
    embedding_model:
      api_key: 'xxx'
      base_url: 'http://${TEI_HOST}:80'
# postgres:
#   name: '${POSTGRES_DBNAME:-rag_flow}'
#   user: '${POSTGRES_USER:-rag_flow}'
#   password: '${POSTGRES_PASSWORD:-infini_rag_flow}'
#   host: '${POSTGRES_HOST:-postgres}'
#   port: 5432
#   max_connections: 100
#   stale_timeout: 30
# s3:
#   access_key: 'access_key'
#   secret_key: 'secret_key'
#   region: 'region'
#   endpoint_url: 'endpoint_url'
#   bucket: 'bucket'
#   prefix_path: 'prefix_path'
#   signature_version: 'v4'
#   addressing_style: 'path'
# oss:
#   access_key: '${ACCESS_KEY}'
#   secret_key: '${SECRET_KEY}'
#   endpoint_url: '${ENDPOINT}'
#   region: '${REGION}'
#   bucket: '${BUCKET}'
#   prefix_path: '${OSS_PREFIX_PATH}'
# azure:
#   auth_type: 'sas'
#   container_url: 'container_url'
#   sas_token: 'sas_token'
# azure:
#   auth_type: 'spn'
#   account_url: 'account_url'
#   client_id: 'client_id'
#   secret: 'secret'
#   tenant_id: 'tenant_id'
#   container_name: 'container_name'
# The OSS object storage uses the MySQL configuration above by default. If you need to switch to another object storage service, please uncomment and configure the following parameters.
# opendal:
#   scheme: 'mysql'  # Storage type, such as s3, oss, azure, etc.
#   config:
#     oss_table: 'opendal_storage'
# user_default_llm:
#   factory: 'BAAI'
#   api_key: 'backup'
#   base_url: 'backup_base_url'
#   default_models:
#     chat_model:
#       name: 'qwen2.5-7b-instruct'
#       factory: 'xxxx'
#       api_key: 'xxxx'
#       base_url: 'https://api.xx.com'
#     embedding_model:
#       name: 'bge-m3'
#     rerank_model: 'bge-reranker-v2'
#     asr_model:
#       model: 'whisper-large-v3' # alias of name
#     image2text_model: ''
# oauth:
#   oauth2:
#     display_name: "OAuth2"
#     client_id: "your_client_id"
#     client_secret: "your_client_secret"
#     authorization_url: "https://your-oauth-provider.com/oauth/authorize"
#     token_url: "https://your-oauth-provider.com/oauth/token"
#     userinfo_url: "https://your-oauth-provider.com/oauth/userinfo"
#     redirect_uri: "https://your-app.com/v1/user/oauth/callback/oauth2"
#   oidc:
#     display_name: "OIDC"
#     client_id: "your_client_id"
#     client_secret: "your_client_secret"
#     issuer: "https://your-oauth-provider.com/oidc"
#     scope: "openid email profile"
#     redirect_uri: "https://your-app.com/v1/user/oauth/callback/oidc"
#   github:
#     type: "github"
#     icon: "github"
#     display_name: "Github"
#     client_id: "your_client_id"
#     client_secret: "your_client_secret"
#     redirect_uri: "https://your-app.com/v1/user/oauth/callback/github"
# authentication:
#   client:
#     switch: false
#     http_app_key:
#     http_secret_key:
#   site:
#     switch: false
# permission:
#   switch: false
#   component: false
#   dataset: false
# smtp:
#   mail_server: ""
#   mail_port: 465
#   mail_use_ssl: true
#   mail_use_tls: false
#   mail_username: ""
#   mail_password: ""
#   mail_default_sender:
#     - "RAGFlow" # display name
#     - "" # sender email address
#   mail_frontend_url: "https://your-frontend.example.com"
# tcadp_config:
#   secret_id: '${TENCENT_SECRET_ID}'
#   secret_key: '${TENCENT_SECRET_KEY}'
#   region: '${TENCENT_REGION}'
#   table_result_type: '1'
#   markdown_image_response_type: '1'

```

## High-Level Overview

# postgres:
#   name: '${POSTGRES_DBNAME:-rag_flow}'
#   user: '${POSTGRES_USER:-rag_flow}'
#   password: '${POSTGRES_PASSWORD:-infini_rag_flow}'
#   host: '${POSTGRES_HOST:-postgres}'
#   port: 5432
#   max_connections: 100
#   stale_timeout: 30
# s3:
#   access_key: 'access_key'
#   secret_key: 'secret_key'

## Detailed Walkthrough

This file's structure is not automatically analyzed. See the 'Original Source' section for content.

## Code Structure Analysis

- Total lines: 147
- Blank lines: 1 (0.7%)
- Comment lines: ~107 (72.8%)
- Code lines: ~39


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `docker` directory, specifically within `docker`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `docker/` directory
- Potential test file: `test_service_conf.yaml.template`

## Keywords

ACCESS_KEY, BAAI, BUCKET, ELASTIC_PASSWORD, ENDPOINT, ES_HOST, ES_USER, Github, INFINITY_HOST, MINIO_HOST, MINIO_PASSWORD, MINIO_USER, MYSQL_DBNAME, MYSQL_HOST, MYSQL_MAX_PACKET, MYSQL_PASSWORD, MYSQL_USER, MySQL, OAuth2, OIDC, OPENSEARCH_PASSWORD, OSS, OSS_PREFIX_PATH, OS_HOST, OS_USER, POSTGRES_DBNAME, POSTGRES_HOST, POSTGRES_PASSWORD, POSTGRES_USER, RAGFLOW_HOST, RAGFlow, REDIS_HOST, REDIS_PASSWORD, REGION, SECRET_KEY, Storage, TEI_HOST, TENCENT_REGION, TENCENT_SECRET_ID, TENCENT_SECRET_KEY, The

---
*Generated by RAGFlow Repository Documentation Generator*
