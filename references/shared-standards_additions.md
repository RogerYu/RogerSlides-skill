# Shared Standards Additions (§9)

以下规则应追加到 ppt-master 的 `references/shared-standards.md` 的 §9 位置。这些规则来源于真实 PPT 项目中发现的视觉缺陷（重叠、错位、溢出）。

---

## 9. Page Layout & Content Positioning

### 9.1 Chart Title Must Be Above the Tallest Chart Element

图表标题（及其装饰下划线）必须位于**每个数据标签、柱顶和视觉元素之上**。如果数据标签 "9,072" 在 y=81 而标题在 y=95，则标题在视觉上位于标签**下方**——错误。

**Rule**: `title_y < min(data_label_y) − 10`

计算最高元素的 y 值（通常是最高柱上方最顶端的数据标签），然后将标题基线放置在至少比其高 10-15px 的位置。标题下划线位于标题和第一个数据元素之间。

### 9.2 Content Must Start Below the Header Rule

使用 `translate(0, N)` 将正文内容移到标题区域下方时，第一个内容元素的 effective y 必须**至少比标题衬线低 7px**。

**Rule**: `first_element_y + translate_N ≥ rule_y + 7`

CICC 4:3 (1024×768): 衬线 y=93, 首内容通常 y=70 → translate ≥ 23. 使用 `translate(0,30)` 作为安全默认值。

### 9.3 Source Footer Outside the translate Group

资料来源（"资料来源：…"）和页码必须放在 `<g transform="translate(0,N)">` 组**外部**。在组内，大的 translate 会把它们推到页面外。

### 9.4 Callout Box Must Cover All Contained Text

彩色 callout 框（如 `<rect fill="#FFF5F5">` + 左侧强调 `<rect fill="#640000">`）必须延伸覆盖**每行文字**——包括次要要点或后续数据。

**Rule**: Box `height = last_text_y − box_top_y + 15px`（15px 底部留白）

左侧强调线必须与父框共享**相同的 y-start 和 height**。

### 9.5 Don't Crop Logo Source Images

品牌 Logo 文件必须保持**原始源分辨率**。定位通过 SVG x/y/width/height 属性完成——绝不通过预裁剪图像文件。

### 9.6 Horizontal Bar Charts Must Scale to Canvas Bounds

水平条形图中数据驱动的柱宽**必须**根据可用画布宽度计算，绝不硬编码或目测。超出画布边缘的柱会破坏整个行布局——标签错位会级联影响后续行。

**Calculation**:
```
bar_start_x = <左边界>
bar_max_width = canvas_right_edge - bar_start_x - label_margin
max_value = <最大数据值>
bar_width_i = max(min_width, (value_i / max_value) × bar_max_width)
```

**Label placement**: 柱宽 < max → 标签在柱后; 柱宽 ≈ max → 标签在柱内（白色文字）

### 9.7 Thank You Page Template

- **不写"谢谢"** — 只用英文 "Thank You"
- **字号**: 32px bold
- **位置**: 居中 y≈280
- **免责声明框**: 从 y≈360 开始

### 9.8 Chart Legend Alignment

色块（图例色标）及其文字标签必须垂直居中。文字基线 y 应为 `rect_y + rect_height * 0.75`（12px 色标 + 10px 文字约等于 `rect_y + 10`）。

**常见错误**: 文字 y 设为 `rect_y - 10`（文字基线在色标上方）。

### 9.9 Section Divider Template

- 英文副标题: **37px**（≈28pt in PPTX; SVG→PPTX 字体转换比 = 0.75）
- 三要素: 红色竖条 + 灰色横线 + 简述
