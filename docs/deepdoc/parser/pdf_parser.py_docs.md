# Documentation: deepdoc/parser/pdf_parser.py

## File Metadata

- **Path**: `deepdoc/parser/pdf_parser.py`
- **Size**: 57432 bytes
- **Type**: .py
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `deepdoc/parser/pdf_parser.py`.

## Python Module Overview

### Imports and Dependencies

This module imports the following dependencies:

- `logging`
- `math`
- `os`
- `random`
- `re`
- `sys`
- `threading`
- `collections`
- `copy`
- `io`
- `timeit`
- `numpy`
- `pdfplumber`
- `trio`
- `xgboost`
- `huggingface_hub`
- `PIL`
- `pypdf`
- `common.file_utils`
- `common.misc_utils`

### Classes Defined

This file defines 3 class(es):

#### Class: `RAGFlowPdfParser` (line 50)

**Methods**: __init__, __char_width, __height, _x_dis, _y_dis, _match_proj, _updown_concat_features, sort_X_by_page, _has_color, _table_transformer_job, __ocr, _layouts_rec, _assign_column, _text_merge, _naive_vertical_merge, _final_reading_order_merge, _concat_downward, _filter_forpages, _merge_with_same_bullet, _extract_table_figure, proj_match, _line_tag, __filterout_scraps, total_page_number, __images__, __call__, parse_into_bboxes, remove_tag, extract_positions, crop, get_position

#### Class: `PlainParser` (line 1316)

**Methods**: __call__, crop, remove_tag

#### Class: `VisionParser` (line 1350)

**Methods**: __init__, __images__, __call__

### Functions Defined

This file defines 53 function(s):

#### Function: `__init__` (line 51)

**Parameters**: self

**Docstring**: If you have trouble downloading HuggingFace models, -_^ this might help!!

For Linux:
export HF_ENDPOINT=https://hf-mirror.com

For Windows:
Good luck
^_-...

#### Function: `__char_width` (line 104)

**Parameters**: self, c

#### Function: `__height` (line 107)

**Parameters**: self, c

#### Function: `_x_dis` (line 110)

**Parameters**: self, a, b

#### Function: `_y_dis` (line 113)

**Parameters**: self, a, b

#### Function: `_match_proj` (line 116)

**Parameters**: self, b

#### Function: `_updown_concat_features` (line 129)

**Parameters**: self, up, down

#### Function: `sort_X_by_page` (line 175)

**Parameters**: arr, threshold

#### Function: `_has_color` (line 187)

**Parameters**: self, o

#### Function: `_table_transformer_job` (line 194)

**Parameters**: self, ZM

#### Function: `__ocr` (line 280)

**Parameters**: self, pagenum, img, chars, ZM, device_id

#### Function: `_layouts_rec` (line 345)

**Parameters**: self, ZM, drop

#### Function: `_assign_column` (line 353)

**Parameters**: self, boxes, zoomin

#### Function: `_text_merge` (line 422)

**Parameters**: self, zoomin

#### Function: `_naive_vertical_merge` (line 460)

**Parameters**: self, zoomin

#### Function: `_final_reading_order_merge` (line 538)

**Parameters**: self, zoomin

#### Function: `_concat_downward` (line 561)

**Parameters**: self, concat_between_pages

#### Function: `_filter_forpages` (line 665)

**Parameters**: self

#### Function: `_merge_with_same_bullet` (line 711)

**Parameters**: self

#### Function: `_extract_table_figure` (line 737)

**Parameters**: self, need_image, ZM, return_html, need_position, separate_tables_figures

#### Function: `proj_match` (line 912)

**Parameters**: self, line

#### Function: `_line_tag` (line 936)

**Parameters**: self, bx, ZM

#### Function: `__filterout_scraps` (line 951)

**Parameters**: self, boxes, ZM

#### Function: `total_page_number` (line 1012)

**Parameters**: fnm, binary

#### Function: `__images__` (line 1022)

**Parameters**: self, fnm, zoomin, page_from, page_to, callback

#### Function: `__call__` (line 1140)

**Parameters**: self, fnm, need_image, zoomin, return_html

#### Function: `parse_into_bboxes` (line 1150)

**Parameters**: self, fnm, callback, zoomin

#### Function: `remove_tag` (line 1235)

**Parameters**: txt

#### Function: `extract_positions` (line 1239)

**Parameters**: txt

#### Function: `crop` (line 1247)

**Parameters**: self, text, ZM, need_position

#### Function: `get_position` (line 1302)

**Parameters**: self, bx, ZM

#### Function: `__call__` (line 1317)

**Parameters**: self, filename, from_page, to_page

#### Function: `crop` (line 1342)

**Parameters**: self, ck, need_position

#### Function: `remove_tag` (line 1346)

**Parameters**: txt

#### Function: `__init__` (line 1351)

**Parameters**: self, vision_model

#### Function: `__images__` (line 1355)

**Parameters**: self, fnm, zoomin, page_from, page_to, callback

#### Function: `__call__` (line 1366)

**Parameters**: self, filename, from_page, to_page

#### Function: `gather` (line 238)

**Parameters**: kwd, fzy, ption

#### Function: `end_with` (line 426)

**Parameters**: b, txt

#### Function: `start_with` (line 431)

**Parameters**: b, txts

#### Function: `x_overlapped` (line 794)

**Parameters**: a, b

#### Function: `cropout` (line 836)

**Parameters**: bxs, ltype, poss

#### Function: `width` (line 952)

**Parameters**: b

#### Function: `height` (line 955)

**Parameters**: b

#### Function: `usefull` (line 958)

**Parameters**: b

#### Function: `insert_table_figures` (line 1176)

**Parameters**: tbls_or_figs, layout_type

#### Function: `dfs` (line 587)

**Parameters**: up, dp

#### Function: `nearest` (line 807)

**Parameters**: tbls

#### Function: `dfs` (line 975)

**Parameters**: line, st

#### Function: `__ocr_preprocess` (line 1104)

**Parameters**: None

#### Function: `min_rectangle_distance` (line 1177)

**Parameters**: rect1, rect2

#### Function: `dfs` (line 1327)

**Parameters**: arr, depth

#### Function: `dfs` (line 1057)

**Parameters**: arr, depth

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

import logging
import math
import os
import random
import re
import sys
import threading
from collections import Counter, defaultdict
from copy import deepcopy
from io import BytesIO
from timeit import default_timer as timer

import numpy as np
import pdfplumber
import trio
import xgboost as xgb
from huggingface_hub import snapshot_download
from PIL import Image
from pypdf import PdfReader as pdf2_read

from common.file_utils import get_project_base_directory
from common.misc_utils import pip_install_torch
from deepdoc.vision import OCR, AscendLayoutRecognizer, LayoutRecognizer, Recognizer, TableStructureRecognizer
from rag.app.picture import vision_llm_chunk as picture_vision_llm_chunk
from rag.nlp import rag_tokenizer
from rag.prompts.generator import vision_llm_describe_prompt
from common import settings

LOCK_KEY_pdfplumber = "global_shared_lock_pdfplumber"
if LOCK_KEY_pdfplumber not in sys.modules:
    sys.modules[LOCK_KEY_pdfplumber] = threading.Lock()


class RAGFlowPdfParser:
    def __init__(self, **kwargs):
        """
        If you have trouble downloading HuggingFace models, -_^ this might help!!

        For Linux:
        export HF_ENDPOINT=https://hf-mirror.com

        For Windows:
        Good luck
        ^_-

        """

        self.ocr = OCR()
        self.parallel_limiter = None
        if settings.PARALLEL_DEVICES > 1:
            self.parallel_limiter = [trio.CapacityLimiter(1) for _ in range(settings.PARALLEL_DEVICES)]

        layout_recognizer_type = os.getenv("LAYOUT_RECOGNIZER_TYPE", "onnx").lower()
        if layout_recognizer_type not in ["onnx", "ascend"]:
            raise RuntimeError("Unsupported layout recognizer type.")

        if hasattr(self, "model_speciess"):
            recognizer_domain = "layout." + self.model_speciess
        else:
            recognizer_domain = "layout"

        if layout_recognizer_type == "ascend":
            logging.debug("Using Ascend LayoutRecognizer")
            self.layouter = AscendLayoutRecognizer(recognizer_domain)
        else:  # onnx
            logging.debug("Using Onnx LayoutRecognizer")
            self.layouter = LayoutRecognizer(recognizer_domain)
        self.tbl_det = TableStructureRecognizer()

        self.updown_cnt_mdl = xgb.Booster()
        try:
            pip_install_torch()
            import torch.cuda
            if torch.cuda.is_available():
                self.updown_cnt_mdl.set_param({"device": "cuda"})
        except Exception:
            logging.info("No torch found.")
        try:
            model_dir = os.path.join(get_project_base_directory(), "rag/res/deepdoc")
            self.updown_cnt_mdl.load_model(os.path.join(model_dir, "updown_concat_xgb.model"))
        except Exception:
            model_dir = snapshot_download(repo_id="InfiniFlow/text_concat_xgb_v1.0", local_dir=os.path.join(get_project_base_directory(), "rag/res/deepdoc"), local_dir_use_symlinks=False)
            self.updown_cnt_mdl.load_model(os.path.join(model_dir, "updown_concat_xgb.model"))

        self.page_from = 0
        self.column_num = 1

    def __char_width(self, c):
        return (c["x1"] - c["x0"]) // max(len(c["text"]), 1)

    def __height(self, c):
        return c["bottom"] - c["top"]

    def _x_dis(self, a, b):
        return min(abs(a["x1"] - b["x0"]), abs(a["x0"] - b["x1"]), abs(a["x0"] + a["x1"] - b["x0"] - b["x1"]) / 2)

    def _y_dis(self, a, b):
        return (b["top"] + b["bottom"] - a["top"] - a["bottom"]) / 2

    def _match_proj(self, b):
        proj_patt = [
            r"第[零一二三四五六七八九十百]+章",
            r"第[零一二三四五六七八九十百]+[条节]",
            r"[零一二三四五六七八九十百]+[、是 　]",
            r"[\(（][零一二三四五六七八九十百]+[）\)]",
            r"[\(（][0-9]+[）\)]",
            r"[0-9]+(、|\.[　 ]|）|\.[^0-9./a-zA-Z_%><-]{4,})",
            r"[0-9]+\.[0-9.]+(、|\.[ 　])",
            r"[⚫•➢①② ]",
        ]
        return any([re.match(p, b["text"]) for p in proj_patt])

    def _updown_concat_features(self, up, down):
        w = max(self.__char_width(up), self.__char_width(down))
        h = max(self.__height(up), self.__height(down))
        y_dis = self._y_dis(up, down)
        LEN = 6
        tks_down = rag_tokenizer.tokenize(down["text"][:LEN]).split()
        tks_up = rag_tokenizer.tokenize(up["text"][-LEN:]).split()
        tks_all = up["text"][-LEN:].strip() + (" " if re.match(r"[a-zA-Z0-9]+", up["text"][-1] + down["text"][0]) else "") + down["text"][:LEN].strip()
        tks_all = rag_tokenizer.tokenize(tks_all).split()
        fea = [
            up.get("R", -1) == down.get("R", -1),
            y_dis / h,
            down["page_number"] - up["page_number"],
            up["layout_type"] == down["layout_type"],
            up["layout_type"] == "text",
            down["layout_type"] == "text",
            up["layout_type"] == "table",
            down["layout_type"] == "table",
            True if re.search(r"([。？！；!?;+)）]|[a-z]\.)$", up["text"]) else False,
            True if re.search(r"[，：‘“、0-9（+-]$", up["text"]) else False,
            True if re.search(r"(^.?[/,?;:\]，。；：’”？！》】）-])", down["text"]) else False,
            True if re.match(r"[\(（][^\(\)（）]+[）\)]$", up["text"]) else False,
            True if re.search(r"[，,][^。.]+$", up["text"]) else False,
            True if re.search(r"[，,][^。.]+$", up["text"]) else False,
            True if re.search(r"[\(（][^\)）]+$", up["text"]) and re.search(r"[\)）]", down["text"]) else False,
            self._match_proj(down),
            True if re.match(r"[A-Z]", down["text"]) else False,
            True if re.match(r"[A-Z]", up["text"][-1]) else False,
            True if re.match(r"[a-z0-9]", up["text"][-1]) else False,
            True if re.match(r"[0-9.%,-]+$", down["text"]) else False,
            up["text"].strip()[-2:] == down["text"].strip()[-2:] if len(up["text"].strip()) > 1 and len(down["text"].strip()) > 1 else False,
            up["x0"] > down["x1"],
            abs(self.__height(up) - self.__height(down)) / min(self.__height(up), self.__height(down)),
            self._x_dis(up, down) / max(w, 0.000001),
            (len(up["text"]) - len(down["text"])) / max(len(up["text"]), len(down["text"])),
            len(tks_all) - len(tks_up) - len(tks_down),
            len(tks_down) - len(tks_up),
            tks_down[-1] == tks_up[-1] if tks_down and tks_up else False,
            max(down["in_row"], up["in_row"]),
            abs(down["in_row"] - up["in_row"]),
            len(tks_down) == 1 and rag_tokenizer.tag(tks_down[0]).find("n") >= 0,
            len(tks_up) == 1 and rag_tokenizer.tag(tks_up[0]).find("n") >= 0,
        ]
        return fea

    @staticmethod
    def sort_X_by_page(arr, threshold):
        # sort using y1 first and then x1
        arr = sorted(arr, key=lambda r: (r["page_number"], r["x0"], r["top"]))
        for i in range(len(arr) - 1):
            for j in range(i, -1, -1):
                # restore the order using th
                if abs(arr[j + 1]["x0"] - arr[j]["x0"]) < threshold and arr[j + 1]["top"] < arr[j]["top"] and arr[j + 1]["page_number"] == arr[j]["page_number"]:
                    tmp = arr[j]
                    arr[j] = arr[j + 1]
                    arr[j + 1] = tmp
        return arr

    def _has_color(self, o):
        if o.get("ncs", "") == "DeviceGray":
            if o["stroking_color"] and o["stroking_color"][0] == 1 and o["non_stroking_color"] and o["non_stroking_color"][0] == 1:
                if re.match(r"[a-zT_\[\]\(\)-]+", o.get("text", "")):
                    return False
        return True

    def _table_transformer_job(self, ZM):
        logging.debug("Table processing...")
        imgs, pos = [], []
        tbcnt = [0]
        MARGIN = 10
        self.tb_cpns = []
        assert len(self.page_layout) == len(self.page_images)
        for p, tbls in enumerate(self.page_layout):  # for page
            tbls = [f for f in tbls if f["type"] == "table"]
            tbcnt.append(len(tbls))
            if not tbls:
                continue
            for tb in tbls:  # for table
                left, top, right, bott = tb["x0"] - MARGIN, tb["top"] - MARGIN, tb["x1"] + MARGIN, tb["bottom"] + MARGIN
                left *= ZM
                top *= ZM
                right *= ZM
                bott *= ZM
                pos.append((left, top))
                imgs.append(self.page_images[p].crop((left, top, right, bott)))

        assert len(self.page_images) == len(tbcnt) - 1
        if not imgs:
            return
        recos = self.tbl_det(imgs)
        tbcnt = np.cumsum(tbcnt)
        for i in range(len(tbcnt) - 1):  # for page
            pg = []
            for j, tb_items in enumerate(recos[tbcnt[i] : tbcnt[i + 1]]):  # for table
                poss = pos[tbcnt[i] : tbcnt[i + 1]]
                for it in tb_items:  # for table components
                    it["x0"] = it["x0"] + poss[j][0]
                    it["x1"] = it["x1"] + poss[j][0]
                    it["top"] = it["top"] + poss[j][1]
                    it["bottom"] = it["bottom"] + poss[j][1]
                    for n in ["x0", "x1", "top", "bottom"]:
                        it[n] /= ZM
                    it["top"] += self.page_cum_height[i]
                    it["bottom"] += self.page_cum_height[i]
                    it["pn"] = i
                    it["layoutno"] = j
                    pg.append(it)
            se

... [Content truncated - file is 56930 bytes] ...

)".format(timer() - start))

        start = timer()
        self._table_transformer_job(zoomin)
        if callback:
            callback(0.83, "Table analysis ({:.2f}s)".format(timer() - start))

        start = timer()
        self._text_merge()
        self._concat_downward()
        self._naive_vertical_merge(zoomin)
        if callback:
            callback(0.92, "Text merged ({:.2f}s)".format(timer() - start))

        start = timer()
        tbls, figs = self._extract_table_figure(True, zoomin, True, True, True)

        def insert_table_figures(tbls_or_figs, layout_type):
            def min_rectangle_distance(rect1, rect2):
                pn1, left1, right1, top1, bottom1 = rect1
                pn2, left2, right2, top2, bottom2 = rect2
                if right1 >= left2 and right2 >= left1 and bottom1 >= top2 and bottom2 >= top1:
                    return 0
                if right1 < left2:
                    dx = left2 - right1
                elif right2 < left1:
                    dx = left1 - right2
                else:
                    dx = 0
                if bottom1 < top2:
                    dy = top2 - bottom1
                elif bottom2 < top1:
                    dy = top1 - bottom2
                else:
                    dy = 0
                return math.sqrt(dx * dx + dy * dy)  # + (pn2-pn1)*10000

            for (img, txt), poss in tbls_or_figs:
                bboxes = [(i, (b["page_number"], b["x0"], b["x1"], b["top"], b["bottom"])) for i, b in enumerate(self.boxes)]
                dists = [
                    (min_rectangle_distance((pn, left, right, top + self.page_cum_height[pn], bott + self.page_cum_height[pn]), rect), i) for i, rect in bboxes for pn, left, right, top, bott in poss
                ]
                min_i = np.argmin(dists, axis=0)[0]
                min_i, rect = bboxes[dists[min_i][-1]]
                if isinstance(txt, list):
                    txt = "\n".join(txt)
                pn, left, right, top, bott = poss[0]
                if self.boxes[min_i]["bottom"] < top + self.page_cum_height[pn]:
                    min_i += 1
                self.boxes.insert(
                    min_i,
                    {
                        "page_number": pn + 1,
                        "x0": left,
                        "x1": right,
                        "top": top + self.page_cum_height[pn],
                        "bottom": bott + self.page_cum_height[pn],
                        "layout_type": layout_type,
                        "text": txt,
                        "image": img,
                        "positions": [[pn + 1, int(left), int(right), int(top), int(bott)]],
                    },
                )

        for b in self.boxes:
            b["position_tag"] = self._line_tag(b, zoomin)
            b["image"] = self.crop(b["position_tag"], zoomin)
            b["positions"] = [[pos[0][-1] + 1, *pos[1:]] for pos in RAGFlowPdfParser.extract_positions(b["position_tag"])]

        insert_table_figures(tbls, "table")
        insert_table_figures(figs, "figure")
        if callback:
            callback(1, "Structured ({:.2f}s)".format(timer() - start))
        return deepcopy(self.boxes)

    @staticmethod
    def remove_tag(txt):
        return re.sub(r"@@[\t0-9.-]+?##", "", txt)

    @staticmethod
    def extract_positions(txt):
        poss = []
        for tag in re.findall(r"@@[0-9-]+\t[0-9.\t]+##", txt):
            pn, left, right, top, bottom = tag.strip("#").strip("@").split("\t")
            left, right, top, bottom = float(left), float(right), float(top), float(bottom)
            poss.append(([int(p) - 1 for p in pn.split("-")], left, right, top, bottom))
        return poss

    def crop(self, text, ZM=3, need_position=False):
        imgs = []
        poss = self.extract_positions(text)
        if not poss:
            if need_position:
                return None, None
            return

        max_width = max(np.max([right - left for (_, left, right, _, _) in poss]), 6)
        GAP = 6
        pos = poss[0]
        poss.insert(0, ([pos[0][0]], pos[1], pos[2], max(0, pos[3] - 120), max(pos[3] - GAP, 0)))
        pos = poss[-1]
        poss.append(([pos[0][-1]], pos[1], pos[2], min(self.page_images[pos[0][-1]].size[1] / ZM, pos[4] + GAP), min(self.page_images[pos[0][-1]].size[1] / ZM, pos[4] + 120)))

        positions = []
        for ii, (pns, left, right, top, bottom) in enumerate(poss):
            right = left + max_width
            bottom *= ZM
            for pn in pns[1:]:
                bottom += self.page_images[pn - 1].size[1]
            imgs.append(self.page_images[pns[0]].crop((left * ZM, top * ZM, right * ZM, min(bottom, self.page_images[pns[0]].size[1]))))
            if 0 < ii < len(poss) - 1:
                positions.append((pns[0] + self.page_from, left, right, top, min(bottom, self.page_images[pns[0]].size[1]) / ZM))
            bottom -= self.page_images[pns[0]].size[1]
            for pn in pns[1:]:
                imgs.append(self.page_images[pn].crop((left * ZM, 0, right * ZM, min(bottom, self.page_images[pn].size[1]))))
                if 0 < ii < len(poss) - 1:
                    positions.append((pn + self.page_from, left, right, 0, min(bottom, self.page_images[pn].size[1]) / ZM))
                bottom -= self.page_images[pn].size[1]

        if not imgs:
            if need_position:
                return None, None
            return
        height = 0
        for img in imgs:
            height += img.size[1] + GAP
        height = int(height)
        width = int(np.max([i.size[0] for i in imgs]))
        pic = Image.new("RGB", (width, height), (245, 245, 245))
        height = 0
        for ii, img in enumerate(imgs):
            if ii == 0 or ii + 1 == len(imgs):
                img = img.convert("RGBA")
                overlay = Image.new("RGBA", img.size, (0, 0, 0, 0))
                overlay.putalpha(128)
                img = Image.alpha_composite(img, overlay).convert("RGB")
            pic.paste(img, (0, int(height)))
            height += img.size[1] + GAP

        if need_position:
            return pic, positions
        return pic

    def get_position(self, bx, ZM):
        poss = []
        pn = bx["page_number"]
        top = bx["top"] - self.page_cum_height[pn - 1]
        bott = bx["bottom"] - self.page_cum_height[pn - 1]
        poss.append((pn, bx["x0"], bx["x1"], top, min(bott, self.page_images[pn - 1].size[1] / ZM)))
        while bott * ZM > self.page_images[pn - 1].size[1]:
            bott -= self.page_images[pn - 1].size[1] / ZM
            top = 0
            pn += 1
            poss.append((pn, bx["x0"], bx["x1"], top, min(bott, self.page_images[pn - 1].size[1] / ZM)))
        return poss


class PlainParser:
    def __call__(self, filename, from_page=0, to_page=100000, **kwargs):
        self.outlines = []
        lines = []
        try:
            self.pdf = pdf2_read(filename if isinstance(filename, str) else BytesIO(filename))
            for page in self.pdf.pages[from_page:to_page]:
                lines.extend([t for t in page.extract_text().split("\n")])

            outlines = self.pdf.outline

            def dfs(arr, depth):
                for a in arr:
                    if isinstance(a, dict):
                        self.outlines.append((a["/Title"], depth))
                        continue
                    dfs(a, depth + 1)

            dfs(outlines, 0)
        except Exception:
            logging.exception("Outlines exception")
        if not self.outlines:
            logging.warning("Miss outlines")

        return [(line, "") for line in lines], []

    def crop(self, ck, need_position):
        raise NotImplementedError

    @staticmethod
    def remove_tag(txt):
        raise NotImplementedError


class VisionParser(RAGFlowPdfParser):
    def __init__(self, vision_model, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.vision_model = vision_model

    def __images__(self, fnm, zoomin=3, page_from=0, page_to=299, callback=None):
        try:
            with sys.modules[LOCK_KEY_pdfplumber]:
                self.pdf = pdfplumber.open(fnm) if isinstance(fnm, str) else pdfplumber.open(BytesIO(fnm))
                self.page_images = [p.to_image(resolution=72 * zoomin).annotated for i, p in enumerate(self.pdf.pages[page_from:page_to])]
                self.total_page = len(self.pdf.pages)
        except Exception:
            self.page_images = None
            self.total_page = 0
            logging.exception("VisionParser __images__")

    def __call__(self, filename, from_page=0, to_page=100000, **kwargs):
        callback = kwargs.get("callback", lambda prog, msg: None)
        zoomin = kwargs.get("zoomin", 3)
        self.__images__(fnm=filename, zoomin=zoomin, page_from=from_page, page_to=to_page, callback=callback)

        total_pdf_pages = self.total_page

        start_page = max(0, from_page)
        end_page = min(to_page, total_pdf_pages)

        all_docs = []

        for idx, img_binary in enumerate(self.page_images or []):
            pdf_page_num = idx  # 0-based
            if pdf_page_num < start_page or pdf_page_num >= end_page:
                continue

            text = picture_vision_llm_chunk(
                binary=img_binary,
                vision_model=self.vision_model,
                prompt=vision_llm_describe_prompt(page=pdf_page_num + 1),
                callback=callback,
            )

            if kwargs.get("callback"):
                kwargs["callback"](idx * 1.0 / len(self.page_images), f"Processed: {idx + 1}/{len(self.page_images)}")

            if text:
                width, height = self.page_images[idx].size
                all_docs.append((
                    text,
                    f"@@{pdf_page_num + 1}\t{0.0:.1f}\t{width / zoomin:.1f}\t{0.0:.1f}\t{height / zoomin:.1f}##"
                ))
        return all_docs, []


if __name__ == "__main__":
    pass

```

## Detailed Analysis

### File Role in Repository

The file `deepdoc/parser/pdf_parser.py` is located in the `deepdoc/parser` directory.

This file is part of the **Document Processing** system.

### Architecture Context

Files in this location typically handle concerns related to parser.

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
- [docling_parser.py](docling_parser.py_docs.md)
- [docx_parser.py](docx_parser.py_docs.md)
- [excel_parser.py](excel_parser.py_docs.md)
- [figure_parser.py](figure_parser.py_docs.md)
- [html_parser.py](html_parser.py_docs.md)
- [json_parser.py](json_parser.py_docs.md)
- [markdown_parser.py](markdown_parser.py_docs.md)
- [mineru_parser.py](mineru_parser.py_docs.md)
- [ppt_parser.py](ppt_parser.py_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
