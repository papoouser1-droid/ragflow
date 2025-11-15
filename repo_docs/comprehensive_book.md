# RAGFlow: The Comprehensive Guide

## Table of Contents

- [Part I: Project Overview](#part-i-project-overview)
- [Part II: Architecture](#part-ii-architecture)
- [Part III: Core Components](#part-iii-core-components)
- [Part IV: Development Guide](#part-iv-development-guide)
- [Part V: Reference](#part-v-reference)

---

# Part I: Project Overview

## Introduction

RAGFlow is an open-source RAG (Retrieval-Augmented Generation) engine based on deep
document understanding. It combines cutting-edge document processing, knowledge graph
construction, and large language model integration to provide powerful question-answering
and document understanding capabilities.

## Mission and Vision

RAGFlow aims to make document understanding and retrieval-augmented generation accessible
to developers and organizations of all sizes. By providing a complete, production-ready
stack with both API and UI components, RAGFlow enables:

- Deep document understanding through advanced parsing and layout analysis
- Accurate information retrieval using hybrid search approaches
- Flexible agent-based workflows for complex tasks
- Knowledge graph construction for relationship discovery
- Multi-modal document processing (text, tables, images)

## Repository Statistics

- Files: 2,233
- Directories: 466
- Lines of Code: ~223300+ (estimated)

---

# Part II: Architecture

## System Architecture

RAGFlow follows a microservices architecture with the following major components:

### Backend Services

1. **API Server** (`/api`)
   - Flask-based REST API
   - Authentication and authorization
   - Request routing and validation
   - Business logic coordination

2. **RAG Engine** (`/rag`)
   - Document parsing and chunking
   - Embedding generation
   - Vector search
   - Reranking

3. **Agent System** (`/agent`)
   - Workflow orchestration
   - Component-based architecture
   - Tool integration
   - Template management

4. **Graph RAG** (`/graphrag`)
   - Knowledge graph construction
   - Graph-based retrieval
   - Entity extraction
   - Relationship mining

5. **Document Processing** (`/deepdoc`)
   - PDF parsing
   - Layout analysis
   - OCR integration
   - Table extraction

### Frontend

- React/TypeScript application (`/web`)
- Real-time chat interface
- Knowledge base management
- Agent canvas for workflow design

### Infrastructure

- Docker containers for easy deployment
- Elasticsearch/Infinity for vector search
- MySQL for relational data
- Redis for caching
- MinIO for object storage

## Data Flow

1. **Document Ingestion**:
   - Upload → Parse → Chunk → Embed → Index

2. **Query Processing**:
   - Question → Retrieve → Rerank → Generate → Respond

3. **Agent Execution**:
   - Trigger → Plan → Execute → Monitor → Complete

---

# Part III: Core Components


## API Component

This folder contains API-related code, including endpoints, request handlers,
and business logic for the RAGFlow REST API.

For detailed documentation, see:
- [Folder Index](./api/index.md)
- [Folder Documentation](./api/doc.md)
- [Keyword Index](./api/sub.md)


## RAG Component

This folder contains core RAG (Retrieval-Augmented Generation) functionality.

For detailed documentation, see:
- [Folder Index](./rag/index.md)
- [Folder Documentation](./rag/doc.md)
- [Keyword Index](./rag/sub.md)


## AGENT Component

This folder contains agent-related code for the RAGFlow agent system.

For detailed documentation, see:
- [Folder Index](./agent/index.md)
- [Folder Documentation](./agent/doc.md)
- [Keyword Index](./agent/sub.md)


## GRAPHRAG Component

This folder contains graph-based RAG implementations.

For detailed documentation, see:
- [Folder Index](./graphrag/index.md)
- [Folder Documentation](./graphrag/doc.md)
- [Keyword Index](./graphrag/sub.md)


## DEEPDOC Component

This folder contains document processing and parsing functionality.

For detailed documentation, see:
- [Folder Index](./deepdoc/index.md)
- [Folder Documentation](./deepdoc/doc.md)
- [Keyword Index](./deepdoc/sub.md)


## WEB Component

This folder contains frontend code, including UI components, styles, and client-side logic.

For detailed documentation, see:
- [Folder Index](./web/index.md)
- [Folder Documentation](./web/doc.md)
- [Keyword Index](./web/sub.md)


---

# Part IV: Development Guide

## Getting Started

### Prerequisites

- Python 3.10-3.12
- Node.js ≥18.20.4
- Docker & Docker Compose
- 16GB+ RAM, 50GB+ disk space

### Local Development

1. Clone the repository
2. Install dependencies: `uv sync --all-extras`
3. Start services: `docker compose -f docker/docker-compose-base.yml up -d`
4. Run backend: `bash docker/launch_backend_service.sh`
5. Run frontend: `cd web && npm install && npm run dev`

## Testing

- Python tests: `uv run pytest`
- Frontend tests: `cd web && npm test`
- API tests: See `/test` directory

## Contributing

See [CONTRIBUTING.md](../CONTRIBUTING.md) for guidelines on:
- Code style and formatting
- Pull request process
- Testing requirements
- Documentation standards

---

# Part V: Reference

## File Index

For a complete index of all files in the repository, see the folder indexes:

- [admin/](./admin/index.md)
- [agent/](./agent/index.md)
- [agentic_reasoning/](./agentic_reasoning/index.md)
- [api/](./api/index.md)
- [chat_demo/](./chat_demo/index.md)
- [common/](./common/index.md)
- [conf/](./conf/index.md)
- [deepdoc/](./deepdoc/index.md)
- [docker/](./docker/index.md)
- [docs/](./docs/index.md)
- [example/](./example/index.md)
- [graphrag/](./graphrag/index.md)
- [helm/](./helm/index.md)
- [intergrations/](./intergrations/index.md)
- [mcp/](./mcp/index.md)
- [plugin/](./plugin/index.md)
- [rag/](./rag/index.md)
- [sandbox/](./sandbox/index.md)
- [sdk/](./sdk/index.md)
- [test/](./test/index.md)
- [web/](./web/index.md)

## Keyword Index

See the [Global Keyword Index](./keywords.md) for a complete listing of all technical
terms, classes, functions, and concepts in the codebase.

## Additional Resources

- Project README: [../README.md](../README.md)
- API Reference: See [docs/references/](../docs/references/)
- Configuration Guide: [../docs/configurations.md](../docs/configurations.md)

---

# Conclusion

This comprehensive book provides a complete overview of the RAGFlow repository.
For detailed information about specific files or components, navigate through the
folder indexes and individual file documentation.

The documentation system is organized hierarchically:

1. **This book** - High-level overview and guide
2. **Folder indexes** - Directory structure and navigation
3. **File documentation** - Detailed analysis of each file
4. **Keyword indexes** - Topic-based navigation

Use the navigation structure that best fits your needs:
- Browse by folder structure for architectural understanding
- Search by keyword for finding specific concepts
- Read file documentation for implementation details

---

*Generated by RAGFlow Repository Documentation Generator*

**Total Documentation Pages**: ~{self.stats['docs_created']:,}
**Coverage**: Complete repository documentation
**Last Generated**: {self.get_timestamp()}
