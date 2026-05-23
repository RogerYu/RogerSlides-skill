# RogerSlides — CICC 投研 PPT 风格层

于钟海 CICC 卖方研报 PPT 风格规范，在 [PPT Master](https://github.com/anthropics/ppt-master) 生成引擎之上叠加品牌约束、投研内容规范和质量门禁。

## 概述

RogerSlides 不是独立的 PPT 生成引擎，而是 **ppt-master 的风格覆盖层**。它通过以下方式扩展 ppt-master：

- **Step 0（前置讨论）**：强制要求用户先讨论框架和目标，收集源文件，再用 analyst-avatar 对每页内容进行丰满，避免"空对空"生成
- **CICC 品牌规范**：颜色、字体、Logo 位置、页面模板
- **投研内容规范**：标题冒号+结论式、正文首句 bold、资料来源每页必有
- **页面模板**：封面页（左上角Logo）、内容页（右上角Logo）、Thank You 页（仅英文）、Section Divider（英文副标题37px）
- **Slop 黑名单**：禁止渐变填充、emoji 图标、毛玻璃效果、全画布白底 rect 等
- **布局约束**：translate ≥ 30、资料来源在 translate 外、图表标题高于最高数据标签、callout box 覆盖全部文字、水平条形图按 canvas 边界缩放

## 安装

```bash
# 1. 安装 ppt-master
# 参见 https://github.com/anthropics/ppt-master

# 2. 安装 RogerSlides 风格层
git clone https://github.com/RogerYu/RogerSlides-skill.git
# 将以下文件/目录复制到 ppt-master 对应位置：
#   SKILL_additions.md                    → 合并到 skills/ppt-master/SKILL.md（Step 0 + 风格规则）
#   references/shared-standards_additions.md → 追加到 skills/ppt-master/references/shared-standards.md（§9）
#   templates/brands/cicc/                 → skills/ppt-master/templates/brands/cicc/
#   patches/svg_to_pptx/                   → skills/ppt-master/scripts/svg_to_pptx/
```

## 补丁说明

### drawingml_converter.py
- **CSS font-family 注入**：解析 SVG `<style>` 中的 CSS 规则，将 font-family 属性注入到匹配元素，修复默认回退到 Segoe UI / Microsoft YaHei 的问题
- **卡片检测**：自动识别浅色背景 rect + accent bar + text 的卡片模式，合并为单一 PowerPoint shape

### drawingml_elements.py
- **rect+text 合并**：`convert_card_group()` 函数将背景 rect 和文字合并为一个 shape，设 `wrap="square"` + `spAutoFit`
  - 宽度锁死 rect 宽度 → 文字不溢出
  - 高度自适应 → 色块包容所有文字
  - 用户点击色块即可编辑文字

## 核心风格规则

- **主色**：CICC Deep Red `#640000`
- **字体**：SimHei (CJK) + Arial (Latin)，禁止微软雅黑/宋体/思源黑体
- **Logo**：封面页左上角 (x=0, y=9, w=161, h=68)；内容页右上角 (x=900, y=0, w=125, h=53)
- **正文**：首句 bold + 后续 regular，完整段落禁止碎片化
- **禁止**：渐变填充、emoji 图标、毛玻璃效果、全画布白底 rect

## 许可证

GPL-3.0 — 详见 [LICENSE](LICENSE)。

## 致谢

本项目基于 [PPT Master](https://github.com/anthropics/ppt-master) 构建。详见 [ACKNOWLEDGMENTS.md](ACKNOWLEDGMENTS.md)。
