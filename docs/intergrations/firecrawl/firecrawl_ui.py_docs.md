# Documentation: intergrations/firecrawl/firecrawl_ui.py

## File Metadata

- **Path**: `intergrations/firecrawl/firecrawl_ui.py`
- **Size**: 10143 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `intergrations/firecrawl/firecrawl_ui.py`.

## Python Module Overview

### Module Docstring

```
UI components for Firecrawl integration in RAGFlow.
```

### Imports and Dependencies

This module imports the following dependencies:

- `typing`
- `dataclasses`

### Classes Defined

This file defines 2 class(es):

#### Class: `FirecrawlUIComponent` (line 10)

**Docstring**: Represents a UI component for Firecrawl integration....

#### Class: `FirecrawlUIBuilder` (line 18)

**Docstring**: Builder for Firecrawl UI components in RAGFlow....

**Methods**: create_data_source_config, create_scraping_form, create_progress_component, create_results_view, create_error_handler, create_validation_rules, create_help_text, create_ui_schema

### Functions Defined

This file defines 8 function(s):

#### Function: `create_data_source_config` (line 22)

**Parameters**: None

**Docstring**: Create configuration for Firecrawl data source....

#### Function: `create_scraping_form` (line 79)

**Parameters**: None

**Docstring**: Create form for scraping configuration....

#### Function: `create_progress_component` (line 164)

**Parameters**: None

**Docstring**: Create progress tracking component....

#### Function: `create_results_view` (line 178)

**Parameters**: None

**Docstring**: Create results display component....

#### Function: `create_error_handler` (line 193)

**Parameters**: None

**Docstring**: Create error handling component....

#### Function: `create_validation_rules` (line 208)

**Parameters**: None

**Docstring**: Create validation rules for Firecrawl integration....

#### Function: `create_help_text` (line 227)

**Parameters**: None

**Docstring**: Create help text for users....

#### Function: `create_ui_schema` (line 238)

**Parameters**: None

**Docstring**: Create complete UI schema for Firecrawl integration....

## Original Source Code

```py
"""
UI components for Firecrawl integration in RAGFlow.
"""

from typing import Dict, Any, List, Optional
from dataclasses import dataclass


@dataclass
class FirecrawlUIComponent:
    """Represents a UI component for Firecrawl integration."""
    
    component_type: str
    props: Dict[str, Any]
    children: Optional[List['FirecrawlUIComponent']] = None


class FirecrawlUIBuilder:
    """Builder for Firecrawl UI components in RAGFlow."""
    
    @staticmethod
    def create_data_source_config() -> Dict[str, Any]:
        """Create configuration for Firecrawl data source."""
        return {
            "name": "firecrawl",
            "display_name": "Firecrawl Web Scraper",
            "description": "Import web content using Firecrawl's powerful scraping capabilities",
            "icon": "🌐",
            "category": "web",
            "version": "1.0.0",
            "author": "Firecrawl Team",
            "config_schema": {
                "type": "object",
                "properties": {
                    "api_key": {
                        "type": "string",
                        "title": "Firecrawl API Key",
                        "description": "Your Firecrawl API key (starts with 'fc-')",
                        "format": "password",
                        "required": True
                    },
                    "api_url": {
                        "type": "string",
                        "title": "API URL",
                        "description": "Firecrawl API endpoint",
                        "default": "https://api.firecrawl.dev",
                        "required": False
                    },
                    "max_retries": {
                        "type": "integer",
                        "title": "Max Retries",
                        "description": "Maximum number of retry attempts",
                        "default": 3,
                        "minimum": 1,
                        "maximum": 10
                    },
                    "timeout": {
                        "type": "integer",
                        "title": "Timeout (seconds)",
                        "description": "Request timeout in seconds",
                        "default": 30,
                        "minimum": 5,
                        "maximum": 300
                    },
                    "rate_limit_delay": {
                        "type": "number",
                        "title": "Rate Limit Delay",
                        "description": "Delay between requests in seconds",
                        "default": 1.0,
                        "minimum": 0.1,
                        "maximum": 10.0
                    }
                },
                "required": ["api_key"]
            }
        }
    
    @staticmethod
    def create_scraping_form() -> Dict[str, Any]:
        """Create form for scraping configuration."""
        return {
            "type": "form",
            "title": "Firecrawl Web Scraping",
            "description": "Configure web scraping parameters",
            "fields": [
                {
                    "name": "urls",
                    "type": "array",
                    "title": "URLs to Scrape",
                    "description": "Enter URLs to scrape (one per line)",
                    "items": {
                        "type": "string",
                        "format": "uri"
                    },
                    "required": True,
                    "minItems": 1
                },
                {
                    "name": "scrape_type",
                    "type": "string",
                    "title": "Scrape Type",
                    "description": "Choose scraping method",
                    "enum": ["single", "crawl", "batch"],
                    "enumNames": ["Single URL", "Crawl Website", "Batch URLs"],
                    "default": "single",
                    "required": True
                },
                {
                    "name": "formats",
                    "type": "array",
                    "title": "Output Formats",
                    "description": "Select output formats",
                    "items": {
                        "type": "string",
                        "enum": ["markdown", "html", "links", "screenshot"]
                    },
                    "default": ["markdown", "html"],
                    "required": True
                },
                {
                    "name": "crawl_limit",
                    "type": "integer",
                    "title": "Crawl Limit",
                    "description": "Maximum number of pages to crawl (for crawl type)",
                    "default": 100,
                    "minimum": 1,
                    "maximum": 1000,
                    "condition": {
                        "field": "scrape_type",
                        "equals": "crawl"
                    }
                },
                {
                    "name": "extract_options",
                    "type": "object",
                    "title": "Extraction Options",
                    "description": "Advanced extraction settings",
                    "properties": {
                        "extractMainContent": {
                            "type": "boolean",
                            "title": "Extract Main Content Only",
                            "default": True
                        },
                        "excludeTags": {
                            "type": "array",
                            "title": "Exclude Tags",
                            "description": "HTML tags to exclude",
                            "items": {"type": "string"},
                            "default": ["nav", "footer", "header", "aside"]
                        },
                        "includeTags": {
                            "type": "array",
                            "title": "Include Tags",
                            "description": "HTML tags to include",
                            "items": {"type": "string"},
                            "default": ["main", "article", "section", "div", "p"]
                        }
                    }
                }
            ]
        }
    
    @staticmethod
    def create_progress_component() -> Dict[str, Any]:
        """Create progress tracking component."""
        return {
            "type": "progress",
            "title": "Scraping Progress",
            "description": "Track the progress of your web scraping job",
            "properties": {
                "show_percentage": True,
                "show_eta": True,
                "show_details": True
            }
        }
    
    @staticmethod
    def create_results_view() -> Dict[str, Any]:
        """Create results display component."""
        return {
            "type": "results",
            "title": "Scraping Results",
            "description": "View and manage scraped content",
            "properties": {
                "show_preview": True,
                "show_metadata": True,
                "allow_editing": True,
                "show_chunks": True
            }
        }
    
    @staticmethod
    def create_error_handler() -> Dict[str, Any]:
        """Create error handling component."""
        return {
            "type": "error_handler",
            "title": "Error Handling",
            "description": "Handle scraping errors and retries",
            "properties": {
                "show_retry_button": True,
                "show_error_details": True,
                "auto_retry": False,
                "max_retries": 3
            }
        }
    
    @staticmethod
    def create_validation_rules() -> Dict[str, Any]:
        """Create validation rules for Firecrawl integration."""
        return {
            "url_validation": {
                "pattern": r"^https?://.+",
                "message": "URL must start with http:// or https://"
            },
            "api_key_validation": {
                "pattern": r"^fc-[a-zA-Z0-9]+$",
                "message": "API key must start with 'fc-' followed by alphanumeric characters"
            },
            "rate_limit_validation": {
                "min": 0.1,
                "max": 10.0,
                "message": "Rate limit delay must be between 0.1 and 10.0 seconds"
            }
        }
    
    @staticmethod
    def create_help_text() -> Dict[str, str]:
        """Create help text for users."""
        return {
            "api_key_help": "Get your API key from https://firecrawl.dev. Sign up for a free account to get started.",
            "url_help": "Enter the URLs you want to scrape. You can add multiple URLs for batch processing.",
            "crawl_help": "Crawling will follow links from the starting URL and scrape all accessible pages within the limit.",
            "formats_help": "Choose the output formats you need. Markdown is recommended for RAG processing.",
            "extract_help": "Extraction options help filter content to get only the main content without navigation and ads."
        }
    
    @staticmethod
    def create_ui_schema() -> Dict[str, Any]:
        """Create complete UI schema for Firecrawl integration."""
        return {
            "version": "1.0.0",
            "components": {
                "data_source_config": FirecrawlUIBuilder.create_data_source_config(),
                "scraping_form": FirecrawlUIBuilder.create_scraping_form(),
                "progress_component": FirecrawlUIBuilder.create_progress_component(),
                "results_view": FirecrawlUIBuilder.create_results_view(),
                "error_handler": FirecrawlUIBuilder.create_error_handler()
            },
            "validation_rules": FirecrawlUIBuilder.create_validation_rules(),
            "help_text": FirecrawlUIBuilder.create_help_text(),
            "workflow": [
                "configure_data_source",
                "setup_scraping_parameters",
                "start_scraping_job",
                "monitor_progress",
                "review_results",
                "import_to_ragflow"
            ]
        }

```

## Detailed Analysis

### File Role in Repository

The file `intergrations/firecrawl/firecrawl_ui.py` is located in the `intergrations/firecrawl` directory.

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
- [integration.py](integration.py_docs.md)
- [ragflow_integration.py](ragflow_integration.py_docs.md)
- [requirements.txt](requirements.txt_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
