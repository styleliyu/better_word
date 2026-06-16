---
name: better-word
description: Build a LaTeX + AI replacement workflow for Word-heavy school or coursework reports. Use when the user wants to extract formatting rules from an official Word/PDF/screenshot template, generate or refine a reusable LaTeX report template, write report content as .tex instead of .docx, compile a polished PDF, or avoid Word formatting drift in team assignments.
---

# Better Word

## Purpose

Turn unstable Word report formatting into a reproducible LaTeX workflow:

1. Extract style rules from the official template.
2. Encode those rules in a LaTeX template.
3. Generate each report as `.tex`.
4. Compile and verify the PDF instead of manually repairing Word output.

## Default Workflow

### 1. Gather the Source Template

Prefer the highest-fidelity source available:

1. Original `.docx` template
2. Official PDF export
3. Full-page screenshots
4. Partial screenshots

When `.docx` is available, inspect its styles, page settings, headers, footers, numbering, and table/figure styles directly. Use screenshots as visual QA, not as the only source of truth unless no structured file exists.

### 2. Extract Formatting Rules

Create a concise template audit before writing LaTeX. Read `references/template-analysis-checklist.md` when the task involves analyzing a Word template, PDF, or screenshots.

Capture at least:

- Page size, margins, header/footer distance, page numbering.
- Chinese and Latin fonts, font sizes, bold/italic rules, line spacing, paragraph indentation.
- Title page fields, abstract/keywords, table of contents, section hierarchy.
- Figure/table captions, numbering, cross-reference format.
- Bibliography style and citation format.
- Any uncertainty, stated as assumptions to verify.

### 3. Generate the LaTeX Template

Start from `assets/course-report-template.tex` unless the school format clearly requires another document class.

Use XeLaTeX or LuaLaTeX for Chinese documents. Prefer `ctexart` or `ctexrep` for coursework reports; avoid pdfLaTeX when Chinese text or CJK fonts are involved.

Keep content and formatting separate:

- Put reusable style decisions in the preamble or custom commands.
- Keep report-specific text in the document body.
- Use labels and references for figures, tables, formulas, and sections.
- Use automatic numbering instead of hand-numbered headings or captions.

### 4. Generate Report Content

When the user asks for report content, produce LaTeX source rather than Word content. Use explicit placeholders for unavailable figures, datasets, or citations instead of inventing source-specific artifacts.

Useful prompt shape:

```text
Use the provided LaTeX template to write a course report about <topic>.
Return compilable .tex source only. Preserve the template structure, heading levels,
caption rules, references, and Chinese typography settings.
```

### 5. Compile and Verify

If a TeX toolchain is available, compile before delivery:

```powershell
latexmk -xelatex -interaction=nonstopmode main.tex
```

If `latexmk` is unavailable, try `xelatex` twice. Inspect the log for missing fonts, missing images, undefined references, overfull boxes, and bibliography failures. Fix the `.tex` or template instead of post-editing the PDF.

When compilation is not possible, say so explicitly and still check the LaTeX source for obvious syntax problems.

## Output Standards

Deliver the minimum useful artifact set:

- A reusable `.tex` template when the task is about building the school format.
- A filled `.tex` report and compiled PDF when the task is about producing a report.
- A short list of assumptions when template evidence is incomplete.

Do not create a `.docx` fallback unless the user specifically asks for Word output. The point of this skill is to avoid Word as the formatting source of truth.

## Quality Bar

- Match the official template before adding aesthetic improvements.
- Prefer simple, stable LaTeX packages over elaborate macro systems.
- Do not hard-code page breaks to hide layout problems unless the official template requires them.
- Keep tables, figures, references, and formulas semantic so numbering and cross-references remain automatic.
- For team reports, recommend separate section files only when the report is large enough to benefit from split ownership.
