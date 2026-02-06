# PDF Translate Skill

> 🌍 学术级PDF翻译工具 - 基于Claude Code的高质量翻译Skill

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Skill Version](https://img.shields.io/badge/version-3.0.0-blue.svg)](SKILL.md)
[![Claude Code](https://img.shields.io/badge/Claude_Code-compatible-green.svg)](https://claude.ai/)

## ✨ 特性

- 🎯 **学术级翻译质量** - 基于"翻译即重写"理念，杜绝翻译腔
- 📋 **三步翻译工作流** - 重写初稿 → 问题诊断 → 润色定稿
- 🎨 **智能字体混排** - 中文黑体 + 英文Helvetica，自动识别关键词
- 📑 **完整目录支持** - 显式目录处理，不丢失任何结构
- 🔄 **Markdown解析** - 完整支持标题、粗体、分页符等格式
- 🌐 **跨平台支持** - macOS、Windows、Linux自动字体检测

## 🚀 快速开始

### 安装依赖

```bash
pip3 install pdfplumber reportlab pypdf
```

### 基础使用

```bash
# 使用skill进行PDF翻译
python3 scripts/translate_pdf.py input.pdf -o output.pdf

# 完整工作流（含目录生成）
python3 scripts/generate_complete_pdf.py
```

### 在Claude Code中使用

1. 将skill安装到Claude Code的skills目录
2. 使用自然语言请求翻译：
   ```
   翻译这个PDF：report.pdf
   ```

## 📖 文档结构

```
pdf-translate/
├── SKILL.md                      # 核心工作流（202行）
├── VERSION_HISTORY.md            # 完整版本历史
├── LICENSE                       # MIT开源许可证
├── README.md                     # 本文件
├── scripts/
│   ├── translate_pdf.py          # 基础翻译脚本
│   └── generate_complete_pdf.py  # 完整工作流脚本
└── references/
    ├── translation-standards.md  # 翻译标准与三步工作流
    ├── font-configuration.md     # 字体配置与混排规则
    ├── troubleshooting.md         # 故障排除指南
    └── complete-example.md        # 完整代码示例
```

## 🎯 核心功能

### 1. 高质量翻译标准

遵循思果和余光中的"翻译即重写"理念：

- ✅ **化形合为意合** - 拆分长句，重组语序
- ✅ **化被动为主动** - 避免"被"字滥用
- ✅ **化抽象为具体** - 名词转动词
- ✅ **精简冗余** - 消除欧化表达

### 2. 智能字体混排

自动识别并应用中英文字体：

```python
# 示例：Claude Code 支持 RESTful API
# 中文使用STHeiti（黑体）
# 英文关键词（AI、API、Claude）使用Helvetica
```

### 3. 完整目录处理

使用显式数据结构确保目录不丢失：

```python
toc_items = [
    ("基础趋势", "构造性变革", "4"),
    ("", "趋势1：软件开发生命周期发生剧变", "4"),
]
```

### 4. Markdown解析

完整支持Markdown格式：

- `## 标题` → 一级标题
- `### 标题` → 二级标题
- `**粗体**` → 粗体文本
- `---` → 分页符

## 📚 使用示例

### 示例1：翻译学术报告

```python
from pdftranslate import translate_pdf

# 翻译PDF并保留格式
translate_pdf(
    input_path="report.pdf",
    output_path="report_translated.pdf",
    preserve_toc=True  # 保留目录
)
```

### 示例2：批量翻译

```bash
# 批量翻译多个PDF
for file in *.pdf; do
    python3 scripts/translate_pdf.py "$file" -o "translated_$file"
done
```

## 🎨 样式配置

### 字体优先级

**macOS**：STHeiti（黑体）> PingFang > Helvetica
**Windows**：Microsoft YaHei > SimHei > Helvetica
**Linux**：Droid Sans Fallback > WenQuanYi > Helvetica

### PDF样式

- **主标题**：24pt, #1a1a1a
- **一级标题**：18pt, #2563eb
- **二级标题**：16pt, #1e40af
- **正文**：11pt, #374151
- **页边距**：0.75英寸

## 🛠️ 故障排除

### 中文字体不显示

**问题**：PDF中文显示为方块

**解决方案**：
1. 检查系统是否安装中文字体
2. 使用 `--font` 参数指定字体路径
3. 确保字体文件包含中文字符集

### 目录丢失

**问题**：生成的PDF缺少目录

**解决方案**：
- 使用显式目录数据结构
- 参考 [troubleshooting.md](references/troubleshooting.md)

更多问题请查看 [故障排除指南](references/troubleshooting.md)。

## 📝 版本历史

- **v3.0.0** (2026-02-02) - 重大重构：渐进式披露优化
- **v2.3.0** - 完整工作流优化：Markdown解析
- **v2.2.0** - 特殊格式处理：目录解析修复
- **v2.1.0** - 字体配置优化：黑体+中英混排
- **v2.0.0** - 学术级翻译标准
- **v1.0.0** - 初始版本

详见 [VERSION_HISTORY.md](VERSION_HISTORY.md)

## 🤝 贡献

欢迎贡献！请随时提交Issue或Pull Request。

### 开发指南

1. Fork本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启Pull Request

## 📄 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件

## 🙏 致谢

- Claude Code团队提供的优秀AI编程工具
- pdfplumber和reportlab项目的优秀库
- 所有贡献者和用户的支持

## 📮 联系方式

- Issues: [GitHub Issues](https://github.com/yourusername/pdf-translate-skill/issues)
- Discussions: [GitHub Discussions](https://github.com/yourusername/pdf-translate-skill/discussions)

## 🌟 Star History

如果这个项目对你有帮助，请给个⭐️支持一下！

---

**Made with ❤️ by Claude Code + chrislee121 Collaboration**
