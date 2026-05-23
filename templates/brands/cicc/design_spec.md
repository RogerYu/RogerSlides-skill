---
brand_id: cicc
kind: brand
summary: 中国国际金融股份有限公司（CICC）投研演示文稿品牌规范
keywords: [cicc, 中金, 投研, 卖方研究, research]
primary_color: "#640000"
---

# CICC 品牌规范

> Identity-only preset. 页面布局由 layout template 或 Executor 自由组合，本文件仅锁定品牌视觉身份。

## I. Brand Overview
| Property | Value |
|---|---|
| Brand Name | 中金公司 CICC |
| Use Cases | 卖方研究报告 PPT、路演演示、行业深度汇报 |
| Tone | formal, institutional, data-driven |

## II. Color Scheme
| Role | HEX | Provenance | Usage |
|---|---|---|---|
| primary | #640000 | [fact] PPTX text fill | 封面标题、Thank You 页、强调色 |
| secondary | #3B3B3B | [fact] PPTX text fill | 内容页标题、正文标题 |
| accent | #CBA97B | [fact] PPTX shape fill | 法律声明/特别声明装饰条、分隔线点缀 |
| text | #3B3B3B | [fact] | 正文标题、小标题 |
| body | #1A1A1A | [user] | 正文内容（默认深色） |
| bg | #FFFFFF | [fact] | 内容页背景 |
| light-gray | #F2F2F2 | [user] | 表格交替行、辅助背景色 |
| blue-gray | #8A90A5 | [user] | 辅助文字、次要信息 |
| orange | #E99753 | [user] | 数据高亮、正增长标识 |

> 注意：#640000 是中金模板中的实际品牌红色，比记忆中的 #C7674B 更深、更沉稳，符合投研机构调性。

## III. Typography
| Role | Family | Weight | Size |
|---|---|---|---|
| cover-title | 黑体 (CJK) + Arial (Latin) | Bold | 66pt |
| cover-subtitle | 黑体 + Arial | Normal | 32pt |
| cover-date | 黑体 + Arial | Normal | 22pt |
| section-title | 黑体 + Arial | Bold | 48pt |
| page-title | 黑体 + Arial | Bold | 40pt |
| subtitle | 黑体 + Arial | Bold | 32pt |
| body | 黑体 + Arial | Regular | 14-16pt |
| legal-body | 黑体 + Arial | Regular | 8pt |
| declaration-title | 黑体 + Arial | Bold | 36pt |

> 黑体+Arial 是 CICC 当前通用字体规范。2021年旧模板用思源黑体，已废弃。禁止使用思源黑体、宋体或任何衬线字体。

## IV. Logo

### CICC Logo（右上角）
- File: `./logo.jpg`
- Usage: **every-page**
- Position: x≈8.2", y≈-0.1"（略超出顶部边缘，视觉上紧贴右上角）
- Size: ≈1.6" × 1.2"
- Provenance: ~/Desktop/Claude/CICC Logo.jpg

### 中金研究部 Header（左上角）
- File: `./header_logo.png`
- Usage: **every-page**（封面页除外）
- Position: x≈0.4", y≈0.4"
- Size: ≈1.8" × 0.3"
- Provenance: 从 CICC PPTX 模板提取

### 封面背景
- File: `./cover_bg.png`
- Usage: cover-only
- Position: (0,0), full canvas 10.0" × 7.5"
- Provenance: 从 CICC PPTX 模板提取

## V. Voice & Tone
- Formality: formal
- Person: we（"我们认为"、"我们预计"）
- Emoji: forbidden
- Abbreviations: spell-out-first（首次出现全称+括号缩写，后续可用缩写）

## VI. Icon Style
- Preference: linear（线性图标，符合投研专业调性）

## VII. Signature Design Elements
- 封面：深红底图 + 白/深红标题 + 金色装饰线 + 封面专用Logo位置（与内容页不同）
- 法律声明/特别声明：居中标题 + #CBA97B 金色矩形装饰条（宽约 2-3.6"，高 0.2"）
- Thank You 页：**只保留英文"Thank You"（32px bold），删除"谢谢"**。免责声明框从页面中部开始
- Section Divider 页：中文大标题（附录/核心等）+ 英文副标题 **37px**（≈28pt in PPTX）+ 灰色衬线

### VII-A. 内容页 Header 模板（4:3 = 1024×768px，强制）

以下坐标为 SVG 像素坐标（4:3 画布），所有内容页必须严格遵守：

```
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

| 元素 | 位置 | 备注 |
|------|------|------|
| CICC Logo | x=900, y=0, w=125, h=53 | 原始源图不裁剪，右1/4自然溢出Canvas |
| 页标题 | x=48, y=75, 24px bold | 向下对齐（baseline锚定y=75）；溢出时首行向上 |
| 衬线（分隔线） | y=93, 全宽, #BEC0C2 0.75px | 标题与内容区的硬分界 |
| 内容区 | translate(0,30) | 确保首元素 effective y ≥ 100（衬线下方≥7px） |
| 资料来源 | y=755，translate组**外部** | 防止被translate推到页面外 |
| 页码 | y=755 right-aligned，translate组**外部** | 同上 |

**关键约束**：
1. **Logo 源图不裁剪** — `logo.jpg` 保持原始尺寸（752×318），通过 SVG 的 x/y/w/h 定位控制显示区域
2. **translate ≥ 30** — 衬线 y=93，首内容元素通常 y=70，translate(0,30) 使 effective y=100，确保内容在衬线下方
3. **资料来源在 translate 外** — 避免 translate 推高导致底部文字溢出页面
4. **图表标题高于最高数据标签** — 标题 y = 最高数据标签 y − 15px（见 shared-standards §9）
5. **Callout box 覆盖全部内含文字** — 红色左衬线与 box 同 y/同 height（见 shared-standards §9）
6. **水平条形图必须按canvas边界缩放** — bar_max_width = canvas_right - bar_start_x - margin; 所有bar按 max_val 等比缩放; 满宽bar的数据标签用白色文字放在bar内部（见 shared-standards §9.6）

### VII-B. 封面页模板（4:3 = 1024×768px）

封面Logo位置与内容页**不同**——封面Logo在**左上角**：

```
<image href="templates/cover_bg.png" x="0" y="0" width="1024" height="768" preserveAspectRatio="xMidYMid slice"/>
<rect x="0" y="0" width="1024" height="768" fill="#FFFFFF" fill-opacity="0.7"/>
<image href="templates/logo.jpg" x="0" y="9" width="161" height="68"/>
<rect x="0" y="0" width="1024" height="6" fill="#640000"/>
<rect x="0" y="6" width="1024" height="2" fill="#CBA97B"/>
```

**封面 vs 内容页 Logo 对比**：

| 页面类型 | Logo SVG坐标 | PPTX位置 |
|---------|-------------|---------|
| 封面 (P01) | x=0, y=9, w=161, h=68 | L=0", T=0.091", **左上角** |
| 内容页 (P02+) | x=900, y=0, w=125, h=53 | L=9.375", T=0", **右上角** |

> Roger手动校准确认：封面Logo放在左上角（比内容页更大更靠左），内容页Logo放在右上角。两者位置和尺寸不同。

### VII-C. Thank You 页模板

```
<image href="templates/logo.jpg" x="900" y="0" width="125" height="53"/>
<line x1="0" y1="93" x2="1024" y2="93" stroke="#BEC0C2" stroke-width="0.75"/>
<g id="pageNN">
  <!-- 只保留英文，删除"谢谢" -->
  <text x="512" y="280" font-family="Arial" font-size="32" font-weight="bold" fill="#3B3B3B" text-anchor="middle">Thank You</text>
  <line x1="412" y1="300" x2="612" y2="300" stroke="#CBA97B" stroke-width="2"/>
  <!-- 免责声明框从 y=360 开始 -->
  ...
</g>
```

**关键约束**：
- **不写"谢谢"** — Thank You 页只用英文标题
- **Thank You 字号 32px bold** — 比之前的 20px 更大，因为是唯一标题
- **免责声明框上移** — 从 y=360 开始，而不是 y=400

### VII-D. Section Divider 页模板

```
<image href="templates/logo.jpg" x="900" y="0" width="125" height="53"/>
<line x1="0" y1="93" x2="1024" y2="93" stroke="#BEC0C2" stroke-width="0.75"/>
<g id="pageNN">
  <text x="48" y="75" font-family="SimHei" font-size="24" font-weight="bold" fill="#3B3B3B">中文标题</text>
  <g transform="translate(0,30)">
    <rect x="50" y="260" width="6" height="80" fill="#640000"/>
    <text x="72" y="332" font-family="SimHei, Arial" font-size="14" fill="#8A90A5">English subtitle</text>
    <line x1="72" y1="348" x2="400" y2="348" stroke="#BEC0C2" stroke-width="1"/>
    <text x="72" y="382" font-family="SimHei, Arial" font-size="13" fill="#3B3B3B">简述内容</text>
  </g>
</g>
```

**关键约束**：
- **英文副标题 37px**（≈28pt in PPTX，Roger手动校准；SVG→PPTX转换比0.75）
- 红色竖条 + 灰色横线 + 简述 构成分隔页三要素
- Logo: `./logo.jpg`
- Header: `./header_logo.png`
- Cover background: `./cover_bg.png`
