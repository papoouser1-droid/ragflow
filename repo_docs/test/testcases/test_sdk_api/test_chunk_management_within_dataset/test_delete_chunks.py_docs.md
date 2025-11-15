# File Documentation: test/testcases/test_sdk_api/test_chunk_management_within_dataset/test_delete_chunks.py

## File Metadata

- **Path**: `test/testcases/test_sdk_api/test_chunk_management_within_dataset/test_delete_chunks.py`
- **Extension**: `.py`
- **Lines**: 114
- **Characters**: 4,582
- **Size**: 4,582 bytes
- **Purpose**: Testing - Contains unit tests, integration tests, or test utilities

## Original Source

```python
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

## High-Level Overview

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

## Detailed Walkthrough

### Classes (1)

- `TestChunksDeletion`: Class definition

### Imports (3)

- `from concurrent.futures import ThreadPoolExecutor, as_completed`
- `import pytest`
- `from common import batch_add_chunks`

## Code Structure Analysis

- Total lines: 114
- Blank lines: 18 (15.8%)
- Comment lines: ~15 (13.2%)
- Code lines: ~81


## Dependencies and Imports

- `from concurrent.futures import ThreadPoolExecutor, as_completed`
- `import pytest`
- `from common import batch_add_chunks`

## Design & Architecture

This file is located in the `test` directory, specifically within `test/testcases/test_sdk_api/test_chunk_management_within_dataset`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 8 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `test/testcases/test_sdk_api/test_chunk_management_within_dataset/` directory
- Imports from `concurrent.futures`
- Imports from `common`
- Imports from `time`

## Keywords

ANY, All, Apache, Authors, BASIS, CONDITIONS, Copyright, Exception, InfiniFlow, KIND, LICENSE, License, Licensed, None, Python, Reserved, Rights, See, TestChunksDeletion, The, ThreadPoolExecutor, TypeError, UnboundLocalError, Unless, Version, WARRANTIES, WITHOUT, You, pytest, test_basic_scenarios, test_concurrent_deletion, test_delete_1k, test_delete_partial_invalid_id, test_duplicate_deletion, test_repeated_deletion

---
*Generated by RAGFlow Repository Documentation Generator*
