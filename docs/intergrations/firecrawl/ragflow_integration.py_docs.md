# Documentation: intergrations/firecrawl/ragflow_integration.py

## File Metadata

- **Path**: `intergrations/firecrawl/ragflow_integration.py`
- **Size**: 6946 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `intergrations/firecrawl/ragflow_integration.py`.

## Python Module Overview

### Module Docstring

```
Main integration file for Firecrawl with RAGFlow.
This file provides the interface between RAGFlow and the Firecrawl plugin.
```

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `typing`
- `firecrawl_connector`
- `firecrawl_config`
- `firecrawl_processor`
- `firecrawl_ui`

### Classes Defined

This file defines 1 class(es):

#### Class: `RAGFlowFirecrawlIntegration` (line 15)

**Docstring**: Main integration class for Firecrawl with RAGFlow....

**Methods**: __init__, get_ui_schema, validate_config, create_config, get_supported_formats, get_supported_scrape_types, get_help_text, get_validation_rules

### Functions Defined

This file defines 9 function(s):

#### Function: `create_firecrawl_integration` (line 161)

**Parameters**: config_dict

**Docstring**: Create a Firecrawl integration instance from configuration....

#### Function: `__init__` (line 18)

**Parameters**: self, config

**Docstring**: Initialize the integration....

#### Function: `get_ui_schema` (line 66)

**Parameters**: self

**Docstring**: Get UI schema for RAGFlow integration....

#### Function: `validate_config` (line 70)

**Parameters**: self, config_dict

**Docstring**: Validate configuration and return any errors....

#### Function: `create_config` (line 110)

**Parameters**: self, config_dict

**Docstring**: Create FirecrawlConfig from dictionary....

#### Function: `get_supported_formats` (line 143)

**Parameters**: self

**Docstring**: Get list of supported output formats....

#### Function: `get_supported_scrape_types` (line 147)

**Parameters**: self

**Docstring**: Get list of supported scrape types....

#### Function: `get_help_text` (line 151)

**Parameters**: self

**Docstring**: Get help text for users....

#### Function: `get_validation_rules` (line 155)

**Parameters**: self

**Docstring**: Get validation rules for configuration....

## Original Source Code

```py
"""
Main integration file for Firecrawl with RAGFlow.
This file provides the interface between RAGFlow and the Firecrawl plugin.
"""

import logging
from typing import List, Dict, Any

from firecrawl_connector import FirecrawlConnector
from firecrawl_config import FirecrawlConfig
from firecrawl_processor import FirecrawlProcessor, RAGFlowDocument
from firecrawl_ui import FirecrawlUIBuilder


class RAGFlowFirecrawlIntegration:
    """Main integration class for Firecrawl with RAGFlow."""
    
    def __init__(self, config: FirecrawlConfig):
        """Initialize the integration."""
        self.config = config
        self.connector = FirecrawlConnector(config)
        self.processor = FirecrawlProcessor()
        self.logger = logging.getLogger(__name__)
    
    async def scrape_and_import(self, urls: List[str], 
                               formats: List[str] = None,
                               extract_options: Dict[str, Any] = None) -> List[RAGFlowDocument]:
        """Scrape URLs and convert to RAGFlow documents."""
        if formats is None:
            formats = ["markdown", "html"]
        
        async with self.connector:
            # Scrape URLs
            scraped_contents = await self.connector.batch_scrape(urls, formats)
            
            # Process into RAGFlow documents
            documents = self.processor.process_batch(scraped_contents)
            
            return documents
    
    async def crawl_and_import(self, start_url: str, 
                              limit: int = 100,
                              scrape_options: Dict[str, Any] = None) -> List[RAGFlowDocument]:
        """Crawl a website and convert to RAGFlow documents."""
        if scrape_options is None:
            scrape_options = {"formats": ["markdown", "html"]}
        
        async with self.connector:
            # Start crawl job
            crawl_job = await self.connector.start_crawl(start_url, limit, scrape_options)
            
            if crawl_job.error:
                raise Exception(f"Failed to start crawl: {crawl_job.error}")
            
            # Wait for completion
            completed_job = await self.connector.wait_for_crawl_completion(crawl_job.job_id)
            
            if completed_job.error:
                raise Exception(f"Crawl failed: {completed_job.error}")
            
            # Process into RAGFlow documents
            documents = self.processor.process_batch(completed_job.data or [])
            
            return documents
    
    def get_ui_schema(self) -> Dict[str, Any]:
        """Get UI schema for RAGFlow integration."""
        return FirecrawlUIBuilder.create_ui_schema()
    
    def validate_config(self, config_dict: Dict[str, Any]) -> Dict[str, Any]:
        """Validate configuration and return any errors."""
        errors = {}
        
        # Validate API key
        api_key = config_dict.get("api_key", "")
        if not api_key:
            errors["api_key"] = "API key is required"
        elif not api_key.startswith("fc-"):
            errors["api_key"] = "API key must start with 'fc-'"
        
        # Validate API URL
        api_url = config_dict.get("api_url", "https://api.firecrawl.dev")
        if not api_url.startswith("http"):
            errors["api_url"] = "API URL must start with http:// or https://"
        
        # Validate numeric fields
        try:
            max_retries = int(config_dict.get("max_retries", 3))
            if max_retries < 1 or max_retries > 10:
                errors["max_retries"] = "Max retries must be between 1 and 10"
        except (ValueError, TypeError):
            errors["max_retries"] = "Max retries must be a valid integer"
        
        try:
            timeout = int(config_dict.get("timeout", 30))
            if timeout < 5 or timeout > 300:
                errors["timeout"] = "Timeout must be between 5 and 300 seconds"
        except (ValueError, TypeError):
            errors["timeout"] = "Timeout must be a valid integer"
        
        try:
            rate_limit_delay = float(config_dict.get("rate_limit_delay", 1.0))
            if rate_limit_delay < 0.1 or rate_limit_delay > 10.0:
                errors["rate_limit_delay"] = "Rate limit delay must be between 0.1 and 10.0 seconds"
        except (ValueError, TypeError):
            errors["rate_limit_delay"] = "Rate limit delay must be a valid number"
        
        return errors
    
    def create_config(self, config_dict: Dict[str, Any]) -> FirecrawlConfig:
        """Create FirecrawlConfig from dictionary."""
        return FirecrawlConfig.from_dict(config_dict)
    
    async def test_connection(self) -> Dict[str, Any]:
        """Test the connection to Firecrawl API."""
        try:
            async with self.connector:
                # Try to scrape a simple URL to test connection
                test_url = "https://httpbin.org/json"
                result = await self.connector.scrape_url(test_url, ["markdown"])
                
                if result.error:
                    return {
                        "success": False,
                        "error": result.error,
                        "message": "Failed to connect to Firecrawl API"
                    }
                
                return {
                    "success": True,
                    "message": "Successfully connected to Firecrawl API",
                    "test_url": test_url,
                    "response_time": "N/A"  # Could be enhanced to measure actual response time
                }
                
        except Exception as e:
            return {
                "success": False,
                "error": str(e),
                "message": "Connection test failed"
            }
    
    def get_supported_formats(self) -> List[str]:
        """Get list of supported output formats."""
        return ["markdown", "html", "links", "screenshot"]
    
    def get_supported_scrape_types(self) -> List[str]:
        """Get list of supported scrape types."""
        return ["single", "crawl", "batch"]
    
    def get_help_text(self) -> Dict[str, str]:
        """Get help text for users."""
        return FirecrawlUIBuilder.create_help_text()
    
    def get_validation_rules(self) -> Dict[str, Any]:
        """Get validation rules for configuration."""
        return FirecrawlUIBuilder.create_validation_rules()


# Factory function for creating integration instance
def create_firecrawl_integration(config_dict: Dict[str, Any]) -> RAGFlowFirecrawlIntegration:
    """Create a Firecrawl integration instance from configuration."""
    config = FirecrawlConfig.from_dict(config_dict)
    return RAGFlowFirecrawlIntegration(config)


# Export main classes and functions
__all__ = [
    "RAGFlowFirecrawlIntegration",
    "create_firecrawl_integration",
    "FirecrawlConfig",
    "FirecrawlConnector",
    "FirecrawlProcessor",
    "RAGFlowDocument"
]

```

## Detailed Analysis

### File Role in Repository

The file `intergrations/firecrawl/ragflow_integration.py` is located in the `intergrations/firecrawl` directory.

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
- [example_usage.py](example_usage.py_docs.md)
- [firecrawl_config.py](firecrawl_config.py_docs.md)
- [firecrawl_connector.py](firecrawl_connector.py_docs.md)
- [firecrawl_processor.py](firecrawl_processor.py_docs.md)
- [firecrawl_ui.py](firecrawl_ui.py_docs.md)
- [integration.py](integration.py_docs.md)
- [requirements.txt](requirements.txt_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
