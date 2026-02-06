---
name: pdf-translate
description: Translates PDF documents with academic-quality standards (rewrite-based workflow). Extracts text, applies three-step translation (rewrite→diagnose→refine), generates PDF with Chinese fonts (STHeiti) and English font mixing. Use when translating PDFs to Chinese with TOC/preservation needs.
---

# PDF Translation Skill

翻译PDF文档并生成新的PDF文件。支持提取PDF内容、按照顶尖学术翻译标准翻译文本内容，并使用中文字体生成新的PDF文档。

## 版本信息

**当前版本**: v3.0.0
**发布日期**: 2026-02-02
**作者**: Claude Code + 用户协作

## 更新记录

### v3.0.0 (2026-02-02) - 重大重构：渐进式披露优化

**核心改进**：
- 📁 按照skill-creator标准完全重构
- 📚 拆分详细内容到 `references/` 目录
- ✂️ 精简SKILL.md到核心工作流（<200行）
- 🔧 添加完整示例脚本到 `scripts/`

**新目录结构**：
```
pdf-translate/
├── SKILL.md (核心工作流，本文件)
├── scripts/
│   ├── translate_pdf.py (基础提取和生成)
│   └── generate_complete_pdf.py (完整工作流，含目录)
└── references/
    ├── translation-standards.md (翻译标准与三步工作流)
    ├── font-configuration.md (字体配置与混排规则)
    ├── troubleshooting.md (故障排除指南)
    └── complete-example.md (完整示例代码)
```

### v2.3.0 - v2.2.0 - v2.1.0 - v2.0.0 - v1.0.0
详见 [VERSION_HISTORY.md](VERSION_HISTORY.md) （包含所有版本的详细更新内容）

## 核心工作流

完整的PDF翻译工作流程分为三个步骤：

### Step 1: 提取PDF文本内容

使用pdfplumber从PDF中提取所有文本内容：

```python
import pdfplumber

pdf_path = "/path/to/input.pdf"

with pdfplumber.open(pdf_path) as pdf:
    for i, page in enumerate(pdf.pages):
        text = page.extract_text()
        print(f"=== PAGE {i+1} ===")
        print(text)
```

**⚠️ 重要：检查特殊格式内容**

提取文本后，必须检查是否包含需要特殊处理的内容：

1. **目录（TOC）**：包含省略号……、页码对齐等特殊格式
2. **索引**：字母顺序排列的词条列表
3. **参考文献**：格式化的引用列表
4. **图表目录**：图片或表格的清单

**识别方法**：
- 目录通常在文档开头1-3页
- 包含大量重复的特殊字符（如……、---等）
- 有规律的缩进和对齐

**处理建议**：
- 对于目录等特殊格式，使用显式数据结构而非自动解析
- 参考 [troubleshooting.md](references/troubleshooting.md) 的"目录或特殊格式内容丢失"解决方案

### Step 2: 翻译内容（核心）

**重要**：翻译质量是整个工作流程的关键。

**📋 详细翻译标准**：参见 [translation-standards.md](references/translation-standards.md)
- 翻译角色定位：顶尖的英汉学术翻译专家
- 三步翻译工作流：重写初稿 → 问题诊断 → 润色定稿
- 四大语言转换策略：形合→意合、被动→主动、抽象→具体、精简冗余
- 核心翻译原则：忠实性与地道性、术语处理、文体对等、格式保留

**快速参考**：

1. **步骤一：应用策略，重写初稿** - 深入理解原文，执行语言转换策略
2. **步骤二：自我批判与问题诊断** - 检查欧化病症、策略执行、表达逻辑
3. **步骤三：润色与定稿** - 全面优化，确保句子完整、意思清晰

### Step 3: 生成新PDF

使用reportlab生成中文PDF：

```python
from reportlab.lib.pagesizes import A4
from reportlab.lib.styles import getSampleStyleSheet, ParagraphStyle
from reportlab.lib.units import inch
from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer, PageBreak
from reportlab.lib.enums import TA_CENTER, TA_JUSTIFY
from reportlab.pdfbase import pdfmetrics
from reportlab.pdfbase.ttfonts import TTFont
from reportlab.lib.colors import HexColor

# 注册中文字体（优先使用黑体）
# 详见 font-configuration.md
chinese_font, english_font = register_fonts()

# 创建PDF
doc = SimpleDocTemplate("output.pdf", pagesize=A4,
                        leftMargin=0.75*inch, rightMargin=0.75*inch,
                        topMargin=0.75*inch, bottomMargin=0.75*inch)

# 定义样式并生成内容
# ...
```

**📚 详细配置指南**：参见 [font-configuration.md](references/font-configuration.md)
- 字体优先级：STHeiti（黑体）> PingFang > 其他
- 中英文字体混排规则
- 样式配置：颜色、字号、间距

## 完整示例脚本

### 基础翻译（简单场景）

```bash
# 使用基础脚本
python3 ${SKILL_DIR}/scripts/translate_pdf.py input.pdf -o output.pdf
```

### 完整工作流（含目录、Markdown解析）

```bash
# 使用完整工作流脚本
python3 ${SKILL_DIR}/scripts/generate_complete_pdf.py
```

**📖 完整代码示例**：参见 [complete-example.md](references/complete-example.md)
- Markdown解析函数
- 中英文字体混排
- 目录生成（显式数据结构）
- 粗体标签正确处理

## 故障排除

**❗ 常见问题**：

1. **目录丢失** → [troubleshooting.md](references/troubleshooting.md#目录或特殊格式内容丢失)
2. **中文字体不显示** → [troubleshooting.md](references/troubleshooting.md#中文字体不显示)
3. **HTML标签嵌套错误** → [troubleshooting.md](references/troubleshooting.md#html标签嵌套错误)
4. **PDF内容提取不完整** → [troubleshooting.md](references/troubleshooting.md#pdf内容提取不完整)

## 依赖安装

```bash
pip3 install pdfplumber reportlab pypdf
```

## Script Directory

所有脚本位于此skill的 `scripts/` 子目录中：

| 脚本 | 用途 |
|------|------|
| `scripts/translate_pdf.py` | 基础PDF提取和生成 |
| `scripts/generate_complete_pdf.py` | 完整工作流（含目录、Markdown解析） |

## 快速参考

### 翻译质量承诺

- ✅ 100%忠实原文信息，完全符合中文学术表达习惯
- ✅ 坚决杜绝"欧化表达"和"翻译腔"
- ✅ 严格执行三步工作流：重写初稿 → 问题诊断 → 润色定稿
- ✅ 产出读起来宛如中文原创的高质量译文

### 字体配置

- **默认中文**：STHeiti（黑体）
- **默认英文**：Helvetica
- **自动混排**：识别AI、API、Claude等英文关键词

### 工作流步骤

1. 提取PDF → 检查特殊格式
2. 高质量翻译（三步工作流）
3. 生成PDF（应用字体混排）

---

**需要更多信息？**
- 📖 [翻译标准](references/translation-standards.md)
- 🎨 [字体配置](references/font-configuration.md)
- 🔧 [故障排除](references/troubleshooting.md)
- 💻 [完整示例](references/complete-example.md)
