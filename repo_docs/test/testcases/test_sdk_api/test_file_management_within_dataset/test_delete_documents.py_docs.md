# File Documentation: test/testcases/test_sdk_api/test_file_management_within_dataset/test_delete_documents.py

## File Metadata

- **Path**: `test/testcases/test_sdk_api/test_file_management_within_dataset/test_delete_documents.py`
- **Extension**: `.py`
- **Lines**: 119
- **Characters**: 4,280
- **Size**: 4,296 bytes
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
from common import bulk_upload_documents


class TestDocumentsDeletion:
    @pytest.mark.p1
    @pytest.mark.parametrize(
        "payload, expected_message, remaining",
        [
            ({"ids": None}, "", 0),
            ({"ids": []}, "", 0),
            ({"ids": ["invalid_id"]}, "Documents not found: ['invalid_id']", 3),
            ({"ids": ["\n!?。；！？\"'"]}, "Documents not found: ['\\n!?。；！？\"\\'']", 3),
            ("not json", "must be a mapping", 3),
            (lambda r: {"ids": r[:1]}, "", 2),
            (lambda r: {"ids": r}, "", 0),
        ],
    )
    def test_basic_scenarios(
        self,
        add_documents_func,
        payload,
        expected_message,
        remaining,
    ):
        dataset, documents = add_documents_func
        if callable(payload):
            payload = payload([document.id for document in documents])

        if expected_message:
            with pytest.raises(Exception) as excinfo:
                dataset.delete_documents(**payload)
            assert expected_message in str(excinfo.value), str(excinfo.value)
        else:
            dataset.delete_documents(**payload)

        documents = dataset.list_documents()
        assert len(documents) == remaining, str(documents)

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "payload",
        [
            lambda r: {"ids": ["invalid_id"] + r},
            lambda r: {"ids": r[:1] + ["invalid_id"] + r[1:3]},
            lambda r: {"ids": r + ["invalid_id"]},
        ],
    )
    def test_delete_partial_invalid_id(self, add_documents_func, payload):
        dataset, documents = add_documents_func
        payload = payload([document.id for document in documents])

        with pytest.raises(Exception) as excinfo:
            dataset.delete_documents(**payload)
        assert "Documents not found: ['invalid_id']" in str(excinfo.value), str(excinfo.value)

        documents = dataset.list_documents()
        assert len(documents) == 0, str(documents)

    @pytest.mark.p2
    def test_repeated_deletion(self, add_documents_func):
        dataset, documents = add_documents_func
        document_ids = [document.id for document in documents]
        dataset.delete_documents(ids=document_ids)
        with pytest.raises(Exception) as excinfo:
            dataset.delete_documents(ids=document_ids)
        assert "Documents not found" in str(excinfo.value), str(excinfo.value)

    @pytest.mark.p2
    def test_duplicate_deletion(self, add_documents_func):
        dataset, documents = add_documents_func
        document_ids = [document.id for document in documents]
        dataset.delete_documents(ids=document_ids + document_ids)
        assert len(dataset.list_documents()) == 0, str(dataset.list_documents())


@pytest.mark.p3
def test_concurrent_deletion(add_dataset, tmp_path):
    count = 100
    dataset = add_dataset
    documents = bulk_upload_documents(dataset, count, tmp_path)

    def delete_doc(doc_id):
        dataset.delete_documents(ids=[doc_id])

    with ThreadPoolExecutor(max_workers=5) as executor:
        futures = [executor.submit(delete_doc, doc.id) for doc in documents]

    responses = list(as_completed(futures))
    assert len(responses) == count, responses


@pytest.mark.p3
def test_delete_1k(add_dataset, tmp_path):
    count = 1_000
    dataset = add_dataset
    documents = bulk_upload_documents(dataset, count, tmp_path)
    assert len(dataset.list_documents(page_size=count * 2)) == count

    dataset.delete_documents(ids=[doc.id for doc in documents])
    assert len(dataset.list_documents()) == 0

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

- `TestDocumentsDeletion`: Class definition

### Functions (2)

- `test_concurrent_deletion()`: Function definition
- `test_delete_1k()`: Function definition

### Imports (3)

- `from concurrent.futures import ThreadPoolExecutor, as_completed`
- `import pytest`
- `from common import bulk_upload_documents`

## Code Structure Analysis

- Total lines: 119
- Blank lines: 19 (16.0%)
- Comment lines: ~15 (12.6%)
- Code lines: ~85


## Dependencies and Imports

- `from concurrent.futures import ThreadPoolExecutor, as_completed`
- `import pytest`
- `from common import bulk_upload_documents`

## Design & Architecture

This file is located in the `test` directory, specifically within `test/testcases/test_sdk_api/test_file_management_within_dataset`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 7 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `test/testcases/test_sdk_api/test_file_management_within_dataset/` directory
- Imports from `concurrent.futures`
- Imports from `common`

## Keywords

ANY, All, Apache, Authors, BASIS, CONDITIONS, Copyright, Documents, Exception, InfiniFlow, KIND, LICENSE, License, Licensed, None, Python, Reserved, Rights, See, TestDocumentsDeletion, The, ThreadPoolExecutor, Unless, Version, WARRANTIES, WITHOUT, You, delete_doc, pytest, test_basic_scenarios, test_concurrent_deletion, test_delete_1k, test_delete_partial_invalid_id, test_duplicate_deletion, test_repeated_deletion

---
*Generated by RAGFlow Repository Documentation Generator*
