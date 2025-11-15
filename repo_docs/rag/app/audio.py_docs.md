# File Documentation: rag/app/audio.py

## File Metadata

- **Path**: `rag/app/audio.py`
- **Extension**: `.py`
- **Lines**: 62
- **Characters**: 2,286
- **Size**: 2,286 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

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

import os
import re
import tempfile

from common.constants import LLMType
from api.db.services.llm_service import LLMBundle
from rag.nlp import rag_tokenizer, tokenize


def chunk(filename, binary, tenant_id, lang, callback=None, **kwargs):
    doc = {"docnm_kwd": filename, "title_tks": rag_tokenizer.tokenize(re.sub(r"\.[a-zA-Z]+$", "", filename))}
    doc["title_sm_tks"] = rag_tokenizer.fine_grained_tokenize(doc["title_tks"])

    # is it English
    eng = lang.lower() == "english"  # is_english(sections)
    try:
        _, ext = os.path.splitext(filename)
        if not ext:
            raise RuntimeError("No extension detected.")

        if ext not in [".da", ".wave", ".wav", ".mp3", ".wav", ".aac", ".flac", ".ogg", ".aiff", ".au", ".midi", ".wma", ".realaudio", ".vqf", ".oggvorbis", ".aac", ".ape"]:
            raise RuntimeError(f"Extension {ext} is not supported yet.")

        tmp_path = ""
        with tempfile.NamedTemporaryFile(suffix=ext, delete=False) as tmpf:
            tmpf.write(binary)
            tmpf.flush()
            tmp_path = os.path.abspath(tmpf.name)

        callback(0.1, "USE Sequence2Txt LLM to transcription the audio")
        seq2txt_mdl = LLMBundle(tenant_id, LLMType.SPEECH2TEXT, lang=lang)
        ans = seq2txt_mdl.transcription(tmp_path)
        callback(0.8, "Sequence2Txt LLM respond: %s ..." % ans[:32])

        tokenize(doc, ans, eng)
        return [doc]
    except Exception as e:
        callback(prog=-1, msg=str(e))
    finally:
        if tmp_path and os.path.exists(tmp_path):
            try:
                os.unlink(tmp_path)
            except Exception:
                pass
    return []

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


### Functions (1)

- `chunk()`: Function definition

### Imports (6)

- `import os`
- `import re`
- `import tempfile`
- `from common.constants import LLMType`
- `from api.db.services.llm_service import LLMBundle`
- `from rag.nlp import rag_tokenizer, tokenize`

## Code Structure Analysis

- Total lines: 62
- Blank lines: 10 (16.1%)
- Comment lines: ~16 (25.8%)
- Code lines: ~36


## Dependencies and Imports

- `import os`
- `import re`
- `import tempfile`
- `from common.constants import LLMType`
- `from api.db.services.llm_service import LLMBundle`
- `from rag.nlp import rag_tokenizer, tokenize`

## Design & Architecture

This file is located in the `rag` directory, specifically within `rag/app`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `rag/app/` directory
- Imports from `common.constants`
- Imports from `api.db.services.llm_service`
- Imports from `rag.nlp`
- Potential test file: `test_audio.py`

## Keywords

ANY, All, Apache, Authors, BASIS, CONDITIONS, Copyright, English, Exception, Extension, False, InfiniFlow, KIND, LICENSE, LLM, LLMBundle, LLMType, License, Licensed, NamedTemporaryFile, None, Python, Reserved, Rights, RuntimeError, SPEECH2TEXT, See, Sequence2Txt, The, USE, Unless, Version, WARRANTIES, WITHOUT, You, chunk

---
*Generated by RAGFlow Repository Documentation Generator*
