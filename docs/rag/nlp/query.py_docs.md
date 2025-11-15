# Documentation: rag/nlp/query.py

## File Metadata

- **Path**: `rag/nlp/query.py`
- **Size**: 10847 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `rag/nlp/query.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `json`
- `re`
- `collections`
- `rag.utils.doc_store_conn`
- `rag.nlp`
- `sklearn.metrics.pairwise`
- `numpy`

### Classes Defined

This file defines 1 class(es):

#### Class: `FulltextQueryer` (line 26)

**Methods**: __init__, sub_special_char, is_chinese, rmWWW, add_space_between_eng_zh, question, hybrid_similarity, token_similarity, similarity, paragraph

### Functions Defined

This file defines 12 function(s):

#### Function: `__init__` (line 27)

**Parameters**: self

#### Function: `sub_special_char` (line 41)

**Parameters**: line

#### Function: `is_chinese` (line 45)

**Parameters**: line

#### Function: `rmWWW` (line 56)

**Parameters**: txt

#### Function: `add_space_between_eng_zh` (line 75)

**Parameters**: txt

#### Function: `question` (line 85)

**Parameters**: self, txt, tbl, min_match

#### Function: `hybrid_similarity` (line 219)

**Parameters**: self, avec, bvecs, atks, btkss, tkweight, vtweight

#### Function: `token_similarity` (line 229)

**Parameters**: self, atks, btkss

#### Function: `similarity` (line 243)

**Parameters**: self, qtwt, dtwt

#### Function: `paragraph` (line 257)

**Parameters**: self, content_tks, keywords, keywords_topn

#### Function: `need_fine_grained_tokenize` (line 133)

**Parameters**: tk

#### Function: `to_dict` (line 230)

**Parameters**: tks

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
import json
import re
from collections import defaultdict

from rag.utils.doc_store_conn import MatchTextExpr
from rag.nlp import rag_tokenizer, term_weight, synonym


class FulltextQueryer:
    def __init__(self):
        self.tw = term_weight.Dealer()
        self.syn = synonym.Dealer()
        self.query_fields = [
            "title_tks^10",
            "title_sm_tks^5",
            "important_kwd^30",
            "important_tks^20",
            "question_tks^20",
            "content_ltks^2",
            "content_sm_ltks",
        ]

    @staticmethod
    def sub_special_char(line):
        return re.sub(r"([:\{\}/\[\]\-\*\"\(\)\|\+~\^])", r"\\\1", line).strip()

    @staticmethod
    def is_chinese(line):
        arr = re.split(r"[ \t]+", line)
        if len(arr) <= 3:
            return True
        e = 0
        for t in arr:
            if not re.match(r"[a-zA-Z]+$", t):
                e += 1
        return e * 1.0 / len(arr) >= 0.7

    @staticmethod
    def rmWWW(txt):
        patts = [
            (
                r"是*(怎么办|什么样的|哪家|一下|那家|请问|啥样|咋样了|什么时候|何时|何地|何人|是否|是不是|多少|哪里|怎么|哪儿|怎么样|如何|哪些|是啥|啥是|啊|吗|呢|吧|咋|什么|有没有|呀|谁|哪位|哪个)是*",
                "",
            ),
            (r"(^| )(what|who|how|which|where|why)('re|'s)? ", " "),
            (
                r"(^| )('s|'re|is|are|were|was|do|does|did|don't|doesn't|didn't|has|have|be|there|you|me|your|my|mine|just|please|may|i|should|would|wouldn't|will|won't|done|go|for|with|so|the|a|an|by|i'm|it's|he's|she's|they|they're|you're|as|by|on|in|at|up|out|down|of|to|or|and|if) ",
                " ")
        ]
        otxt = txt
        for r, p in patts:
            txt = re.sub(r, p, txt, flags=re.IGNORECASE)
        if not txt:
            txt = otxt
        return txt

    @staticmethod
    def add_space_between_eng_zh(txt):
        # (ENG/ENG+NUM) + ZH
        txt = re.sub(r'([A-Za-z]+[0-9]+)([\u4e00-\u9fa5]+)', r'\1 \2', txt)
        # ENG + ZH
        txt = re.sub(r'([A-Za-z])([\u4e00-\u9fa5]+)', r'\1 \2', txt)
        # ZH + (ENG/ENG+NUM)
        txt = re.sub(r'([\u4e00-\u9fa5]+)([A-Za-z]+[0-9]+)', r'\1 \2', txt)
        txt = re.sub(r'([\u4e00-\u9fa5]+)([A-Za-z])', r'\1 \2', txt)
        return txt

    def question(self, txt, tbl="qa", min_match: float = 0.6):
        txt = FulltextQueryer.add_space_between_eng_zh(txt)
        txt = re.sub(
            r"[ :|\r\n\t,，。？?/`!！&^%%()\[\]{}<>]+",
            " ",
            rag_tokenizer.tradi2simp(rag_tokenizer.strQ2B(txt.lower())),
        ).strip()
        otxt = txt
        txt = FulltextQueryer.rmWWW(txt)

        if not self.is_chinese(txt):
            txt = FulltextQueryer.rmWWW(txt)
            tks = rag_tokenizer.tokenize(txt).split()
            keywords = [t for t in tks if t]
            tks_w = self.tw.weights(tks, preprocess=False)
            tks_w = [(re.sub(r"[ \\\"'^]", "", tk), w) for tk, w in tks_w]
            tks_w = [(re.sub(r"^[a-z0-9]$", "", tk), w) for tk, w in tks_w if tk]
            tks_w = [(re.sub(r"^[\+-]", "", tk), w) for tk, w in tks_w if tk]
            tks_w = [(tk.strip(), w) for tk, w in tks_w if tk.strip()]
            syns = []
            for tk, w in tks_w[:256]:
                syn = self.syn.lookup(tk)
                syn = rag_tokenizer.tokenize(" ".join(syn)).split()
                keywords.extend(syn)
                syn = ["\"{}\"^{:.4f}".format(s, w / 4.) for s in syn if s.strip()]
                syns.append(" ".join(syn))

            q = ["({}^{:.4f}".format(tk, w) + " {})".format(syn) for (tk, w), syn in zip(tks_w, syns) if
                 tk and not re.match(r"[.^+\(\)-]", tk)]
            for i in range(1, len(tks_w)):
                left, right = tks_w[i - 1][0].strip(), tks_w[i][0].strip()
                if not left or not right:
                    continue
                q.append(
                    '"%s %s"^%.4f'
                    % (
                        tks_w[i - 1][0],
                        tks_w[i][0],
                        max(tks_w[i - 1][1], tks_w[i][1]) * 2,
                    )
                )
            if not q:
                q.append(txt)
            query = " ".join(q)
            return MatchTextExpr(
                self.query_fields, query, 100
            ), keywords

        def need_fine_grained_tokenize(tk):
            if len(tk) < 3:
                return False
            if re.match(r"[0-9a-z\.\+#_\*-]+$", tk):
                return False
            return True

        txt = FulltextQueryer.rmWWW(txt)
        qs, keywords = [], []
        for tt in self.tw.split(txt)[:256]:  # .split():
            if not tt:
                continue
            keywords.append(tt)
            twts = self.tw.weights([tt])
            syns = self.syn.lookup(tt)
            if syns and len(keywords) < 32:
                keywords.extend(syns)
            logging.debug(json.dumps(twts, ensure_ascii=False))
            tms = []
            for tk, w in sorted(twts, key=lambda x: x[1] * -1):
                sm = (
                    rag_tokenizer.fine_grained_tokenize(tk).split()
                    if need_fine_grained_tokenize(tk)
                    else []
                )
                sm = [
                    re.sub(
                        r"[ ,\./;'\[\]\\`~!@#$%\^&\*\(\)=\+_<>\?:\"\{\}\|，。；‘’【】、！￥……（）——《》？：“”-]+",
                        "",
                        m,
                    )
                    for m in sm
                ]
                sm = [FulltextQueryer.sub_special_char(m) for m in sm if len(m) > 1]
                sm = [m for m in sm if len(m) > 1]

                if len(keywords) < 32:
                    keywords.append(re.sub(r"[ \\\"']+", "", tk))
                    keywords.extend(sm)

                tk_syns = self.syn.lookup(tk)
                tk_syns = [FulltextQueryer.sub_special_char(s) for s in tk_syns]
                if len(keywords) < 32:
                    keywords.extend([s for s in tk_syns if s])
                tk_syns = [rag_tokenizer.fine_grained_tokenize(s) for s in tk_syns if s]
                tk_syns = [f"\"{s}\"" if s.find(" ") > 0 else s for s in tk_syns]

                if len(keywords) >= 32:
                    break

                tk = FulltextQueryer.sub_special_char(tk)
                if tk.find(" ") > 0:
                    tk = '"%s"' % tk
                if tk_syns:
                    tk = f"({tk} OR (%s)^0.2)" % " ".join(tk_syns)
                if sm:
                    tk = f'{tk} OR "%s" OR ("%s"~2)^0.5' % (" ".join(sm), " ".join(sm))
                if tk.strip():
                    tms.append((tk, w))

            tms = " ".join([f"({t})^{w}" for t, w in tms])

            if len(twts) > 1:
                tms += ' ("%s"~2)^1.5' % rag_tokenizer.tokenize(tt)

            syns = " OR ".join(
                [
                    '"%s"'
                    % rag_tokenizer.tokenize(FulltextQueryer.sub_special_char(s))
                    for s in syns
                ]
            )
            if syns and tms:
                tms = f"({tms})^5 OR ({syns})^0.7"

            qs.append(tms)

        if qs:
            query = " OR ".join([f"({t})" for t in qs if t])
            if not query:
                query = otxt
            return MatchTextExpr(
                self.query_fields, query, 100, {"minimum_should_match": min_match}
            ), keywords
        return None, keywords

    def hybrid_similarity(self, avec, bvecs, atks, btkss, tkweight=0.3, vtweight=0.7):
        from sklearn.metrics.pairwise import cosine_similarity
        import numpy as np

        sims = cosine_similarity([avec], bvecs)
        tksim = self.token_similarity(atks, btkss)
        if np.sum(sims[0]) == 0:
            return np.array(tksim), tksim, sims[0]
        return np.array(sims[0]) * vtweight + np.array(tksim) * tkweight, tksim, sims[0]

    def token_similarity(self, atks, btkss):
        def to_dict(tks):
            if isinstance(tks, str):
                tks = tks.split()
            d = defaultdict(int)
            wts = self.tw.weights(tks, preprocess=False)
            for i, (t, c) in enumerate(wts):
                d[t] += c
            return d

        atks = to_dict(atks)
        btkss = [to_dict(tks) for tks in btkss]
        return [self.similarity(atks, btks) for btks in btkss]

    def similarity(self, qtwt, dtwt):
        if isinstance(dtwt, type("")):
            dtwt = {t: w for t, w in self.tw.weights(self.tw.split(dtwt), preprocess=False)}
        if isinstance(qtwt, type("")):
            qtwt = {t: w for t, w in self.tw.weights(self.tw.split(qtwt), preprocess=False)}
        s = 1e-9
        for k, v in qtwt.items():
            if k in dtwt:
                s += v #* dtwt[k]
        q = 1e-9
        for k, v in qtwt.items():
            q += v #* v
        return s/q #math.sqrt(3. * (s / q / math.log10( len(dtwt.keys()) + 512 )))

    def paragraph(self, content_tks: str, keywords: list = [], keywords_topn=30):
        if isinstance(content_tks, str):
            content_tks = [c.strip() for c in content_tks.strip() if c.strip()]
        tks_w = self.tw.weights(content_tks, preprocess=False)

        keywords = [f'"{k.strip()}"' for k in keywords]
        for tk, w in sorted(tks_w, key=lambda x: x[1] * -1)[:keywords_topn]:
            tk_syns = self.syn.lookup(tk)
            tk_syns = [FulltextQueryer.sub_special_char(s) for s in tk_syns]
            tk_syns = [rag_tokenizer.fine_grained_tokenize(s) for s in tk_syns if s]
            tk_syns = [f"\"{s}\"" if s.find(" ") > 0 else s for s in tk_syns]
            tk = FulltextQueryer.sub_special_char(tk)
            if tk.find(" ") > 0:
                tk = '"%s"' % tk
            if tk_syns:
                tk = f"({tk} OR (%s)^0.2)" % " ".join(tk_syns)
            if tk:
                keywords.append(f"{tk}^{w}")

        return MatchTextExpr(self.query_fields, " ".join(keywords), 100,
                             {"minimum_should_match": min(3, len(keywords) // 10)})

```

## Detailed Analysis

### File Role in Repository

The file `rag/nlp/query.py` is located in the `rag/nlp` directory.

This file is part of the **RAG (Retrieval-Augmented Generation)** core engine.

### Architecture Context

Files in this location typically handle concerns related to nlp.

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
- [rag_tokenizer.py](rag_tokenizer.py_docs.md)
- [search.py](search.py_docs.md)
- [surname.py](surname.py_docs.md)
- [synonym.py](synonym.py_docs.md)
- [term_weight.py](term_weight.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
