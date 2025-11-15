# File Documentation: generate_docs.py

## File Metadata

- **Path**: `generate_docs.py`
- **Extension**: `.py`
- **Lines**: 1,413
- **Characters**: 48,822
- **Size**: 48,848 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

## Original Source

```python
#!/usr/bin/env python3
"""
RAGFlow Repository Documentation Generator

This script generates comprehensive documentation for the entire RAGFlow repository,
creating:
- Per-file documentation (_docs.md and _kw.md)
- Per-folder documentation (index.md, doc.md, sub.md)
- Global documentation (comprehensive_book.md, keywords.md, index.md)
"""

import os
import json
import re
from pathlib import Path
from typing import Dict, List, Set, Tuple
from collections import defaultdict

# Configuration
REPO_ROOT = Path("/home/user/ragflow")
DOCS_ROOT = REPO_ROOT / "docs_new"
EXCLUDE_PATTERNS = [
    ".git", "__pycache__", "node_modules", ".venv", "dist", "build",
    ".pytest_cache", ".mypy_cache", "*.pyc", "*.pyo", "*.so", "*.dylib",
    "*.egg-info", ".DS_Store", "docs_new"
]
MAX_FILE_SIZE = 10 * 1024 * 1024  # 10MB max for text files
BINARY_EXTENSIONS = {
    '.png', '.jpg', '.jpeg', '.gif', '.bmp', '.ico', '.svg', '.webp',
    '.mp4', '.avi', '.mov', '.wmv', '.flv', '.mkv',
    '.mp3', '.wav', '.ogg', '.flac',
    '.zip', '.tar', '.gz', '.bz2', '.xz', '.7z', '.rar',
    '.pdf', '.doc', '.docx', '.xls', '.xlsx', '.ppt', '.pptx',
    '.exe', '.dll', '.so', '.dylib', '.bin', '.dat',
    '.woff', '.woff2', '.ttf', '.eot', '.otf',
    '.lock', '.lockb'  # Lock files can be huge
}

class RepoAnalyzer:
    """Analyzes repository structure and generates documentation."""

    def __init__(self):
        self.all_files: List[Path] = []
        self.all_dirs: List[Path] = []
        self.file_keywords: Dict[Path, Set[str]] = defaultdict(set)
        self.global_keywords: Dict[str, List[Path]] = defaultdict(list)
        self.stats = {
            'files_processed': 0,
            'dirs_processed': 0,
            'docs_created': 0,
            'skipped_binary': 0,
            'skipped_large': 0,
            'errors': 0
        }

    def should_exclude(self, path: Path) -> bool:
        """Check if path should be excluded."""
        path_str = str(path)
        for pattern in EXCLUDE_PATTERNS:
            if pattern in path_str:
                return True
        return False

    def is_binary_file(self, file_path: Path) -> bool:
        """Check if file is likely binary."""
        return file_path.suffix.lower() in BINARY_EXTENSIONS

    def scan_repository(self):
        """Scan repository and collect all files and directories."""
        print("Scanning repository structure...")

        for item in REPO_ROOT.rglob("*"):
            if self.should_exclude(item):
                continue

            if item.is_file():
                self.all_files.append(item.relative_to(REPO_ROOT))
            elif item.is_dir():
                self.all_dirs.append(item.relative_to(REPO_ROOT))

        self.all_files.sort()
        self.all_dirs.sort()

        print(f"Found {len(self.all_files)} files and {len(self.all_dirs)} directories")

    def extract_keywords(self, content: str, file_path: Path) -> Set[str]:
        """Extract keywords from file content."""
        keywords = set()

        # Extract identifiers (function names, class names, variables)
        # Python/JS/TS patterns
        patterns = [
            r'\bclass\s+(\w+)',  # class definitions
            r'\bdef\s+(\w+)',  # function definitions
            r'\bfunction\s+(\w+)',  # JS function definitions
            r'\bconst\s+(\w+)',  # const declarations
            r'\blet\s+(\w+)',  # let declarations
            r'\bvar\s+(\w+)',  # var declarations
            r'\binterface\s+(\w+)',  # TypeScript interfaces
            r'\btype\s+(\w+)',  # TypeScript types
            r'\benum\s+(\w+)',  # enums
            r'@(\w+)',  # decorators
            r'import\s+.*\s+from\s+["\']([^"\']+)["\']',  # imports
        ]

        for pattern in patterns:
            matches = re.finditer(pattern, content)
            for match in matches:
                keywords.add(match.group(1))

        # Extract important technical terms (case-sensitive words with capital letters)
        tech_terms = re.findall(r'\b[A-Z][a-zA-Z0-9_]*\b', content)
        keywords.update(term for term in tech_terms if len(term) > 2)

        # Add file-specific context
        ext = file_path.suffix.lower()
        if ext == '.py':
            keywords.add('Python')
        elif ext in ['.js', '.jsx']:
            keywords.add('JavaScript')
        elif ext in ['.ts', '.tsx']:
            keywords.add('TypeScript')
        elif ext == '.md':
            keywords.add('Documentation')

        return keywords

    def generate_file_docs(self, file_path: Path) -> Tuple[str, str]:
        """Generate _docs.md and _kw.md for a single file."""
        full_path = REPO_ROOT / file_path
        rel_path_str = str(file_path)

        # Check if binary
        if self.is_binary_file(file_path):
            self.stats['skipped_binary'] += 1
            return self.generate_binary_file_docs(file_path)

        # Check file size
        try:
            if full_path.stat().st_size > MAX_FILE_SIZE:
                self.stats['skipped_large'] += 1
                return self.generate_large_file_docs(file_path)
        except Exception:
            pass

        # Read file content
        try:
            content = full_path.read_text(encoding='utf-8', errors='ignore')
        except Exception as e:
            print(f"Error reading {file_path}: {e}")
            self.stats['errors'] += 1
            return self.generate_error_file_docs(file_path, str(e))

        # Extract keywords
        keywords = self.extract_keywords(content, file_path)
        self.file_keywords[file_path] = keywords
        for kw in keywords:
            self.global_keywords[kw].append(file_path)

        # Generate _docs.md
        docs_md = self.create_file_documentation(file_path, content, keywords)

        # Generate _kw.md
        kw_md = self.create_file_keywords(file_path, keywords)

        return docs_md, kw_md

    def create_file_documentation(self, file_path: Path, content: str, keywords: Set[str]) -> str:
        """Create comprehensive documentation for a file."""
        lines = content.split('\n')
        line_count = len(lines)
        char_count = len(content)

        # Analyze file type and purpose
        ext = file_path.suffix.lower()
        purpose = self.infer_file_purpose(file_path, content)

        # Truncate content if too long for display
        display_content = content
        if len(content) > 50000:
            display_content = content[:50000] + "\n\n[... Content truncated for brevity ...]"

        docs = f"""# File Documentation: {file_path}

## File Metadata

- **Path**: `{file_path}`
- **Extension**: `{ext or 'none'}`
- **Lines**: {line_count:,}
- **Characters**: {char_count:,}
- **Size**: {len(content.encode('utf-8')):,} bytes
- **Purpose**: {purpose}

## Original Source

```{self.get_language_for_fence(ext)}
{display_content}
```

## High-Level Overview

{self.generate_overview(file_path, content)}

## Detailed Walkthrough

{self.generate_walkthrough(file_path, content, ext)}

## Code Structure Analysis

{self.analyze_code_structure(content, ext)}

## Dependencies and Imports

{self.analyze_dependencies(content, ext)}

## Design & Architecture

{self.analyze_architecture(file_path, content)}

## Performance & Complexity

{self.analyze_performance(content, ext)}

## Security & Safety Considerations

{self.analyze_security(content, ext)}

## Testing & Usage Notes

{self.generate_testing_notes(file_path, content)}

## Related Files

{self.identify_related_files(file_path, content)}

## Keywords

{', '.join(sorted(keywords)[:50])}{'...' if len(keywords) > 50 else ''}

---
*Generated by RAGFlow Repository Documentation Generator*
"""
        return docs

    def create_file_keywords(self, file_path: Path, keywords: Set[str]) -> str:
        """Create keyword index for a file."""
        # Calculate relative paths for navigation
        depth = len(file_path.parts) - 1
        back_to_repo = '../' * (depth + 1)
        back_to_docs = '../' * depth if depth > 0 else './'

        kw_md = f"""# Keyword Map: {file_path}

## File Path and Links

- **Original File**: [{file_path}]({back_to_repo}{file_path})
- **Documentation**: [{file_path.name}_docs.md](./{file_path.name}_docs.md)
- **Repository Root**: [{back_to_repo}]({back_to_repo})

## Keywords ({len(keywords)} total)

"""
        # Sort keywords alphabetically
        sorted_keywords = sorted(keywords)

        # Group by first letter
        current_letter = None
        for kw in sorted_keywords:
            first_letter = kw[0].upper() if kw else '?'
            if first_letter != current_letter:
                current_letter = first_letter
                kw_md += f"\n### {current_letter}\n\n"

            kw_md += f"- **{kw}**: Found in {file_path.name}\n"
            kw_md += f"  - [View in documentation](./{file_path.name}_docs.md)\n"

        kw_md += f"""
## Keyword Summary

This file contains {len(keywords)} unique keywords extracted from the source code,
including identifiers, function names, class names, and technical terms.

---
*Generated by RAGFlow Repository Documentation Generator*
"""
        return kw_md

    def generate_binary_file_docs(self, file_path: Path) -> Tuple[str, str]:
        """Generate docs for binary files."""
        full_path = REPO_ROOT / file_path
        size = full_path.stat().st_size if full_path.exists() else 0

        docs = f"""# File Documentation: {file_path}

## File Metadata

- **Path**: `{file_path}`
- **Type**: Binary file
- **Extension**: `{file_path.suffix}`
- **Size**: {size:,} bytes

## Overview

This is a binary file that cannot be directly analyzed as text. Binary files in the repository
may include images, compiled libraries, archives, or other non-text data.

## Purpose

{self.infer_binary_purpose(file_path)}

---
*Generated by RAGFlow Repository Documentation Generator*
"""

        kw = f"""# Keyword Map: {file_path}

## File Information

- **Original File**: `{file_path}`
- **Type**: Binary file
- **Keywords**: N/A (binary file)

---
*Generated by RAGFlow Repository Documentation Generator*
"""
        return docs, kw

    def generate_large_file_docs(self, file_path: Path) -> Tuple[str, str]:
        """Generate docs for very large files."""
        full_path = REPO_ROOT / file_path
        size = full_path.stat().st_size

        docs = f"""# File Documentation: {file_path}

## File Metadata

- **Path**: `{file_path}`
- **Size**: {size:,} bytes ({size / 1024 / 1024:.2f} MB)
- **Status**: Too large for full analysis

## Overview

This file exceeds the maximum size threshold ({MAX_FILE_SIZE / 1024 / 1024:.2f} MB) for
detailed analysis. It likely contains generated code, large data files, or dependency locks.

---
*Generated by RAGFlow Repository Documentation Generator*
"""

        kw = f"""# Keyword Map: {file_path}

## File Information

- **Original File**: `{file_path}`
- **Status**: Too large for keyword extraction

---
*Generated by RAGFlow Repository Documentation Generator*
"""
        return docs, kw

    def generate_error_file_docs(self, file_path: Path, error: str) -> Tuple[str, str]:
        """Generate docs for files that couldn't be read."""
        docs = f"""# File Documentation: {file_path}

## Error

Unable to read file: {error}

---
*Generated by RAGFlow Repository Documentation Generator*
"""
        kw = f"""# Keyword Map: {file_path}

## Error

Unable to extract keywords: {error}

---
*Generated by RAGFlow Repository Documentation Generator*
"""
        return docs, kw

    # Helper methods for analysis

    def infer_file_purpose(self, file_path: Path, content: str) -> str:
        """Infer the purpose of a file based on path and content."""
        name = file_path.name.lower()
        path_str = str(file_path).lower()

        if 'test' in path_str:
            return "Testing - Contains unit tests, integration tests, or test utilities"
        elif name in ['readme.md', 'readme.rst', 'readme.txt']:
            return "Documentation - Project or module README file"
        elif name in ['setup.py', 'setup.cfg', 'pyproject.toml', 'package.json']:
            return "Configuration - Package/project configuration and dependencies"
        elif name in ['dockerfile', 'docker-compose.yml', 'docker-compose.yaml']:
            return "Infrastructure - Docker container configuration"
        elif name.endswith('.py'):
            if 'class' in content or 'def ' in content:
                return "Python Module - Contains classes, functions, or business logic"
            return "Python Script - Executable Python code"
        elif name.endswith(('.js', '.jsx', '.ts', '.tsx')):
            return "JavaScript/TypeScript - Frontend or backend JavaScript code"
        elif name.endswith('.md'):
            return "Documentation - Markdown documentation file"
        elif name.endswith(('.yml', '.yaml')):
            return "Configuration - YAML configuration file"
        elif name.endswith('.json'):
            return "Data/Configuration - JSON data or configuration file"
        else:
            return "General file in the repository"

    def infer_binary_purpose(self, file_path: Path) -> str:
        """Infer purpose of binary file."""
        ext = file_path.suffix.lower()

        if ext in ['.png', '.jpg', '.jpeg', '.gif', '.svg', '.ico']:
            return "Image asset used in the application or documentation"
        elif ext in ['.woff', '.woff2', '.ttf', '.eot', '.otf']:
            return "Font file for web or application UI"
        elif ext in ['.zip', '.tar', '.gz', '.bz2']:
            return "Archive file containing compressed data"
        elif ext == '.pdf':
            return "PDF document"
        elif ext in ['.so', '.dylib', '.dll']:
            return "Compiled library or shared object"
        else:
            return f"Binary file with extension {ext}"

    def get_language_for_fence(self, ext: str) -> str:
        """Get language identifier for code fence."""
        mapping = {
            '.py': 'python',
            '.js': 'javascript',
            '.jsx': 'jsx',
            '.ts': 'typescript',
            '.tsx': 'tsx',
            '.md': 'markdown',
            '.json': 'json',
            '.yml': 'yaml',
            '.yaml': 'yaml',
            '.sh': 'bash',
            '.bash': 'bash',
            '.toml': 'toml',
            '.ini': 'ini',
            '.cfg': 'ini',
            '.sql': 'sql',
            '.html': 'html',
            '.css': 'css',
            '.scss': 'scss',
            '.xml': 'xml',
            '.cpp': 'cpp',
            '.c': 'c',
            '.h': 'c',
            '.java': 'java',
            '.go': 'go',
            '.rs': 'rust',
            '.rb': 'ruby',
            '.php': 'php',
        }
        return mapping.get(ext.lower(), '')

    def generate_overview(self, file_path: Path, content: str) -> str:
        """Generate high-level overview of file."""
        lines = content.split('\n')

        # Look for docstrings or comments at the top
        overview_lines = []
        for i, line in enumerate(lines[:50]):  # Check first 50 lines
            stripped = line.strip()
            if stripped.startswith('"""') or stripped.startswith("'''") or \
               stripped.startswith('/*') or stripped.startswith('//') or \
               stripped.startswith('#'):
                overview_lines.append(line)
            elif overview_lines and not stripped:
                break

        if overview_lines:
            return '\n'.join(overview_lines)

        # Generic overview
        return f"""This file is part of the RAGFlow repository located at `{file_path}`.

Based on the file structure and naming, it appears to be a {self.infer_file_purpose(file_path, content).lower()}.

The file contains approximately {len(lines)} lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system."""

    def generate_walkthrough(self, file_path: Path, content: str, ext: str) -> str:
        """Generate detailed walkthrough of file contents."""
        if ext == '.py':
            return self.walkthrough_python(content)
        elif ext in ['.js', '.jsx', '.ts', '.tsx']:
            return self.walkthrough_javascript(content)
        elif ext in ['.md', '.rst']:
            return "This is a documentation file. See the 'Original Source' section for full content."
        elif ext in ['.json', '.yaml', '.yml', '.toml']:
            return "This is a configuration or data file. See the 'Original Source' section for full content."
        else:
            return "This file's structure is not automatically analyzed. See the 'Original Source' section for content."

    def walkthrough_python(self, content: str) -> str:
        """Generate walkthrough for Python files."""
        walkthrough = []

        # Find classes
        classes = re.findall(r'^class\s+(\w+).*?:', content, re.MULTILINE)
        if classes:
            walkthrough.append(f"### Classes ({len(classes)})\n")
            for cls in classes[:20]:  # Limit to first 20
                walkthrough.append(f"- `{cls}`: Class definition")

        # Find functions
        functions = re.findall(r'^def\s+(\w+)\s*\(', content, re.MULTILINE)
        if functions:
            walkthrough.append(f"\n### Functions ({len(functions)})\n")
            for func in functions[:20]:  # Limit to first 20
                walkthrough.append(f"- `{func}()`: Function definition")

        # Find imports
        imports = re.findall(r'^(?:from\s+[\w.]+\s+)?import\s+.*', content, re.MULTILINE)
        if imports:
            walkthrough.append(f"\n### Imports ({len(imports)})\n")
            for imp in imports[:10]:  # Limit to first 10
                walkthrough.append(f"- `{imp}`")

        return '\n'.join(walkthrough) if walkthrough else "No significant structural elements detected."

    def walkthrough_javascript(self, content: str) -> str:
        """Generate walkthrough for JavaScript/TypeScript files."""
        walkthrough = []

        # Find exports
        exports = re.findall(r'^export\s+(?:default\s+)?(?:class|function|const|let|var)\s+(\w+)', content, re.MULTILINE)
        if exports:
            walkthrough.append(f"### Exports ({len(exports)})\n")
            for exp in exports[:20]:
                walkthrough.append(f"- `{exp}`: Exported entity")

        # Find functions
        functions = re.findall(r'(?:function\s+(\w+)|const\s+(\w+)\s*=\s*(?:\([^)]*\)|[^=]+)\s*=>)', content)
        func_names = [f[0] or f[1] for f in functions if f[0] or f[1]]
        if func_names:
            walkthrough.append(f"\n### Functions ({len(func_names)})\n")
            for func in func_names[:20]:
                walkthrough.append(f"- `{func}()`: Function definition")

        # Find imports
        imports = re.findall(r'^import\s+.*', content, re.MULTILINE)
        if imports:
            walkthrough.append(f"\n### Imports ({len(imports)})\n")
            for imp in imports[:10]:
                walkthrough.append(f"- `{imp}`")

        return '\n'.join(walkthrough) if walkthrough else "No significant structural elements detected."

    def analyze_code_structure(self, content: str, ext: str) -> str:
        """Analyze code structure."""
        lines = content.split('\n')
        blank_lines = sum(1 for line in lines if not line.strip())
        comment_lines = sum(1 for line in lines if line.strip().startswith(('#', '//', '/*', '*', '"""', "'''")))

        return f"""- Total lines: {len(lines)}
- Blank lines: {blank_lines} ({blank_lines/len(lines)*100:.1f}%)
- Comment lines: ~{comment_lines} ({comment_lines/len(lines)*100:.1f}%)
- Code lines: ~{len(lines) - blank_lines - comment_lines}
"""

    def analyze_dependencies(self, content: str, ext: str) -> str:
        """Analyze file dependencies."""
        if ext == '.py':
            imports = re.findall(r'^(?:from\s+([\w.]+)\s+)?import\s+(.+)', content, re.MULTILINE)
            if imports:
                deps = []
                for from_part, import_part in imports[:20]:
                    if from_part:
                        deps.append(f"- `from {from_part} import {import_part}`")
                    else:
                        deps.append(f"- `import {import_part}`")
                return '\n'.join(deps)
        elif ext in ['.js', '.jsx', '.ts', '.tsx']:
            imports = re.findall(r'import\s+.*\s+from\s+["\']([^"\']+)["\']', content)
            if imports:
                return '\n'.join(f"- `{imp}`" for imp in imports[:20])

        return "No explicit dependencies detected or not applicable for this file type."

    def analyze_architecture(self, file_path: Path, content: str) -> str:
        """Analyze how file fits into architecture."""
        path_parts = file_path.parts

        analysis = f"This file is located in the `{path_parts[0] if path_parts else 'root'}` directory"

        if len(path_parts) > 1:
            analysis += f", specifically within `{'/'.join(path_parts[:-1])}`"

        analysis += ".\n\n"

        # Infer architectural role
        if 'api' in str(file_path).lower():
            analysis += "As part of the API layer, this file likely handles HTTP requests, business logic, or data access."
        elif 'test' in str(file_path).lower():
            analysis += "This is a test file, contributing to the quality assurance and validation of the codebase."
        elif 'component' in str(file_path).lower() or 'web' in str(file_path).lower():
            analysis += "This appears to be a UI component or frontend module."
        elif 'model' in str(file_path).lower() or 'schema' in str(file_path).lower():
            analysis += "This file likely defines data models or schemas used throughout the application."
        else:
            analysis += "This file contributes to the overall functionality of the RAGFlow system."

        return analysis

    def analyze_performance(self, content: str, ext: str) -> str:
        """Analyze performance considerations."""
        analysis = []

        # Look for loops
        if 'for ' in content or 'while ' in content:
            loop_count = content.count('for ') + content.count('while ')
            analysis.append(f"- Contains {loop_count} loop(s) - consider algorithmic complexity")

        # Look for database queries
        if 'SELECT' in content or 'query' in content.lower():
            analysis.append("- Contains database queries - ensure proper indexing and query optimization")

        # Look for async patterns
        if 'async ' in content or 'await ' in content:
            analysis.append("- Uses asynchronous patterns for better performance")

        if not analysis:
            analysis.append("- No specific performance concerns identified through static analysis")

        return '\n'.join(analysis)

    def analyze_security(self, content: str, ext: str) -> str:
        """Analyze security considerations."""
        concerns = []

        # Check for SQL
        if 'SELECT' in content or 'INSERT' in content or 'UPDATE' in content:
            concerns.append("- **SQL Operations**: Ensure parameterized queries to prevent SQL injection")

        # Check for user input
        if 'request' in content.lower() or 'input' in content.lower():
            concerns.append("- **User Input**: Validate and sanitize all user input")

        # Check for authentication
        if 'password' in content.lower() or 'auth' in content.lower():
            concerns.append("- **Authentication**: Ensure secure password handling and authentication")

        # Check for file operations
        if 'open(' in content or 'read' in content or 'write' in content:
            concerns.append("- **File Operations**: Validate file paths to prevent directory traversal")

        # Check for eval or exec
        if 'eval(' in content or 'exec(' in content:
            concerns.append("- **Code Execution**: Avoid eval/exec with user input - potential code injection")

        if not concerns:
            concerns.append("- No immediate security concerns identified through static analysis")

        return '\n'.join(concerns)

    def generate_testing_notes(self, file_path: Path, content: str) -> str:
        """Generate testing and usage notes."""
        if 'test' in str(file_path).lower():
            return "This is a test file. Run it using the project's test framework (pytest, jest, etc.)."

        notes = []
        notes.append(f"To work with this file:")
        notes.append(f"1. Understand its dependencies (see Dependencies section)")
        notes.append(f"2. Review the code structure and main components")
        notes.append(f"3. Check for existing tests in the test directories")
        notes.append(f"4. Consider edge cases and error handling")

        return '\n'.join(notes)

    def identify_related_files(self, file_path: Path, content: str) -> str:
        """Identify related files."""
        related = []

        # Look at directory siblings
        parent = file_path.parent
        related.append(f"- Other files in `{parent}/` directory")

        # Look at imports to find related files
        if file_path.suffix == '.py':
            imports = re.findall(r'from\s+([\w.]+)\s+import', content)
            for imp in imports[:5]:
                if not imp.startswith(('os', 'sys', 're', 'json', 'typing')):
                    related.append(f"- Imports from `{imp}`")

        # Look at test files
        if 'test' not in str(file_path).lower():
            test_file = f"test_{file_path.stem}{file_path.suffix}"
            related.append(f"- Potential test file: `{test_file}`")

        return '\n'.join(related) if related else "No specific related files identified."

    def generate_folder_index(self, folder_path: Path) -> str:
        """Generate index.md for a folder."""
        full_path = REPO_ROOT / folder_path

        # Get immediate children
        subfolders = []
        files = []

        try:
            for item in full_path.iterdir():
                rel_item = item.relative_to(REPO_ROOT)
                if self.should_exclude(rel_item):
                    continue

                if item.is_dir():
                    subfolders.append(item.name)
                elif item.is_file():
                    files.append(item.name)
        except Exception as e:
            print(f"Error reading folder {folder_path}: {e}")
            return f"# Index: {folder_path}\n\nError reading folder: {e}"

        subfolders.sort()
        files.sort()

        # Build index
        index = f"""# Index: {folder_path}

## Overview

This directory contains {len(files)} file(s) and {len(subfolders)} subdirectory(ies).

## Subfolders ({len(subfolders)})

"""

        if subfolders:
            for subfolder in subfolders:
                index += f"- **[{subfolder}/](./{subfolder}/index.md)** - Subdirectory\n"
        else:
            index += "*No subfolders*\n"

        index += f"""
## Files ({len(files)})

| File | Documentation | Keywords |
|------|---------------|----------|
"""

        for file in files:
            safe_name = file.replace('|', '\\|')
            index += f"| {safe_name} | [{file}_docs.md](./{file}_docs.md) | [{file}_kw.md](./{file}_kw.md) |\n"

        index += """
## Navigation

- **[Parent Directory](../index.md)** - Go up one level
- **[Repository Root](../../index.md)** - Return to repository root

---
*Generated by RAGFlow Repository Documentation Generator*
"""

        return index

    def generate_folder_doc(self, folder_path: Path) -> str:
        """Generate doc.md for a folder."""
        doc = f"""# Documentation: {folder_path}

## Role in the Project

{self.infer_folder_role(folder_path)}

## Key Concepts

This folder contains components that contribute to the RAGFlow system's functionality.
Key concepts and patterns used in this folder will depend on the specific files contained within.

## Important Files

See the [folder index](./index.md) for a complete list of files in this directory.

## Data Flows & Interactions

Files in this folder interact with other parts of the RAGFlow system according to the
overall architecture. Refer to individual file documentation for specific dependencies
and interactions.

## How to Work with This Folder

1. Review the folder index to understand the contents
2. Read individual file documentation for detailed information
3. Check test files (if present) for usage examples
4. Follow the project's contribution guidelines when making changes

## Cross References

- [Parent folder documentation](../doc.md)
- [Repository root documentation](../../doc.md)

---
*Generated by RAGFlow Repository Documentation Generator*
"""
        return doc

    def infer_folder_role(self, folder_path: Path) -> str:
        """Infer the role of a folder in the project."""
        name = folder_path.name.lower()
        path_str = str(folder_path).lower()

        if name == 'api' or 'api' in path_str:
            return """This folder contains API-related code, including endpoints, request handlers,
and business logic for the RAGFlow REST API."""
        elif name == 'test' or 'test' in path_str:
            return """This folder contains test files for validating the functionality of the codebase."""
        elif name == 'web' or 'frontend' in path_str:
            return """This folder contains frontend code, including UI components, styles, and client-side logic."""
        elif name == 'agent' or 'agents' in path_str:
            return """This folder contains agent-related code for the RAGFlow agent system."""
        elif name == 'rag':
            return """This folder contains core RAG (Retrieval-Augmented Generation) functionality."""
        elif name == 'graphrag':
            return """This folder contains graph-based RAG implementations."""
        elif name == 'deepdoc':
            return """This folder contains document processing and parsing functionality."""
        elif name == 'db' or 'database' in path_str:
            return """This folder contains database models, migrations, and data access code."""
        elif name == 'utils' or 'common' in path_str:
            return """This folder contains utility functions and common code used throughout the project."""
        elif name == 'config' or 'conf' in path_str:
            return """This folder contains configuration files and settings."""
        elif name == 'docker':
            return """This folder contains Docker-related files for containerization and deployment."""
        else:
            return f"""This folder (`{folder_path}`) is part of the RAGFlow repository structure
and contains related functionality organized under this directory."""

    def generate_folder_keywords(self, folder_path: Path) -> str:
        """Generate sub.md keyword index for folder and all descendants."""
        # Collect all files in this folder and subfolders
        full_path = REPO_ROOT / folder_path
        all_keywords: Dict[str, List[Path]] = defaultdict(list)

        try:
            for file_path in self.all_files:
                if str(file_path).startswith(str(folder_path)) or folder_path == Path('.'):
                    keywords = self.file_keywords.get(file_path, set())
                    for kw in keywords:
                        all_keywords[kw].append(file_path)
        except Exception as e:
            print(f"Error generating keywords for {folder_path}: {e}")

        # Build keyword index
        kw_md = f"""# Subtree Keyword Index: {folder_path}

## Scope

This keyword index covers all files in `{folder_path}/` and its subdirectories.
Total keywords: {len(all_keywords)}

## Keywords A-Z

"""

        # Sort keywords
        sorted_keywords = sorted(all_keywords.keys())

        current_letter = None
        for kw in sorted_keywords[:1000]:  # Limit to first 1000 keywords
            first_letter = kw[0].upper() if kw else '?'
            if first_letter != current_letter:
                current_letter = first_letter
                kw_md += f"\n### {current_letter}\n\n"

            files = all_keywords[kw]
            kw_md += f"**{kw}** ({len(files)} file(s)):\n"
            for file_path in files[:10]:  # Limit to 10 files per keyword
                # Calculate relative path
                try:
                    rel_path = os.path.relpath(
                        DOCS_ROOT / file_path.parent / f"{file_path.name}_docs.md",
                        DOCS_ROOT / folder_path
                    )
                    kw_md += f"  - [{file_path}]({rel_path})\n"
                except:
                    kw_md += f"  - {file_path}\n"
            if len(files) > 10:
                kw_md += f"  - ... and {len(files) - 10} more\n"
            kw_md += "\n"

        if len(all_keywords) > 1000:
            kw_md += f"\n*Showing first 1000 of {len(all_keywords)} keywords*\n"

        kw_md += """
---
*Generated by RAGFlow Repository Documentation Generator*
"""

        return kw_md

    def generate_global_keywords(self) -> str:
        """Generate global keywords.md file."""
        kw_md = """# Global Keyword Index

## Overview

This file contains a comprehensive index of all keywords found across the RAGFlow repository.
Keywords are automatically extracted from source code and include class names, function names,
identifiers, and technical terms.

## Usage

Use this index to quickly find where specific concepts, functions, or classes are defined
or used throughout the codebase.

## Keywords by Letter

"""

        # Sort all keywords
        sorted_keywords = sorted(self.global_keywords.keys())

        current_letter = None
        keyword_count = 0
        max_keywords = 2000  # Limit for global index

        for kw in sorted_keywords:
            if keyword_count >= max_keywords:
                break

            first_letter = kw[0].upper() if kw else '?'
            if first_letter != current_letter:
                current_letter = first_letter
                kw_md += f"\n### {current_letter}\n\n"

            files = self.global_keywords[kw]
            kw_md += f"**{kw}** ({len(files)} occurrence(s)):\n"

            for file_path in files[:5]:  # Limit to 5 files per keyword
                # Calculate relative path from docs root
                rel_path = str(file_path.parent / f"{file_path.name}_docs.md")
                kw_md += f"  - [{file_path}](./{rel_path})\n"

            if len(files) > 5:
                kw_md += f"  - ... and {len(files) - 5} more\n"
            kw_md += "\n"

            keyword_count += 1

        kw_md += f"""
## Statistics

- Total unique keywords: {len(self.global_keywords):,}
- Keywords shown: {min(len(self.global_keywords), max_keywords):,}
- Files indexed: {len(self.all_files):,}

---
*Generated by RAGFlow Repository Documentation Generator*
"""

        return kw_md

    def generate_global_index(self) -> str:
        """Generate global index.md file."""
        index = """# RAGFlow Repository Documentation

## Overview

Welcome to the comprehensive documentation for the RAGFlow repository. This documentation
system provides detailed information about every file and folder in the codebase.

RAGFlow is an open-source RAG (Retrieval-Augmented Generation) engine based on deep
document understanding.

## Repository Statistics

"""

        index += f"""- **Total Files**: {len(self.all_files):,}
- **Total Directories**: {len(self.all_dirs):,}
- **Documentation Files Created**: {self.stats['docs_created']:,}
- **Unique Keywords**: {len(self.global_keywords):,}

## Repository Structure

### Core Directories

"""

        # List top-level directories
        top_level_dirs = set(f.parts[0] for f in self.all_dirs if len(f.parts) > 0)
        for dirname in sorted(top_level_dirs):
            index += f"- **[{dirname}/](./{dirname}/index.md)** - {self.infer_folder_role(Path(dirname)).split('.')[0]}\n"

        index += """
## Global Documentation

- **[Comprehensive Book](./comprehensive_book.md)** - Complete documentation book
- **[Global Keyword Index](./keywords.md)** - Find any keyword across the repository
- **[Root Folder Index](./index.md)** - This file

## Navigation Guide

1. **By Structure**: Browse the directory tree using folder index files
2. **By Keyword**: Use the global keyword index to find specific concepts
3. **By Book**: Read the comprehensive book for a guided tour

## Quick Links

### By Component

- [API Documentation](./api/index.md)
- [RAG Engine](./rag/index.md)
- [Agent System](./agent/index.md)
- [Web Frontend](./web/index.md)
- [Graph RAG](./graphrag/index.md)
- [Document Processing](./deepdoc/index.md)

---
*Generated by RAGFlow Repository Documentation Generator*
"""

        return index

    def generate_comprehensive_book(self) -> str:
        """Generate the comprehensive book."""
        book = """# RAGFlow: The Comprehensive Guide

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

"""

        book += f"""- Files: {len(self.all_files):,}
- Directories: {len(self.all_dirs):,}
- Lines of Code: ~{sum(1 for _ in range(len(self.all_files) * 100))}+ (estimated)

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

"""

        # Add sections for each major directory
        major_dirs = ['api', 'rag', 'agent', 'graphrag', 'deepdoc', 'web']

        for dirname in major_dirs:
            dir_path = Path(dirname)
            if dir_path in self.all_dirs or any(str(f).startswith(dirname) for f in self.all_files):
                book += f"""
## {dirname.upper()} Component

{self.infer_folder_role(dir_path)}

For detailed documentation, see:
- [Folder Index](./{dirname}/index.md)
- [Folder Documentation](./{dirname}/doc.md)
- [Keyword Index](./{dirname}/sub.md)

"""

        book += """
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

"""

        # List all top-level directories
        top_level = sorted(set(f.parts[0] for f in self.all_dirs if len(f.parts) > 0))
        for dirname in top_level:
            book += f"- [{dirname}/](./{dirname}/index.md)\n"

        book += """
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
"""

        return book

    def get_timestamp(self) -> str:
        """Get current timestamp."""
        from datetime import datetime
        return datetime.now().strftime("%Y-%m-%d %H:%M:%S")

    def write_documentation(self):
        """Write all documentation files."""
        print("\nGenerating documentation files...")

        # Process all files
        print(f"\nProcessing {len(self.all_files)} files...")
        for i, file_path in enumerate(self.all_files):
            if i % 100 == 0:
                print(f"  Progress: {i}/{len(self.all_files)} files...")

            try:
                # Generate documentation
                docs_md, kw_md = self.generate_file_docs(file_path)

                # Determine output location
                output_dir = DOCS_ROOT / file_path.parent
                output_dir.mkdir(parents=True, exist_ok=True)

                # Write files
                docs_file = output_dir / f"{file_path.name}_docs.md"
                kw_file = output_dir / f"{file_path.name}_kw.md"

                docs_file.write_text(docs_md, encoding='utf-8')
                kw_file.write_text(kw_md, encoding='utf-8')

                self.stats['files_processed'] += 1
                self.stats['docs_created'] += 2

            except Exception as e:
                print(f"  Error processing {file_path}: {e}")
                self.stats['errors'] += 1

        print(f"  Completed: {self.stats['files_processed']}/{len(self.all_files)} files")

        # Process all directories
        print(f"\nProcessing {len(self.all_dirs)} directories...")

        # Add root directory
        dirs_to_process = [Path('.')] + self.all_dirs

        for i, dir_path in enumerate(dirs_to_process):
            if i % 50 == 0:
                print(f"  Progress: {i}/{len(dirs_to_process)} directories...")

            try:
                # Generate folder documentation
                index_md = self.generate_folder_index(dir_path)
                doc_md = self.generate_folder_doc(dir_path)
                sub_md = self.generate_folder_keywords(dir_path)

                # Determine output location
                if dir_path == Path('.'):
                    output_dir = DOCS_ROOT
                else:
                    output_dir = DOCS_ROOT / dir_path
                output_dir.mkdir(parents=True, exist_ok=True)

                # Write files
                (output_dir / "index.md").write_text(index_md, encoding='utf-8')
                (output_dir / "doc.md").write_text(doc_md, encoding='utf-8')
                (output_dir / "sub.md").write_text(sub_md, encoding='utf-8')

                self.stats['dirs_processed'] += 1
                self.stats['docs_created'] += 3

            except Exception as e:
                print(f"  Error processing directory {dir_path}: {e}")
                self.stats['errors'] += 1

        print(f"  Completed: {self.stats['dirs_processed']}/{len(dirs_to_process)} directories")

        # Generate global files
        print("\nGenerating global documentation files...")

        try:
            print("  - keywords.md")
            keywords_md = self.generate_global_keywords()
            (DOCS_ROOT / "keywords.md").write_text(keywords_md, encoding='utf-8')
            self.stats['docs_created'] += 1

            print("  - index.md")
            index_md = self.generate_global_index()
            (DOCS_ROOT / "index.md").write_text(index_md, encoding='utf-8')
            self.stats['docs_created'] += 1

            print("  - comprehensive_book.md")
            book_md = self.generate_comprehensive_book()
            (DOCS_ROOT / "comprehensive_book.md").write_text(book_md, encoding='utf-8')
            self.stats['docs_created'] += 1

        except Exception as e:
            print(f"  Error generating global files: {e}")
            self.stats['errors'] += 1

        print("\nDocumentation generation complete!")

    def print_statistics(self):
        """Print final statistics."""
        print("\n" + "="*80)
        print("DOCUMENTATION GENERATION STATISTICS")
        print("="*80)
        print(f"Files in repository:        {len(self.all_files):,}")
        print(f"Directories in repository:  {len(self.all_dirs):,}")
        print(f"Files processed:            {self.stats['files_processed']:,}")
        print(f"Directories processed:      {self.stats['dirs_processed']:,}")
        print(f"Documentation files created: {self.stats['docs_created']:,}")
        print(f"Binary files skipped:       {self.stats['skipped_binary']:,}")
        print(f"Large files skipped:        {self.stats['skipped_large']:,}")
        print(f"Errors encountered:         {self.stats['errors']:,}")
        print(f"Unique keywords extracted:  {len(self.global_keywords):,}")
        print("="*80)

        # Calculate sizes
        total_size = 0
        try:
            for file in DOCS_ROOT.rglob("*.md"):
                total_size += file.stat().st_size
            print(f"\nTotal documentation size:   {total_size / 1024 / 1024:.2f} MB")
        except:
            pass

        print(f"\nDocumentation location:     {DOCS_ROOT}")
        print("\n" + "="*80)


def main():
    """Main entry point."""
    print("="*80)
    print("RAGFlow Repository Documentation Generator")
    print("="*80)
    print()
    print("This tool will generate comprehensive documentation for the entire")
    print("RAGFlow repository, including:")
    print("  - Per-file documentation (_docs.md and _kw.md)")
    print("  - Per-folder indexes (index.md, doc.md, sub.md)")
    print("  - Global documentation (comprehensive_book.md, keywords.md)")
    print()
    print("="*80)
    print()

    analyzer = RepoAnalyzer()

    # Scan repository
    analyzer.scan_repository()

    # Generate documentation
    analyzer.write_documentation()

    # Print statistics
    analyzer.print_statistics()

    print("\nDone!")


if __name__ == "__main__":
    main()

```

## High-Level Overview

#!/usr/bin/env python3
"""

## Detailed Walkthrough

### Classes (1)

- `RepoAnalyzer`: Class definition

### Functions (1)

- `main()`: Function definition

### Imports (6)

- `import os`
- `import json`
- `import re`
- `from pathlib import Path`
- `from typing import Dict, List, Set, Tuple`
- `from collections import defaultdict`

## Code Structure Analysis

- Total lines: 1413
- Blank lines: 346 (24.5%)
- Comment lines: ~216 (15.3%)
- Code lines: ~851


## Dependencies and Imports

- `import os`
- `import json`
- `import re`
- `from pathlib import Path`
- `from typing import Dict, List, Set, Tuple`
- `from collections import defaultdict`

## Design & Architecture

This file is located in the `generate_docs.py` directory.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 106 loop(s) - consider algorithmic complexity
- Contains database queries - ensure proper indexing and query optimization
- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **SQL Operations**: Ensure parameterized queries to prevent SQL injection
- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal
- **Code Execution**: Avoid eval/exec with user input - potential code injection

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `./` directory
- Imports from `pathlib`
- Imports from `collections`
- Imports from `datetime`
- Potential test file: `test_generate_docs.py`

## Keywords

API, Accurate, Add, Additional, Agent, Analysis, Analyze, Analyzes, Architecture, Archive, Augmented, Authentication, Avoid, BINARY_EXTENSIONS, Backend, Based, Binary, Blank, Book, Browse, Build, Business, CONTRIBUTING, Calculate, Characters, Check, Chunk, Class, Classes, Clone, Code, Collect, Comment, Compiled, Complete, Completed, Complexity, Component, Components, Compose, Comprehensive, Concepts, Conclusion, Configuration, Consider, Considerations, Contains, Content, Contents, Contributing...

---
*Generated by RAGFlow Repository Documentation Generator*
