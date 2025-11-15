# Documentation: test/testcases/test_sdk_api/test_chunk_management_within_dataset/test_retrieval_chunks.py

## File Metadata

- **Path**: `test/testcases/test_sdk_api/test_chunk_management_within_dataset/test_retrieval_chunks.py`
- **Size**: 10136 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_sdk_api/test_chunk_management_within_dataset/test_retrieval_chunks.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `os`
- `concurrent.futures`
- `pytest`

### Classes Defined

This file defines 1 class(es):

#### Class: `TestChunksRetrieval` (line 22)

**Methods**: test_basic_scenarios, test_page, test_page_size, test_vector_similarity_weight, test_top_k, test_rerank_id, test_keyword, test_concurrent_retrieval

### Functions Defined

This file defines 8 function(s):

#### Function: `test_basic_scenarios` (line 33)

**Parameters**: self, client, add_chunks, payload, expected_page_size, expected_message

#### Function: `test_page` (line 81)

**Parameters**: self, client, add_chunks, payload, expected_page_size, expected_message

#### Function: `test_page_size` (line 114)

**Parameters**: self, client, add_chunks, payload, expected_page_size, expected_message

#### Function: `test_vector_similarity_weight` (line 141)

**Parameters**: self, client, add_chunks, payload, expected_page_size, expected_message

#### Function: `test_top_k` (line 190)

**Parameters**: self, client, add_chunks, payload, expected_page_size, expected_message

#### Function: `test_rerank_id` (line 210)

**Parameters**: self, client, add_chunks, payload, expected_message

#### Function: `test_keyword` (line 233)

**Parameters**: self, client, add_chunks, payload, expected_page_size, expected_message

#### Function: `test_concurrent_retrieval` (line 246)

**Parameters**: self, client, add_chunks

## Original Source Code

```py
#
#  Copyright 2025 The InfiniFlow Authors. All Rights Reserved.
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
import os
from concurrent.futures import ThreadPoolExecutor, as_completed

import pytest


class TestChunksRetrieval:
    @pytest.mark.p1
    @pytest.mark.parametrize(
        "payload, expected_page_size, expected_message",
        [
            ({"question": "chunk", "dataset_ids": None}, 4, ""),
            ({"question": "chunk", "document_ids": None}, 0, "missing 1 required positional argument"),
            ({"question": "chunk", "dataset_ids": None, "document_ids": None}, 4, ""),
            ({"question": "chunk"}, 0, "missing 1 required positional argument"),
        ],
    )
    def test_basic_scenarios(self, client, add_chunks, payload, expected_page_size, expected_message):
        dataset, document, _ = add_chunks
        if "dataset_ids" in payload:
            payload["dataset_ids"] = [dataset.id]
        if "document_ids" in payload:
            payload["document_ids"] = [document.id]

        if expected_message:
            with pytest.raises(Exception) as excinfo:
                client.retrieve(**payload)
            assert expected_message in str(excinfo.value), str(excinfo.value)
        else:
            chunks = client.retrieve(**payload)
            assert len(chunks) == expected_page_size, str(chunks)

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "payload, expected_page_size, expected_message",
        [
            pytest.param(
                {"page": None, "page_size": 2},
                2,
                """TypeError("int() argument must be a string, a bytes-like object or a real number, not \'NoneType\'")""",
                marks=pytest.mark.skip,
            ),
            pytest.param(
                {"page": 0, "page_size": 2},
                0,
                "ValueError('Search does not support negative slicing.')",
                marks=pytest.mark.skip,
            ),
            ({"page": 2, "page_size": 2}, 2, ""),
            ({"page": 3, "page_size": 2}, 0, ""),
            ({"page": "3", "page_size": 2}, 0, ""),
            pytest.param(
                {"page": -1, "page_size": 2},
                0,
                "ValueError('Search does not support negative slicing.')",
                marks=pytest.mark.skip,
            ),
            pytest.param(
                {"page": "a", "page_size": 2},
                0,
                """ValueError("invalid literal for int() with base 10: \'a\'")""",
                marks=pytest.mark.skip,
            ),
        ],
    )
    def test_page(self, client, add_chunks, payload, expected_page_size, expected_message):
        dataset, _, _ = add_chunks
        payload.update({"question": "chunk", "dataset_ids": [dataset.id]})

        if expected_message:
            with pytest.raises(Exception) as excinfo:
                client.retrieve(**payload)
            assert expected_message in str(excinfo.value), str(excinfo.value)
        else:
            chunks = client.retrieve(**payload)
            assert len(chunks) == expected_page_size, str(chunks)

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "payload, expected_page_size, expected_message",
        [
            pytest.param(
                {"page_size": None},
                0,
                """TypeError("int() argument must be a string, a bytes-like object or a real number, not \'NoneType\'")""",
                marks=pytest.mark.skip,
            ),
            pytest.param({"page_size": 1}, 1, "", marks=pytest.mark.skip(reason="issues/10692")),
            ({"page_size": 5}, 4, ""),
            pytest.param({"page_size": "1"}, 1, "", marks=pytest.mark.skip(reason="issues/10692")),
            pytest.param(
                {"page_size": "a"},
                0,
                """ValueError("invalid literal for int() with base 10: \'a\'")""",
                marks=pytest.mark.skip,
            ),
        ],
    )
    def test_page_size(self, client, add_chunks, payload, expected_page_size, expected_message):
        dataset, _, _ = add_chunks
        payload.update({"question": "chunk", "dataset_ids": [dataset.id]})

        if expected_message:
            with pytest.raises(Exception) as excinfo:
                client.retrieve(**payload)
            assert expected_message in str(excinfo.value), str(excinfo.value)
        else:
            chunks = client.retrieve(**payload)
            assert len(chunks) == expected_page_size, str(chunks)

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "payload, expected_page_size, expected_message",
        [
            ({"vector_similarity_weight": 0}, 4, ""),
            ({"vector_similarity_weight": 0.5}, 4, ""),
            ({"vector_similarity_weight": 10}, 4, ""),
            pytest.param(
                {"vector_similarity_weight": "a"},
                0,
                """ValueError("could not convert string to float: 'a'")""",
                marks=pytest.mark.skip,
            ),
        ],
    )
    def test_vector_similarity_weight(self, client, add_chunks, payload, expected_page_size, expected_message):
        dataset, _, _ = add_chunks
        payload.update({"question": "chunk", "dataset_ids": [dataset.id]})

        if expected_message:
            with pytest.raises(Exception) as excinfo:
                client.retrieve(**payload)
            assert expected_message in str(excinfo.value), str(excinfo.value)
        else:
            chunks = client.retrieve(**payload)
            assert len(chunks) == expected_page_size, str(chunks)

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "payload, expected_page_size, expected_message",
        [
            ({"top_k": 10}, 4, ""),
            pytest.param(
                {"top_k": 1},
                4,
                "",
                marks=pytest.mark.skipif(os.getenv("DOC_ENGINE") in ["infinity", "opensearch"], reason="Infinity"),
            ),
            pytest.param(
                {"top_k": 1},
                1,
                "",
                marks=pytest.mark.skipif(os.getenv("DOC_ENGINE") in [None, "opensearch", "elasticsearch"], reason="elasticsearch"),
            ),
            pytest.param(
                {"top_k": -1},
                4,
                "must be greater than 0",
                marks=pytest.mark.skipif(os.getenv("DOC_ENGINE") in ["infinity", "opensearch"], reason="Infinity"),
            ),
            pytest.param(
                {"top_k": -1},
                4,
                "3014",
                marks=pytest.mark.skipif(os.getenv("DOC_ENGINE") in [None, "opensearch", "elasticsearch"], reason="elasticsearch"),
            ),
            pytest.param(
                {"top_k": "a"},
                0,
                """ValueError("invalid literal for int() with base 10: \'a\'")""",
                marks=pytest.mark.skip,
            ),
        ],
    )
    def test_top_k(self, client, add_chunks, payload, expected_page_size, expected_message):
        dataset, _, _ = add_chunks
        payload.update({"question": "chunk", "dataset_ids": [dataset.id]})

        if expected_message:
            with pytest.raises(Exception) as excinfo:
                client.retrieve(**payload)
            assert expected_message in str(excinfo.value), str(excinfo.value)
        else:
            chunks = client.retrieve(**payload)
            assert len(chunks) == expected_page_size, str(chunks)

    @pytest.mark.skip
    @pytest.mark.parametrize(
        "payload, expected_message",
        [
            ({"rerank_id": "BAAI/bge-reranker-v2-m3"}, ""),
            pytest.param({"rerank_id": "unknown"}, "LookupError('Model(unknown) not authorized')", marks=pytest.mark.skip),
        ],
    )
    def test_rerank_id(self, client, add_chunks, payload, expected_message):
        dataset, _, _ = add_chunks
        payload.update({"question": "chunk", "dataset_ids": [dataset.id]})

        if expected_message:
            with pytest.raises(Exception) as excinfo:
                client.retrieve(**payload)
            assert expected_message in str(excinfo.value), str(excinfo.value)
        else:
            chunks = client.retrieve(**payload)
            assert len(chunks) > 0, str(chunks)

    @pytest.mark.skip
    @pytest.mark.parametrize(
        "payload, expected_page_size, expected_message",
        [
            ({"keyword": True}, 5, ""),
            ({"keyword": "True"}, 5, ""),
            ({"keyword": False}, 5, ""),
            ({"keyword": "False"}, 5, ""),
            ({"keyword": None}, 5, ""),
        ],
    )
    def test_keyword(self, client, add_chunks, payload, expected_page_size, expected_message):
        dataset, _, _ = add_chunks
        payload.update({"question": "chunk test", "dataset_ids": [dataset.id]})

        if expected_message:
            with pytest.raises(Exception) as excinfo:
                client.retrieve(**payload)
            assert expected_message in str(excinfo.value), str(excinfo.value)
        else:
            chunks = client.retrieve(**payload)
            assert len(chunks) == expected_page_size, str(chunks)

    @pytest.mark.p3
    def test_concurrent_retrieval(self, client, add_chunks):
        dataset, _, _ = add_chunks
        count = 100
        payload = {"question": "chunk", "dataset_ids": [dataset.id]}

        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [executor.submit(client.retrieve, **payload) for _ in range(count)]
        responses = list(as_completed(futures))
        assert len(responses) == count, responses

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_sdk_api/test_chunk_management_within_dataset/test_retrieval_chunks.py` is located in the `test/testcases/test_sdk_api/test_chunk_management_within_dataset` directory.

This file is part of the **Testing** infrastructure.

### Architecture Context

Files in this location typically handle concerns related to test_chunk_management_within_dataset.

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

- [conftest.py](conftest.py_docs.md)
- [test_add_chunk.py](test_add_chunk.py_docs.md)
- [test_delete_chunks.py](test_delete_chunks.py_docs.md)
- [test_list_chunks.py](test_list_chunks.py_docs.md)
- [test_update_chunk.py](test_update_chunk.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
