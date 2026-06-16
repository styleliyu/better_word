# Template Analysis Checklist

Use this checklist when converting an official Word/PDF/screenshot report template into LaTeX rules.

## Source Priority

1. `.docx`: inspect `word/styles.xml`, `word/document.xml`, `word/settings.xml`, `word/numbering.xml`, and header/footer XML when needed.
2. PDF: inspect page geometry and visually compare rendered output.
3. Screenshots: extract visible typography, spacing, and structure; mark uncertain values.

## Formatting Fields

Record findings in this shape:

```markdown
## Template Audit

- Page: A4; margins left/right/top/bottom = ...
- Body: Chinese font = ...; Latin font = ...; size = ...; line spacing = ...; paragraph indent = ...
- Title: ...
- Headings:
  - Level 1: ...
  - Level 2: ...
  - Level 3: ...
- Header/footer: ...
- Page numbers: ...
- Figures: caption position/name/numbering = ...
- Tables: caption position/name/numbering = ...
- Formulas: numbering = ...
- References: citation style = ...; bibliography format = ...
- Assumptions: ...
```

## Chinese Font Size Reference

Common mapping:

| 中文字号 | pt |
| --- | ---: |
| 小五 | 9 |
| 五号 | 10.5 |
| 小四 | 12 |
| 四号 | 14 |
| 小三 | 15 |
| 三号 | 16 |
| 小二 | 18 |
| 二号 | 22 |

## Extraction Notes

- In Word, styles can cascade. Check both direct formatting and paragraph/character styles.
- If a screenshot and `.docx` disagree, treat `.docx` as source of truth and use the screenshot to identify rendering differences.
- For Chinese reports, verify whether the school requires 宋体, 黑体, 仿宋, 楷体, or a platform-specific replacement.
- For Overleaf, prefer XeLaTeX with `ctex`; upload required images and bibliography files alongside `main.tex`.
- Mark unknown values instead of guessing silently.
