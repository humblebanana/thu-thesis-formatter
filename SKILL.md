---
name: thesis-formatter
description: "Format raw content (Markdown, plain text, or unformatted DOCX) into a Tsinghua-style (清华大学) graduation thesis DOCX with exact formatting. Use this skill whenever the user asks to format a thesis, graduation paper, 毕业论文, 综合论文训练, or any academic document that should follow Tsinghua University's thesis template. Also trigger when the user mentions '按照毕业论文格式排版', '论文排版', 'thesis formatting', or references a Tsinghua thesis template. This skill handles cover pages, chapter numbering (第1章, 1.1, 1.1.1), footnotes/citations, TOC, page layout, font specifications, and all other thesis-specific formatting requirements. Based on《清华大学综合论文训练写作规范》."
---

# Tsinghua Thesis Formatter

Based on **《清华大学综合论文训练写作规范》**.

**IMPORTANT — TWO NON-NEGOTIABLE RULES:**

### Rule 1: Content is Sacred — Never Modify User Text

This skill is a **formatter only**. Its sole job is to apply visual formatting to the user's existing content.

- **DO NOT** rewrite, rephrase, summarise, expand, or improve any sentence.
- **DO NOT** add new paragraphs, arguments, examples, or citations that the user did not provide.
- **DO NOT** remove any content, even if it seems redundant or off-topic.
- **DO NOT** infer or fabricate missing sections (e.g. abstract, acknowledgments). If a section is absent from the source, leave it as a clearly-labelled placeholder: `【请在此处填写摘要】`.
- The only text changes permitted are **structural labels** required by the format: chapter number prefixes (`第 1 章`), section number prefixes (`1.1`), and fixed boilerplate strings (e.g. the authorization statement template).

> **Guiding principle**: If you opened the source document and the output document side-by-side, every sentence written by the user must appear **word-for-word identical** in both.

### Rule 2: Load the `docx` Skill First

This skill must be used in conjunction with the `docx` skill. Before creating a document, you MUST load the `docx` skill's knowledge into context:
```bash
# Read the docx skill first to understand how to create DOCX files properly
cat /home/ubuntu/skills/docx/SKILL.md
```
Then use `docx-js` (`npm install -g docx`) to generate the DOCX according to the rules below.

---

## 1. Document Structure (论文组成及顺序)

Each part starts on a new page. The order is:

1. 封面 (Cover)
2. 关于论文使用授权的说明 (Authorization)
3. 摘要 (Chinese Abstract)
4. Abstract (English Abstract)
5. 目录 (Table of Contents)
6. 插图和附表清单 (List of Figures/Tables, if any)
7. 符号和缩略语说明 (Symbols/Abbreviations, if any)
8. 正文 (Body): 第1章 引言/绪论 → … → 结论
9. 参考文献 (References)
10. 附录 (Appendices)
11. 致谢 (Acknowledgments)
12. 声明 (Declaration)
13. 在学期间参加课题的研究成果 (Research achievements, if any)
14. 综合论文训练记录表 (Thesis record form)

---

## 2. Page Setup (页面设置)

Paper: A4 (width=11906, height=16838 DXA; 21cm × 29.7cm)

### Cover Page Margins
| Edge | Metric | DXA |
|------|--------|-----|
| Top | 3.8 cm | 2155 |
| Bottom | 3.2 cm | 1814 |
| Left | 3.0 cm | 1701 |
| Right | 3.0 cm | 1701 |
| Gutter | 0.2 cm (left) | 113 |

### All Other Pages Margins
| Edge | Metric | DXA |
|------|--------|-----|
| Top | 3.0 cm | 1701 |
| Bottom | 3.0 cm | 1701 |
| Left | 3.0 cm | 1701 |
| Right | 3.0 cm | 1701 |
| Gutter | 0 | 0 |

### Headers & Page Numbers
- **No headers** (页眉：无)
- Page numbers: bottom center
  - Before Chapter 1: uppercase Roman numerals (I, II, III…)
  - From Chapter 1 onward: Arabic numerals (1, 2, 3…) continuously
  - Thesis record form: no page number

---

## 3. Complete Font & Paragraph Specification Table

This is the **authoritative** formatting table from the official spec.

### 3.1 Chapter Title (章标题) — e.g. "第 1 章 引言"
- Chinese: **黑体 三号** (SimHei, 16pt / sz=32)
- English/Numbers: **Arial 三号** (Arial, 16pt / sz=32)
- Alignment: **Centered** (居中)
- Line spacing: **Single** (line=240, lineRule="auto")
- Spacing before: **24 pt** (before=480)
- Spacing after: **18 pt** (after=360)
- Chapter number format: "第 1 章", number-to-title separated by 1 Chinese char space
- Also applies to: 摘要, Abstract, 目录, 参考文献, 附录, 致谢, 声明 titles

### 3.2 Level-1 Section Title (一级节标题) — e.g. "2.1 实验方法"
- Chinese: **黑体 四号** (SimHei, 14pt / sz=28)
- English/Numbers: **Arial 14pt** (sz=28)
- Alignment: **Left** (居左)
- Line spacing: **Fixed 20pt** (line=400, lineRule="exact")
- Spacing before: **24 pt** (before=480)
- Spacing after: **6 pt** (after=120)

### 3.3 Level-2 Section Title (二级节标题) — e.g. "3.2.2 实验装置"
- Chinese: **黑体 13pt** (SimHei, sz=26)
- English/Numbers: **Arial 13pt** (sz=26)
- Alignment: **Left** (居左)
- Line spacing: **Fixed 20pt** (line=400, lineRule="exact")
- Spacing before: **12 pt** (before=240)
- Spacing after: **6 pt** (after=120)

### 3.4 Level-3 Section Title (三级节标题) — e.g. "5.3.3.2 原材料"
- Chinese: **黑体 小四** (SimHei, 12pt / sz=24)
- English/Numbers: **Arial 12pt** (sz=24)
- Alignment: **Left** (居左)
- Line spacing: **Fixed 20pt** (line=400, lineRule="exact")
- Spacing before: **12 pt** (before=240)
- Spacing after: **6 pt** (after=120)
- Note: generally discouraged (不建议使用)

### 3.5 Body Text (正文段落)
- Chinese: **宋体 小四** (SimSun, 12pt / sz=24)
- English/Numbers: **Times New Roman 12pt** (sz=24)
- Alignment: **Justified** (两端对齐)
- First line indent: **2 Chinese chars** (~480 DXA, or use firstLineChars=200)
- Line spacing: **Fixed 20pt** (line=400, lineRule="exact")
- Spacing before: **0 pt**
- Spacing after: **0 pt**
- Also applies to: 附录, 致谢 body text

### 3.6 Footnote Text (脚注)
- Chinese: **宋体 小五** (SimSun, 9pt / sz=18)
- English/Numbers: **Times New Roman 小五** (9pt / sz=18)
- Alignment: **Justified** (两端对齐)
- Hanging indent: **1.5 chars** (~225 DXA)
- Spacing before/after: **0 pt**
- Line spacing: **Single** (line=240, lineRule="auto")
- Numbering: ①②③… per page (page-restart), superscript in body

### 3.7 Figure Caption (图题)
- Chinese: **宋体 五号** (SimSun, 10.5pt / sz=21)
- English/Numbers: **Times New Roman 11pt** (sz=22)
- Alignment: **Centered**, below figure
- Spacing before: **6 pt** / after: **12 pt**, single line spacing

### 3.8 Table Caption (表题)
- Chinese: **宋体 五号** (SimSun, 10.5pt / sz=21)
- English/Numbers: **Times New Roman 11pt** (sz=22)
- Alignment: **Centered**, above table
- Spacing before: **12 pt** / after: **6 pt**, single line spacing

### 3.9 Table Cell Text
- Font: 宋体/Times New Roman **11pt** (sz=22)
- Alignment: center, single line spacing, before/after **3 pt**

### 3.10 References (参考文献)
- Chinese: **宋体 五号** (SimSun, 10.5pt / sz=21)
- English/Numbers: **Times New Roman 五号** (10.5pt / sz=21)
- Line spacing: **Fixed 16pt** (line=320, lineRule="exact")
- Spacing before: **3 pt** (before=60) / after: **0 pt**
- Hanging indent: **2 Chinese chars or 1cm** (~567 DXA)
- Citation style: GB/T 7714-2015, or APA, or 清华学报 format

### 3.11 Expressions/Equations
- Font: Cambria Math or Times New Roman **12pt**
- Centered or indent 2 chars (consistent throughout)
- Single line spacing, before/after **6 pt**
- Number: right-aligned, e.g. (3-1)

---

## 4. Cover Page Details (封面)

### Title
- Max **25 Chinese characters**
- Font: **黑体 一号** (SimHei, 26pt / sz=52), centered
- If two lines: 1.25× line spacing, break at logical points

### Author Info Fields (系别/专业/姓名/指导教师)
- Font: **仿宋 三号** (FangSong, 16pt / sz=32)
- If name ≤4 chars: equal-width spacing

### Date
- Font: **宋体 三号** (SimSun, 16pt / sz=32)
- No Arabic numerals: e.g. "二○二四年六月"

---

## 5. Abstract (摘要)

### Chinese
- Title "摘要": 黑体三号, centered, before=24pt, after=18pt, single line
- Body: 宋体小四, justified, fixed 20pt, before/after=0
- Keywords: 3-5, new line, semicolon-separated

### English
- Title "Abstract": Arial三号, centered, before=24pt, after=18pt, single line
- Body: TNR小四, justified, fixed 20pt, before/after=0, English punctuation
- "Keywords": semicolon-separated, matching Chinese

---

## 6. Table of Contents (目录)

- Title "目录": 黑体三号, centered, single line, before=24pt, after=18pt
- Chapter entries: **小四**, Chinese=**黑体**, English/Numbers=**Arial**, left-aligned
- Level-1 entries: **宋体小四**, TNR for numbers, indent **1 Chinese char** (~210 DXA)
- Level-2 entries: **宋体小四**, indent **2 Chinese chars** (~420 DXA)
- Line spacing: fixed 20pt, before/after=0
- List up to level-2 sections (e.g. 1.2.5)
- Use dot leader tab stops, right-aligned at ~8306 DXA
- Content: from Chapter 1 through end

---

## 7. Chapter Numbering

- Level 0: "第%1章" (第 1 章, 第 2 章…)
- Level 1: "%1.%2" (1.1, 2.3…)
- Level 2: "%1.%2.%3" (1.1.1, 3.2.2…)
- Level 3: "%1.%2.%3.%4" (rarely used)
- Number and title: separated by **1 Chinese char space**

---

## 8. Tables

- Recommended: **three-line table** (三线表)
- Top/bottom border: **1.5pt** single line
- Header separator: **1pt** single line
- Cell text: 11pt, 宋体/TNR, single line, before/after 3pt, centered

---

## 9. Implementation Constants

```javascript
const PW = 11906, PH = 16838; // A4
const COVER_M = { top:2155, bottom:1814, left:1701, right:1701 }; // + gutter:113
const BODY_M = { top:1701, bottom:1701, left:1701, right:1701 };
// Content width = 11906 - 1701 - 1701 = 8504 DXA
// TOC dot leader tab stop position: ~8306 DXA (content width minus small margin)
```

---

## 10. Key Reminders

1. **Never use `\n`** in docx-js — use separate Paragraph objects
2. **Always set both Chinese and English fonts** on every TextRun
3. Body: **小四 (12pt)**, fixed 20pt line spacing, first indent 2 chars
4. Chapter titles: **三号 (16pt)**, **centered**, single, before 24pt / after 18pt
5. Section titles: **居左** (NOT centered!), level-1 before 24pt/after 6pt; level-2+ before 12pt/after 6pt
6. Footnotes: **小五 (9pt)**, ①②③ per page, hanging 1.5 chars
7. References: **五号 (10.5pt)**, fixed 16pt, hanging 2 chars
8. Page numbers: Roman before Ch1, Arabic from Ch1
9. **No page header**
10. Cover title: **一号 (26pt)**, max 25 chars; date in 宋体三号, no Arabic
11. All margins **3cm** except cover (top 3.8, bottom 3.2, gutter 0.2)
12. Smart quotes: `\u201C` `\u201D` for 中文引号
13. **Integration with `docx` skill**: Ensure that you follow the guidelines in the `docx` skill for table widths (use DXA), page breaks, and proper structure. After generation, ALWAYS validate the DOCX using the `docx` skill's validation script (`python scripts/office/validate.py doc.docx`).
