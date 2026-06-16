---
name: better-word
description: Build a LaTeX + AI replacement workflow for Word-heavy school or coursework reports, with optional DOCX export when Word output is required. Use when the user wants to extract formatting rules from an official Word/PDF/screenshot template, generate or refine a reusable LaTeX report template, write report content as .tex instead of .docx, compile a polished PDF, export a Word .docx via Pandoc/reference-doc workflows, or avoid Word formatting drift in team assignments.
---

# Better Word

## Purpose

Turn unstable Word report formatting into a reproducible LaTeX-centered workflow:

1. Extract style rules from the official template.
2. Encode those rules in a LaTeX template.
3. Generate each report as `.tex`.
4. Compile and verify the PDF, and export `.docx` when Word delivery is required.

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

### 5. Export and Verify

For PDF output, compile before delivery when a TeX toolchain is available:

```powershell
latexmk -xelatex -interaction=nonstopmode main.tex
```

If `latexmk` is unavailable, try `xelatex` twice. Inspect the log for missing fonts, missing images, undefined references, overfull boxes, and bibliography failures. Fix the `.tex` or template instead of post-editing the PDF.

When compilation is not possible, say so explicitly and still check the LaTeX source for obvious syntax problems.

For Word output, read `references/word-export.md`. Prefer Pandoc with a `reference.docx` derived from the official school template:

```powershell
pandoc main.tex --from latex --to docx --output main.docx --reference-doc reference.docx
```

If the LaTeX uses complex `ctex`, custom title, table, figure, or bibliography macros that Pandoc cannot preserve, create a Pandoc Markdown intermediate for the Word version and keep the LaTeX source as the PDF source of truth.

## Output Standards

Deliver the minimum useful artifact set:

- A reusable `.tex` template when the task is about building the school format.
- A filled `.tex` report and compiled PDF when the task is about producing a report.
- A `.docx` export when the user requests Word output or the assignment requires Word submission.
- A short list of assumptions when template evidence is incomplete.

Do not make Word the formatting source of truth unless the user explicitly requires editable `.docx` delivery. Prefer LaTeX for canonical structure and PDF fidelity, then generate DOCX as an export artifact.

## Quality Bar

- Match the official template before adding aesthetic improvements.
- Prefer simple, stable LaTeX packages over elaborate macro systems.
- Do not hard-code page breaks to hide layout problems unless the official template requires them.
- Keep tables, figures, references, and formulas semantic so numbering and cross-references remain automatic.
- For DOCX exports, verify the resulting Word file visually because Pandoc cannot preserve every LaTeX layout command.
- For team reports, recommend separate section files only when the report is large enough to benefit from split ownership.
