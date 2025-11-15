# File Documentation: rag/nlp/synonym.py

## File Metadata

- **Path**: `rag/nlp/synonym.py`
- **Extension**: `.py`
- **Lines**: 102
- **Characters**: 3,127
- **Size**: 3,131 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

## Original Source

```python
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
import json
import os
import time
import re
from nltk.corpus import wordnet
from common.file_utils import get_project_base_directory


class Dealer:
    def __init__(self, redis=None):

        self.lookup_num = 100000000
        self.load_tm = time.time() - 1000000
        self.dictionary = None
        path = os.path.join(get_project_base_directory(), "rag/res", "synonym.json")
        try:
            self.dictionary = json.load(open(path, 'r'))
            self.dictionary = { (k.lower() if isinstance(k, str) else k): v for k, v in self.dictionary.items() }
        except Exception:
            logging.warning("Missing synonym.json")
            self.dictionary = {}

        if not redis:
            logging.warning(
                "Realtime synonym is disabled, since no redis connection.")
        if not len(self.dictionary.keys()):
            logging.warning("Fail to load synonym")

        self.redis = redis
        self.load()

    def load(self):
        if not self.redis:
            return

        if self.lookup_num < 100:
            return
        tm = time.time()
        if tm - self.load_tm < 3600:
            return

        self.load_tm = time.time()
        self.lookup_num = 0
        d = self.redis.get("kevin_synonyms")
        if not d:
            return
        try:
            d = json.loads(d)
            self.dictionary = d
        except Exception as e:
            logging.error("Fail to load synonym!" + str(e))


    def lookup(self, tk, topn=8):
        if not tk or not isinstance(tk, str):
            return []

        # 1) Check the custom dictionary first (both keys and tk are already lowercase)
        self.lookup_num += 1
        self.load()
        key = re.sub(r"[ \t]+", " ", tk.strip())
        res = self.dictionary.get(key, [])
        if isinstance(res, str):
            res = [res]
        if res:  # Found in dictionary → return directly
            return res[:topn]

        # 2) If not found and tk is purely alphabetical → fallback to WordNet
        if re.fullmatch(r"[a-z]+", tk):
            wn_set = {
                re.sub("_", " ", syn.name().split(".")[0])
                for syn in wordnet.synsets(tk)
            }
            wn_set.discard(tk)  # Remove the original token itself
            wn_res = [t for t in wn_set if t]
            return wn_res[:topn]

        # 3) Nothing found in either source
        return []
    

if __name__ == '__main__':
    dl = Dealer()
    print(dl.dictionary)

```

## High-Level Overview

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

## Detailed Walkthrough

### Classes (1)

- `Dealer`: Class definition

### Imports (7)

- `import logging`
- `import json`
- `import os`
- `import time`
- `import re`
- `from nltk.corpus import wordnet`
- `from common.file_utils import get_project_base_directory`

## Code Structure Analysis

- Total lines: 102
- Blank lines: 17 (16.7%)
- Comment lines: ~18 (17.6%)
- Code lines: ~67


## Dependencies and Imports

- `import logging`
- `import json`
- `import os`
- `import time`
- `import re`
- `from nltk.corpus import wordnet`
- `from common.file_utils import get_project_base_directory`

## Design & Architecture

This file is located in the `rag` directory, specifically within `rag/nlp`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 4 loop(s) - consider algorithmic complexity

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

- Other files in `rag/nlp/` directory
- Imports from `nltk.corpus`
- Imports from `common.file_utils`
- Potential test file: `test_synonym.py`

## Keywords

ANY, All, Apache, Authors, BASIS, CONDITIONS, Check, Copyright, Dealer, Exception, Fail, Found, InfiniFlow, KIND, LICENSE, License, Licensed, Missing, None, Nothing, Python, Realtime, Remove, Reserved, Rights, See, The, Unless, Version, WARRANTIES, WITHOUT, WordNet, You, __init__, load, lookup

---
*Generated by RAGFlow Repository Documentation Generator*
