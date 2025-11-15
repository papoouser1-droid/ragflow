# File Documentation: conf/service_conf.yaml

## File Metadata

- **Path**: `conf/service_conf.yaml`
- **Extension**: `.yaml`
- **Lines**: 144
- **Characters**: 3,869
- **Size**: 3,869 bytes
- **Purpose**: Configuration - YAML configuration file

## Original Source

```yaml
ragflow:
  host: 0.0.0.0
  http_port: 9380
admin:
  host: 0.0.0.0
  http_port: 9381
mysql:
  name: 'rag_flow'
  user: 'root'
  password: 'infini_rag_flow'
  host: 'localhost'
  port: 5455
  max_connections: 900
  stale_timeout: 300
  max_allowed_packet: 1073741824
minio:
  user: 'rag_flow'
  password: 'infini_rag_flow'
  host: 'localhost:9000'
es:
  hosts: 'http://localhost:1200'
  username: 'elastic'
  password: 'infini_rag_flow'
os:
  hosts: 'http://localhost:1201'
  username: 'admin'
  password: 'infini_rag_flow_OS_01'
infinity:
  uri: 'localhost:23817'
  db_name: 'default_db'
redis:
  db: 1
  password: 'infini_rag_flow'
  host: 'localhost:6379'
task_executor:
  message_queue_type: 'redis'
user_default_llm:
  default_models:
    embedding_model:
      api_key: 'xxx'
      base_url: 'http://localhost:6380'
# postgres:
#   name: 'rag_flow'
#   user: 'rag_flow'
#   password: 'infini_rag_flow'
#   host: 'postgres'
#   port: 5432
#   max_connections: 100
#   stale_timeout: 30
# s3:
#   access_key: 'access_key'
#   secret_key: 'secret_key'
#   region: 'region'
# oss:
#   access_key: 'access_key'
#   secret_key: 'secret_key'
#   endpoint_url: 'http://oss-cn-hangzhou.aliyuncs.com'
#   region: 'cn-hangzhou'
#   bucket: 'bucket_name'
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
#       api_key: 'xxx'
#       base_url: 'http://localhost:6380'
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
#  secret_id: 'tencent_secret_id'
#  secret_key: 'tencent_secret_key'
#  region: 'tencent_region'
#  table_result_type: '1'
#  markdown_image_response_type: '1'

```

## High-Level Overview

# postgres:
#   name: 'rag_flow'
#   user: 'rag_flow'
#   password: 'infini_rag_flow'
#   host: 'postgres'
#   port: 5432
#   max_connections: 100
#   stale_timeout: 30
# s3:

## Detailed Walkthrough

This is a configuration or data file. See the 'Original Source' section for full content.

## Code Structure Analysis

- Total lines: 144
- Blank lines: 1 (0.7%)
- Comment lines: ~102 (70.8%)
- Code lines: ~41


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `conf` directory, specifically within `conf`.

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

- Other files in `conf/` directory
- Potential test file: `test_service_conf.yaml`

## Keywords

BAAI, Github, MySQL, OAuth2, OIDC, OSS, RAGFlow, Storage, The

---
*Generated by RAGFlow Repository Documentation Generator*
