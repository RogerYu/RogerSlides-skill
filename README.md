# RogerSlides — CICC 投研 PPT 风格层

于钟海 CICC 卖方研报 PPT 风格规范，在 [PPT Master](https://github.com/hugohe3/ppt-master) 生成引擎之上叠加品牌约束、投研内容规范和质量门禁。

**RogerSlides 不是独立的 PPT 生成工具**——它是 ppt-master 的"皮肤"。ppt-master 负责把文字变成 PPTX 文件，RogerSlides 负责让这个 PPTX 长得像中金的研报。

---

## 它能做什么？

用 Claude Code（或任何支持 Skill 的 AI 编程工具）说一句"帮我做一份关于 AI 伴侣赛道的研报 PPT"，RogerSlides 就会：

1. 和你讨论框架和核心论点
2. 搜索数据、丰满内容
3. 按中金品牌规范生成 4:3 SVG 幻灯片
4. 自动质检（字号层级、颜色合规、z-order、文字溢出）
5. 导出为可直接在 PowerPoint/WPS 中编辑的 `.pptx` 文件

生成的 PPTX 里每个文字框、矩形、图表都是**原生 PowerPoint shape**——不是截图，不是图片嵌入，你可以像编辑普通 PPT 一样逐字修改。

---

## 前置条件

- **Python 3.10+**
- **Claude Code**（Anthropic 的命令行 AI 工具）或 **OpenClaw**（开源 AI Agent 平台）
- **LibreOffice**（用于 Visual QA 时 PDF 转换，可选）
- **智谱 API Key**（用于 GLM-4V-Flash 视觉质检，可选）

---

## 安装

### 方式一：Claude Code 本地安装

```bash
# 1. 安装 ppt-master
npx skills add hugohe3/ppt-master
# 或者直接 clone：
git clone https://github.com/hugohe3/ppt-master.git
cd ppt-master
pip install -r requirements.txt

# 2. 安装 RogerSlides 风格层
git clone https://github.com/RogerYu/RogerSlides-skill.git

# 3. 把 RogerSlides 的文件覆盖到 ppt-master 对应位置
cd RogerSlides-skill

# SKILL.md 是完整的风格规范（给 AI 读的），复制到 ppt-master 目录
cp SKILL.md /path/to/ppt-master/skills/ppt-master/SKILL.md

# 补丁文件：修复 svg_to_pptx converter 的已知 bug
cp patches/svg_to_pptx/drawingml_converter.py /path/to/ppt-master/skills/ppt-master/scripts/svg_to_pptx/drawingml_converter.py
cp patches/svg_to_pptx/drawingml_elements.py /path/to/ppt-master/skills/ppt-master/scripts/svg_to_pptx/drawingml_elements.py

# CICC 品牌模板（封面背景、Logo、设计规范）
cp -r templates/brands/cicc /path/to/ppt-master/skills/ppt-master/templates/brands/cicc

# 投研内容补充规范
cp references/shared-standards_additions.md /path/to/ppt-master/skills/ppt-master/references/shared-standards_additions.md
```

### 方式二：OpenClaw 部署

OpenClaw 支持通过 Skill URL 直接加载外部技能。在 OpenClaw 的 skill 配置中添加：

```yaml
skills:
  - name: rogerslides
    type: url
    url: https://raw.githubusercontent.com/RogerYu/RogerSlides-skill/main/SKILL.md
```

同时需要确保 ppt-master 已在 OpenClaw 的 Docker 镜像中安装：

```dockerfile
# Dockerfile 片段
RUN git clone https://github.com/hugohe3/ppt-master.git /opt/ppt-master && \
    cd /opt/ppt-master && pip install -r requirements.txt

# 部署 RogerSlides 补丁
RUN git clone https://github.com/RogerYu/RogerSlides-skill.git /tmp/rogerslides && \
    cp /tmp/rogerslides/patches/svg_to_pptx/*.py /opt/ppt-master/skills/ppt-master/scripts/svg_to_pptx/ && \
    cp -r /tmp/rogerslides/templates/brands/cicc /opt/ppt-master/skills/ppt-master/templates/brands/ && \
    rm -rf /tmp/rogerslides
```

---

## 快速开始

安装完成后，在 Claude Code 中打开你的工作目录，直接说：

```
帮我做一份14页的PPT，主题是AI伴侣赛道，4:3比例
```

RogerSlides 会自动介入 ppt-master 的生成流程，应用中金品牌规范。

### 你也可以更具体：

```
做一份关于AI伴侣赛道的研究报告PPT，14页，4:3比例。
我已经有了框架文档和社区调研数据，稍后上传。
风格按中金研报PPT来，CICC品牌色，黑体+Arial字体。
```

### 生成的 PPTX 在哪？

```
ppt-master/projects/<你的项目名>/exports/
```

用 PowerPoint 或 WPS 直接打开即可编辑。

---

## 核心风格一览

| 规范 | 说明 |
|------|------|
| **主色** | CICC Deep Red `#640000`，不是一般的红色 |
| **字体** | 黑体（中文）+ Arial（英文），禁止微软雅黑/宋体/思源黑体 |
| **Logo** | 封面页左上角，内容页右上角，每页必有 |
| **标题** | 冒号+结论式，如"收入端：OpenAI与Anthropic均快速放量" |
| **正文** | 首句 bold（核心结论），后续 regular（支撑数据和上下文） |
| **资料来源** | 每页底部必须有，格式："资料来源：XXX，中金公司研究部" |
| **4:3 画布** | 1024×768 px，投研报告默认比例 |

### 禁止项

- 渐变填充、emoji 图标、毛玻璃效果
- 全画布白底矩形（PPT模板已白底，加 rect 会变成删不掉的底层 shape）
- 碎片化 bullet list（必须是完整段落）
- 中性标题（必须有结论）

---

## 补丁说明

RogerSlides 对 ppt-master 的 `svg_to_pptx` converter 做了两处 bug 修复：

### drawingml_converter.py

1. **CSS font-family 注入**：解析 SVG `<style>` 中的 CSS 规则，将 font-family 注入到匹配元素，修复 xml.etree 不解析 CSS 导致字体回退到 Segoe UI 的问题

2. **卡片检测增强**：自动识别浅色背景 rect + accent bar + text 的卡片模式，合并为单一可编辑 shape

3. **sib 变量泄漏修复**：删除了 accent bar 检测循环中泄漏到 card text 列表的变量引用，防止页面底部文字（如"资料来源"）被错误地复制进每个卡片

4. **容器 rect 跳过检测**：当候选 card 背景 rect 内部存在其他大 rect（宽>10px 且高>10px）时，跳过该 card 合并。这防止了容器级 rect 被误识别为简单卡片，避免深色标题栏 rect 覆盖白色文字的 z-order 问题

### drawingml_elements.py

- **rect+text 合并**：`convert_card_group()` 将背景 rect 和文字合并为一个 shape，设 `wrap="square"` + `spAutoFit`，宽度锁死 rect 宽度防止文字溢出，高度自适应

---

## 目录结构

```
RogerSlides-skill/
├── SKILL.md                          # 完整风格规范（给 AI 读的主要文件）
├── SKILL_additions.md                # 风格规范精简版
├── README.md                         # 你正在读的这个文件
├── LICENSE                           # GPL-3.0
├── ACKNOWLEDGMENTS.md                # 致谢
├── patches/
│   └── svg_to_pptx/
│       ├── drawingml_converter.py    # converter 补丁（含 bug 修复）
│       └── drawingml_elements.py     # elements 补丁（card 合并）
├── references/
│   └── shared-standards_additions.md # 投研内容补充规范
└── templates/
    └── brands/
        └── cicc/
            ├── cover_bg.png          # 封面背景图
            ├── design_spec.md        # CICC 品牌设计规范
            ├── header_logo.png       # 内容页 Header Logo
            └── logo.jpg              # CICC Logo 原图
```

---

## 常见问题

### Q: 生成的 PPTX 里白色文字看不到？

确保你使用了 `patches/svg_to_pptx/drawingml_converter.py` 的最新版本。旧版 converter 的 card detection 会把深色标题栏上的白色文字合并到错误的 shape 里，导致被遮挡。

### Q: 每个格子/卡片里都出现了"资料来源"？

同上——这是 converter 的 sib 变量泄漏 bug，已在本仓库的补丁中修复。

### Q: 我想用 16:9 而不是 4:3？

告诉 AI "16:9 比例"即可，RogerSlides 会自动切换。

### Q: 我不在中金，可以用吗？

当然可以。把 `templates/brands/cicc/` 替换成你自己公司的品牌资产（logo、配色、字体），修改 SKILL.md 中的颜色和字体配置即可。

### Q: OpenClaw 上跑需要什么特殊配置？

确保 Docker 镜像中安装了 ppt-master 的 Python 依赖和 RogerSlides 的补丁文件。Skill 本身通过 URL 直接加载 SKILL.md 即可，不需要额外注册。

---

## 许可证

GPL-3.0 — 详见 [LICENSE](LICENSE)。

## 致谢

本项目基于 [PPT Master](https://github.com/hugohe3/ppt-master) 构建。详见 [ACKNOWLEDGMENTS.md](ACKNOWLEDGMENTS.md)。
