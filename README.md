# better-word

`better-word` 是一个 Codex skill，用 AI + LaTeX 替代容易漂移的 Word 课程报告排版流程，并在需要时导出 Word `.docx`。

核心思路是：不要让 AI 直接生成难以稳定排版的 Word。先把学校或课程的官方 Word/PDF/截图模板转换成格式规则，再让 AI 按这个模板输出 `.tex`，最后用 XeLaTeX、LuaLaTeX 或 Overleaf 编译成 PDF；如果必须提交 Word，则用 Pandoc 和 `reference.docx` 导出 `.docx`。

## 适合做什么

- 从学校官方 Word、PDF 或截图模板中提取页边距、字体字号、行距、标题层级、页眉页脚、图表编号和参考文献规则。
- 生成可复用的中文课程报告 LaTeX 模板。
- 让 AI 按固定模板写课程报告源码，而不是输出需要手动修的 `.docx`。
- 在需要提交 Word 时，从 LaTeX 或 Pandoc Markdown 导出 `.docx`。
- 编译并检查 PDF，减少 Word 合并、图片拖动、样式联动导致的格式问题。

## 安装

把这个仓库放到 Codex skills 目录下，目录名保持为 `better-word`。

在 GitHub 页面点击 `Code` 复制仓库地址，然后运行：

Windows：

```powershell
git clone <复制的仓库地址> "$env:USERPROFILE\.codex\skills\better-word"
```

macOS/Linux：

```bash
git clone <复制的仓库地址> ~/.codex/skills/better-word
```

如果已经下载为压缩包，也可以手动解压到对应目录：

```text
~/.codex/skills/better-word
```

或 Windows：

```text
C:\Users\<你的用户名>\.codex\skills\better-word
```

## 使用示例

分析学校模板：

```text
Use $better-word to analyze this official course report template and extract the formatting rules.
```

生成 LaTeX 模板：

```text
Use $better-word to generate a reusable LaTeX course report template from these extracted rules.
```

按模板写报告：

```text
Use $better-word to write a course report about <主题>. Return compilable .tex source and keep the template formatting rules.
```

同时导出 Word：

```text
Use $better-word to write a course report about <主题> and export both PDF and DOCX. Use the official Word template as reference.docx for the DOCX export.
```

## 依赖

必须：

- 支持 Codex skills 的 Codex 环境。

可选：

- 本地 TeX 工具链：TeX Live 或 MiKTeX。
- 编译命令：`latexmk -xelatex`，或运行 `xelatex` 两次。
- Word 导出：Pandoc；如果有官方 Word 模板，建议作为 `--reference-doc reference.docx` 使用。
- 在线替代方案：Overleaf，建议选择 XeLaTeX 或 LuaLaTeX 编译器。

中文报告建议优先使用 XeLaTeX 或 LuaLaTeX。模板中默认使用 `ctexart`，适合作为中文课程报告的起点；具体学校字体、字号和页边距应按官方模板审计结果调整。

DOCX 导出建议优先使用 Pandoc。复杂 LaTeX 宏、标题页、浮动图表和严格 Word 样式不一定能被 Pandoc 完整保留；这类场景应同时维护 `main.tex` 作为 PDF 源文件，以及 `main.md` 作为 Word 导出的 Pandoc Markdown 中间稿。

## 仓库结构

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
├── assets/
│   └── course-report-template.tex
├── references/
│   ├── template-analysis-checklist.md
│   └── word-export.md
├── LICENSE
└── README.md
```

## 验证

在本仓库根目录运行 skill 校验：

```powershell
python "$env:USERPROFILE\.codex\skills\.system\skill-creator\scripts\quick_validate.py" .
```

如果本机安装了 TeX 工具链，可以复制模板到临时目录后编译：

```powershell
Copy-Item .\assets\course-report-template.tex $env:TEMP\better-word-template.tex
Push-Location $env:TEMP
latexmk -xelatex -interaction=nonstopmode better-word-template.tex
Pop-Location
```

如果安装了 Pandoc，可以导出 Word：

```powershell
pandoc main.tex --from latex --to docx --output main.docx --reference-doc reference.docx
```

## 许可证

MIT
