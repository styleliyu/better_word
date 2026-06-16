# Word Export Guide

Use this guide when the user needs a `.docx` file in addition to, or instead of, PDF.

## Recommended Route

Use LaTeX as the PDF source of truth, then export Word with Pandoc:

```powershell
pandoc main.tex --from latex --to docx --output main.docx
```

When an official Word template exists, use it as a reference document so Pandoc can inherit Word styles:

```powershell
pandoc main.tex --from latex --to docx --output main.docx --reference-doc reference.docx
```

Use the official template directly as `reference.docx` when possible. If the template contains sample content, first save a clean copy that keeps the styles, margins, headers, footers, numbering, and caption styles but removes assignment-specific text.

## When LaTeX to DOCX Is Not Enough

Pandoc handles common sections, paragraphs, lists, tables, images, citations, and math, but it can lose or simplify complex LaTeX layout commands.

Prefer a Pandoc Markdown intermediate when the report contains:

- Heavy `ctex`, `titlesec`, custom title page, or custom caption macros.
- Complex tables that rely on LaTeX-only packages.
- Strict Word styles that must remain editable.
- A school requirement to submit `.docx` rather than PDF.

In that case, generate:

1. `main.tex` for high-fidelity PDF output.
2. `main.md` for Word export with Pandoc-friendly structure.
3. `reference.docx` copied or cleaned from the official Word template.

Export with:

```powershell
pandoc main.md --from markdown --to docx --output main.docx --reference-doc reference.docx
```

## Authoring Rules for Better DOCX

- Use normal heading levels instead of visual-only headings.
- Use semantic captions such as `图 1` and `表 1` if automatic Word caption fields are not required.
- Keep tables simple when Word editability matters.
- Put figures in ordinary image blocks with captions.
- Keep citations in a Pandoc-supported citation format when bibliography automation is needed.
- Avoid relying on LaTeX-only spacing, page-break, font, and floating rules for the Word version.

## Verification

Always open or inspect the `.docx` after export and check:

- Page size and margins.
- Chinese and Latin fonts.
- Heading levels and table of contents behavior.
- Figure/table captions and numbering.
- Equations and symbols.
- Header, footer, and page numbering.

If Pandoc is not installed, say that DOCX export cannot be completed locally and provide the exact Pandoc command the user can run.
