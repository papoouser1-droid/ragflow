# Documentation: intergrations/firecrawl/example_usage.py

## File Metadata

- **Path**: `intergrations/firecrawl/example_usage.py`
- **Size**: 7722 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `intergrations/firecrawl/example_usage.py`.

## Python Module Overview

### Module Docstring

```
Example usage of the Firecrawl integration with RAGFlow.
```

### Imports and Dependencies

This module imports the following dependencies:

- `asyncio`
- `logging`
- `ragflow_integration`
- `firecrawl_config`

## Original Source Code

```py
"""
Example usage of the Firecrawl integration with RAGFlow.
"""

import asyncio
import logging

from .ragflow_integration import RAGFlowFirecrawlIntegration, create_firecrawl_integration
from .firecrawl_config import FirecrawlConfig


async def example_single_url_scraping():
    """Example of scraping a single URL."""
    print("=== Single URL Scraping Example ===")
    
    # Configuration
    config = {
        "api_key": "fc-your-api-key-here",  # Replace with your actual API key
        "api_url": "https://api.firecrawl.dev",
        "max_retries": 3,
        "timeout": 30,
        "rate_limit_delay": 1.0
    }
    
    # Create integration
    integration = create_firecrawl_integration(config)
    
    # Test connection
    connection_test = await integration.test_connection()
    print(f"Connection test: {connection_test}")
    
    if not connection_test["success"]:
        print("Connection failed, please check your API key")
        return
    
    # Scrape a single URL
    urls = ["https://httpbin.org/json"]
    documents = await integration.scrape_and_import(urls)
    
    for doc in documents:
        print(f"Title: {doc.title}")
        print(f"URL: {doc.source_url}")
        print(f"Content length: {len(doc.content)}")
        print(f"Language: {doc.language}")
        print(f"Metadata: {doc.metadata}")
        print("-" * 50)


async def example_website_crawling():
    """Example of crawling an entire website."""
    print("=== Website Crawling Example ===")
    
    # Configuration
    config = {
        "api_key": "fc-your-api-key-here",  # Replace with your actual API key
        "api_url": "https://api.firecrawl.dev",
        "max_retries": 3,
        "timeout": 30,
        "rate_limit_delay": 1.0
    }
    
    # Create integration
    integration = create_firecrawl_integration(config)
    
    # Crawl a website
    start_url = "https://httpbin.org"
    documents = await integration.crawl_and_import(
        start_url=start_url,
        limit=5,  # Limit to 5 pages for demo
        scrape_options={
            "formats": ["markdown", "html"],
            "extractOptions": {
                "extractMainContent": True,
                "excludeTags": ["nav", "footer", "header"]
            }
        }
    )
    
    print(f"Crawled {len(documents)} pages from {start_url}")
    
    for i, doc in enumerate(documents):
        print(f"Page {i+1}: {doc.title}")
        print(f"URL: {doc.source_url}")
        print(f"Content length: {len(doc.content)}")
        print("-" * 30)


async def example_batch_processing():
    """Example of batch processing multiple URLs."""
    print("=== Batch Processing Example ===")
    
    # Configuration
    config = {
        "api_key": "fc-your-api-key-here",  # Replace with your actual API key
        "api_url": "https://api.firecrawl.dev",
        "max_retries": 3,
        "timeout": 30,
        "rate_limit_delay": 1.0
    }
    
    # Create integration
    integration = create_firecrawl_integration(config)
    
    # Batch scrape multiple URLs
    urls = [
        "https://httpbin.org/json",
        "https://httpbin.org/html",
        "https://httpbin.org/xml"
    ]
    
    documents = await integration.scrape_and_import(
        urls=urls,
        formats=["markdown", "html"],
        extract_options={
            "extractMainContent": True,
            "excludeTags": ["nav", "footer", "header"]
        }
    )
    
    print(f"Processed {len(documents)} URLs")
    
    for doc in documents:
        print(f"Title: {doc.title}")
        print(f"URL: {doc.source_url}")
        print(f"Content length: {len(doc.content)}")
        
        # Example of chunking for RAG processing
        chunks = integration.processor.chunk_content(doc, chunk_size=500, chunk_overlap=100)
        print(f"Number of chunks: {len(chunks)}")
        print("-" * 30)


async def example_content_processing():
    """Example of content processing and chunking."""
    print("=== Content Processing Example ===")
    
    # Configuration
    config = {
        "api_key": "fc-your-api-key-here",  # Replace with your actual API key
        "api_url": "https://api.firecrawl.dev",
        "max_retries": 3,
        "timeout": 30,
        "rate_limit_delay": 1.0
    }
    
    # Create integration
    integration = create_firecrawl_integration(config)
    
    # Scrape content
    urls = ["https://httpbin.org/html"]
    documents = await integration.scrape_and_import(urls)
    
    for doc in documents:
        print(f"Original document: {doc.title}")
        print(f"Content length: {len(doc.content)}")
        
        # Chunk the content
        chunks = integration.processor.chunk_content(
            doc, 
            chunk_size=1000, 
            chunk_overlap=200
        )
        
        print(f"Number of chunks: {len(chunks)}")
        
        for i, chunk in enumerate(chunks):
            print(f"Chunk {i+1}:")
            print(f"  ID: {chunk['id']}")
            print(f"  Content length: {len(chunk['content'])}")
            print(f"  Metadata: {chunk['metadata']}")
            print()


async def example_error_handling():
    """Example of error handling."""
    print("=== Error Handling Example ===")
    
    # Configuration with invalid API key
    config = {
        "api_key": "invalid-key",
        "api_url": "https://api.firecrawl.dev",
        "max_retries": 3,
        "timeout": 30,
        "rate_limit_delay": 1.0
    }
    
    # Create integration
    integration = create_firecrawl_integration(config)
    
    # Test connection (should fail)
    connection_test = await integration.test_connection()
    print(f"Connection test with invalid key: {connection_test}")
    
    # Try to scrape (should fail gracefully)
    try:
        urls = ["https://httpbin.org/json"]
        documents = await integration.scrape_and_import(urls)
        print(f"Documents scraped: {len(documents)}")
    except Exception as e:
        print(f"Error occurred: {e}")


async def example_configuration_validation():
    """Example of configuration validation."""
    print("=== Configuration Validation Example ===")
    
    # Test various configurations
    test_configs = [
        {
            "api_key": "fc-valid-key",
            "api_url": "https://api.firecrawl.dev",
            "max_retries": 3,
            "timeout": 30,
            "rate_limit_delay": 1.0
        },
        {
            "api_key": "invalid-key",  # Invalid format
            "api_url": "https://api.firecrawl.dev"
        },
        {
            "api_key": "fc-valid-key",
            "api_url": "invalid-url",  # Invalid URL
            "max_retries": 15,  # Too high
            "timeout": 500,  # Too high
            "rate_limit_delay": 15.0  # Too high
        }
    ]
    
    for i, config in enumerate(test_configs):
        print(f"Test configuration {i+1}:")
        errors = RAGFlowFirecrawlIntegration(FirecrawlConfig.from_dict(config)).validate_config(config)
        
        if errors:
            print("  Errors found:")
            for field, error in errors.items():
                print(f"    {field}: {error}")
        else:
            print("  Configuration is valid")
        print()


async def main():
    """Run all examples."""
    # Set up logging
    logging.basicConfig(level=logging.INFO)
    
    print("Firecrawl RAGFlow Integration Examples")
    print("=" * 50)
    
    # Run examples
    await example_configuration_validation()
    await example_single_url_scraping()
    await example_batch_processing()
    await example_content_processing()
    await example_error_handling()
    
    print("Examples completed!")


if __name__ == "__main__":
    asyncio.run(main())

```

## Detailed Analysis

### File Role in Repository

The file `intergrations/firecrawl/example_usage.py` is located in the `intergrations/firecrawl` directory.

### Architecture Context

Files in this location typically handle concerns related to firecrawl.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

- Ensure all user inputs are validated
- Check for SQL injection vulnerabilities
- Verify authentication and authorization

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [INSTALLATION.md](INSTALLATION.md_docs.md)
- [README.md](README.md_docs.md)
- [__init__.py](__init__.py_docs.md)
- [firecrawl_config.py](firecrawl_config.py_docs.md)
- [firecrawl_connector.py](firecrawl_connector.py_docs.md)
- [firecrawl_processor.py](firecrawl_processor.py_docs.md)
- [firecrawl_ui.py](firecrawl_ui.py_docs.md)
- [integration.py](integration.py_docs.md)
- [ragflow_integration.py](ragflow_integration.py_docs.md)
- [requirements.txt](requirements.txt_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
