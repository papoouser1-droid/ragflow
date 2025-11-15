# File Documentation: CLAUDE.md

## File Metadata

- **Path**: `CLAUDE.md`
- **Extension**: `.md`
- **Lines**: 116
- **Characters**: 3,561
- **Size**: 3,561 bytes
- **Purpose**: Documentation - Markdown documentation file

## Original Source

```markdown
# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

RAGFlow is an open-source RAG (Retrieval-Augmented Generation) engine based on deep document understanding. It's a full-stack application with:
- Python backend (Flask-based API server)
- React/TypeScript frontend (built with UmiJS)
- Microservices architecture with Docker deployment
- Multiple data stores (MySQL, Elasticsearch/Infinity, Redis, MinIO)

## Architecture

### Backend (`/api/`)
- **Main Server**: `api/ragflow_server.py` - Flask application entry point
- **Apps**: Modular Flask blueprints in `api/apps/` for different functionalities:
  - `kb_app.py` - Knowledge base management
  - `dialog_app.py` - Chat/conversation handling
  - `document_app.py` - Document processing
  - `canvas_app.py` - Agent workflow canvas
  - `file_app.py` - File upload/management
- **Services**: Business logic in `api/db/services/`
- **Models**: Database models in `api/db/db_models.py`

### Core Processing (`/rag/`)
- **Document Processing**: `deepdoc/` - PDF parsing, OCR, layout analysis
- **LLM Integration**: `rag/llm/` - Model abstractions for chat, embedding, reranking
- **RAG Pipeline**: `rag/flow/` - Chunking, parsing, tokenization
- **Graph RAG**: `graphrag/` - Knowledge graph construction and querying

### Agent System (`/agent/`)
- **Components**: Modular workflow components (LLM, retrieval, categorize, etc.)
- **Templates**: Pre-built agent workflows in `agent/templates/`
- **Tools**: External API integrations (Tavily, Wikipedia, SQL execution, etc.)

### Frontend (`/web/`)
- React/TypeScript with UmiJS framework
- Ant Design + shadcn/ui components
- State management with Zustand
- Tailwind CSS for styling

## Common Development Commands

### Backend Development
```bash
# Install Python dependencies
uv sync --python 3.10 --all-extras
uv run download_deps.py
pre-commit install

# Start dependent services
docker compose -f docker/docker-compose-base.yml up -d

# Run backend (requires services to be running)
source .venv/bin/activate
export PYTHONPATH=$(pwd)
bash docker/launch_backend_service.sh

# Run tests
uv run pytest

# Linting
ruff check
ruff format
```

### Frontend Development
```bash
cd web
npm install
npm run dev        # Development server
npm run build      # Production build
npm run lint       # ESLint
npm run test       # Jest tests
```

### Docker Development
```bash
# Full stack with Docker
cd docker
docker compose -f docker-compose.yml up -d

# Check server status
docker logs -f ragflow-server

# Rebuild images
docker build --platform linux/amd64 -f Dockerfile -t infiniflow/ragflow:nightly .
```

## Key Configuration Files

- `docker/.env` - Environment variables for Docker deployment
- `docker/service_conf.yaml.template` - Backend service configuration
- `pyproject.toml` - Python dependencies and project configuration
- `web/package.json` - Frontend dependencies and scripts

## Testing

- **Python**: pytest with markers (p1/p2/p3 priority levels)
- **Frontend**: Jest with React Testing Library
- **API Tests**: HTTP API and SDK tests in `test/` and `sdk/python/test/`

## Database Engines

RAGFlow supports switching between Elasticsearch (default) and Infinity:
- Set `DOC_ENGINE=infinity` in `docker/.env` to use Infinity
- Requires container restart: `docker compose down -v && docker compose up -d`

## Development Environment Requirements

- Python 3.10-3.12
- Node.js >=18.20.4
- Docker & Docker Compose
- uv package manager
- 16GB+ RAM, 50GB+ disk space
```

## High-Level Overview

# CLAUDE.md

## Detailed Walkthrough

This is a documentation file. See the 'Original Source' section for full content.

## Code Structure Analysis

- Total lines: 116
- Blank lines: 26 (22.4%)
- Comment lines: ~23 (19.8%)
- Code lines: ~67


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `CLAUDE.md` directory.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 4 loop(s) - consider algorithmic complexity
- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `./` directory
- Potential test file: `test_CLAUDE.md`

## Keywords

API, Agent, Ant, Apps, Architecture, Augmented, Backend, Business, CLAUDE, CSS, Chat, Check, Chunking, Claude, Code, Commands, Common, Components, Compose, Configuration, Core, DOC_ENGINE, Database, Design, Development, Docker, Dockerfile, Document, Documentation, ESLint, Elasticsearch, Engines, Environment, External, File, Files, Flask, Frontend, Full, Generation, Graph, HTTP, Infinity, Install, Integration, Jest, Key, Knowledge, LLM, Library...

---
*Generated by RAGFlow Repository Documentation Generator*
