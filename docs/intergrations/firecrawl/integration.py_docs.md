# Documentation: intergrations/firecrawl/integration.py

## File Metadata

- **Path**: `intergrations/firecrawl/integration.py`
- **Size**: 5090 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `intergrations/firecrawl/integration.py`.

## Python Module Overview

### Module Docstring

```
RAGFlow Integration Entry Point for Firecrawl

This file provides the main entry point for the Firecrawl integration with RAGFlow.
It follows RAGFlow's integration patterns and provides the necessary interfaces.
```

### Imports and Dependencies

This module imports the following dependencies:

- `typing`
- `logging`
- `ragflow_integration`
- `firecrawl_ui`
- `asyncio`

### Classes Defined

This file defines 1 class(es):

#### Class: `FirecrawlRAGFlowPlugin` (line 18)

**Docstring**: Main plugin class for Firecrawl integration with RAGFlow.
This class provides the interface that RAGFlow expects from integrations....

**Methods**: __init__, get_plugin_info, get_config_schema, get_ui_schema, validate_config, test_connection, create_integration, get_help_text, get_validation_rules

### Functions Defined

This file defines 15 function(s):

#### Function: `get_plugin` (line 96)

**Parameters**: None

**Docstring**: Get the plugin instance for RAGFlow....

#### Function: `get_integration` (line 101)

**Parameters**: config

**Docstring**: Get an integration instance with the given configuration....

#### Function: `get_config_schema` (line 106)

**Parameters**: None

**Docstring**: Get the configuration schema....

#### Function: `get_ui_schema` (line 111)

**Parameters**: None

**Docstring**: Get the UI schema....

#### Function: `validate_config` (line 116)

**Parameters**: config

**Docstring**: Validate configuration....

#### Function: `test_connection` (line 125)

**Parameters**: config

**Docstring**: Test connection to Firecrawl API....

#### Function: `__init__` (line 24)

**Parameters**: self

**Docstring**: Initialize the Firecrawl plugin....

#### Function: `get_plugin_info` (line 36)

**Parameters**: self

**Docstring**: Get plugin information for RAGFlow....

#### Function: `get_config_schema` (line 50)

**Parameters**: self

**Docstring**: Get configuration schema for RAGFlow....

#### Function: `get_ui_schema` (line 54)

**Parameters**: self

**Docstring**: Get UI schema for RAGFlow....

#### Function: `validate_config` (line 58)

**Parameters**: self, config

**Docstring**: Validate configuration and return any errors....

#### Function: `test_connection` (line 67)

**Parameters**: self, config

**Docstring**: Test connection to Firecrawl API....

#### Function: `create_integration` (line 82)

**Parameters**: self, config

**Docstring**: Create and return a Firecrawl integration instance....

#### Function: `get_help_text` (line 86)

**Parameters**: self

**Docstring**: Get help text for users....

#### Function: `get_validation_rules` (line 90)

**Parameters**: self

**Docstring**: Get validation rules for configuration....

## Original Source Code

```py
"""
RAGFlow Integration Entry Point for Firecrawl

This file provides the main entry point for the Firecrawl integration with RAGFlow.
It follows RAGFlow's integration patterns and provides the necessary interfaces.
"""

from typing import Dict, Any
import logging

from ragflow_integration import RAGFlowFirecrawlIntegration, create_firecrawl_integration
from firecrawl_ui import FirecrawlUIBuilder

# Set up logging
logger = logging.getLogger(__name__)


class FirecrawlRAGFlowPlugin:
    """
    Main plugin class for Firecrawl integration with RAGFlow.
    This class provides the interface that RAGFlow expects from integrations.
    """
    
    def __init__(self):
        """Initialize the Firecrawl plugin."""
        self.name = "firecrawl"
        self.display_name = "Firecrawl Web Scraper"
        self.description = "Import web content using Firecrawl's powerful scraping capabilities"
        self.version = "1.0.0"
        self.author = "Firecrawl Team"
        self.category = "web"
        self.icon = "🌐"
        
        logger.info(f"Initialized {self.display_name} plugin v{self.version}")
    
    def get_plugin_info(self) -> Dict[str, Any]:
        """Get plugin information for RAGFlow."""
        return {
            "name": self.name,
            "display_name": self.display_name,
            "description": self.description,
            "version": self.version,
            "author": self.author,
            "category": self.category,
            "icon": self.icon,
            "supported_formats": ["markdown", "html", "links", "screenshot"],
            "supported_scrape_types": ["single", "crawl", "batch"]
        }
    
    def get_config_schema(self) -> Dict[str, Any]:
        """Get configuration schema for RAGFlow."""
        return FirecrawlUIBuilder.create_data_source_config()["config_schema"]
    
    def get_ui_schema(self) -> Dict[str, Any]:
        """Get UI schema for RAGFlow."""
        return FirecrawlUIBuilder.create_ui_schema()
    
    def validate_config(self, config: Dict[str, Any]) -> Dict[str, Any]:
        """Validate configuration and return any errors."""
        try:
            integration = create_firecrawl_integration(config)
            return integration.validate_config(config)
        except Exception as e:
            logger.error(f"Configuration validation error: {e}")
            return {"general": str(e)}
    
    def test_connection(self, config: Dict[str, Any]) -> Dict[str, Any]:
        """Test connection to Firecrawl API."""
        try:
            integration = create_firecrawl_integration(config)
            # Run the async test_connection method
            import asyncio
            return asyncio.run(integration.test_connection())
        except Exception as e:
            logger.error(f"Connection test error: {e}")
            return {
                "success": False,
                "error": str(e),
                "message": "Connection test failed"
            }
    
    def create_integration(self, config: Dict[str, Any]) -> RAGFlowFirecrawlIntegration:
        """Create and return a Firecrawl integration instance."""
        return create_firecrawl_integration(config)
    
    def get_help_text(self) -> Dict[str, str]:
        """Get help text for users."""
        return FirecrawlUIBuilder.create_help_text()
    
    def get_validation_rules(self) -> Dict[str, Any]:
        """Get validation rules for configuration."""
        return FirecrawlUIBuilder.create_validation_rules()


# RAGFlow integration entry points
def get_plugin() -> FirecrawlRAGFlowPlugin:
    """Get the plugin instance for RAGFlow."""
    return FirecrawlRAGFlowPlugin()


def get_integration(config: Dict[str, Any]) -> RAGFlowFirecrawlIntegration:
    """Get an integration instance with the given configuration."""
    return create_firecrawl_integration(config)


def get_config_schema() -> Dict[str, Any]:
    """Get the configuration schema."""
    return FirecrawlUIBuilder.create_data_source_config()["config_schema"]


def get_ui_schema() -> Dict[str, Any]:
    """Get the UI schema."""
    return FirecrawlUIBuilder.create_ui_schema()


def validate_config(config: Dict[str, Any]) -> Dict[str, Any]:
    """Validate configuration."""
    try:
        integration = create_firecrawl_integration(config)
        return integration.validate_config(config)
    except Exception as e:
        return {"general": str(e)}


def test_connection(config: Dict[str, Any]) -> Dict[str, Any]:
    """Test connection to Firecrawl API."""
    try:
        integration = create_firecrawl_integration(config)
        return integration.test_connection()
    except Exception as e:
        return {
            "success": False,
            "error": str(e),
            "message": "Connection test failed"
        }


# Export main functions and classes
__all__ = [
    "FirecrawlRAGFlowPlugin",
    "get_plugin",
    "get_integration",
    "get_config_schema",
    "get_ui_schema",
    "validate_config",
    "test_connection",
    "RAGFlowFirecrawlIntegration",
    "create_firecrawl_integration"
]

```

## Detailed Analysis

### File Role in Repository

The file `intergrations/firecrawl/integration.py` is located in the `intergrations/firecrawl` directory.

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
- [ragflow_integration.py](ragflow_integration.py_docs.md)
- [requirements.txt](requirements.txt_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
