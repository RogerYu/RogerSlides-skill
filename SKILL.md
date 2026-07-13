---
name: RogerSlides
description: >
  于钟海 CICC 卖方研报 PPT 风格层。在 ppt-master 生成引擎之上叠加中金品牌约束、
  投研内容规范、Slop 黑名单、三层质量门禁和 Kimi K2.6 视觉质检。
  触发词："slides", "deck", "pptx", "路演", "PPT", "研究报告", "做一页",
  "做一份"，或任何要求生成投研演示文稿的请求。
  必须与 ppt-master 配合使用——本 Skill 不独立生成 PPTX，而是覆盖 ppt-master 的
  风格决策和质量检查。
---

# RogerSlides v2 — 于钟海 CICC 投研 PPT 风格层

## 前置依赖

**必须先安装 ppt-master**。RogerSlides 是 ppt-master 的风格覆盖层，不是独立生成引擎。

```bash
# 安装 ppt-master（如果尚未安装）
npx skills add hugohe3/ppt-master
# 或
git clone https://github.com/hugohe3/ppt-master.git
pip install -r requirements.txt
```

当 Roger 请求生成 PPT 时，**先走 ppt-master 的完整工作流**（Step 1–7），但在 Strategist 阶段（Step 4）和 Executor 阶段（Step 6）覆盖本 Skill 的规则。

---

## 1. Canvas & Dimensions

**默认 4:3**，用户要求 16:9 或其他尺寸时按用户要求。

| Format | Width | Height | ppt-master format | 用途 |
|--------|-------|--------|-------------------|------|
| 4:3（默认） | 10.0" | 7.5" | `ppt43` | 投研报告标准 |
| 16:9 | 10.0" | 5.625" | `ppt169` | 宽屏演示 |

- **可用内容区**: x: 0.4"–9.6", y: 0.85"–(height-0.5")
- **顶部边距**（header line 下方）: ~0.85"–0.95"
- **底部边距**（source citation 上方）: ~(height-0.6")
- **左右边距**: 0.4"–0.5"

在 ppt-master Strategist 阶段的 Eight Confirmations 中，canvas format 默认填 `ppt43`，用户指定时覆盖。

---

## 2. CICC Brand Color System

```
CICC Brand Colors (from CICC PPTX template 2021-04-06):
  CICC Deep Red  RGB(100, 0, 0)      #640000   ← Primary, cover titles, Thank You, bars (FACT from PPTX)
  CICC Gold      RGB(203, 169, 123)  #CBA97B   ← Accent, legal declaration bars (FACT from PPTX)
  CICC Orange    RGB(233, 151, 83)   #E99753   ← Tertiary accent, callouts, positive data
  CICC Blue-Gray RGB(138, 144, 165)  #8A90A5   ← Quaternary, muted labels
  CICC Lt Gray   RGB(190, 192, 194)  #BEC0C2   ← Structural dividers, rules
  Dark Gray      RGB(59, 59, 59)     #3B3B3B   ← Content page titles (FACT from PPTX)
  CICC Navy      RGB(31, 73, 125)    #1F497D   ← Timeline keywords (FACT from PPTX theme)
```

### Semantic color mapping

| Element | Color | Hex |
|---------|-------|-----|
| Cover / section title text | CICC Deep Red | `#640000` |
| Content page title | Dark Gray | `#3B3B3B` |
| Chart/panel sub-title | CICC Deep Red | `#640000` |
| Body paragraph text | Near-black | `#1A1A1A` |
| Dates / keywords in timeline | CICC Navy | `#1F497D` |
| Source citation text | Theme gray | ~50% opacity |
| Horizontal rule | Light gray | `#BEC0C2` |
| Legal declaration accent bar | CICC Gold | `#CBA97B` |
| Slide background | White | `#FFFFFF` |
| Cover / section divider | Full-bleed image | — |

### Entity color convention

- US / OpenAI: CICC Navy `#1F497D`
- Google / Alphabet: dark blue-gray `#8A90A5`
- Anthropic: warm gold `#DEC9AC`
- Chinese companies: CICC Deep Red `#640000` or CICC Gold `#CBA97B`

在 ppt-master Strategist 阶段的 confirmation (e) Color Scheme 中，直接填入上述调色板。**禁止使用调色板以外的颜色作为主色**。

---

## 3. Typography

### Font stack

| Script | Font | Weight | Fallback |
|--------|------|--------|---------|
| Latin / English / numbers | **Arial** | Bold / Regular | Calibri |
| CJK (Chinese) | **黑体** (Heiti) | Bold / Regular | Microsoft YaHei |

> 黑体+Arial 是 CICC 当前通用字体规范（2021年旧模板用思源黑体，已废弃）。
> **禁止使用 SimSun、宋体、思源黑体或任何衬线字体。**

### Size scale (from CICC template)

| Element | pt | Weight | Color |
|---------|-----|--------|-------|
| Cover title | 66pt | Bold | `#640000` |
| Cover subtitle | 32pt | Normal | `#640000` |
| Cover date | 22pt | Normal | `#640000` |
| Section title | 48pt | Bold | `#3B3B3B` |
| Page title | 40pt | Bold | `#3B3B3B` |
| Subtitle | 32pt | Bold | `#3B3B3B` |
| Chart sub-title | 12pt | Bold | `#640000` |
| Body paragraph | 14–16pt | Light | `#1A1A1A` |
| Timeline annotation | 10pt | Bold labels, regular description | per entity color |
| Source citation | ~8pt | Regular | theme gray |
| Legal body | 8pt | Light | `#1A1A1A` |
| Declaration title | 36pt | Heavy | `#3B3B3B` |

### Text emphasis convention

Roger 的正文段遵循 **两阶段粗体模式**：首句 bold（核心结论），后续 regular（支撑数据和上下文）。

> **OpenAI全面加速商业化，收入预期继续抬高。** OpenAI在2025年下半年的算力建设以及商业化层面的动作都相当激进，因此……

每段分析性文字必须复制此模式。禁止全 bold 或全 regular 正文。

### Technical terms

英文产品名、指标、缩写始终保留英文：`OpenAI`, `Anthropic`, `API`, `ARR`, `Coding`, `Agent`, `RDMA`, `InfiniBand`, `NVLink`, `SaaS`。禁止翻译。

### Global style consistency (全局样式一致)

**核心铁律**：字体颜色、加粗、斜体、box颜色、字号都不是装饰，它们传达信息层级。**同样层级、同样类型的元素必须严格一致。**

在写任何 SVG 之前，先定义全局样式映射表：

| 元素类型 | 呈现方式 | 字号(px) | 字重 | 颜色 | 容器 |
|---------|---------|---------|------|------|------|
| 关键判断/核心结论 | 统一浅粉box | 14-16 | Bold标题+Regular正文 | #640000标题 #3B3B3B正文 | #FFF5F5 bg + 4px #640000 left bar |
| 核心数字callout | 无box纯数字 | 36-48 | Bold | #E99753 | 无背景 |
| 图表标题 | 左对齐+灰衬线 | 14 | Bold | #640000 | + #BEC0C2 line |
| 正文段落 | 12-13px完整句子 | 12-13 | Regular | #1A1A1A | 无 |
| 注释/说明 | 10-11px | 10-11 | Regular | #8A90A5 | 无 |
| 资料来源 | 10px | 10 | Regular | #8A90A5 | 无 |

**写每页 SVG 前必须自问**：前几页的"关键判断"是什么样式？必须沿用，不能每页重新发明。

### Prohibited: fragmented language (禁止碎片化语言)

PPT是报告，不是bullet list。所有文字区必须用**完整段落**，不是"关键词 | 关键词 | 关键词"。

- **禁止**用 `|` 分隔的流水账格式（如 `速度↑35x | 延迟↓42% | 能耗↓40%`）
- **禁止**没有主语没有逻辑的碎片（如 `Artificial Analysis: Kimi K2.6 (#6), MiMo V2.5 (#7)`）
- **必须**用完整句子：`推理速度提升35倍，延迟下降42%，能耗降低40%，部署成本仅为英伟达方案的三分之一`
- "一句话总结"不是真的只写一句话，是完整的一小段分析

### Callout 位置规则

核心数字 callout（如倍数、比率）不能压在图表数据区上方。位置规则：
- **横向柱状图** → callout 放在柱状图右侧空白区
- **竖向柱状图** → callout 放在两个柱子的上方中间
- **数据卡片** → callout 作为独立数字行放在卡片下方

---

## 4. CICC Logo

**Logo 文件**: `~/Desktop/Claude/CICC Logo.jpg`

**位置**: 每一页右上角。Content slides、section dividers、cover slides 均须包含。

**等距原则**：logo上沿到canvas顶边距离 = logo右边到canvas右边距离。这是logo最舒服的视觉位置。4:3画布(1024×768)下：CICC Logo宽高比约5:1，height=45px时width≈225px，因此right=10px, top=10px即可满足等距。

**尺寸**: 宽 ~1.1"–1.4", 高 ~0.18"–0.22"。位置: x≈8.2"–9.2", y≈0.1"。

**水平分隔线**: Logo 下方，全宽，颜色 `#BEC0C2`，粗细 ~0.75pt，y≈0.35"–0.40"。

**页码**: 右下角或底部居中（section divider 用大章节号）。

在 ppt-master `spec_lock.md` 的 logo 字段中填入此路径，确保每页 SVG 都引用此 logo。

---

## 5. Slide Structure

### Standard content slide

```
┌──────────────────────────────────────────────────┐
│ ────────────────────────────── (rule)  [CICC Logo]│
│                                         右上角    │
│ 主题领域：具体结论 (26.5pt bold #3B3B3B)         │
│                                                   │
│ [Chart 1 sub-title bold maroon]                   │
│ [Chart 1 data area]              [Chart 2 sub-t]  │
│                                   [Chart 2 data]  │
│                                                   │
│ **Bold lead sentence 核心结论。** Regular text     │
│ with supporting data and English terms inline.    │
│                                                   │
│ 资料来源：Source1，Source2，中金公司研究部 (~8pt)  │
└──────────────────────────────────────────────────┘
```

### Section divider

```
┌──────────────────────────────────────────────────┐
│ [Full-bleed background — dark/gradient]           │
│ [CICC Logo — 右上角]                              │
│ 中金研究部                                         │
│                                                   │
│           第 X 章                                 │
│           章节标题                                 │
└──────────────────────────────────────────────────┘
```

### Cover slide

```
┌──────────────────────────────────────────────────┐
│ [Full-bleed background image]                     │
│ [CICC Logo — 右上角]                              │
│                                                   │
│ Report Title (large)                              │
│ 副标题                                            │
│                                                   │
│ YYYY.MM  中金公司研究部                            │
│ 于钟海  执行总经理，计算机行业首席分析师            │
└──────────────────────────────────────────────────┘
```

---

## 6. Layout Patterns

在 ppt-master Executor 阶段，按内容类型选择布局：

| Content type | Layout | Key specs |
|-------------|--------|-----------|
| 双图+文字 | Two-chart | 两图并排 (~4.0" each)，0.3" 间隔，各带 maroon sub-title，下方正文横跨全宽 |
| 全宽图+文字 | Full-width chart | 单图占 60–70% 高度，sub-title 在图上方，1–2段正文在图下 |
| 时间轴 | Timeline/Roadmap | 水平轴横穿中部，日期标记在轴上方（bold，按实体着色），事件描述在下方 |
| 四象限 | 2×2 matrix | 轴标签 CICC Blue-Gray，象限标签 CICC Red 或深色文字 |
| 数据表格 | Table | 表头 CICC Red 背景 + 白字 bold，交替行白/浅灰 `#F5F5F5` |

---

## 7. Slide Title Convention

**冒号分隔两段式标题**，格式：

```
主题领域 / 分类标签：具体结论或核心发现
```

标题即结论。禁止中性描述性标题。

Examples:
- `收入端：OpenAI与Anthropic均快速放量`
- `Model领域的竞争格局：分四象限看，中国模型处于"便宜大碗"象限为主`
- `C端市场：OpenAI目前仍然领先，但Google加速追赶`

---

## 8. Source Citation

每页内容 slide 底部：

```
资料来源：[Source1]，[Source2]，中金公司研究部
```

~8pt, regular, theme gray，左对齐，y≈6.9"–7.1"。英文 source 保留英文。

---

## 9. Writing Tone & Content Conventions

- **语言**: 中文正文 + 英文术语内嵌。禁止全英文正文。
- **声调**: 卖方分析师——直接、assertive、投资导向。非学术腔。
- **数字**: 中文单位（亿、万）+ 阿拉伯数字（130亿美元，非一百三十亿美元）
- **投资框定**: 结论必须明确市场含义（"利好"、"压力更大"、"竞争格局"）
- **预测标注**: 估值年份用 E 后缀（2026E），标题中不用"预测"
- **双语 sub-title**: 图表标签可双语（`ChatGPT vs Gemini的月活用户趋势`）

---

## 9.5 Research Protocol — 内容填充的研究流程

Framework/大纲只是骨架，**内容填充必须通过搜索补充实质性数据和观点**。禁止仅凭 LLM 内部知识填满整页。

### 搜索工具优先级

**核心原则：区分公开信息 vs 私有数据，选择正确通道。**

| 数据类型 | 推荐通道 | 示例 |
|---------|---------|------|
| **公司特定信息**（财务、估值、盈利预测、公告） | **Private data 优先** | 阿里Capex、DeepSeek估值、MiniMax营收 |
| **行业指标**（增速、份额、市场规模） | **Private data 优先** | AI推理Token消耗、中国MaaS ARR |
| **卖方观点/调研纪要** | **Private data 优先** | 中金研报、3C计算机纪要 |
| **通用宏观/技术趋势** | Public search 即可 | Chatbot Arena排名、Stanford HAI报告 |
| **学术论文/开源项目** | Public search 即可 | arxiv论文、GitHub数据 |
| **新闻/事件** | Public search 即可 | 产品发布、融资新闻 |

**Private data 通道**（公司/行业/投研专有）：

| 优先级 | 工具 | 用途 |
|--------|------|------|
| 1 | 点睛 MCP (`dianjing_mcp`) | **13个子工具，必须按需选用，不要只会 web_search** |
| 2 | AlphaPai (`alphapai-research`) | recall检索、公司一页纸、投资逻辑、业绩点评 |
| 3 | IMA (`ima-skill`) | 卖方研报、调研纪要（3C计算机纪要库、浑水调研、长安投研） |
| 4 | gBrain (`gbrain`) | 个人知识库（recency: strong + since 近3月） |
| 5 | kdocs (`kdocs-cli`) | 云盘文档兜底 |

**点睛 MCP 子工具详解**（13个，按场景选用）：

| 子工具 | 类型 | 用途 | 使用场景 |
|--------|------|------|---------|
| `fetch_cicc_report_meeting` | **Private** | 中金研报/会议搜索 | 查中金官方研报——最权威的公司/行业深度观点 |
| `fetch_other_securities_report` | **Private** | 其他券商研报 | 非中金研报补充，交叉验证 |
| `fetch_public_notes` | **Private** | 纪要/公告 | 公司公告原文、电话会纪要、调研纪要 |
| `ind_data` | **Private** | 行业数据 | 行业指标具体数值（市场规模、增速、份额） |
| `ind_search` | **Private** | 行业搜索 | 搜索行业分类和行业名称 |
| `query_indicators_by_info_mcp` | **Private** | 指标查询（按信息） | 用关键词查行业指标（如"AI推理Token消耗量"） |
| `query_quarter_indicators_mcp` | **Private** | 季度指标查询 | 时间序列行业数据（如季度Capex趋势） |
| `query_stock_fin_model_data` | **Private** | 公司财务模型 | 盈利预测、收入拆分、利润表（公司特定） |
| `query_stock_model_date` | **Private** | 模型数据日期 | 查某公司财务模型最新更新日期 |
| `query_stock_valuation_data` | **Private** | 估值数据 | PE/PB/PS/EV/EBITDA、Forward multiples（公司特定） |
| `web_search` | **Public** | 通用网页搜索 | **等同于公开搜索，不是Private data！最后手段** |

**点睛 MCP 使用规则**：
- `web_search` = Public search，**不算** Private data。用它搜出来的东西和点睛WebSearch/Tavily没区别。
- 公司特定数据（Capex、营收、估值、盈利预测）→ **必须用** `query_stock_fin_model_data` + `query_stock_valuation_data`
- 行业指标（市场规模、增速）→ **必须用** `ind_data` + `query_indicators_by_info_mcp`
- 研报/纪要 → **必须用** `fetch_cicc_report_meeting` 或 `fetch_other_securities_report`
- `web_search` 只在上述 Private 工具都查不到时使用，**不是默认选项**

**Public search 通道**（通用信息）：

| 优先级 | 工具 | 用途 |
|--------|------|------|
| 6 | 点睛 WebSearch | 通用网页搜索（非Private数据场景） |
| 7 | Tavily | 深度研究（额度有限时节约） |

**判断规则**：涉及具体公司名+财务数据 → 走 Private；涉及行业排名/通用趋势 → Public 即可；不确定 → **先 Private 再 Public 补充**。

### Extensive Research 原则

**每一页数据密集型 slide，必须跑通所有相关数据源**，不能只搜一个就停：

1. **公司数据**：dianjing_mcp (fin_model + valuation + cicc_report) → AlphaPai (公司一页纸/投资逻辑) → IMA (3C纪要库) → gBrain → kdocs → web_search
2. **行业数据**：dianjing_mcp (ind_data + ind_search + indicators) → AlphaPai (recall) → IMA → gBrain → web_search
3. **观点/框架**：dianjing_mcp (cicc_report + other_report) → IMA → gBrain → web_search → 学术论文

**禁止行为**：只跑 web_search 就宣称"搜索完成"。每次搜索必须覆盖至少 2 个 Private data 源 + 1 个 Public 源，才算 extensive。

### 学术论文搜索

学术论文的 Introduction 和 Conclusion 段落包含高质量的框架性结论和图表，对投研内容填充极有价值。通过 web search 搜 `[topic] + paper / research / arxiv` 获取。

### Subagent 研究策略

- 每一页内容 slide 在填充前，评估是否需要额外研究素材
- 需要时**开 Subagent** 执行搜索，不阻塞主流程
- Subagent 返回的结构化数据直接注入该页 SVG 的图表区或正文
- 搜索必须 **extensive**——宁可多搜少用，不可搜不够就硬编

### 搜索触发条件

- Framework 中只有标题/框架、无具体数据 → 必须搜索
- 涉及市场规模、增速、份额、预测 → 必须搜索
- 涉及公司财务/估值 → 必须搜索（dianjing_mcp 优先）
- 涉及技术对比/性能指标 → 优先搜学术论文
- 连续 2 页以上无数据支撑 → 触发搜索补充

---

## 10. Content Density & Layout Integrity

### Text spill prohibition

文字不得溢出容器。任何文字元素必须在指定区域内：
- 正文段落不得超出底边距（y≈6.85"）
- 图表标题不得重叠图表区
- 表格单元格不得超出列边界

**溢出处理**: 内容过长时，缩小字号（最低 10pt）、拆页、或精简文字。禁止文字渗入边距、重叠其他元素、或延伸到页面边界外。

### Blank space prohibition

投研内容页追求 **packed**——整页要塞得饱满，这是 CICC 研报 PPT 的视觉标志。任何 slide 不得有超过可用内容区 ~20% 的连续空白区域。

| Problem | Fix |
|---------|-----|
| 图表下方大面积空白 | 扩大图表尺寸、添加第二个图表、或增加分析文字段 |
| 标题与内容间距过大 | 上移内容、或添加 subtitle/key-takeaway 行 |
| 半页空白仅一个小图 | 加第二个图、扩大第一图、或并排数据表 |
| 底部三分之一空白 | 加底部摘要栏、关键指标条、或来源上下文段落 |

**原则**: 可用内容区每一平方英寸都应服务于叙事。呼吸留白可以，懒惰留白不行。

### Data richness

纯文字、框架和示意图的 deck 不是 CICC 研报 PPT——那是提纲。**每 3 页内容 slide 至少 1 页包含数据驱动图表**（非纯示意图）。

数据缺乏时：
1. **先搜索** — 用 §9.5 搜索工具链找相关数据
2. **从数据建图** — 在 SVG 中用手工 SVG 图表生成柱状图/折线图/饼图
3. **dianjing_mcp 数据建图** — 行业指标、估值数据、盈利预测 → 直接绘图，这是最高质量来源
4. **无数据时** — 创建结构化对比表或量化框架（市场规模 waterfall、竞争评分矩阵）

### JS 动态渲染网站的数据获取

很多高质量数据网站（如 Artificial Analysis）内容是 JS 动态渲染的，无法直接爬取 HTML：
1. **先尝试搜索公开数据** — web search 搜 `[site] + data / benchmark / results`，看是否有 API 或公开 CSV
2. **提示用户截图** — 告知用户"XX 网站的数据很有价值，建议打开截图给我"，用户截图后用 GLM-5V-Turbo 解析
3. **解析截图插入 PPT** — 用 GLM-5V-Turbo 识别截图中的数据和图表结构，转成 SVG 图表插入

### 图片插入规范

- **严禁调整长宽比** — 图片变矮/变胖是 PPT 最常见的丑化原因
- 图片必须保持原始 aspect ratio，通过裁剪或留白适配
- 如果图片尺寸无法适配布局，用 GLM-5V-Turbo 判断后做轻微裁剪
- 可爬取的公开数据源（如 SWE Bench 排行榜、Artificial Analysis API）优先于截图

---

## 10.5 Layout Precision Rules — 布局精度硬规则

以下规则是从实际产出中反复犯的错误提炼的。**每条都是血的教训，不是建议。**

### 字号层级强制执行

| 元素级别 | 字号(px, 1024×768 canvas) | 字重 | 颜色 |
|---------|--------------------------|------|------|
| 页面标题 | 26–28px | Bold | `#3B3B3B` |
| 页面副标题 | 26–28px | Bold | `#640000` |
| 图表/面板标题 | 14–15px | Bold | `#640000` |
| 图表标题衬线 | — | — | `#BEC0C2` 1px line |
| 数据标签（大数字）| 18–22px | Bold | 品牌色 |
| 正文 | 12–13px | Regular | `#1A1A1A` |
| 注释/说明 | 10–11px | Regular | `#8A90A5` |
| 资料来源 | 10px | Regular | `#8A90A5` |

**铁律**：同一级别的元素字号必须完全一致。图表标题全页同字号，正文全页同字号，注释全页同字号。关键判断/核心结论的字号必须 > 普通正文。字号差异传达信息层级——乱用字号 = 乱传达信息。

### 图表标题灰色衬线

每个图表/面板的标题下方必须加一条 1px 灰色线（`#BEC0C2`），横跨图表数据区宽度。这是区分"图表标题"和"普通文字"的关键视觉信号。

```xml
<!-- 图表标题 + 衬线 -->
<text x="50" y="180" font-size="14" font-weight="bold" fill="#640000">图表标题</text>
<line x1="50" y1="185" x2="480" y2="185" stroke="#BEC0C2" stroke-width="1"/>
```

### 对齐精度

- **左右并排元素**：上边缘严格对齐，下边缘尽量对齐，中间不留缝隙
- **上下堆叠元素**：左边缘严格对齐（x 坐标一致）
- **全页网格**：所有内容块应该看起来是填在看不见的格子里，不是浮在页面上
- **图表标题**：左对齐，不居中（除非是全宽横幅的居中标题）

### 图表边界隔离

左右并排的图表，必须设置明确的中线分隔。左图的右边界 = 中线左移几px，右图的左边界 = 中线右移几px。**左图数据区不得侵入右图区域**。

推荐分栏比例：
- 左 50% / 右 50%：x=50–490 / x=530–970
- 左 45% / 右 55%：x=50–440 / x=470–970
- 给中间留 20–40px 间隔

### 禁止多余色块衬底

数字 callout（如 "20-40x"、"2.11x"、"23x"）本身已通过大字号+品牌色足够醒目，**不需要额外的色块背景**。色块衬底只在以下情况使用：
- 作为信息卡片的完整背景（如英雄数字卡片 4/5、41%、62%）
- 作为结论框的淡色背景（#FFF5F5 或 #F5F5F5）

单独的倍数/百分比数字，靠字号和颜色本身传达视觉权重，不加框。

### 图例位置

图例不得与数据区域重叠。放在图表右上角空白处，或数据区下方右侧。

### Packed 执行标准

"Packed"不是"没有 >20% 空白"这种最低标准。Packed 是**像专业投研PPT那样，每一个角落都有内容，元素间距紧凑一致，没有懒惰的空隙**。

具体执行：
- 图表/卡片之间的纵向间距 ≤ 15px
- 图表标题到图表数据的间距 ≤ 8px
- 结论框紧贴上方内容，不留 >20px 的间隙
- 页面底部不要有大段空白——如果内容不够，加大图表或加补充文字
- 左右两栏的垂直方向要"见缝插针"，不要一栏到头另一栏半空

### 禁止白底全屏 rect

**PPT模板本身已是白底**，在 SVG 中加 `<rect width="1024" height="768" fill="#FFFFFF"/>` 会被 ppt-master 转成一个覆盖全页的可编辑矩形 shape，挡在所有内容最底层，用户在 PowerPoint 中无法轻松删除。**禁止在 SVG 中添加全画布白底矩形。**

### 闭合图必须视觉闭合

飞轮、循环、回路等"闭合"类示意图，箭头必须形成**完整闭环**。如果只有单向推进箭头，没有返回箭头，那就不是飞轮——Roger 会说"怎么叫飞轮？"。

具体执行：
- 画出所有节点后，追踪箭头路径，确保最后一个节点有箭头回到第一个节点
- 返回箭头用 `<path>` 画 L 形或 U 形，避免与主流程箭头交叉
- 在返回箭头旁可加小标签（如"闭环"）强化视觉信号

### 框内内容必须居中

红色/深色背景框内的文字、图标、流程图等元素，必须在框内水平居中。如果右侧空间明显多于左侧，说明内容偏左了——这是 QA 必查项。

---

## 11. Slop Blacklist — 投研交付物禁止模式

以下规则是**结构性**的，非建议性。severity=block 命中后该页必须重新生成（最多 3 次重试，prompt 中注入违规规则 ID 和描述）。severity=warn 标注但不阻断。

| ID | Name | Severity | Detection |
|----|------|----------|-----------|
| `cicc-serif-font` | 衬线字体 | block | font-family 含 SimSun/Songti/serif/宋体/思源黑体 |
| `cicc-16x9-canvas` | 非预期画布 | block | slide width≠10"（4:3 默认）或用户未明确要求 16:9 时使用非 4:3 |
| `cicc-neutral-title` | 标题无结论 | block | 标题不含冒号，或冒号后无实质性判断 |
| `cicc-missing-source` | 缺少资料来源 | block | 底部无"资料来源"或无"中金公司研究部" |
| `cicc-all-regular-body` | 正文无粗体引导 | block | body paragraph 全部 regular，无 bold lead |
| `cicc-purple-teal` | 非 CICC 品牌色 | block | 主色包含 #800080/#008080/#6A5ACD 或其他非 §2 调色板颜色 |
| `cicc-bullet-only` | 纯要点无正文 | block | slide 只有 bullet points 无 paragraph text |
| `cicc-no-logo` | 缺少 CICC logo | block | 页面右上角无 CICC Logo |
| `cicc-gradient-fill` | 渐变填充 | block | 任何元素使用 gradient fill |
| `cicc-emoji-icon` | emoji 当图标 | block | slide 中出现 emoji 字符作为视觉元素 |
| `cicc-glassmorphism` | 毛玻璃效果 | block | 半透明模糊叠加效果 |
| `cicc-text-spill` | 文字溢出 | block | 文字超出容器边界或重叠其他元素 |
| `cicc-large-blank` | 大面积留白 | block | 可用内容区 >20% 连续空白 |
| `cicc-placeholder-text` | 占位文本残留 | block | Lorem ipsum / "在此输入" / placeholder text |
| `cicc-accent-underline` | 标题下装饰线 | warn | title 下方独立装饰性下划线（非 header 区分隔线） |
| `cicc-translated-term` | 英文术语中文化 | warn | 英文术语的中文音译（如"奥佩恩艾"代 OpenAI） |
| `cicc-pill-button` | 药丸形标签 | warn | 全圆角 pill 元素 |
| `cicc-uniform-cards` | 等尺寸卡片网格 | warn | 内容区仅由等尺寸卡片组成，无视觉层级变化 |
| `cicc-all-english-body` | 全英文正文 | warn | body paragraph 全英文（术语和数字除外） |
| `cicc-no-data` | 纯文字无数据 | warn | 连续3页以上无任何图表、数据表或量化指标 |
| `cicc-font-size-mismatch` | 字号层级混乱 | block | 同级别元素字号不一致（如图表标题不同字号、正文不同字号） |
| `cicc-chart-title-center` | 图表标题居中 | block | 图表/面板标题居中对齐，应左对齐 |
| `cicc-chart-no-underline` | 图表标题缺衬线 | block | 图表/面板标题下方无灰色衬线（#BEC0C2 line） |
| `cicc-chart-invasion` | 图表越界 | block | 左右并排图表，左图侵入右图区域 |
| `cicc-callout-bg-bloat` | 数字callout多余衬底 | warn | 独立数字（倍数/百分比）加了多余色块背景 |
| `cicc-unpacked-gap` | 松散间距 | block | 元素间纵向间距>20px，页面有大段空白但内容不够packed |
| `cicc-misaligned-grid` | 网格未对齐 | block | 并排元素上/下/左边缘不对齐，看起来歪歪扭扭 |
| `cicc-fragmented-lang` | 碎片化语言 | block | 用 `|` 分隔流水账、无主语无逻辑的碎片文字 |
| `cicc-style-inconsistent` | 同层级样式不一致 | block | 同类型元素（如关键判断）在不同页用不同颜色/box/字号 |
| `cicc-callout-overlap` | callout压图表 | block | 数字callout覆盖在图表数据区上方 |
| `cicc-white-bg-rect` | 白底全屏rect | block | SVG中包含全画布白底矩形（模板已白底，rect会变成不可删的shape） |
| `cicc-unclosed-loop` | 飞轮未闭合 | block | 循环/飞轮示意图箭头未形成闭环 |
| `cicc-off-center-box` | 框内内容偏移 | block | 深色/彩色背景框内的文字或图标未水平居中 |

### Slop check execution

在 ppt-master 的 SVG Quality Check（Step 6）之后、Post-processing（Step 7）之前，对每页 SVG 逐条检查上表所有规则：
1. 所有 `block` 规则必须通过。命中 → 重新生成该页 SVG，prompt 注入"规避 [ID]: [描述]"。最多 3 次。
2. `warn` 规则记录但不阻断。
3. 3 次重试仍命中 `block` → 标记该页为 `quality-failed`，报告给用户并列出具体违规。

---

## 12. Forced Visual Constraints (Prompt Injection)

以下约束在 ppt-master Executor 阶段（Step 6）生成每页 SVG 之前注入，不可违反：

**禁止项**:
- 禁止使用 CICC 调色板及辅助色（#640000, #CBA97B, #E99753, #8A90A5, #BEC0C2, #3B3B3B, #1F497D, #1A1A1A, #F5F5F5, #FFFFFF）以外的颜色作为填充色、背景色或主题色
- 禁止自行决定 font-family（Latin=Arial, CJK=黑体，无例外）
- 禁止自行决定画布尺寸（默认 4:3 即 10"×7.5"，用户要求时按指定尺寸）
- 禁止在标题下方添加装饰性线条
- 禁止使用渐变填充
- 禁止使用 emoji 作为视觉元素
- 禁止使用毛玻璃/半透明模糊效果
- 禁止文字溢出容器——内容太长时缩小字号（最低10pt）、拆页、或精简文字
- 禁止在 SVG 中添加全画布白底矩形——PPT模板已白底，rect会变成底层不可删的shape

**必须项**:
- 每页必须有 CICC Logo（`~/Desktop/Claude/CICC Logo.jpg`）在右上角（x≈8.2"–9.2", y≈0.1"）
- 每页必须有水平分隔线（logo 区域下方）
- 每页必须有资料来源（含"中金公司研究部"）
- 标题必须含冒号，冒号后为结论性判断
- 正文首句必须 bold
- 表格表头必须 CICC Red 背景 + 白字
- 可用内容区不得有 >20% 连续空白

---

## 13. Quality Gate (Pre-Delivery)

### L1 — 结构完整性（block 级，缺任何一项即阻断交付）

- [ ] 画布 = 10" × 7.5"（4:3 默认）或用户指定尺寸
- [ ] 每页有 CICC Logo（右上角，`~/Desktop/Claude/CICC Logo.jpg`）
- [ ] 每页有水平分隔线
- [ ] 每页有资料来源（含"中金公司研究部"）
- [ ] 标题含冒号且冒号后有结论
- [ ] 无占位文本残留
- [ ] 无文字溢出

### L2 — 视觉合规（block/warn 混合）

- [ ] 所有主色在 CICC 调色板范围内 → block
- [ ] 字体仅 Arial(Latin) + 黑体(CJK) → block
- [ ] 正文首句 bold → block
- [ ] 无渐变填充 → block
- [ ] 无 emoji 图标 → block
- [ ] 无装饰性下划线 → warn
- [ ] 表格表头 CICC Red → warn
- [ ] 无大面积空白（>20%）→ block
- [ ] 每3页至少1页含数据图表 → warn

### L3 — 投研语义（可选，按需触发）

- [ ] 标题结论与正文论证逻辑一致
- [ ] 数据引用有来源标注
- [ ] Forward 估值标注 E 后缀
- [ ] 英文术语未翻译成中文

---

## 14. Visual QA Protocol (Kimi K2.6 via DashScope)

在 ppt-master Post-processing & Export（Step 7）完成后、交付前，执行视觉质检。

### ⚠ 核心教训：Visual QA 是最后防线，标准必须极高

过往错误：用宽松 prompt 给出 60+/70 分，但实际输出有大量布局缺陷。根本原因是 QA prompt 问的是"有没有严重问题"（二元判断），而不是"是否达到专业投研PPT水准"（质量判断）。**不能把"没有FAIL"等同于"质量达标"。**

### 为什么用 Kimi K2.6 而不是 GLM-5V-Turbo

2026年6月实测结论：
- **MMMU-Pro benchmark**：Kimi K2.5 得分 78.5，Qwen2.5-VL-72B 只有 51.1，Kimi 视觉能力全面碾压
- **Zhipu API Key 经常认证失败**（401），DashScope API 稳定可用
- **Kimi K2.6 支持 vision**（base64图片输入），时延 ~10-20s/页
- GLM-5V-Turbo 是思考模型，需读 reasoning_content，解析复杂；Kimi 直接返回 content

### API 配置

```
Endpoint: https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions
API Key: 从环境变量 DASHSCOPE_API_KEY 读取（存于 ~/.ppt-master/.env，勿硬编码进文件）
Model: kimi-k2.6
```

### Step 1: SVG → PNG（关键：字体替换）

**⚠ 核心教训：SimHei 字体在 macOS 上无法被 cairosvg 渲染！**必须先替换为系统中文字体再转 PNG，否则中文全部显示为方框。

```python
import cairosvg, os

svg_dir = "<project>/svg_final"
qa_dir = "<project>/qa"
os.makedirs(qa_dir, exist_ok=True)

# ⚠ 必须替换字体！SimHei 在 macOS 上 cairosvg 找不到，中文会变方框
for f in sorted(os.listdir(svg_dir)):
    if not f.endswith('.svg'):
        continue
    with open(os.path.join(svg_dir, f), 'r') as fh:
        content = fh.read()
    # 替换 SimHei 为 macOS 可用的中文字体
    content = content.replace(
        'SimHei, Arial, sans-serif',
        'Heiti SC, PingFang SC, Arial, sans-serif'
    )
    temp_svg = os.path.join(qa_dir, f"temp_{f}")
    with open(temp_svg, 'w') as fh:
        fh.write(content)
    cairosvg.svg2png(url=temp_svg, write_to=os.path.join(qa_dir, f.replace('.svg', '.png')), scale=2)
```

**字体说明**：
- SVG 中写 `SimHei` 是给 PPTX 用的（Windows/Office 有 SimHei）
- PNG QA 时替换为 `Heiti SC` 或 `PingFang SC`（macOS 可用）
- **SVG 源文件保持 SimHei 不变**，只在 QA 转 PNG 时临时替换

### Step 2: 图片压缩（必做！）

**⚠ 核心教训：原始 PNG base64 可能 500KB+，直接发给 API 会超时或被限流。**必须压缩到 800px 宽 JPEG。

```python
from PIL import Image
import io, base64

img = Image.open(png_path).convert("RGB")  # RGBA→RGB，否则 JPEG 保存报错
w, h = img.size
if w > 800:
    img = img.resize((800, int(h * 800 / w)), Image.LANCZOS)
buf = io.BytesIO()
img.save(buf, format="JPEG", quality=80)  # ~46KB，API 可承受
img_b64 = base64.b64encode(buf.getvalue()).decode()
```

### Step 3: 并行 QA（5线程，必做！）

**⚠ 核心教训：串行 23 页每页 20s = 460s，并行 5 线程 = 62s，7x 加速。**不要串行逐页跑。

```python
import os, requests
from concurrent.futures import ThreadPoolExecutor, as_completed

# DASHSCOPE_API_KEY 从环境变量读取，存于 ~/.ppt-master/.env，勿硬编码进文件
qa_prompt = '''你是投研PPT视觉质检专家。严格质检此PPT，只报问题：
1.文字溢出 2.对齐 3.空白>20% 4.图表可读性 5.CICC Logo/分隔线 6.文字缺失
JSON:{"issues":[{"type":"","description":"","severity":"high/medium/low"}]}
无问题则{"issues":[]}'''

def qa_one(slide_num):
    # ... compress image (Step 2) ...
    payload = {
        "model": "kimi-k2.6",
        "messages": [{"role": "user", "content": [
            {"type": "image_url", "image_url": {"url": f"data:image/jpeg;base64,{img_b64}"}},
            {"type": "text", "text": qa_prompt}
        ]}],
        "max_tokens": 512, "temperature": 0.1
    }
    headers = {
        "Authorization": f"Bearer {os.environ['DASHSCOPE_API_KEY']}",
        "Content-Type": "application/json"
    }
    resp = requests.post(
        "https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions",
        headers=headers, json=payload, timeout=90
    )
    # ... parse JSON response ...

with ThreadPoolExecutor(max_workers=5) as executor:
    futures = {executor.submit(qa_one, i): i for i in range(1, total_slides+1)}
    for future in as_completed(futures):
        result = future.result()
        # ... process result ...
```

**为什么 5 线程**：DashScope API 限流窗口内 5 个并发稳定，10+ 开始有 429 错误。

### Step 4: 严格版 QA Prompt（7维度）

当需要更严格的质量检查时，使用以下 prompt：

```
你是顶级投行PPT的视觉设计总监。标准：这页PPT放在中金/高盛投研报告里，会不会被合伙人说"重做"？

逐项 PASS/FAIL：
A. 字体统一性：整页只有黑体(CJK)+Arial(Latin)？同级字号一致？
B. Packed密度：内容塞满？间距紧凑？角落无浪费？
C. 对齐精度：并排元素上下对齐？堆叠元素左对齐？像填在网格里？
D. 图表边界：并排图表水平分离？标题左对齐+灰衬线？
E. 字号层级：核心结论字号>正文？同级一致？
F. 色块滥用：数字callout无背景色块？多余色块去掉更干净？
G. 图例位置：图例在右侧/上方，不与数据区重叠？

输出：A:[PASS/FAIL] B-G同 总计X/7 PASS
FAIL项详细说明
```

### Step 5: Fix and re-verify

- 有 High severity → 修改 SVG → 重跑 finalize_svg + svg_to_pptx → 重跑 QA
- **Medium severity 图表可读性问题需区分**：
  - 图片在 800px 压缩后模糊 ≠ PPTX 中模糊，仅修确认在 PPTX 中也模糊的
  - 原图分辨率不足 → 需要换高清源图或重绘为 SVG
- 最多 3 轮 Visual QA
- **每轮 QA 后必须自问**：我是不是又在"差不多就行了"？标准够不够高？

### QA 常见问题速查表

| 问题类型 | 根因 | 修复方法 |
|---------|------|---------|
| 中文显示为方框 | macOS 无 SimHei，cairosvg 渲染失败 | QA转PNG时替换为 Heiti SC，SVG源文件保持SimHei |
| API 401/超时 | 图片base64过大，或并发过高 | 压缩到800px JPEG quality=80，5线程并行 |
| RGBA→JPEG 报错 | PIL 不支持 RGBA 直接存 JPEG | 先 `.convert("RGB")` |
| 大面积空白 | 图片 preserveAspectRatio 留白 | 用 `xMinYMin slice` 或手动调 width/height |
| 文字溢出/截断 | 正文过长超出容器 | 缩短文字或加大容器高度 |
| 图表模糊(QA中) | 800px压缩损失，非PPTX问题 | 仅修确认在PPTX中也模糊的；PPTX用SimHei渲染正常 |

---
## 15. Revision Protocol

当用户对已生成的 PPTX 提出修改要求时：

- **增量修改** — 只修改指定页/元素，不重新生成整个 deck
- 常见修订：换配色、改文字、增删页面、调整布局/图表类型
- 修改后仅对修改页重跑 Quality Gate（L1 + L2）
- 修改后仅对修改页重跑 Visual QA（GLM-5V-Turbo）
- 用户说"重新做"或"从零开始"时才全量重新生成

---

## 16. CICC Brand Template

### 品牌模板位置

已创建完成，路径：`skills/ppt-master/templates/brands/cicc/`

包含文件：
- `design_spec.md` — 品牌规范（颜色、字体、Logo、声调）
- `logo.jpg` — CICC Logo（右上角）
- `header_logo.png` — "中金研究部" header（左上角，内容页）
- `cover_bg.png` — 封面背景图

品牌数据来源：`/Users/rogeryu/Desktop/Temp Works/中金PPT新模板1-20210406.pptx`（CICC 官方模板），所有颜色和字体为 `[fact]` 级提取。

### 使用 CICC 模板生成 PPT

在请求 ppt-master 生成时，在消息中包含模板路径：

```
用这个品牌模板做一份 AI 行业展望 PPT：
/Users/rogeryu/Desktop/Claude/ppt-master/skills/ppt-master/templates/brands/cicc/
```

ppt-master 会自动将品牌模板的颜色、字体、Logo 注入项目，RogerSlides 的其余规则（Slop 黑名单、内容密度、Quality Gate、Visual QA）由本 Skill 覆盖。

---

## 17. Parallel Workflow Protocol — 并行化工作流

**适用条件**：deck 页数 ≥ 5 时必须使用并行工作流。≤4 页时串行即可。

### 总体架构

```
主agent
  ├─ Phase 1: 准备（串行，主agent独自完成）
  │   ├─ 读取所有SVG + 用户反馈
  │   ├─ 输出 global_styles.md 到项目目录
  │   └─ 为每页生成结构化修改指令
  │
  ├─ Phase 2: SVG修改（并行，sub-agent）
  │   ├─ Batch 1: 5个sub-agent并行改 slide 01-05
  │   ├─ 汇总检查
  │   └─ Batch 2: 4个sub-agent并行改 slide 06-09
  │
  ├─ Phase 3: Pipeline（串行，主agent）
  │   ├─ finalize_svg.py
  │   └─ svg_to_pptx.py
  │
  ├─ Phase 4: Visual QA（并行，sub-agent）
  │   └─ 每页1个sub-agent，并行跑GLM-5V-Turbo QA
  │
  └─ Phase 5: 跨页一致性检查（串行，主agent）
      ├─ 读取所有已修改SVG
      ├─ 逐项比对同层级元素的样式一致性
      └─ 不一致→定向修改→重跑pipeline
```

### Phase 1: 准备 — 输出全局样式规范

主agent在改任何SVG之前，**必须**先输出 `global_styles.md` 到项目根目录。这份文件是所有sub-agent的不可违反约束。

```markdown
# Global Styles — [项目名]

## 元素样式映射表
| 元素类型 | 字号(px) | 字重 | 颜色 | 容器 |
|---------|---------|------|------|------|
| 页面标题 | 26 | Bold | #3B3B3B | 无 |
| 页面副标题 | 26 | Bold | #640000 | 无 |
| 核心结论框-标题 | 14 | Bold | #640000 | #FFF5F5 bg + 4px #640000 left bar |
| 核心结论框-正文 | 13 | Regular | #3B3B3B | 同上 |
| 图表标题 | 14 | Bold | #640000 | + #BEC0C2 1px underline |
| 核心数字callout | 36-48 | Bold | #E99753 | 无背景 |
| 正文段落 | 12-13 | Regular | #1A1A1A | 无 |
| 注释/说明 | 10-11 | Regular | #8A90A5 | 无 |
| 资料来源 | 10 | Regular | #8A90A5 | 无 |
| 中栏accent bar box | — | — | — | #F5F5F5 bg + 3px colored left bar |
| 表格表头 | 10-11 | Bold | #FFFFFF | #640000 bg |

## 禁止项
- 禁止全画布白底rect
- 禁止|分隔的碎片化语言
- 禁止数字callout加色块背景
- 禁止飞轮/循环图箭头不闭合
- 禁止框内内容不居中

---

## 27. 实战经验（2026-06 AI框架培训30页，多轮大改复盘）

### 27.1 截图根治：提取原PPT图表对象，不要整页截图

原PPT图表是图片对象（python-pptx `shape_type==13`），提取出来干净无logo/source。整页截图（删header/footer）删不干净，用户会看到残留logo/source反复返工。

提取脚本：`for sh in slide.shapes: if sh.shape_type==13: open(fn,'wb').write(sh.image.blob)`，跳过小logo（`w<200*9525 and h<80*9525`）。

### 27.2 资料来源：内部PPT不写，只写"中金公司研究部"+具体来源

未公开发表的内部PPT（Reshuffle/启示录/AI Outlook）不能写在资料来源。写"中金公司研究部"+图的原始数据来源（OpenRouter/Ramp AI/METR/Stack Overflow等）。用原PPT图的页，source写原图对应slide的来源。

### 27.3 图表选择：原报告有图优先贴原图，手画仅补充

原报告图表专业准确，手画易出错（意思不对/比例错/丑）。贴原图前用zai确认图内容（slide文字推断不可靠，曾把"模型采用率折线"误当"收入饼图"、"Codex对比柱"误当"ARR曲线"）。zai一次一张，prompt问"图表类型/数据/关键数字"。

### 27.4 口号页禁忌：杜绝大字口号放中间

用户痛恨"大字口号往中间一放"，要数据密集、论点论据论证。breathing页也要有数据支撑。好的breathing：大数字+一句解读+来源，非空洞口号。

### 27.5 图表类型与比例

- 成本拆解用饼图（占比），不用柱图
- 金字塔/漏斗比例正确：值大的层必须宽（4万亿块不能比6000亿块小）
- 纵轴范围合理：数据0-50%不要画到100%（线不明显）
- 横向条形按值比例最清晰

### 27.6 packed：左右顶到边

用户表扬P16四卡"左右顶到头非常漂亮"，要求其他页参考。示意图/卡片/产业链图左右顶到边（40-984），撑满最好看。

### 27.7 两图并排统一容器

并排两图统一容器（相同width×height），方正对仗。容器浅灰rect底+图meet居中，大小一致。

### 27.8 封面：官方背景+SAC编号

不自创封面（深红rect），用中金官方cover背景（从原PPT提取s01_图片2 1280x720）。封面加SAC执证编号+SFC CE Ref（从原PPT封面提取）。

### 27.9 Excel测算表是数据源

PPT图是图片对象（提取），Excel是数据源（openpyxl读数据重绘）。原PPT没有的图表，可读Excel数据SVG重绘。

### 27.10 live preview用run_in_background

nohup &会被harness回收，run_in_background持续运行。server watch svg_output，改SVG自动刷新。

### 27.11 口径净化（录音录像流传）

只说认知不说坏话，不点名批评任何公司。具身智能等讲产业趋势+技术认知+投资方向，不讲泡沫。估值向下触发条件改中性"跟踪信号"。

### 27.12 字重合

大字（48px）与上方留够间距不重合，挪10px+下方相应挪。
```

每页的修改指令格式：
```markdown
## slide_XX.svg 修改指令

1. [具体修改项] — 原因/目标
2. ...
```

### Phase 2: SVG修改 — 并行sub-agent

**并发数**：5个sub-agent同时运行。

**Sub-agent context**（每个sub-agent只拿到）：
- 当前页SVG内容
- `global_styles.md` 完整内容
- 该页的修改指令
- CICC品牌色值参考

**Sub-agent不拿**其他页SVG（省token），跨页一致性由 `global_styles.md` 硬约束保障。

**分批策略**：
- Batch 1: slide 01-05（5个sub-agent并行）
- 汇总检查：确认5页全部写入成功
- Batch 2: slide 06-09（4个sub-agent并行）
- 汇总检查

每批完成后，主agent快速验证写入结果（文件存在+非空+无语法错误），然后启动下一批。

### Phase 3: Pipeline — 串行执行

finalize_svg.py 和 svg_to_pptx.py 是全量跑的，无法拆成单页，且执行很快（几秒），串行即可。

```bash
python3 finalize_svg.py <project_dir>
python3 svg_to_pptx.py <project_dir>
```

### Phase 4: Visual QA — Kimi K2.6 并行5线程

**严肃机构标准**：用 Kimi K2.6 (DashScope API) 并行做 Visual QA，5个并发线程。

QA 执行流程：
1. SVG → PNG：替换 SimHei→Heiti SC，cairosvg 渲染 2x scale
2. PNG 压缩：800px JPEG quality=80（原始 PNG 太大会导致 API 超时）
3. 并行调用 kimi-k2.6，5线程，每页 ~10-20s，23页总计 ~60-90s
4. 解析 JSON 结果，汇总 High/Medium/Low 问题

**核心教训**：
- 不要串行逐页跑，23页×20s = 460s；5线程并行 = 62s（7x加速）
- 不要发原始 PNG base64（500KB+），压缩到 ~46KB JPEG 才稳定
- 不要忘记字体替换，否则中文全变方框，QA 结果全是假阳性

主agent汇总所有QA结果，识别需要修改的页面。

### Phase 5: 跨页一致性检查

**这是串行流程无法自动化的关键步骤**。主agent在所有sub-agent完成后，必须逐项比对同层级元素：

| 检查项 | 方法 | 不一致时处理 |
|-------|------|------------|
| 核心结论框样式 | 比对所有页的 `<rect fill="#FFF5F5">` + left bar width/color | 改为统一的 |
| 图表标题字号+颜色 | grep所有图表标题text元素 | 统一到14px #640000 bold |
| 正文字号+颜色 | grep所有正文text元素 | 统一到12-13px #1A1A1A |
| 资料来源格式 | 比对底部text元素 | 统一格式 |
| Callout样式 | 比对大数字callout，确认无背景色块 | 移除多余色块 |
| 白底rect | grep `<rect width="1024" height="768"` | 删除 |

不一致→定向修改对应SVG→重跑pipeline（仅pipeline，不需要重跑QA，除非改动影响视觉）。

### 效率预期

| 场景 | 串行耗时 | 并行耗时 | 加速比 |
|------|---------|---------|-------|
| 9页SVG修改 | ~9轮交互 | ~2轮（5+4） | ~4.5x |
| 23页Visual QA (Kimi K2.6) | ~460s (20s×23) | ~62s (5线程) | ~7x |
| 修改+QA合计 | ~18轮 | ~6轮 | ~3x |

---

## 17.5 Color Reference Block (append to every spec)

```
=== CICC Color Reference ===
CICC Deep Red  RGB(100, 0, 0)      #640000   Primary, cover titles, accent bars
CICC Gold      RGB(203, 169, 123)  #CBA97B   Declaration bars, dividers
CICC Orange    RGB(233, 151, 83)   #E99753   Tertiary accent, callouts
CICC Blue-Gray RGB(138, 144, 165)  #8A90A5   Muted labels
CICC Lt Gray   RGB(190, 192, 194)  #BEC0C2   Structural dividers
Dark Gray      RGB(59, 59, 59)     #3B3B3B   Content page titles
CICC Navy      RGB(31, 73, 125)    #1F497D   Timeline keywords
Near-black     #1A1A1A                        Body text
Light BG       #F5F5F5                        Alternating table rows
CICC Logo      ~/Desktop/Claude/CICC Logo.jpg (每页右上角)
============================
```

---

## Quick Checklist (Before Every Delivery)

- [ ] Canvas is 10" × 7.5" (4:3 default) or user-specified size
- [ ] CICC logo top-right + horizontal rule below it (every page)
- [ ] Slide title uses colon format and states a conclusion
- [ ] Chart sub-titles are bold maroon (~12pt, `#640000`)
- [ ] Body paragraphs: bold lead sentence + regular elaboration
- [ ] English terms NOT translated
- [ ] Source citation at bottom: "资料来源：…，中金公司研究部"
- [ ] No fonts other than Arial (Latin) and 黑体 (CJK)
- [ ] Colors from CICC palette only
- [ ] No text spill
- [ ] No large blank areas (>20% content area)
- [ ] No white background rect in SVG (template already white)
- [ ] Loops/flywheels visually closed with return arrows
- [ ] Content inside colored boxes horizontally centered
- [ ] Cross-page style consistency verified (same-level elements identical)
- [ ] At least 1 data chart per 3 content slides
- [ ] CICC color reference block in spec
- [ ] If deck ≥5 pages: parallel workflow used (§17)

---

## 19. Text-in-Shape Merge（思路一：SVG 管线增量修复）

### 核心问题

SVG `<text>` 没有自动换行能力。LLM 写 SVG 时必须手动断行，但它无法精确估算中英混排文字的实际渲染宽度，导致两个顽疾：
1. **Text spill**：文字溢出色块边界
2. **Premature line break**：文字还没撑满行宽就换行了

这两个问题在 ppt-master 的 `merge_paragraphs` 模式下也无法根治，因为根源是 LLM 的字符宽度估算不准。

### 解决方案：rect+text 合并

ppt-master 的 svg_to_pptx 转换器已支持自动检测卡片模式（浅色背景 rect + accent bar + text 元素），将其合并为**一个 PowerPoint shape**：
- **宽度锁死 rect 宽度** → 文字永远不会水平溢出
- **高度用 spAutoFit 自适应** → 色块自动适应文字高度，不会截断
- **wrap="square"** → 文字在 shape 内自然换行
- 用户在 PowerPoint 里点击色块即可直接编辑文字

### 检测启发式

卡片检测条件：
1. 背景 rect：fill ∈ {#F5F5F5, #FFF5F5, #FFF0F0, ...}，width > 100px，height > 40px
2. Accent bar：width ≤ 10px，与背景 rect 同 y 同 height
3. 文本元素：x 坐标在 rect 内，y 坐标在 rect 范围内
4. **收紧规则**：所有文本元素必须共享相同的左对齐 x 坐标（±6px 容差），过滤掉碰巧落在 rect 内的图表标签

### 已知局限

- **提前换行**：LLM 仍会在 SVG 中预断行，每行一个 `<text>` 元素。合并后每个 `<text>` 变成一个 `<a:p>` 段落，PowerPoint 不会在段落内重新换行。**正确做法是把正文行合并成一个连续段落**，让 PowerPoint 自己在 rect 宽度内换行（待实现）。
- **字号混乱**：LLM 给不同行设不同字号。卡片内可统一正文字号，但图表标签等独立文字仍需修 SVG 生成逻辑。
- **spAutoFit 后间距过近**：高度自适应后，相邻卡片可能挤在一起。需要 visual QA 后调整文字量或手动调整位置。

---

## 20. CSS Font-Family 注入

### 问题

SVG `<style>` 中的 CSS 规则（如 `text { font-family: SimHei, Arial, sans-serif; }`）不会被 xml.etree 解析。大部分 `<text>` 元素没有显式 `font-family` 属性，导致 svg_to_pptx 回退到默认字体 Segoe UI / Microsoft YaHei——**不是**我们要求的 SimHei + Arial。

### 解决方案

svg_to_pptx 在转换前自动解析 `<style>` 中的简单 CSS 规则（标签选择器），将属性注入到匹配的元素上。这样 `_get_attr(elem, 'font-family', ctx)` 就能正确返回 CSS 中定义的值。

**LLM 仍然必须确保**：SVG 中有 `<style>text { font-family: SimHei, Arial, sans-serif; }</style>`，或者每个 `<text>` 元素都带显式 `font-family` 属性。两种方式均可。

---

## 21. Logo 定位规则

Logo 的右边距 = 上边距（等距原则），这是最美观的放置方式。

4:3 画布（1024×768）：
- Logo 尺寸：240×45px
- 上边距 = 右边距 = 10px
- Logo 位置：x = 1024 - 240 - 10 = **774**，y = **10**
- 分隔线：y ≈ **58**（Logo 底部 y=55 下方 3px）

16:9 画布（1024×576）同理调整。

---

## 22. 长期路线：HTML→PPTX（思路二）

### 当前架构的根本矛盾

SVG 是画图格式，不是排版格式。LLM 永远会在 SVG 里预断行，因为它不知道 rect 有多宽。不管 `merge_paragraphs` 加多少补丁，只要 LLM 还在手动断行，text spill / premature break 就无法根治。

### 思路二：HTML 中间层

参考 Huashu Design（[alchaincyf/huashu-design](https://github.com/alchaincyf/huashu-design)）和 Anthropic Claude Design 的做法：

1. **LLM 生成 HTML**（CSS Grid/Flexbox 布局 + `word-wrap: break-word`）
2. **浏览器渲染 HTML** → 计算出每个元素的精确位置、大小、换行
3. **html2pptx.js 读取 DOM** → `getComputedStyle()` 拿颜色/字号，`getBoundingClientRect()` 拿位置/尺寸
4. **用 PptxGenJS 生成 PPTX** → 每个 HTML 元素变成一个 PowerPoint shape

核心优势：**浏览器做了 LLM 做不到的事——精确计算文字宽度和换行位置。** html2pptx 不需要估算字符宽度，因为浏览器已经算好了。

### RogerSlides two 计划

Fork 当前 RogerSlides 为 RogerSlides two，用 HTML→PPTX 管线重构。与思路一做 AB test 后决定长期方案。

---

## 23. SVG 生成铁律（从本次 PPT 踩坑总结）

1. **每个 `<text>` 必须有显式 `font-family`**，或确保 `<style>` 中有 `text { font-family: SimHei, Arial, sans-serif; }`
2. **Logo 位置**：x=774, y=10, width=240, height=45（4:3 画布，等距原则）
3. **分隔线位置**：y=58（Logo 底部下方 3px）
4. **卡片文字**：不要用 `<text>` 元素逐行预断行，应该写完整段落让 PowerPoint 自己换行（如果使用 rect+text 合并）
5. **同层级字号严格一致**：所有正文 12-13px，所有注释 10-11px，所有图表标题 14px。禁止"差不多的字号"
6. **卡片 accent bar**：width=4px，紧跟背景 rect 左边缘
7. **结论框**：fill=#FFF5F5 + 4px #640000 left bar，是 CICC 投研 PPT 最常见的视觉模式

## 6.5 Chart-First Layout — 图表优先布局铁律

**这是投研PPT最核心的布局决策原则，违反即导致"页面铺不满"。**

### 执行顺序（必须严格遵循）

```
Step 1: 清点本页图表资源
  → 几张图？每张长宽比？内容复杂度？

Step 2: 图表优化决策
  → 裁剪空白？拆分子图？重绘为SVG？
  （见下方决策矩阵）

Step 3: 放置图表到最优位置和尺寸
  → 图表内文字最小8-9pt可读
  → 图表之间间距15-20px
  → 图表标题左对齐 + 灰色衬线

Step 4: 计算剩余空间，填充文字
  → 正文段落（粗体首句+常规展开）
  → 核心数字callout
  → 结论框（#FFF5F5 + 4px #640000 left bar）
  → 补充卡片/表格

Step 5: 检查packed密度
  → 可用内容区不得有>20%连续空白
  → 有空白→加大图表、增加文字、加补充数据
```

### 图表优化决策矩阵

| 图表特征 | 优化手段 | 操作 |
|---------|---------|------|
| 上下有大量空白像素 | **裁剪** | PIL自动trim空白行 |
| 多个子图拼合在一张图中 | **拆分** | 识别边界→切为独立图→平行放置 |
| 简单柱状图/折线图/饼图 | **重绘为SVG** | 数据点少，SVG更清晰+尺寸可控 |
| 表格（财务/对比） | **重绘为SVG** | SVG表格灵活调整列宽，无固定长宽比困扰 |
| 产品照片/实物图 | **嵌入原图** | 无法重绘 |
| 复杂示意图/流程图/架构图 | **嵌入原图** | 重绘成本远高于收益 |
| 双Y轴复合图 | **看情况** | 数据简单→重绘；数据密集→嵌入 |

### McKinsey金字塔布局法则

**左右双栏**（最常用，占80%内容页）：
- **不对称**：可左宽右窄(4:6)或右宽左窄(6:4)
- **逻辑流向**：左栏(前提/数据)→右栏(推导/图表)→Lead结论框
- 左栏放文字/卡片/callout，右栏放图表
- 双栏间隔20-30px

**上下布局**（宽幅图片适用）：
- 图片扁平（长宽比>1.5）时，上下叠放+右侧Textbox说明
- 或自上而下逻辑推导：标题→callout→图表→结论框

**三栏并排**（多个平行概念）：
- 三张子图/三个产品卡片等宽并排
- 各栏宽度=(内容区宽-2×间隔)/3

### 长宽比→布局映射

| 图表长宽比 | 推荐布局 | 说明 |
|-----------|---------|------|
| >2.0（超宽） | 顶部全宽 + 文字在下 | 图表横跨页面宽度 |
| 1.5-2.0（宽幅） | 左右分栏，图表占55-65% | 最常见的投研图表比例 |
| 1.0-1.5（标准） | 左右分栏，图表占50-60% | |
| <1.0（近方/竖长） | 左侧窄栏或上下布局 | 考虑裁剪或重绘 |

### 图片裁剪自动化

生成SVG前，对所有需要嵌入的图片执行自动裁剪：

```python
from PIL import Image, ImageChops
def auto_crop(image_path, output_path, padding=10):
    img = Image.open(image_path)
    bg = Image.new(img.mode, img.size, img.getpixel((0,0)))
    diff = ImageChops.difference(img, bg)
    bbox = diff.getbbox()
    if bbox:
        cropped = img.crop(bbox)
        # Add small padding
        from PIL import ImageOps
        cropped = ImageOps.expand(cropped, padding, fill=(255,255,255))
        cropped.save(output_path)
```

### 为什么AI生成的PPT总是"松散"？

LLM默认先写文字（这是它擅长的），然后把图表当作"附件"塞进去。但投研PPT的逻辑是反过来的——**图表是主角，文字是解说词**。先定主角位置，再安排解说词的位置。此原则必须在Executor阶段的每页SVG生成前严格执行。

---

## 24. Render Consistency — card group 坐标丢失（2026-06-21 踩坑，影响深远）

### 现象
SVG 预览界面（浏览器按坐标精确渲染）非常漂亮，但导出到 PPTX/PDF/WPS 后文字整体**向上向右偏移、行距挤、格子没填满、跨 card 不对齐**。Roger 称之为"渲染不一致"。

### 根因
svg_to_pptx 的 `convert_card_group` 分支（rect+text 卡片合并成单个 shape）默认**丢弃了 text 元素的 x/y 坐标**，四处在丢失信息：

| 丢失 | 默认行为 | 后果 |
|------|---------|------|
| text 的 y（首行） | `tIns=0` + `anchor="t"`，文字贴 rect 顶 | 向上 |
| text 的 y（行间距） | 固定 `lnSpc=1.3*fs` 堆叠 | 行距紧、格子没填满 |
| text 的 x（居中） | `algn="l"` 硬编码左对齐 | 居中文字向右 |
| text 的 x（左边界） | `left_inset=min(text x)`，居中文字的 x（中心点）被当左边界 | 居中区被推右 |

card group 的设计目的是"宽度锁死 rect、文字自动换行防溢出"，但代价是丢失纵向坐标——而投研 PPT 几乎每页都是 rect+text 卡片结构，所以这是**全局性**问题。

### 修复（`drawingml_elements.py` → `convert_card_group`）

1. **tIns = 首个 text.y − rect.y − 0.85\*font_size**：和独立 text 的 `box_y = y − 0.85*fs` 统一逻辑，card 内首行文字顶 = 独立 text 文字顶，**跨 card 对齐**。
2. **algn 按 text-anchor 动态**：`start→l, middle→ctr, end→r`。居中文字真正居中。
3. **left_inset 只取 start-anchor text 的 x**：middle/end 文字不参与 left_inset 计算，居中文字不被推右。
4. **spcBef = (当前 text.y − 前 text.y) − 1.3\*前 fs**：保留每个 text 的设计 y 间距，行距从固定 1.3fs 变成设计间距，**格子填满、行距松**。

### 验证方法（像素级，比视觉模型猜更硬）
用 PIL 检测渲染图里文字深色像素的 bbox，和色块框坐标对比，算实际偏移。注意颜色阈值：橙色类别名 `#E99753`（R=233）和灰色说明 `#8A90A5`（R=138）都不满足 `<120` 的深色阈值，要按实际文字颜色调阈值，否则漏检。

### 铁律：改 svg_to_pptx 包代码后必须 `rm -rf __pycache__`
Python 加载 `__pycache__` 旧字节码，改了 `.py` 不清缓存则改动**完全不生效**（表现为改系数后测量值不变）。每次改 `skills/ppt-master/scripts/svg_to_pptx/` 下任何文件，必须 `rm -rf __pycache__` 再重转。

### 预览必须走 soffice→PDF→pdftoppm
cairosvg 不渲染 `<image>` 引用的外部 jpg（logo 缺失）、字体度量也不准。真实预览只有 `soffice --headless --convert-to pdf` + `pdftoppm -png`。cairosvg 只能看 SVG 结构，不能当渲染效果判据。

### 沉淀结论
**渲染不一致不是"LibreOffice/WPS 兼容性"这种泛泛问题，而是转换器在 card 合并时丢失了 SVG 坐标。** 修转换器一处，惠及所有 card 页 + 所有未来项目。诊断时先用 PIL 像素测量定位偏移方向和量，再读转换器源码找坐标换算逻辑，不要靠猜。

---

## 25. 卖方报告体 — PPT 是报告不是提词器

### 核心原则
卖方 PPT 是**报告**，不是提词器。光看 PPT 不听演讲者说，也能看懂完整逻辑和结论。文字要**丰满饱满**，每个要点是完整论述句，带数据、因果、投资含义——不是关键词、highlights、抽象短词。

### AI 的"惜字如金"病
LLM 默认倾向用简单、highlight、抽象的词表达，PPT 只起提示作用。这是要克服的反模式。一旦克服"惜字如金"，文字饱满度上去，留白问题自然消失（格子被论述填满）。

### 执行
- 每个要点：从短词 → 完整论述句（"控制力最强" → "本地部署控制力最强，能直接读写本地文件、调用本地脚本和 MCP，延迟最低，是日常改 skill、调脚本、生成 PPT 的主力形态"）
- 保留 bold 引导句（核心结论）+ regular 展开（数据/因果/含义）
- 禁止"关键词 | 关键词 | 关键词"碎片化，必须完整句子

---

## 26. QA 认知 — packed 程度与对齐敏锐度

### packed 程度认知
QA 时要对每个 textbox 的 packed 程度有判断：card 内文字是否**舒适填满格子**（行距松、不留大空、不挤一起）。发现"格子没填满"或"文字挤在上部"要主动提出，不能等用户发现。

### 对齐敏锐度
代码里两个元素，写代码时可能不知道它们应该在同一层级对齐，但**视觉上一眼能看出应该对齐**。QA 要主动判断：
- 同一行的类别名和内容（如 P15 "LLM推理" 和右边的 "GLM..." 应同水平）
- 跨 card 的同类文字（多个 card 的标题应顶对齐）
- 左右两栏的标题、底部应齐平

不依赖代码注释或 SVG 坐标判断"应该对齐"——用视觉常识。代码里 x/y 不同不代表不该对齐（可能字号不同导致 baseline 不同，但视觉应同顶/同基线）。

### QA 不是"没 FAIL 就达标"
宽松 prompt 问"有没有严重问题"会得 60/70 分但实际有大量布局缺陷。QA 要用严格 7 维度 prompt（字体统一/packed/对齐/边界/字号层级/色块/文字完整性），且对"渲染一致性"单独验证（PIL 像素测量，不靠视觉模型猜）。
