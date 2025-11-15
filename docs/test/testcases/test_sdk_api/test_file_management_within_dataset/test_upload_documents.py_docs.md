# Documentation: test/testcases/test_sdk_api/test_file_management_within_dataset/test_upload_documents.py

## File Metadata

- **Path**: `test/testcases/test_sdk_api/test_file_management_within_dataset/test_upload_documents.py`
- **Size**: 8120 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `test/testcases/test_sdk_api/test_file_management_within_dataset/test_upload_documents.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `string`
- `concurrent.futures`
- `pytest`
- `configs`
- `utils.file_utils`

### Classes Defined

This file defines 1 class(es):

#### Class: `TestDocumentsUpload` (line 24)

**Methods**: test_valid_single_upload, test_file_type_validation, test_unsupported_file_type, test_missing_file, test_empty_file, test_filename_empty, test_filename_max_length, test_duplicate_files, test_same_file_repeat, test_filename_special_characters, test_multiple_files, test_concurrent_upload

### Functions Defined

This file defines 13 function(s):

#### Function: `test_valid_single_upload` (line 26)

**Parameters**: self, add_dataset_func, tmp_path

#### Function: `test_file_type_validation` (line 54)

**Parameters**: self, add_dataset_func, generate_test_files, request

#### Function: `test_unsupported_file_type` (line 71)

**Parameters**: self, add_dataset_func, tmp_path, file_type

#### Function: `test_missing_file` (line 84)

**Parameters**: self, add_dataset_func

#### Function: `test_empty_file` (line 91)

**Parameters**: self, add_dataset_func, tmp_path

#### Function: `test_filename_empty` (line 104)

**Parameters**: self, add_dataset_func, tmp_path

#### Function: `test_filename_max_length` (line 116)

**Parameters**: self, add_dataset_func, tmp_path

#### Function: `test_duplicate_files` (line 129)

**Parameters**: self, add_dataset_func, tmp_path

#### Function: `test_same_file_repeat` (line 145)

**Parameters**: self, add_dataset_func, tmp_path

#### Function: `test_filename_special_characters` (line 161)

**Parameters**: self, add_dataset_func, tmp_path

#### Function: `test_multiple_files` (line 179)

**Parameters**: self, client, add_dataset_func, tmp_path

#### Function: `test_concurrent_upload` (line 195)

**Parameters**: self, client, add_dataset_func, tmp_path

#### Function: `upload_file` (line 200)

**Parameters**: fp

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
import string
from concurrent.futures import ThreadPoolExecutor, as_completed

import pytest
from configs import DOCUMENT_NAME_LIMIT
from utils.file_utils import create_txt_file


class TestDocumentsUpload:
    @pytest.mark.p1
    def test_valid_single_upload(self, add_dataset_func, tmp_path):
        dataset = add_dataset_func
        fp = create_txt_file(tmp_path / "ragflow_test.txt")
        with fp.open("rb") as f:
            blob = f.read()

        documents = dataset.upload_documents([{"display_name": fp.name, "blob": blob}])
        for document in documents:
            assert document.dataset_id == dataset.id, str(document)
            assert document.name == fp.name, str(document)

    @pytest.mark.p1
    @pytest.mark.parametrize(
        "generate_test_files",
        [
            "docx",
            "excel",
            "ppt",
            "image",
            "pdf",
            "txt",
            "md",
            "json",
            "eml",
            "html",
        ],
        indirect=True,
    )
    def test_file_type_validation(self, add_dataset_func, generate_test_files, request):
        dataset = add_dataset_func
        fp = generate_test_files[request.node.callspec.params["generate_test_files"]]

        with fp.open("rb") as f:
            blob = f.read()

        documents = dataset.upload_documents([{"display_name": fp.name, "blob": blob}])
        for document in documents:
            assert document.dataset_id == dataset.id, str(document)
            assert document.name == fp.name, str(document)

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "file_type",
        ["exe", "unknown"],
    )
    def test_unsupported_file_type(self, add_dataset_func, tmp_path, file_type):
        dataset = add_dataset_func
        fp = tmp_path / f"ragflow_test.{file_type}"
        fp.touch()

        with fp.open("rb") as f:
            blob = f.read()

        with pytest.raises(Exception) as excinfo:
            dataset.upload_documents([{"display_name": fp.name, "blob": blob}])
        assert str(excinfo.value) == f"ragflow_test.{file_type}: This type of file has not been supported yet!", str(excinfo.value)

    @pytest.mark.p2
    def test_missing_file(self, add_dataset_func):
        dataset = add_dataset_func
        with pytest.raises(Exception) as excinfo:
            dataset.upload_documents([])
        assert str(excinfo.value) == "No file part!", str(excinfo.value)

    @pytest.mark.p3
    def test_empty_file(self, add_dataset_func, tmp_path):
        dataset = add_dataset_func
        fp = tmp_path / "empty.txt"
        fp.touch()

        with fp.open("rb") as f:
            blob = f.read()

        documents = dataset.upload_documents([{"display_name": fp.name, "blob": blob}])
        for document in documents:
            assert document.size == 0, str(document)

    @pytest.mark.p3
    def test_filename_empty(self, add_dataset_func, tmp_path):
        dataset = add_dataset_func
        fp = create_txt_file(tmp_path / "ragflow_test.txt")

        with fp.open("rb") as f:
            blob = f.read()

        with pytest.raises(Exception) as excinfo:
            dataset.upload_documents([{"display_name": "", "blob": blob}])
        assert str(excinfo.value) == "No file selected!", str(excinfo.value)

    @pytest.mark.p2
    def test_filename_max_length(self, add_dataset_func, tmp_path):
        dataset = add_dataset_func
        fp = create_txt_file(tmp_path / f"{'a' * (DOCUMENT_NAME_LIMIT - 4)}.txt")

        with fp.open("rb") as f:
            blob = f.read()

        documents = dataset.upload_documents([{"display_name": fp.name, "blob": blob}])
        for document in documents:
            assert document.dataset_id == dataset.id, str(document)
            assert document.name == fp.name, str(document)

    @pytest.mark.p2
    def test_duplicate_files(self, add_dataset_func, tmp_path):
        dataset = add_dataset_func
        fp = create_txt_file(tmp_path / "ragflow_test.txt")

        with fp.open("rb") as f:
            blob = f.read()

        documents = dataset.upload_documents([{"display_name": fp.name, "blob": blob}, {"display_name": fp.name, "blob": blob}])

        assert len(documents) == 2, str(documents)
        for i, document in enumerate(documents):
            assert document.dataset_id == dataset.id, str(document)
            expected_name = fp.name if i == 0 else f"{fp.stem}({i}){fp.suffix}"
            assert document.name == expected_name, str(document)

    @pytest.mark.p2
    def test_same_file_repeat(self, add_dataset_func, tmp_path):
        dataset = add_dataset_func
        fp = create_txt_file(tmp_path / "ragflow_test.txt")

        with fp.open("rb") as f:
            blob = f.read()

        for i in range(3):
            documents = dataset.upload_documents([{"display_name": fp.name, "blob": blob}])
            assert len(documents) == 1, str(documents)
            document = documents[0]
            assert document.dataset_id == dataset.id, str(document)
            expected_name = fp.name if i == 0 else f"{fp.stem}({i}){fp.suffix}"
            assert document.name == expected_name, str(document)

    @pytest.mark.p3
    def test_filename_special_characters(self, add_dataset_func, tmp_path):
        dataset = add_dataset_func
        illegal_chars = '<>:"/\\|?*'
        translation_table = str.maketrans({char: "_" for char in illegal_chars})
        safe_filename = string.punctuation.translate(translation_table)
        fp = tmp_path / f"{safe_filename}.txt"
        fp.write_text("Sample text content")

        with fp.open("rb") as f:
            blob = f.read()

        documents = dataset.upload_documents([{"display_name": fp.name, "blob": blob}])
        assert len(documents) == 1, str(documents)
        document = documents[0]
        assert document.dataset_id == dataset.id, str(document)
        assert document.name == fp.name, str(document)

    @pytest.mark.p1
    def test_multiple_files(self, client, add_dataset_func, tmp_path):
        dataset = add_dataset_func
        expected_document_count = 20
        document_infos = []
        for i in range(expected_document_count):
            fp = create_txt_file(tmp_path / f"ragflow_test_upload_{i}.txt")
            with fp.open("rb") as f:
                blob = f.read()
            document_infos.append({"display_name": fp.name, "blob": blob})
        documents = dataset.upload_documents(document_infos)
        assert len(documents) == expected_document_count, str(documents)

        retrieved_dataset = client.get_dataset(name=dataset.name)
        assert retrieved_dataset.document_count == expected_document_count, str(retrieved_dataset)

    @pytest.mark.p3
    def test_concurrent_upload(self, client, add_dataset_func, tmp_path):
        dataset = add_dataset_func
        count = 20
        fps = [create_txt_file(tmp_path / f"ragflow_test_{i}.txt") for i in range(count)]

        def upload_file(fp):
            with fp.open("rb") as f:
                blob = f.read()
            return dataset.upload_documents([{"display_name": fp.name, "blob": blob}])

        with ThreadPoolExecutor(max_workers=5) as executor:
            futures = [executor.submit(upload_file, fp) for fp in fps]
        responses = list(as_completed(futures))
        assert len(responses) == count, responses

        retrieved_dataset = client.get_dataset(name=dataset.name)
        assert retrieved_dataset.document_count == count, str(retrieved_dataset)

```

## Detailed Analysis

### File Role in Repository

The file `test/testcases/test_sdk_api/test_file_management_within_dataset/test_upload_documents.py` is located in the `test/testcases/test_sdk_api/test_file_management_within_dataset` directory.

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
- [test_delete_documents.py](test_delete_documents.py_docs.md)
- [test_download_document.py](test_download_document.py_docs.md)
- [test_list_documents.py](test_list_documents.py_docs.md)
- [test_parse_documents.py](test_parse_documents.py_docs.md)
- [test_stop_parse_documents.py](test_stop_parse_documents.py_docs.md)
- [test_update_document.py](test_update_document.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
