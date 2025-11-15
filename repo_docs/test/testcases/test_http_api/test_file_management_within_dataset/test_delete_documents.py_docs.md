# File Documentation: test/testcases/test_http_api/test_file_management_within_dataset/test_delete_documents.py

## File Metadata

- **Path**: `test/testcases/test_http_api/test_file_management_within_dataset/test_delete_documents.py`
- **Extension**: `.py`
- **Lines**: 184
- **Characters**: 6,527
- **Size**: 6,543 bytes
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
from common import bulk_upload_documents, delete_documents, list_documents
from configs import INVALID_API_TOKEN
from libs.auth import RAGFlowHttpApiAuth


@pytest.mark.p1
class TestAuthorization:
    @pytest.mark.parametrize(
        "invalid_auth, expected_code, expected_message",
        [
            (None, 0, "`Authorization` can't be empty"),
            (
                RAGFlowHttpApiAuth(INVALID_API_TOKEN),
                109,
                "Authentication error: API key is invalid!",
            ),
        ],
    )
    def test_invalid_auth(self, invalid_auth, expected_code, expected_message):
        res = delete_documents(invalid_auth, "dataset_id")
        assert res["code"] == expected_code
        assert res["message"] == expected_message


class TestDocumentsDeletion:
    @pytest.mark.p1
    @pytest.mark.parametrize(
        "payload, expected_code, expected_message, remaining",
        [
            (None, 0, "", 0),
            ({"ids": []}, 0, "", 0),
            ({"ids": ["invalid_id"]}, 102, "Documents not found: ['invalid_id']", 3),
            (
                {"ids": ["\n!?。；！？\"'"]},
                102,
                """Documents not found: [\'\\n!?。；！？"\\\'\']""",
                3,
            ),
            (
                "not json",
                100,
                "AttributeError(\"'str' object has no attribute 'get'\")",
                3,
            ),
            (lambda r: {"ids": r[:1]}, 0, "", 2),
            (lambda r: {"ids": r}, 0, "", 0),
        ],
    )
    def test_basic_scenarios(
        self,
        HttpApiAuth,
        add_documents_func,
        payload,
        expected_code,
        expected_message,
        remaining,
    ):
        dataset_id, document_ids = add_documents_func
        if callable(payload):
            payload = payload(document_ids)
        res = delete_documents(HttpApiAuth, dataset_id, payload)
        assert res["code"] == expected_code
        if res["code"] != 0:
            assert res["message"] == expected_message

        res = list_documents(HttpApiAuth, dataset_id)
        assert len(res["data"]["docs"]) == remaining
        assert res["data"]["total"] == remaining

    @pytest.mark.p3
    @pytest.mark.parametrize(
        "dataset_id, expected_code, expected_message",
        [
            ("", 100, "<MethodNotAllowed '405: Method Not Allowed'>"),
            (
                "invalid_dataset_id",
                102,
                "You don't own the dataset invalid_dataset_id. ",
            ),
        ],
    )
    def test_invalid_dataset_id(self, HttpApiAuth, add_documents_func, dataset_id, expected_code, expected_message):
        _, document_ids = add_documents_func
        res = delete_documents(HttpApiAuth, dataset_id, {"ids": document_ids[:1]})
        assert res["code"] == expected_code
        assert res["message"] == expected_message

    @pytest.mark.p2
    @pytest.mark.parametrize(
        "payload",
        [
            lambda r: {"ids": ["invalid_id"] + r},
            lambda r: {"ids": r[:1] + ["invalid_id"] + r[1:3]},
            lambda r: {"ids": r + ["invalid_id"]},
        ],
    )
    def test_delete_partial_invalid_id(self, HttpApiAuth, add_documents_func, payload):
        dataset_id, document_ids = add_documents_func
        if callable(payload):
            payload = payload(document_ids)
        res = delete_documents(HttpApiAuth, dataset_id, payload)
        assert res["code"] == 102
        assert res["message"] == "Documents not found: ['invalid_id']"

        res = list_documents(HttpApiAuth, dataset_id)
        assert len(res["data"]["docs"]) == 0
        assert res["data"]["total"] == 0

    @pytest.mark.p2
    def test_repeated_deletion(self, HttpApiAuth, add_documents_func):
        dataset_id, document_ids = add_documents_func
        res = delete_documents(HttpApiAuth, dataset_id, {"ids": document_ids})
        assert res["code"] == 0

        res = delete_documents(HttpApiAuth, dataset_id, {"ids": document_ids})
        assert res["code"] == 102
        assert "Documents not found" in res["message"]

    @pytest.mark.p2
    def test_duplicate_deletion(self, HttpApiAuth, add_documents_func):
        dataset_id, document_ids = add_documents_func
        res = delete_documents(HttpApiAuth, dataset_id, {"ids": document_ids + document_ids})
        assert res["code"] == 0
        assert "Duplicate document ids" in res["data"]["errors"][0]
        assert res["data"]["success_count"] == 3

        res = list_documents(HttpApiAuth, dataset_id)
        assert len(res["data"]["docs"]) == 0
        assert res["data"]["total"] == 0


@pytest.mark.p3
def test_concurrent_deletion(HttpApiAuth, add_dataset, tmp_path):
    count = 100
    dataset_id = add_dataset
    document_ids = bulk_upload_documents(HttpApiAuth, dataset_id, count, tmp_path)

    with ThreadPoolExecutor(max_workers=5) as executor:
        futures = [
            executor.submit(
                delete_documents,
                HttpApiAuth,
                dataset_id,
                {"ids": document_ids[i : i + 1]},
            )
            for i in range(count)
        ]
    responses = list(as_completed(futures))
    assert len(responses) == count, responses
    assert all(future.result()["code"] == 0 for future in futures)


@pytest.mark.p3
def test_delete_1k(HttpApiAuth, add_dataset, tmp_path):
    documents_num = 1_000
    dataset_id = add_dataset
    document_ids = bulk_upload_documents(HttpApiAuth, dataset_id, documents_num, tmp_path)
    res = list_documents(HttpApiAuth, dataset_id)
    assert res["data"]["total"] == documents_num

    res = delete_documents(HttpApiAuth, dataset_id, {"ids": document_ids})
    assert res["code"] == 0

    res = list_documents(HttpApiAuth, dataset_id)
    assert res["data"]["total"] == 0

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

### Classes (2)

- `TestAuthorization`: Class definition
- `TestDocumentsDeletion`: Class definition

### Functions (2)

- `test_concurrent_deletion()`: Function definition
- `test_delete_1k()`: Function definition

### Imports (5)

- `from concurrent.futures import ThreadPoolExecutor, as_completed`
- `import pytest`
- `from common import bulk_upload_documents, delete_documents, list_documents`
- `from configs import INVALID_API_TOKEN`
- `from libs.auth import RAGFlowHttpApiAuth`

## Code Structure Analysis

- Total lines: 184
- Blank lines: 21 (11.4%)
- Comment lines: ~16 (8.7%)
- Code lines: ~147


## Dependencies and Imports

- `from concurrent.futures import ThreadPoolExecutor, as_completed`
- `import pytest`
- `from common import bulk_upload_documents, delete_documents, list_documents`
- `from configs import INVALID_API_TOKEN`
- `from libs.auth import RAGFlowHttpApiAuth`

## Design & Architecture

This file is located in the `test` directory, specifically within `test/testcases/test_http_api/test_file_management_within_dataset`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains 3 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `test/testcases/test_http_api/test_file_management_within_dataset/` directory
- Imports from `concurrent.futures`
- Imports from `common`
- Imports from `configs`
- Imports from `libs.auth`

## Keywords

ANY, API, All, Allowed, Apache, AttributeError, Authentication, Authorization, Authors, BASIS, CONDITIONS, Copyright, Documents, Duplicate, HttpApiAuth, INVALID_API_TOKEN, InfiniFlow, KIND, LICENSE, License, Licensed, Method, MethodNotAllowed, None, Not, Python, RAGFlowHttpApiAuth, Reserved, Rights, See, TestAuthorization, TestDocumentsDeletion, The, ThreadPoolExecutor, Unless, Version, WARRANTIES, WITHOUT, You, pytest, test_basic_scenarios, test_concurrent_deletion, test_delete_1k, test_delete_partial_invalid_id, test_duplicate_deletion, test_invalid_auth, test_invalid_dataset_id, test_repeated_deletion

---
*Generated by RAGFlow Repository Documentation Generator*
