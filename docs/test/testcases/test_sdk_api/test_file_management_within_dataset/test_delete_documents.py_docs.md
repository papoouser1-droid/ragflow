# Documentation: test/testcases/test_sdk_api/test_file_management_within_dataset/test_delete_documents.py

## File Metadata

- **Path**: `test/testcases/test_sdk_api/test_file_management_within_dataset/test_delete_documents.py`
- **Size**: 4296 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_sdk_api/test_file_management_within_dataset/test_delete_documents.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `concurrent.futures`
- `pytest`
- `common`

### Classes Defined

This file defines 1 class(es):

#### Class: `TestDocumentsDeletion` (line 22)

**Methods**: test_basic_scenarios, test_delete_partial_invalid_id, test_repeated_deletion, test_duplicate_deletion

### Functions Defined

This file defines 7 function(s):

#### Function: `test_concurrent_deletion` (line 95)

**Parameters**: add_dataset, tmp_path

#### Function: `test_delete_1k` (line 111)

**Parameters**: add_dataset, tmp_path

#### Function: `test_basic_scenarios` (line 36)

**Parameters**: self, add_documents_func, payload, expected_message, remaining

#### Function: `test_delete_partial_invalid_id` (line 66)

**Parameters**: self, add_documents_func, payload

#### Function: `test_repeated_deletion` (line 78)

**Parameters**: self, add_documents_func

#### Function: `test_duplicate_deletion` (line 87)

**Parameters**: self, add_documents_func

#### Function: `delete_doc` (line 100)

**Parameters**: doc_id

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

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_sdk_api/test_file_management_within_dataset/test_delete_documents.py` is located in the `test/testcases/test_sdk_api/test_file_management_within_dataset` directory.

This file is part of the **Testing** infrastructure.

### Architecture Context

Files in this location typically handle concerns related to test_file_management_within_dataset.

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
- [test_download_document.py](test_download_document.py_docs.md)
- [test_list_documents.py](test_list_documents.py_docs.md)
- [test_parse_documents.py](test_parse_documents.py_docs.md)
- [test_stop_parse_documents.py](test_stop_parse_documents.py_docs.md)
- [test_update_document.py](test_update_document.py_docs.md)
- [test_upload_documents.py](test_upload_documents.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
