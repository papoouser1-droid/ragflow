# Documentation: download_deps.py

## File Metadata

- **Path**: `download_deps.py`
- **Size**: 3442 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `download_deps.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `argparse`
- `os`
- `urllib.request`
- `typing`
- `nltk`
- `huggingface_hub`

### Functions Defined

This file defines 2 function(s):

#### Function: `get_urls` (line 20)

**Parameters**: use_china_mirrors

#### Function: `download_model` (line 50)

**Parameters**: repo_id

## Original Source Code

```py
#!/usr/bin/env python3

# PEP 723 metadata
# /// script
# requires-python = ">=3.10"
# dependencies = [
#   "nltk",
# ]
# ///

import argparse
import os
import urllib.request
from typing import Union

import nltk
from huggingface_hub import snapshot_download


def get_urls(use_china_mirrors=False) -> list[Union[str, list[str]]]:
    if use_china_mirrors:
        return [
            "http://mirrors.tuna.tsinghua.edu.cn/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2_amd64.deb",
            "http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2_arm64.deb",
            "https://repo.huaweicloud.com/repository/maven/org/apache/tika/tika-server-standard/3.0.0/tika-server-standard-3.0.0.jar",
            "https://repo.huaweicloud.com/repository/maven/org/apache/tika/tika-server-standard/3.0.0/tika-server-standard-3.0.0.jar.md5",
            "https://openaipublic.blob.core.windows.net/encodings/cl100k_base.tiktoken",
            ["https://registry.npmmirror.com/-/binary/chrome-for-testing/121.0.6167.85/linux64/chrome-linux64.zip", "chrome-linux64-121-0-6167-85"],
            ["https://registry.npmmirror.com/-/binary/chrome-for-testing/121.0.6167.85/linux64/chromedriver-linux64.zip", "chromedriver-linux64-121-0-6167-85"],
        ]
    else:
        return [
            "http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2_amd64.deb",
            "http://ports.ubuntu.com/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2_arm64.deb",
            "https://repo1.maven.org/maven2/org/apache/tika/tika-server-standard/3.0.0/tika-server-standard-3.0.0.jar",
            "https://repo1.maven.org/maven2/org/apache/tika/tika-server-standard/3.0.0/tika-server-standard-3.0.0.jar.md5",
            "https://openaipublic.blob.core.windows.net/encodings/cl100k_base.tiktoken",
            ["https://storage.googleapis.com/chrome-for-testing-public/121.0.6167.85/linux64/chrome-linux64.zip", "chrome-linux64-121-0-6167-85"],
            ["https://storage.googleapis.com/chrome-for-testing-public/121.0.6167.85/linux64/chromedriver-linux64.zip", "chromedriver-linux64-121-0-6167-85"],
        ]


repos = [
    "InfiniFlow/text_concat_xgb_v1.0",
    "InfiniFlow/deepdoc",
    "InfiniFlow/huqie",
]


def download_model(repo_id):
    local_dir = os.path.abspath(os.path.join("huggingface.co", repo_id))
    os.makedirs(local_dir, exist_ok=True)
    snapshot_download(repo_id=repo_id, local_dir=local_dir)


if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="Download dependencies with optional China mirror support")
    parser.add_argument("--china-mirrors", action="store_true", help="Use China-accessible mirrors for downloads")
    args = parser.parse_args()

    urls = get_urls(args.china_mirrors)

    for url in urls:
        download_url = url[0] if isinstance(url, list) else url
        filename = url[1] if isinstance(url, list) else url.split("/")[-1]
        print(f"Downloading {filename} from {download_url}...")
        if not os.path.exists(filename):
            urllib.request.urlretrieve(download_url, filename)

    local_dir = os.path.abspath("nltk_data")
    for data in ["wordnet", "punkt", "punkt_tab"]:
        print(f"Downloading nltk {data}...")
        nltk.download(data, download_dir=local_dir)

    for repo_id in repos:
        print(f"Downloading huggingface repo {repo_id}...")
        download_model(repo_id)

```

## Detailed Analysis

### File Role in Repository

The file `download_deps.py` is located in the `.` directory.

### Architecture Context

Files in this location typically handle concerns related to .

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

- [CLAUDE.md](CLAUDE.md_docs.md)
- [Dockerfile](Dockerfile_docs.md)
- [Dockerfile.deps](Dockerfile.deps_docs.md)
- [Dockerfile.scratch.oc9](Dockerfile.scratch.oc9_docs.md)
- [Dockerfile_tei](Dockerfile_tei_docs.md)
- [LICENSE](LICENSE_docs.md)
- [README.md](README.md_docs.md)
- [README_id.md](README_id.md_docs.md)
- [README_ja.md](README_ja.md_docs.md)
- [README_ko.md](README_ko.md_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
