# Documentation: test/testcases/test_sdk_api/test_chunk_management_within_dataset/test_delete_chunks.py

## File Metadata

- **Path**: `test/testcases/test_sdk_api/test_chunk_management_within_dataset/test_delete_chunks.py`
- **Size**: 4582 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_sdk_api/test_chunk_management_within_dataset/test_delete_chunks.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `concurrent.futures`
- `pytest`
- `common`
- `time`

### Classes Defined

This file defines 1 class(es):

#### Class: `TestChunksDeletion` (line 22)

**Methods**: test_delete_partial_invalid_id, test_repeated_deletion, test_duplicate_deletion, test_concurrent_deletion, test_delete_1k, test_basic_scenarios

### Functions Defined

This file defines 6 function(s):

#### Function: `test_delete_partial_invalid_id` (line 31)

**Parameters**: self, add_chunks_func, payload

#### Function: `test_repeated_deletion` (line 44)

**Parameters**: self, add_chunks_func

#### Function: `test_duplicate_deletion` (line 54)

**Parameters**: self, add_chunks_func

#### Function: `test_concurrent_deletion` (line 62)

**Parameters**: self, add_document

#### Function: `test_delete_1k` (line 74)

**Parameters**: self, add_document

#### Function: `test_basic_scenarios` (line 99)

**Parameters**: self, add_chunks_func, payload, expected_message, remaining

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
from concurrent.futures import ThreadPoolExecutor, as_completed

import pytest
from common import batch_add_chunks


class TestChunksDeletion:
    @pytest.mark.parametrize(
        "payload",
        [
            pytest.param(lambda r: {"ids": ["invalid_id"] + r}, marks=pytest.mark.p3),
            pytest.param(lambda r: {"ids": r[:1] + ["invalid_id"] + r[1:4]}, marks=pytest.mark.p1),
            pytest.param(lambda r: {"ids": r + ["invalid_id"]}, marks=pytest.mark.p3),
        ],
    )
    def test_delete_partial_invalid_id(self, add_chunks_func, payload):
        _, document, chunks = add_chunks_func
        chunk_ids = [chunk.id for chunk in chunks]
        payload = payload(chunk_ids)

        with pytest.raises(Exception) as excinfo:
            document.delete_chunks(**payload)
        assert "rm_chunk deleted chunks" in str(excinfo.value), str(excinfo.value)

        remaining_chunks = document.list_chunks()
        assert len(remaining_chunks) == 1, str(remaining_chunks)

    @pytest.mark.p3
    def test_repeated_deletion(self, add_chunks_func):
        _, document, chunks = add_chunks_func
        chunk_ids = [chunk.id for chunk in chunks]
        document.delete_chunks(ids=chunk_ids)

        with pytest.raises(Exception) as excinfo:
            document.delete_chunks(ids=chunk_ids)
        assert "rm_chunk deleted chunks 0, expect" in str(excinfo.value), str(excinfo.value)

    @pytest.mark.p3
    def test_duplicate_deletion(self, add_chunks_func):
        _, document, chunks = add_chunks_func
        chunk_ids = [chunk.id for chunk in chunks]
        document.delete_chunks(ids=chunk_ids * 2)
        remaining_chunks = document.list_chunks()
        assert len(remaining_chunks) == 1, str(remaining_chunks)

    @pytest.mark.p3
    def test_concurrent_deletion(self, add_document):
        count = 100
        _, document = add_document
        chunks = batch_add_chunks(document, count)
        chunk_ids = [chunk.id for chunk in chunks]

        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [executor.submit(document.delete_chunks, ids=[chunk_id]) for chunk_id in chunk_ids]
        responses = list(as_completed(futures))
        assert len(responses) == count, responses

    @pytest.mark.p3
    def test_delete_1k(self, add_document):
        count = 1_000
        _, document = add_document
        chunks = batch_add_chunks(document, count)
        chunk_ids = [chunk.id for chunk in chunks]

        from time import sleep

        sleep(1)

        document.delete_chunks(ids=chunk_ids)
        remaining_chunks = document.list_chunks()
        assert len(remaining_chunks) == 0, str(remaining_chunks)

    @pytest.mark.parametrize(
        "payload, expected_message, remaining",
        [
            pytest.param(None, "TypeError", 5, marks=pytest.mark.skip),
            pytest.param({"ids": ["invalid_id"]}, "rm_chunk deleted chunks 0, expect 1", 5, marks=pytest.mark.p3),
            pytest.param("not json", "UnboundLocalError", 5, marks=pytest.mark.skip(reason="pull/6376")),
            pytest.param(lambda r: {"ids": r[:1]}, "", 4, marks=pytest.mark.p3),
            pytest.param(lambda r: {"ids": r}, "", 1, marks=pytest.mark.p1),
            pytest.param({"ids": []}, "", 0, marks=pytest.mark.p3),
        ],
    )
    def test_basic_scenarios(self, add_chunks_func, payload, expected_message, remaining):
        _, document, chunks = add_chunks_func
        chunk_ids = [chunk.id for chunk in chunks]
        if callable(payload):
            payload = payload(chunk_ids)

        if expected_message:
            with pytest.raises(Exception) as excinfo:
                document.delete_chunks(**payload)
            assert expected_message in str(excinfo.value), str(excinfo.value)
        else:
            document.delete_chunks(**payload)

        remaining_chunks = document.list_chunks()
        assert len(remaining_chunks) == remaining, str(remaining_chunks)

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_sdk_api/test_chunk_management_within_dataset/test_delete_chunks.py` is located in the `test/testcases/test_sdk_api/test_chunk_management_within_dataset` directory.

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
- [test_list_chunks.py](test_list_chunks.py_docs.md)
- [test_retrieval_chunks.py](test_retrieval_chunks.py_docs.md)
- [test_update_chunk.py](test_update_chunk.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
