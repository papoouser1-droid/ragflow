# Documentation: intergrations/firecrawl/firecrawl_config.py

## File Metadata

- **Path**: `intergrations/firecrawl/firecrawl_config.py`
- **Size**: 2879 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `intergrations/firecrawl/firecrawl_config.py`.

## Python Module Overview

### Module Docstring

```
Configuration management for Firecrawl integration with RAGFlow.
```

### Imports and Dependencies

This module imports the following dependencies:

- `os`
- `typing`
- `dataclasses`
- `json`

### Classes Defined

This file defines 1 class(es):

#### Class: `FirecrawlConfig` (line 12)

**Docstring**: Configuration class for Firecrawl integration....

**Methods**: __post_init__, from_env, from_dict, to_dict, to_json, from_json

### Functions Defined

This file defines 6 function(s):

#### Function: `__post_init__` (line 22)

**Parameters**: self

**Docstring**: Validate configuration after initialization....

#### Function: `from_env` (line 40)

**Parameters**: cls

**Docstring**: Create configuration from environment variables....

#### Function: `from_dict` (line 56)

**Parameters**: cls, config_dict

**Docstring**: Create configuration from dictionary....

#### Function: `to_dict` (line 60)

**Parameters**: self

**Docstring**: Convert configuration to dictionary....

#### Function: `to_json` (line 71)

**Parameters**: self

**Docstring**: Convert configuration to JSON string....

#### Function: `from_json` (line 76)

**Parameters**: cls, json_str

**Docstring**: Create configuration from JSON string....

## Original Source Code

```py
"""
Configuration management for Firecrawl integration with RAGFlow.
"""

import os
from typing import Dict, Any
from dataclasses import dataclass
import json


@dataclass
class FirecrawlConfig:
    """Configuration class for Firecrawl integration."""
    
    api_key: str
    api_url: str = "https://api.firecrawl.dev"
    max_retries: int = 3
    timeout: int = 30
    rate_limit_delay: float = 1.0
    max_concurrent_requests: int = 5
    
    def __post_init__(self):
        """Validate configuration after initialization."""
        if not self.api_key:
            raise ValueError("Firecrawl API key is required")
        
        if not self.api_key.startswith("fc-"):
            raise ValueError("Invalid Firecrawl API key format. Must start with 'fc-'")
        
        if self.max_retries < 1 or self.max_retries > 10:
            raise ValueError("Max retries must be between 1 and 10")
        
        if self.timeout < 5 or self.timeout > 300:
            raise ValueError("Timeout must be between 5 and 300 seconds")
        
        if self.rate_limit_delay < 0.1 or self.rate_limit_delay > 10.0:
            raise ValueError("Rate limit delay must be between 0.1 and 10.0 seconds")
    
    @classmethod
    def from_env(cls) -> "FirecrawlConfig":
        """Create configuration from environment variables."""
        api_key = os.getenv("FIRECRAWL_API_KEY")
        if not api_key:
            raise ValueError("FIRECRAWL_API_KEY environment variable not set")
        
        return cls(
            api_key=api_key,
            api_url=os.getenv("FIRECRAWL_API_URL", "https://api.firecrawl.dev"),
            max_retries=int(os.getenv("FIRECRAWL_MAX_RETRIES", "3")),
            timeout=int(os.getenv("FIRECRAWL_TIMEOUT", "30")),
            rate_limit_delay=float(os.getenv("FIRECRAWL_RATE_LIMIT_DELAY", "1.0")),
            max_concurrent_requests=int(os.getenv("FIRECRAWL_MAX_CONCURRENT", "5"))
        )
    
    @classmethod
    def from_dict(cls, config_dict: Dict[str, Any]) -> "FirecrawlConfig":
        """Create configuration from dictionary."""
        return cls(**config_dict)
    
    def to_dict(self) -> Dict[str, Any]:
        """Convert configuration to dictionary."""
        return {
            "api_key": self.api_key,
            "api_url": self.api_url,
            "max_retries": self.max_retries,
            "timeout": self.timeout,
            "rate_limit_delay": self.rate_limit_delay,
            "max_concurrent_requests": self.max_concurrent_requests
        }
    
    def to_json(self) -> str:
        """Convert configuration to JSON string."""
        return json.dumps(self.to_dict(), indent=2)
    
    @classmethod
    def from_json(cls, json_str: str) -> "FirecrawlConfig":
        """Create configuration from JSON string."""
        config_dict = json.loads(json_str)
        return cls.from_dict(config_dict)

```

## Detailed Analysis

### File Role in Repository

The file `intergrations/firecrawl/firecrawl_config.py` is located in the `intergrations/firecrawl` directory.

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
