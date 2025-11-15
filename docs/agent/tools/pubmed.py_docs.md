# Documentation: agent/tools/pubmed.py

## File Metadata

- **Path**: `agent/tools/pubmed.py`
- **Size**: 6564 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `agent/tools/pubmed.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `os`
- `time`
- `abc`
- `Bio`
- `re`
- `xml.etree.ElementTree`
- `agent.tools.base`
- `common.connection_utils`

### Classes Defined

This file defines 2 class(es):

#### Class: `PubMedParam` (line 27)

**Docstring**: Define the PubMed component parameters....

**Methods**: __init__, check, get_input_form

#### Class: `PubMed` (line 69)

**Methods**: _invoke, _format_pubmed_content, thoughts

### Functions Defined

This file defines 7 function(s):

#### Function: `__init__` (line 32)

**Parameters**: self

#### Function: `check` (line 58)

**Parameters**: self

#### Function: `get_input_form` (line 61)

**Parameters**: self

#### Function: `_invoke` (line 73)

**Parameters**: self

#### Function: `_format_pubmed_content` (line 118)

**Parameters**: self, child

**Docstring**: Extract structured reference info from PubMed XML...

#### Function: `thoughts` (line 163)

**Parameters**: self

#### Function: `safe_find` (line 120)

**Parameters**: path

## Original Source Code

```py
#
#  Copyright 2024 The InfiniFlow Authors. All Rights Reserved.
#
#  Licensed under the Apache License, Version 2.0 (the "License");
#  you may not use this file except in compliance with the License.
#  You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
#  Unless required by applicable law or agreed to in writing, software
#  distributed under the License is distributed on an "AS IS" BASIS,
#  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#  See the License for the specific language governing permissions and
#  limitations under the License.
#
import logging
import os
import time
from abc import ABC
from Bio import Entrez
import re
import xml.etree.ElementTree as ET
from agent.tools.base import ToolParamBase, ToolMeta, ToolBase
from common.connection_utils import timeout


class PubMedParam(ToolParamBase):
    """
    Define the PubMed component parameters.
    """

    def __init__(self):
        self.meta:ToolMeta = {
            "name": "pubmed_search",
            "description": """
PubMed is an openly accessible, free database which includes primarily the MEDLINE database of references and abstracts on life sciences and biomedical topics.
In addition to MEDLINE, PubMed provides access to:
 - older references from the print version of Index Medicus, back to 1951 and earlier
 - references to some journals before they were indexed in Index Medicus and MEDLINE, for instance Science, BMJ, and Annals of Surgery
 - very recent entries to records for an article before it is indexed with Medical Subject Headings (MeSH) and added to MEDLINE
 - a collection of books available full-text and other subsets of NLM records[4]
 - PMC citations
 - NCBI Bookshelf
            """,
            "parameters": {
                "query": {
                    "type": "string",
                    "description": "The search keywords to execute with PubMed. The keywords should be the most important words/terms(includes synonyms) from the original request.",
                    "default": "{sys.query}",
                    "required": True
                }
            }
        }
        super().__init__()
        self.top_n = 12
        self.email = "A.N.Other@example.com"

    def check(self):
        self.check_positive_integer(self.top_n, "Top N")

    def get_input_form(self) -> dict[str, dict]:
        return {
            "query": {
                "name": "Query",
                "type": "line"
            }
        }

class PubMed(ToolBase, ABC):
    component_name = "PubMed"

    @timeout(int(os.environ.get("COMPONENT_EXEC_TIMEOUT", 12)))
    def _invoke(self, **kwargs):
        if self.check_if_canceled("PubMed processing"):
            return

        if not kwargs.get("query"):
            self.set_output("formalized_content", "")
            return ""

        last_e = ""
        for _ in range(self._param.max_retries+1):
            if self.check_if_canceled("PubMed processing"):
                return

            try:
                Entrez.email = self._param.email
                pubmedids = Entrez.read(Entrez.esearch(db='pubmed', retmax=self._param.top_n, term=kwargs["query"]))['IdList']

                if self.check_if_canceled("PubMed processing"):
                    return

                pubmedcnt = ET.fromstring(re.sub(r'<(/?)b>|<(/?)i>', '', Entrez.efetch(db='pubmed', id=",".join(pubmedids),
                                                                                       retmode="xml").read().decode("utf-8")))

                if self.check_if_canceled("PubMed processing"):
                    return

                self._retrieve_chunks(pubmedcnt.findall("PubmedArticle"),
                                      get_title=lambda child: child.find("MedlineCitation").find("Article").find("ArticleTitle").text,
                                      get_url=lambda child: "https://pubmed.ncbi.nlm.nih.gov/" + child.find("MedlineCitation").find("PMID").text,
                                      get_content=lambda child: self._format_pubmed_content(child),)
                return self.output("formalized_content")
            except Exception as e:
                if self.check_if_canceled("PubMed processing"):
                    return

                last_e = e
                logging.exception(f"PubMed error: {e}")
                time.sleep(self._param.delay_after_error)

        if last_e:
            self.set_output("_ERROR", str(last_e))
            return f"PubMed error: {last_e}"

        assert False, self.output()

    def _format_pubmed_content(self, child):
        """Extract structured reference info from PubMed XML"""
        def safe_find(path):
            node = child
            for p in path.split("/"):
                if node is None:
                    return None
                node = node.find(p)
            return node.text if node is not None and node.text else None

        title = safe_find("MedlineCitation/Article/ArticleTitle") or "No title"
        abstract = safe_find("MedlineCitation/Article/Abstract/AbstractText") or "No abstract available"
        journal = safe_find("MedlineCitation/Article/Journal/Title") or "Unknown Journal"
        volume = safe_find("MedlineCitation/Article/Journal/JournalIssue/Volume") or "-"
        issue = safe_find("MedlineCitation/Article/Journal/JournalIssue/Issue") or "-"
        pages = safe_find("MedlineCitation/Article/Pagination/MedlinePgn") or "-"

        # Authors
        authors = []
        for author in child.findall(".//AuthorList/Author"):
            lastname = safe_find("LastName") or ""
            forename = safe_find("ForeName") or ""
            fullname = f"{forename} {lastname}".strip()
            if fullname:
                authors.append(fullname)
        authors_str = ", ".join(authors) if authors else "Unknown Authors"

        # DOI
        doi = None
        for eid in child.findall(".//ArticleId"):
            if eid.attrib.get("IdType") == "doi":
                doi = eid.text
                break

        return (
            f"Title: {title}\n"
            f"Authors: {authors_str}\n"
            f"Journal: {journal}\n"
            f"Volume: {volume}\n"
            f"Issue: {issue}\n"
            f"Pages: {pages}\n"
            f"DOI: {doi or '-'}\n"
            f"Abstract: {abstract.strip()}"
        )

    def thoughts(self) -> str:
        return "Looking for scholarly papers on `{}`,” prioritising reputable sources.".format(self.get_input().get("query", "-_-!"))

```

## Detailed Analysis

### File Role in Repository

The file `agent/tools/pubmed.py` is located in the `agent/tools` directory.

This file is part of the **Agent System** for workflow management.

### Architecture Context

Files in this location typically handle concerns related to tools.

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

- [__init__.py](__init__.py_docs.md)
- [akshare.py](akshare.py_docs.md)
- [arxiv.py](arxiv.py_docs.md)
- [base.py](base.py_docs.md)
- [code_exec.py](code_exec.py_docs.md)
- [crawler.py](crawler.py_docs.md)
- [deepl.py](deepl.py_docs.md)
- [duckduckgo.py](duckduckgo.py_docs.md)
- [email.py](email.py_docs.md)
- [exesql.py](exesql.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
