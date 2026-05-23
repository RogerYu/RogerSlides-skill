---
name: RogerSlides
description: >
  于钟海 CICC 卖方研报 PPT 风格层。在 ppt-master 生成引擎之上叠加中金品牌约束、
  投研内容规范、Slop 黑名单、三层质量门禁和 GLM-5V-Turbo 视觉质检。
  触发词："slides", "deck", "pptx", "路演", "PPT", "研究报告", "做一页",
  "做一份"，或任何要求生成投研演示文稿的请求。
  必须与 ppt-master 配合使用——本 Skill 不独立生成 PPTX，而是覆盖 ppt-master 的
  风格决策和质量检查。
---

# RogerSlides v2 — 于钟海 CICC 投研 PPT 风格层

## 前置依赖

**必须先安装 ppt-master**。RogerSlides 是 ppt-master 的风格覆盖层，不是独立生成引擎。

当请求生成 PPT 时，**先走 ppt-master 的完整工作流**（Step 1–7），但在 Strategist 阶段（Step 4）和 Executor 阶段（Step 6）覆盖本 Skill 的规则。

---

## Step 0: Framework Discussion & Content Enrichment（强烈建议）

**在 ppt-master Step 1 之前执行**。投资时间 upfront，产出更好的 deck。

**0.1 讨论框架和目标** — 要求用户明确：
- Deck 的**核心论点**（每个页面都应支持的一句话）
- **目标受众**和基调（卖方研究、内部更新、客户路演）
- **关键数据点**要突出的
- **页数目标**和结构偏好

⛔ BLOCKING — 等用户确认框架后再继续。清晰的框架避免 mid-generation 返工。

**0.2 收集源文件** — 要求用户上传所有相关材料：
- 原始报告、文件、数据表（PDF, DOCX, XLSX）
- 品牌资产（logo, 配色, 现有 PPTX 模板）
- 参考deck或视觉灵感
- 任何将出现在幻灯片上的数据

> 更多 context → 更丰富的幻灯片。AI 无法编造准确数据，它需要原始素材。

**0.3 Analyst Avatar 内容丰满** — 对每个计划页面，使用 analyst-avatar skill：
- 对照一手数据源（S-1 filing、业绩会、行业数据库）校验数据
- 增加分析深度（比较、趋势、含义），超越表面描述
- 标记需要用户输入或额外研究的数据缺口
- 生成页面级关键结论（每页一个 bold 结论）

> 这一步把"数据堆砌"deck 变成**分析叙事**——"收入 8B"和"收入 8B 但 AI 收入仅 0.8B，90% 来自正以 -15% YoY 萎缩的广告"之间的差距。

**✅ Checkpoint — 框架确认、素材收集完成、内容已丰满。进入 ppt-master Step 1。**

---

## 1. Canvas & Dimensions

| Format | Canvas | Aspect | Use Case |
|--------|--------|--------|----------|
| ppt43 | 1024×768 | 4:3 | CICC 投研报告（默认） |

---

## 2. Color Palette

| Role | HEX | Usage |
|------|-----|-------|
| primary | #640000 | 封面标题、Thank You 页、强调色 |
| secondary | #3B3B3B | 内容页标题、正文标题 |
| accent | #CBA97B | 法律声明装饰条、分隔线点缀 |
| text | #3B3B3B | 正文标题、小标题 |
| body | #1A1A1A | 正文内容 |
| bg | #FFFFFF | 内容页背景 |
| light-gray | #F2F2F2 | 表格交替行、辅助背景 |
| blue-gray | #8A90A5 | 辅助文字、次要信息 |
| orange | #E99753 | 数据高亮、正增长标识 |

> #640000 是中金模板中的实际品牌红色，比 #C7674B 更深更沉稳。

---

## 3. Typography

| Role | Family | Weight | Size |
|------|--------|--------|------|
| cover-title | 黑体 + Arial | Bold | 66pt |
| page-title | 黑体 + Arial | Bold | 40pt (SVG 24px) |
| body | 黑体 + Arial | Regular | 14-16pt |
| source-footer | SimHei + Arial | Regular | 10px |

> 黑体+Arial 是 CICC 当前通用字体规范。禁止思源黑体、宋体或任何衬线字体。

---

## 4. Logo Positioning

| 页面类型 | SVG坐标 | PPTX位置 |
|---------|-------------|---------|
| 封面 (P01) | x=0, y=9, w=161, h=68 | L=0", T=0.091", **左上角** |
| 内容页 (P02+) | x=900, y=0, w=125, h=53 | L=9.375", T=0", **右上角** |

> 封面和内容页的 Logo 位置、尺寸**不同**。

---

## 5. Page Templates

### 5.1 内容页 Header（4:3 1024×768px）

```xml
<image href="templates/logo.jpg" x="900" y="0" width="125" height="53"/>
<line x1="0" y1="93" x2="1024" y2="93" stroke="#BEC0C2" stroke-width="0.75"/>
<g id="pageNN">
  <text x="48" y="75" font-family="SimHei, Microsoft YaHei, Arial, sans-serif"
        font-size="24" font-weight="bold" fill="#3B3B3B">PAGE TITLE</text>
  <g transform="translate(0,30)">
    ... all body content ...
  </g>
  <text x="50" y="755" font-family="SimHei, Arial, sans-serif"
        font-size="10" fill="#8A90A5">资料来源：…</text>
  <text x="960" y="755" font-family="Arial, SimHei, sans-serif"
        font-size="10" fill="#8A90A5" text-anchor="end">NN</text>
</g>
```

### 5.2 封面页

```xml
<image href="templates/cover_bg.png" x="0" y="0" width="1024" height="768" preserveAspectRatio="xMidYMid slice"/>
<rect x="0" y="0" width="1024" height="768" fill="#FFFFFF" fill-opacity="0.7"/>
<image href="templates/logo.jpg" x="0" y="9" width="161" height="68"/>
<rect x="0" y="0" width="1024" height="6" fill="#640000"/>
<rect x="0" y="6" width="1024" height="2" fill="#CBA97B"/>
```

### 5.3 Thank You 页

- **不写"谢谢"** — 只用英文 "Thank You"
- **字号**: 32px bold centered
- **免责声明框**: 从 y=360 开始

### 5.4 Section Divider 页

- 英文副标题: **37px**（≈28pt in PPTX; SVG→PPTX 转换比 0.75）
- 三要素: 红色竖条 + 灰色横线 + 简述

---

## 6. Layout Constraints

1. **Logo 源图不裁剪** — 保持原始尺寸，通过 SVG x/y/w/h 控制显示
2. **translate ≥ 30** — 内容页衬线 y=93，首元素 y=70 → translate(0,30) 使 effective y=100
3. **资料来源在 translate 外** — 防止被 translate 推出页面底部
4. **图表标题高于最高数据标签** — title_y < min(data_label_y) − 10
5. **Callout box 覆盖全部内含文字** — 红色左衬线与 box 同 y/同 height
6. **水平条形图按 canvas 边界缩放** — bar_max_width = canvas_right - bar_start_x - margin
7. **Legend 对齐** — text y = rect_y + rect_height * 0.75

---

## 7. Content Rules

- **标题冒号+结论式** — "收入拆解：广告仍占90%但AI增速亮眼"
- **正文首句 bold** — 首句为核心结论，后续 regular
- **资料来源每页必有** — y=755，translate 组外部
- **完整段落禁止碎片化** — 不切成 bullet list，用自然段落
- **CICC Logo 每页必有** — 封面左上，内容页右上

---

## 8. Slop 黑名单 (Block)

以下在 RogerSlides 中**绝对禁止**：

| ID | 规则 | 类别 |
|----|------|------|
| S1 | 渐变填充（linearGradient/radialGradient 除外用于图片叠加） | 视觉 |
| S2 | Emoji 图标 | 视觉 |
| S3 | 毛玻璃/模糊效果 | 视觉 |
| S4 | 全画布白底 `<rect>` | 布局 |
| S5 | 微软雅黑/宋体/思源黑体字体 | 字体 |
| S6 | 无资料来源的页面 | 内容 |
| S7 | 无 CICC Logo 的页面 | 内容 |
| S8 | 标题非结论式 | 内容 |
| S9 | 正文碎片化（bullet list 替代段落） | 内容 |

---

## 9. Quality Gates

1. **svg_quality_checker.py** — 0 errors
2. **Slop 黑名单** — 全部 block 规则通过
3. **Visual QA** (GLM-5V-Turbo) — 每页 ≥5/7 PASS，最多3轮修复

---

## Integration with ppt-master

RogerSlides 在 ppt-master 流水线中的插入点：

| ppt-master Step | RogerSlides 覆盖 |
|-----------------|------------------|
| Before Step 1 | **Step 0**: 框架讨论 + 源文件收集 + Analyst Avatar 丰满 |
| Step 3 (Template) | 自动加载 CICC brand spec |
| Step 4 (Strategist) | Eight Confirmations 中应用 CICC 配色/字体/Logo规则 |
| Step 6 (Executor) | 应用页面模板、布局约束、Slop 黑名单 |
| Post Step 6 | Visual QA（GLM-5V-Turbo） |
