# RogerSlides — CICC 投研 PPT 风格层

于钟海 CICC 卖方研报 PPT 风格规范，在 [ppt-master](https://github.com/hugohe3/ppt-master) 生成引擎之上叠加品牌约束、投研内容规范和质量门禁。

## 安装

```bash
# 1. 安装 ppt-master
npx skills add hugohe3/ppt-master

# 2. 安装 RogerSlides
git clone https://github.com/RogerYu/RogerSlides-skill.git ~/.claude/skills/rogerslides

# 3. 应用 svg_to_pptx 补丁（卡片合并 + CSS font 注入）
cp patches/svg_to_pptx/drawingml_converter.py <ppt-master>/scripts/svg_to_pptx/
cp patches/svg_to_pptx/drawingml_elements.py <ppt-master>/scripts/svg_to_pptx/
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
- **Logo**：等距原则（右边距=上边距），4:3 画布 x=774, y=10
- **正文**：首句 bold + 后续 regular，完整段落禁止碎片化
- **禁止**：渐变填充、emoji 图标、毛玻璃效果、全画布白底 rect

详见 [SKILL.md](SKILL.md)。
