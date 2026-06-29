---
name: ppt-skill-luna-simpletech
version: 1.2.0
description: Generate Luna SimpleTech style HTML presentations with smooth morphing transitions. Supports 16:9 and 4:3 aspect ratios, audience-adaptive detail levels, and concise/detailed/narrative writing styles.
description_zh: 生成 Luna SimpleTech 风格的 HTML 演示文稿，带 Morphing 翻页动效。支持 16:9/4:3 比例、面向对象（受众）适配详细度、凝练/详细/叙事三种语言风格。
category: content-creation
name_zh: Luna SimpleTech 演示文稿
---

# PPT-Skill-Luna-SimpleTech

当用户需要生成**Luna SimpleTech风格**的HTML演示文稿时使用此技能。输出为单个自包含HTML文件，带 Morphing 翻页动效，支持16:9或4:3比例。每次调用需询问：比例、面向对象（受众）、语言风格三个必选参数。

## 设计规范

### 色彩系统

| Token | 值 | 用途 |
|-------|------|------|
| --color-primary | #E8923A | 主强调色，标题高亮、链接、关键词 |
| --color-primary-light | #F0A85C | 悬浮/渐变辅助 |
| --color-accent | #7FB5B0 | 辅助装饰色，图标指示器 |
| --color-accent-dark | #5A9E98 | 深色装饰方块 |
| --color-text | #333333 | 正文主色 |
| --color-text-secondary | #777777 | 辅助/说明文字 |
| --color-text-light | #AAAAAA | 来源/脚注 |
| --color-bg | #FFFFFF | 页面背景 |
| --color-border | #E0E0E0 | 分隔线 |

### 字体系统

**英文字体：Nexa**（已内嵌于 `fonts/` 目录，通过 base64 嵌入HTML）

```css
/* @font-face 声明 - 从 fonts/ 目录读取 base64 文件内嵌到生成的HTML中 */
@font-face {
  font-family: 'Nexa';
  src: url(data:font/woff2;base64,[nexa-light-woff2.b64]) format('woff2'),
       url(data:font/woff;base64,[nexa-light-woff.b64]) format('woff');
  font-weight: 300;
  font-style: normal;
  font-display: swap;
}
@font-face {
  font-family: 'Nexa';
  src: url(data:font/woff2;base64,[nexa-bold-woff2.b64]) format('woff2'),
       url(data:font/woff;base64,[nexa-bold-woff.b64]) format('woff');
  font-weight: 700;
  font-style: normal;
  font-display: swap;
}

--font-en: 'Nexa', sans-serif;  /* 英文专用 */
--font-zh: 'PingFang SC', 'Microsoft YaHei', sans-serif;  /* 中文 */
--font-primary: 'Nexa', 'PingFang SC', 'Microsoft YaHei', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
--font-mono: 'SF Mono', 'Fira Code', 'Consolas', monospace;
```

**字体内嵌说明：**
- `fonts/nexa-light.woff2` + `fonts/nexa-light.woff` — Nexa Light (weight 300)，用于正文、副标题
- `fonts/nexa-bold.woff2` + `fonts/nexa-bold.woff` — Nexa Bold (weight 700)，用于标题、强调
- 生成HTML时，从 `.b64` 文件读取 base64 内容替换占位符 `[nexa-light-woff2.b64]` 等
- 这确保HTML文件完全自包含，无需外部字体文件

### 字重与字号

**注意：以下为1920px画布上的绝对像素值，非rem。在1920x1080固定画布中，文字必须有足够的视觉重量——所有层级字号已整体上调，避免远距离投影时"字太小看不清"。**

| 层级 | 字号 (16:9) | 字号 (4:3) | 字重 | 字间距 | 行高 |
|------|------------|------------|------|--------|------|
| 封面主标题 (H1) | **82px** | **68px** | 800 | 0.02em | 1.15 |
| 封面副标题 / 英文标题 | **34px** | **28px** | 300 | 0.06em | 1.4 |
| 封面 eyebrow / 元信息 | **20px** | **18px** | 700 | 0.18em | 1.4 |
| 章节大标题 (TOC H2) | **64px** | **54px** | 700 | 0.24em | 1.2 |
| 目录章节项 | **30px** | **26px** | 700 | 0.02em | 1.3 |
| 内容页标题 (H2) | **46px** | **40px** | 700 | 0.02em | 1.2 |
| 卡片标题 / 内容副标题 (H3) | **30px** | **26px** | 700 | 0.01em | 1.3 |
| 卡片大数字 / Metric Value | **52px** | **44px** | 700 | 0.02em | 1 |
| 正文段落 | **22px** | **20px** | 400 | 0.005em | 1.65 |
| 列表 / 项目要点 | **20px** | **18px** | 400 | 0.005em | 1.6 |
| 卡片内文字 | **19px** | **17px** | 400 | 0 | 1.6 |
| 标签 / Tag / Eyebrow | **16px** | **14px** | 700 | 0.16em | 1 |
| 表格表头 | **16px** | **14px** | 700 | 0.06em | 1.4 |
| 表格单元格 | **17px** | **15px** | 400 | 0 | 1.55 |
| 脚注 / 来源 | **15px** | **13px** | 300 | 0.02em | 1.4 |

**最小可读字号：16px**（脚注除外）。任何卡片正文、列表项、表格单元格不得低于 16px。

**字号节奏（黄金比例感）**：相邻层级字号比建议 ≥ 1.25，最大与最小之比 ≥ 3，确保层级一眼可辨。

**字重策略**：标题区 700–800；正文 400；强调色 + 700 用于关键词高亮；切勿用 300 写正文（远距投影易糊）。

### 版式规则

1. **封面页 (Cover)**：左侧大视觉/3D图(38%)，右侧标题+元信息(62%)，适度留白但文字区域饱满
2. **目录页 (TOC)**：左侧装饰图(35%)，右侧结构化文字列表(65%)，列表撑满可用空间
3. **内容页 (Content)**：顶部窄导航栏(标题+logo+分隔线)，下方内容区尽量填满，body padding 控制在 40-50px
4. **结尾页 (End)**：居中感谢文字，可选装饰

**留白原则**：slide-body 内边距不超过 50px，组件间距 16-24px，卡片内边距 24-32px。内容应占据版面 85%+ 面积。

### 溢出控制（HARD RULE，v1.2.0 新增）

**第一原则：每一张 slide 都是 1080px 固定高度的硬画布，任何文字、卡片、列表、表格在生成时就必须放得下，不允许出现底部被裁切或撑破画布的情况。** 一旦字号、行距、padding、子项数量任意一项放大，必须重新核算高度预算并相应缩减其它项。

**1. 高度预算算术（生成前强制核算）**

每一类页面有一份"垂直预算公式"，在写 HTML 前用纸笔/心算或代码注释列一次：

| 页面 | 公式（16:9，画布 1080px） |
|------|---------------------------|
| Cover | `padding-top + padding-bottom + eyebrow(28) + 主标题(82×1.15) + 副标题(34×1.4) + divider(5) + 元信息(2 行×30) + 上述元素 margin 之和 ≤ 1080` |
| TOC | `padding-top + padding-bottom + header(64×1.2) + subhead(22×1.4) + (N × 单项块高) + ((N-1) × gap) ≤ 1080`，单项块高 = h3(30×1.3) + p 行数×(20×1.6) + padding-bottom |
| Content | `header(78) + body padding-top(40~50) + section-title(46×1.2 + margin 24) + content-area(主体) + body padding-bottom(40~50) + footer(40) ≤ 1080`，因此 **content-area 主体可用高度 ≈ 1080 − 280 = 800px** |
| End | 居中布局，主元素总和 ≤ 600px 留出对称留白 |

**当预算不够时**，按优先级削减：① 减少子项数量 → ② 缩小 padding（≥ 32 即可） → ③ 收紧 gap（≥ 16） → ④ 整体下调字号到 4:3 列的数值 → ⑤ 最后才考虑去掉某个组件。**禁止用 `overflow: hidden` 静默裁切**——那是最后的保护层，不是布局策略。

**2. 容器双保险（保护层，必须加）**

- `.slide` 始终保留 `overflow: hidden`（防止溢出溢到下一页的视觉空间）；
- `.toc-content`、`.slide-body`、`.content-area` 三个文本主容器加 `overflow: hidden; min-height: 0;`（解决 flex 子项 min-content 不会收缩的问题）；
- 仅当某区块自身明确支持滚动时才允许 `overflow: auto`，PPT 场景一律禁用滚动条。

**3. TOC 专项约束**

TOC 列表是最容易溢出的页面，按章节数量 N 选预设：

| N（章节数） | toc-content padding | toc-list gap | toc-section padding-bottom | h3 → p margin-bottom | 备注 |
|-------------|---------------------|--------------|----------------------------|----------------------|------|
| 3 | 110px 110px | 36px | 28px | 12px | 留白舒朗 |
| 4 | 100px 100px | 30px | 24px | 10px | 默认 |
| **5** | **80px 100px** | **24px** | **18px** | **8px** | **最常用，必须按此压缩** |
| 6 | 70px 90px | 20px | 14px | 6px | 临界值 |
| 7 | — | — | — | — | **不允许**，必须拆为两页 TOC |

同时：
- 每个 TOC 章节子项最多 2 条，且单条文本控制在 24 个汉字内（避免折行后高度翻倍）；
- `toc-list` 加 `flex: 1; min-height: 0;`，并把外层 `.toc-content` 设为 `display: flex; flex-direction: column;`，章节项按可用空间自然撑开；
- 最后一项的 `border-bottom: none`，且去掉 `margin-bottom`，防止累积溢出。

**4. Content 页 content-area 高度守门**

`.content-area` 必须显式 `flex: 1; min-height: 0; overflow: hidden; justify-content: center;`，并且：

- **`justify-content: center` 必加**——`.content-area` 的高度由 `flex:1` 撑满 slide-body 剩余空间（≈800px），其内部组件（metric-grid + bullet-list 混排、tri-grid 卡片数偏少、quad-grid 行不齐、单段大字 highlight 等）若没有自身 `height:100%` 撑开，就会按自然内容高度排布。此时**不加 justify-content:center 会导致内容贴顶、底部大块空白**（被用户视为"排版没居中"），必须默认垂直居中。对于显式 `height: 100%`（如 .two-col / .sensor-grid / .summary-cards / .table-wrap flex:1）或 `grid-template-rows: 1fr ...` 撑满父级的容器，stretch 行为优先，居中规则自动失效，不会破坏布局。
- 表格变体：`.table-wrap { max-height: 100%; overflow: hidden; }`，超出 8 行的表必须拆页或换 Phase Flow / 卡片组；
- 卡片网格（2×2 / 3×1）：grid-template-rows 用 `1fr` 而非固定 px，避免内容长短不齐撑高；
- bullet-list 单页不超过 6 条，每条不超过 2 行；超量必须拆 sub-h 分组或开第二页；
- Phase Flow：横向 5 卡为上限，每张 `phase-card-sub` 限制 2 行（CSS 加 `display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; overflow: hidden;`）。

**5. 内部容器垂直居中（防贴顶，治本规则）**

仅在 `.content-area` 上居中还不够。**通用治本规则**：

> **任何"被父级 stretch 到全高、但自身内容自然高度小于父高"的容器，都必须显式垂直居中**——否则内部一律贴顶、底部留大块空白。这种贴顶现象会在 4 类容器上反复出现，必须分类处理：

A. **中间层 flex-column 容器**（最常见）
- `.col-left` / `.col-right`（two-col 布局两侧列）——卡片组、`kv-grid`、`metric-grid` 常只占半屏高度，必须 `justify-content: center`；
- 其它任何自定义 `flex-direction: column` 且被父级 `flex:1` / `height:100%` 拉到全高的包装容器——同理处理。

B. **Grid 子项（cell）容器**（高频陷阱，v1.2.0 #3 新增）

Grid 默认 `align-items: stretch` 会把每个 cell 拉到整行最高高度（且当行只有 `1fr` 时 cell 高 = `.content-area` 内可分配的全部高度）。但 cell 内部如果是 `display: flex; flex-direction: column`，子项总和又小于 cell 高，子项会贴顶。

- `.sensor-camp`（`.sensor-grid` 内的传感器阵营卡，3 列等高 grid cell，内部 5-6 条自然高度元素）——**必须**在 `.sensor-camp` 自身加 `display: flex; flex-direction: column; justify-content: center;`；
- `.summary-card` / `.tri-card` / `.quad-card`（任何 grid 单元卡片，内容偏少时）——同理；
- 同时 `.sensor-grid` / `.summary-cards` / `.tri-grid` / `.quad-grid` 的 `grid-template-rows` 用 `1fr` 而非 `auto`，保证 cell 高度可预期。

C. **表格容器**（v1.2.0 #3 新增）

`<table>` 元素不响应 flex/grid 拉伸（它的高度永远 = 行高之和）。只在外壳 `.table-wrap` 上 `flex:1` 不够——`.table-wrap` 会被拉满，但内部 table 仍贴顶，表与父底之间留大段空白。

- `.table-wrap` **必须**改为 `display: flex; flex-direction: column; justify-content: center;`（保留 `flex:1; overflow:hidden; border-radius; border;`）；
- 若需要表格上下都留同等空白（视觉居中），不要改 table 本身，而是改 wrap 的 flex 对齐方式；
- 配合 §4 的"超 8 行必拆页"规则，wrap 高度永远够装下整张表。

D. **例外（保持 flex-start）**

列表本身就要按内容紧凑排列（如 TOC 章节列表、Phase Flow 横向卡、有意图的瀑布堆叠）时，可以保留默认 `justify-content: flex-start`，但要保证父容器对该列表的高度策略是"按内容自然高度"（不加 `flex:1` / `height:100%`），让父子高度一致，避免出现 stretch 父 + 紧凑子 的错配。

**6. 字号自降级（fallback）**

当核算后预算仍紧张时，允许在该单页内将"正文/列表/卡片内文字"按 4:3 列字号直接下调一档（22→20→19→17），但**标题层级（H1/H2/H3）不许下调**——层级感优先级最高。

**7. 验收清单（生成后必查）**

每生成一张 slide，开发者/agent 心里跑一遍：
- [ ] 主容器是否带 `overflow: hidden`？
- [ ] `.content-area` 是否带 `justify-content: center`？
- [ ] two-col 的 `.col-left/.col-right` 是否也带 `justify-content: center`？
- [ ] Grid 子项（`.sensor-camp` / `.summary-card` / `.tri-card` / `.quad-card` 等）是否各自带 `display:flex; flex-direction:column; justify-content:center;`？（grid stretch 父 + 紧凑子 必崩贴顶）
- [ ] `.table-wrap` 是否改为 `display:flex; flex-direction:column; justify-content:center;`？（table 不响应 flex 拉伸，必须由 wrap 居中）
- [ ] 文字最低行是否距底部至少留 32px？
- [ ] 表格/列表是否在 1080px 内完整呈现？
- [ ] TOC 章节数与上表预设匹配？
- [ ] 任一卡片是否出现"半行被切"？若有，立刻执行优先级削减。

不通过任意一条，禁止交付 HTML。

### 装饰元素

- **三色方块指示器**：内容页标题前置 `■■■` 装饰，颜色为 accent-dark / accent / accent-light
- **顶部分隔线**：1px solid --color-border

### 封面 / 目录视觉资产（强制 SVG，禁用 CSS 几何拼贴）

> 历史版本曾用"CSS 渐变 + 三个圆角矩形伪 3D 方块"作为封面视觉，效果生硬、缺乏设计感。**新版本一律使用 SVG 矢量插画**，由路径、渐变、光晕组合构成。生成时从下方四种范式中按主题语义选 1 种作为封面 hero、再为目录选另 1 种作为变奏（避免雷同）。

#### 通用规则
- **尺寸**：左侧 visual 列内置 SVG `viewBox="0 0 600 800"`（或与 visual 列宽高比一致），`preserveAspectRatio="xMidYMid slice"` 让插画铺满。
- **配色**：仅使用 `--color-primary`、`--color-primary-light`、`--color-accent`、`--color-accent-dark` + 白/浅灰背景；最多 3 个主色 + 1 个中性灰，不堆色。
- **光感**：用 `radialGradient` 或 `feGaussianBlur` 制造柔光焦点，**忌纯硬色块拼贴**。
- **细节密度**：背景层 + 主体层 + 高光层，至少 3 个 z 层；并加细线（stroke-width: 1, opacity 0.15-0.35）增强工艺感。
- **禁用清单**：纯字符 emoji（如 ⊙ ◐ ▣）作为大图、CSS 3D rotate 方块、SVG `<rect>` 直接叠加无渐变。

#### 范式 A · Aperture Iris（光圈虹彩，默认）
适合：摄影 / 媒体 / 设计 / 美学相关主题。
构成：多层同心圆 + 6-8 片光圈叶（`<polygon>`）按 60°/45°旋转分布 + 中心高光（白色 radialGradient）+ 外圈柔光晕。叶片用线性渐变（primary → primary-light），透明度 0.85 + `mix-blend-mode: multiply` 营造交叠层次。背景配 1-2 圈极细同心环（stroke dashed, opacity 0.2）。

```svg
<svg viewBox="0 0 600 800" preserveAspectRatio="xMidYMid slice">
  <defs>
    <radialGradient id="glow" cx="50%" cy="45%" r="60%">
      <stop offset="0%" stop-color="#FFF4E8"/>
      <stop offset="60%" stop-color="#F0A85C" stop-opacity="0.4"/>
      <stop offset="100%" stop-color="#E8923A" stop-opacity="0"/>
    </radialGradient>
    <linearGradient id="blade" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0%" stop-color="#E8923A"/>
      <stop offset="100%" stop-color="#F0A85C"/>
    </linearGradient>
  </defs>
  <rect width="600" height="800" fill="#FFF8F0"/>
  <circle cx="300" cy="360" r="320" fill="url(#glow)"/>
  <g transform="translate(300 360)" opacity="0.9">
    <!-- 6 片光圈叶，每片旋转 60° -->
    <polygon points="0,-180 60,-30 -60,-30" fill="url(#blade)" transform="rotate(0)"/>
    <polygon points="0,-180 60,-30 -60,-30" fill="url(#blade)" transform="rotate(60)" opacity="0.85"/>
    <polygon points="0,-180 60,-30 -60,-30" fill="url(#blade)" transform="rotate(120)" opacity="0.75"/>
    <polygon points="0,-180 60,-30 -60,-30" fill="url(#blade)" transform="rotate(180)" opacity="0.85"/>
    <polygon points="0,-180 60,-30 -60,-30" fill="url(#blade)" transform="rotate(240)" opacity="0.75"/>
    <polygon points="0,-180 60,-30 -60,-30" fill="url(#blade)" transform="rotate(300)" opacity="0.9"/>
  </g>
  <circle cx="300" cy="360" r="40" fill="#FFFFFF"/>
  <circle cx="300" cy="360" r="18" fill="#E8923A"/>
  <!-- 外圈装饰细环 -->
  <circle cx="300" cy="360" r="240" fill="none" stroke="#E8923A" stroke-width="1" stroke-dasharray="2 6" opacity="0.25"/>
  <circle cx="300" cy="360" r="280" fill="none" stroke="#7FB5B0" stroke-width="1" opacity="0.2"/>
</svg>
```

#### 范式 B · Topographic Lines（等高线地形）
适合：战略 / 分析 / 探索 / 数据相关主题。
构成：20-40 条平滑等高线（`<path d="M... Q... Q...">`），由低到高密度疏密变化；底层大色块色（primary 渐变），线条用 white/primary-dark 描边，stroke-width 1-1.5，opacity 0.5-0.9 渐变。可叠一颗 primary 圆作为"峰顶坐标点"。

#### 范式 C · Layered Geometry（几何分层）
适合：架构 / 流程 / 系统 / 技术堆栈类主题。
构成：3-5 个平行四边形或六边形按 isometric 等距投影错位堆叠，每层有自己的渐变（primary → accent 横跨）+ 不同 opacity（0.95 / 0.8 / 0.65 / 0.5）。最上层叠 1-2 个白色细线网格 (`<line>`, opacity 0.3)。底层背景轻微 radial glow。

#### 范式 D · Particle Field（粒子矩阵）
适合：AI / 模型 / 神经网络 / 算法主题。
构成：80-150 个小圆点（`<circle r="1.5-4">`）按规则网格 + 噪声偏移分布，半径与透明度按到画面中心距离衰减；用 `<line>` 在距离 < 阈值的相邻点之间连细线（stroke-width 0.6, opacity 0.25）；中心区域聚集成 2-3 个明亮的 primary 大节点。

#### 选用决策
- 用户主题含"摄影/影像/媒体/设计/视觉" → 范式 A
- 用户主题含"地图/探索/趋势/分析/路线" → 范式 B
- 用户主题含"架构/系统/技术栈/组件/工程" → 范式 C
- 用户主题含"AI/算法/模型/智能/数据" → 范式 D
- 不明确时默认范式 A；目录页用与封面不同的另一种以制造节奏。

#### 与文字版的关系
SVG 视觉占封面/目录左侧 35-40% 宽度。左右过渡处加 8-16px 的细微 `radial-gradient` 阴影或竖向 `linear-gradient` 柔化，避免视觉与文字硬切。

### 翻页动效

页面切换使用 **Morphing 过渡（Crossfade + Scale）**：
- 所有幻灯片绝对定位堆叠，当前页可见（`.active`）
- 切换时：旧页 crossfade 淡出 + 微缩（0.92x），新页 crossfade 淡入 + 从微放（1.04x）回归正常
- 内部子元素（`.anim-item`）依次 stagger 入场（translateY + scale + opacity）
- 无方向性水平位移，避免位置突变
- 支持键盘左右箭头导航
- 支持触摸左右滑动
- 支持鼠标滚轮切换
- 底部导航栏含页码指示器和箭头按钮

## 执行流程

### 0. 强制询问参数（必选步骤）

**每次调用此技能时，必须首先使用 `AskUserQuestion` 工具依次询问以下三个参数。** 即使用户已在对话中提到相关偏好，也需要确认。此步骤不可跳过。

**首先询问画面比例：**

```
AskUserQuestion:
  question: "请选择演示文稿的画面比例："
  header: "比例"
  options:
    - label: "16:9 宽屏"
      description: "适合大屏演示、投影、线上会议共享，视觉更开阔"
    - label: "4:3 传统"
      description: "适合打印、传统投影仪、内容密集的汇报场景"
  multiSelect: false
```

**紧接着询问面向对象（受众），决定内容详细程度：**

```
AskUserQuestion:
  question: "这份演示文稿面向什么受众？"
  header: "受众"
  options:
    - label: "高层决策者"
      description: "精简核心结论，突出数据亮点与决策建议，避免技术细节"
    - label: "专业同行"
      description: "包含方法论、技术细节、对比分析，假设受众有领域背景"
    - label: "跨部门/通识"
      description: "平衡深度与可读性，关键概念需简要解释，适合混合受众"
    - label: "学习/培训"
      description: "教程式结构，概念递进讲解，辅以示例和类比"
  multiSelect: false
```

**然后询问语言风格，决定文案表达方式：**

```
AskUserQuestion:
  question: "希望文案采用什么风格？"
  header: "风格"
  options:
    - label: "凝练"
      description: "关键词+短句为主，高信息密度，适合快速浏览"
    - label: "详细"
      description: "完整句式，充分展开论述，适合需要理解上下文的场景"
    - label: "叙事"
      description: "故事线串联，引导式表达，适合演讲和路演"
  multiSelect: false
```

用户选择后，**受众**与**风格**共同决定：

| 受众 | 凝练 | 详细 | 叙事 |
|------|------|------|------|
| 高层决策者 | 极简数据+结论 | 结论+支撑逻辑 | 问题→洞察→建议 |
| 专业同行 | 术语密集+对比表 | 方法论全展开 | 技术演进故事线 |
| 跨部门/通识 | 要点提炼+图解 | 概念解释+案例 | 场景引入→原理→应用 |
| 学习/培训 | 知识卡片式 | 教程递进式 | 从问题出发逐步构建 |

三项参数确认完毕后，**比例**决定画布尺寸，**受众**+**风格**决定内容详细度和文案表达。以下为各参数取值对应的具体规则：

**画布尺寸（由比例决定）：**

| 比例 | 画布宽度 | 画布高度 | CSS 变量 |
|------|----------|----------|----------|
| 16:9 | 1920px | 1080px | --slide-w: 1920px; --slide-h: 1080px |
| 4:3 | 1440px | 1080px | --slide-w: 1440px; --slide-h: 1080px |

生成的HTML页面内每一张 slide 使用**固定像素尺寸**，通过 `transform: scale()` 适配视口，确保在任何屏幕上比例不变形。

### 1. 确认内容需求

从用户输入中提取：
- 演示主题/标题
- 页数/章节结构
- 关键内容要点
- 汇报人/日期（可选）

### 2. 生成内容结构

基于主题生成结构化内容：
```
- 封面：主标题、副标题、汇报人、日期
- 目录：章节列表
- 内容页 x N：每页含标题、正文/数据/图表
- 结尾页：感谢语
```

### 3. 生成HTML文件

生成**单个自包含HTML文件**，包含所有CSS和JS。文件结构：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[演示标题]</title>
  <style>
    /* 所有样式内联（含base64字体） */
  </style>
</head>
<body>
  <div class="presentation" data-ratio="16:9">
    <section class="slide slide-cover">...</section>
    <section class="slide slide-toc">...</section>
    <section class="slide slide-content">...</section>
    ...
    <section class="slide slide-end">...</section>

    <!-- 左右导航箭头 -->
    <button class="nav-arrow prev">‹</button>
    <button class="nav-arrow next">›</button>

    <!-- 底部导航栏 -->
    <div class="nav-bar">
      <div class="nav-dot active"></div>
      <div class="nav-dot"></div>
      ...
      <span class="nav-counter">1 / N</span>
    </div>
  </div>
  <script>
    /* 翻页逻辑内联 */
  </script>
</body>
</html>
```

### 4. 核心CSS模板

```css
:root {
  /* 16:9 时用 1920x1080, 4:3 时用 1440x1080 */
  --slide-w: 1920px;
  --slide-h: 1080px;
  --s: 0.5; /* JS动态计算的缩放值 */
  --color-primary: #E8923A;
  --color-primary-light: #F0A85C;
  --color-accent: #7FB5B0;
  --color-accent-dark: #5A9E98;
  --color-text: #333333;
  --color-text-secondary: #777777;
  --color-text-light: #AAAAAA;
  --color-bg: #FFFFFF;
  --color-border: #E0E0E0;
  --font-primary: 'Nexa', 'PingFang SC', 'Microsoft YaHei', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
}

* { margin: 0; padding: 0; box-sizing: border-box; }

html, body {
  margin: 0;
  overflow: hidden;
  background: #1a1a1a;
  width: 100vw;
  height: 100vh;
}

body {
  font-family: var(--font-primary);
  color: var(--color-text);
}

/* 演示容器 - 全屏居中 */
.presentation {
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

/* 固定画布幻灯片 - 绝对定位堆叠 */
.slide {
  width: 1920px; /* 或 1440px for 4:3 */
  height: 1080px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(var(--s, 0.5));
  transform-origin: center center;
  overflow: hidden;
  background: #FFFFFF;
  box-shadow: 0 4px 30px rgba(0,0,0,0.4);
  opacity: 0;
  pointer-events: none;
}

.slide.active {
  opacity: 1 !important;
  pointer-events: auto;
  z-index: 2;
}

/* === Morphing 过渡动画 === */
.slide.morphing-out {
  z-index: 1;
  animation: morphOut 0.65s cubic-bezier(0.4, 0, 0.2, 1) forwards;
}
.slide.morphing-in {
  z-index: 3;
  animation: morphIn 0.65s cubic-bezier(0.25, 0.1, 0.25, 1) forwards;
}

@keyframes morphOut {
  0%   { opacity: 1; transform: translate(-50%, -50%) scale(var(--s, 0.5)); }
  100% { opacity: 0; transform: translate(-50%, -50%) scale(calc(var(--s, 0.5) * 0.92)); }
}
@keyframes morphIn {
  0%   { opacity: 0; transform: translate(-50%, -50%) scale(calc(var(--s, 0.5) * 1.04)); }
  100% { opacity: 1; transform: translate(-50%, -50%) scale(var(--s, 0.5)); }
}

/* === 内部元素 stagger 入场 === */
.anim-item {
  opacity: 0;
  transform: translateY(16px) scale(0.97);
  transition: opacity 0.5s cubic-bezier(0.25, 0.1, 0.25, 1),
              transform 0.5s cubic-bezier(0.25, 0.1, 0.25, 1);
}
.anim-item:nth-child(1) { transition-delay: 0.08s; }
.anim-item:nth-child(2) { transition-delay: 0.18s; }
.anim-item:nth-child(3) { transition-delay: 0.28s; }
.anim-item:nth-child(4) { transition-delay: 0.36s; }
.anim-item:nth-child(5) { transition-delay: 0.44s; }

.slide.active .anim-item {
  opacity: 1;
  transform: translateY(0) scale(1);
}

/* 底部导航栏 */
.nav-bar {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  align-items: center;
  gap: 8px;
  z-index: 100;
  background: rgba(0,0,0,0.6);
  padding: 8px 16px;
  border-radius: 20px;
}
.nav-dot {
  width: 8px; height: 8px;
  border-radius: 50%;
  background: rgba(255,255,255,0.3);
  cursor: pointer;
  transition: background 0.3s;
}
.nav-dot.active { background: var(--color-primary); }
.nav-counter {
  color: rgba(255,255,255,0.7);
  font-size: 12px;
  margin-left: 8px;
}

/* 左右箭头覆盖 */
.nav-arrow {
  position: fixed;
  top: 50%;
  transform: translateY(-50%);
  width: 40px; height: 40px;
  background: rgba(0,0,0,0.4);
  border: none;
  border-radius: 50%;
  color: white;
  font-size: 18px;
  cursor: pointer;
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 0.3s;
}
.presentation:hover .nav-arrow { opacity: 1; }
.nav-arrow.prev { left: 20px; }
.nav-arrow.next { right: 20px; }
```

### 5. 核心JS模板

```javascript
const slides = document.querySelectorAll('.slide');
const totalSlides = slides.length;
let current = 0;
let animating = false;

// 视口缩放 - 固定像素画布适配任意屏幕
function scaleSlides() {
  const vw = window.innerWidth || document.documentElement.clientWidth || 800;
  const vh = window.innerHeight || document.documentElement.clientHeight || 600;
  const scale = Math.max(0.1, Math.min(vw / 1920, vh / 1080) * 0.92);
  document.documentElement.style.setProperty('--s', scale);
}
window.addEventListener('resize', scaleSlides);
scaleSlides();

// 初始化第一页
slides[0].classList.add('active');

// Morphing 切换
function goTo(index) {
  if (animating || index === current || index < 0 || index >= totalSlides) return;
  animating = true;

  const oldSlide = slides[current];
  const newSlide = slides[index];

  oldSlide.classList.remove('active');
  oldSlide.classList.add('morphing-out');
  newSlide.classList.add('active', 'morphing-in');

  current = index;
  updateNav();

  setTimeout(() => {
    oldSlide.classList.remove('morphing-out');
    newSlide.classList.remove('morphing-in');
    animating = false;
  }, 680);
}

function next() { goTo(current + 1); }
function prev() { goTo(current - 1); }

// 键盘导航（左右箭头）
document.addEventListener('keydown', (e) => {
  if (e.key === 'ArrowRight' || e.key === 'PageDown' || e.key === ' ') {
    e.preventDefault(); next();
  }
  if (e.key === 'ArrowLeft' || e.key === 'PageUp') {
    e.preventDefault(); prev();
  }
});

// 鼠标滚轮（防抖）
let wheelTimeout;
document.addEventListener('wheel', (e) => {
  e.preventDefault();
  clearTimeout(wheelTimeout);
  wheelTimeout = setTimeout(() => {
    if (e.deltaY > 0 || e.deltaX > 0) next();
    else prev();
  }, 60);
}, { passive: false });

// 触摸滑动
let touchStartX = 0;
document.addEventListener('touchstart', (e) => { touchStartX = e.touches[0].clientX; });
document.addEventListener('touchend', (e) => {
  const diff = touchStartX - e.changedTouches[0].clientX;
  if (Math.abs(diff) > 50) { diff > 0 ? next() : prev(); }
});

// 点击箭头
document.querySelector('.nav-arrow.prev')?.addEventListener('click', prev);
document.querySelector('.nav-arrow.next')?.addEventListener('click', next);

// 导航圆点
document.querySelectorAll('.nav-dot').forEach((dot, i) => {
  dot.addEventListener('click', () => goTo(i));
});
```

### 6. 页面模板详细规范

#### Cover (封面页)

布局：左 38% 大视觉区 + 右 62% 文字区
- 视觉区：内嵌**SVG 矢量插画**（从「封面/目录视觉资产」四种范式中选 1），铺满整列，禁止再用 `.glass-cube` 等 CSS 几何拼贴
- 文字区：eyebrow（小字英文） + 主标题（82px / 800） + 副标题英文（34px / 300） + 4px 高色块分隔线（80px 宽 primary） + 元信息（汇报人 + 日期，20px / 700）
- 动效：visual 与文字区子元素 stagger 入场

```html
<section class="slide slide-cover">
  <div class="cover-visual anim-item">
    <!-- 选用范式 A / B / C / D 之一的完整 SVG -->
    <svg class="cover-svg" viewBox="0 0 600 800" preserveAspectRatio="xMidYMid slice">
      <!-- defs + 多层路径，参见上文「封面/目录视觉资产」 -->
    </svg>
  </div>
  <div class="cover-text">
    <div class="cover-eyebrow anim-item">[英文 eyebrow · 如 REPORT · 2026]</div>
    <h1 class="cover-title anim-item">[主标题]</h1>
    <h2 class="cover-subtitle anim-item">[副标题 / 英文]</h2>
    <div class="cover-divider anim-item"></div>
    <div class="cover-meta anim-item">
      <span class="meta-name">[汇报人]</span>
      <span class="meta-divider">|</span>
      <span class="meta-label">汇报人</span>
    </div>
    <div class="cover-meta anim-item">
      <span class="meta-date">[日期]</span>
      <span class="meta-divider">|</span>
      <span class="meta-label">汇报日期</span>
    </div>
  </div>
</section>
```

配套 CSS 要点：
```css
.cover-visual { width: 38%; position: relative; overflow: hidden; }
.cover-svg { width: 100%; height: 100%; display: block; }
.cover-text { width: 62%; padding: 110px 100px; display: flex; flex-direction: column; justify-content: center; }
.cover-eyebrow { font-family: var(--font-en); font-size: 20px; font-weight: 700; letter-spacing: 0.18em; color: var(--color-primary); margin-bottom: 40px; }
.cover-title { font-size: 82px; font-weight: 800; letter-spacing: 0.02em; line-height: 1.15; color: var(--color-text); margin-bottom: 28px; }
.cover-subtitle { font-family: var(--font-en); font-size: 34px; font-weight: 300; letter-spacing: 0.06em; line-height: 1.4; color: var(--color-text-secondary); margin-bottom: 36px; }
.cover-divider { width: 80px; height: 4px; background: var(--color-primary); margin-bottom: 48px; }
.cover-meta { font-size: 20px; letter-spacing: 0.04em; color: var(--color-text-secondary); display: flex; gap: 16px; margin-bottom: 14px; }
.meta-name, .meta-date { color: var(--color-text); font-weight: 700; }
```

#### TOC (目录页)

布局：左 35% SVG 视觉 + 右 65% 目录内容
- 视觉区：选用与封面**不同**的 SVG 范式（例如封面用 A 光圈，目录用 C 几何分层），制造节奏差异
- "CONTENT" 标题：64px / 700 / letter-spacing 0.24em / `--color-primary`
- 每个章节项左侧 80px 宽编号（Nexa 40px / 700 / primary 主色），右侧标题 30px / 700 + 子项 20px / 400
- 章节项之间以 1px 浅灰分隔线收尾

```html
<section class="slide slide-toc">
  <div class="toc-visual anim-item">
    <svg class="cover-svg" viewBox="0 0 600 800" preserveAspectRatio="xMidYMid slice">
      <!-- 与封面不同的范式 SVG -->
    </svg>
  </div>
  <div class="toc-content">
    <h2 class="toc-header anim-item">CONTENT</h2>
    <div class="toc-subhead anim-item">[本次汇报 N 个章节]</div>
    <div class="toc-list">
      <div class="toc-section anim-item">
        <div class="toc-num">01</div>
        <div class="toc-content-block">
          <h3>[章节标题]</h3>
          <p>[子项1] · [子项2]</p>
        </div>
      </div>
      <!-- ... -->
    </div>
  </div>
</section>
```

配套 CSS 要点：
```css
.toc-visual { width: 35%; overflow: hidden; }
.toc-content { width: 65%; padding: 100px 100px; display: flex; flex-direction: column; }
.toc-header { font-family: var(--font-en); font-size: 64px; font-weight: 700; letter-spacing: 0.24em; color: var(--color-primary); margin-bottom: 12px; }
.toc-subhead { font-size: 22px; letter-spacing: 0.04em; color: var(--color-text-secondary); margin-bottom: 56px; }
.toc-section { display: flex; gap: 28px; padding-bottom: 24px; border-bottom: 1px solid var(--color-border); }
.toc-section:last-child { border-bottom: none; }
.toc-num { font-family: var(--font-en); font-size: 40px; font-weight: 700; letter-spacing: 0.04em; color: var(--color-primary); min-width: 80px; line-height: 1; }
.toc-content-block h3 { font-size: 30px; font-weight: 700; color: var(--color-text); margin-bottom: 10px; }
.toc-content-block p { font-size: 20px; color: var(--color-text-secondary); line-height: 1.6; }
```

#### Content (内容页)

布局：顶部导航栏 + 内容区
- 导航栏：左侧小图标+演示标题(灰色)，右侧logo/机构名，下方1px分隔线
- 标题区：三色方块 ■■■ + 粗体标题
- 内容区：灵活布局（文字、图表、对比、列表等）
- 底部可选来源标注

```html
<section class="slide slide-content">
  <header class="slide-header">
    <div class="header-left">
      <span class="header-icon">
        <svg viewBox="0 0 16 16" width="16" height="16" aria-hidden="true">
          <circle cx="8" cy="8" r="6.5" fill="none" stroke="#E8923A" stroke-width="1.5"/>
          <circle cx="8" cy="8" r="2.5" fill="#E8923A"/>
        </svg>
      </span>
      <span class="header-title">[演示标题]</span>
    </div>
    <div class="header-right">[机构/品牌 · 章节标识]</div>
  </header>
  <div class="slide-body">
    <div class="section-title">
      <span class="title-indicator">■■■</span>
      <h2>[页面标题]</h2>
    </div>
    <div class="content-area">
      <!-- 灵活内容 -->
    </div>
  </div>
  <footer class="slide-footer">
    <span class="source">Source: [来源]</span>
  </footer>
</section>
```

> 注意：内容页中**不要使用**像 ⊙ ◐ ▣ ▶ 这样的字符 emoji 当作"图标"。需要图示时一律使用 inline SVG（圆/同心圆/方块组合即可），保持矢量精度。

#### End (结尾页)

布局：居中大文字 + 装饰
- eyebrow（"END OF REPORT" 类小字英文，primary 色，20px / 700 / 0.3em）
- 主感谢语（Nexa 130-160px / 700）
- 副标题（重述主题 + 日期，22px / 400 / secondary 色）
- 背景：极浅 radial-gradient（中心 `#FFF4E8` → 边缘 `#FFFFFF`），可叠一组极轻的 SVG 同心圆装饰

```html
<section class="slide slide-end">
  <div class="end-content">
    <div class="end-eyebrow anim-item">END OF REPORT</div>
    <h1 class="end-title anim-item">Thank You</h1>
    <p class="end-sub anim-item">[主题简述] · [日期]</p>
  </div>
</section>
```

### 7. 比例适配（固定像素 + Scale）

页面比例在生成时就已确定（通过步骤0的用户选择），**不使用响应式布局**。

**16:9**：`--slide-w: 1920px; --slide-h: 1080px`
**4:3**：`--slide-w: 1440px; --slide-h: 1080px`

所有幻灯片使用绝对定位堆叠在 `.presentation` 容器中，通过 CSS 变量 `--slide-scale` 控制缩放：

```css
.presentation {
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  background: #1a1a1a;
}

.slide {
  width: var(--slide-w);
  height: var(--slide-h);
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(var(--slide-scale, 1));
  transform-origin: center center;
  background: var(--color-bg);
  box-shadow: 0 4px 24px rgba(0,0,0,0.3);
}
```

**JS缩放逻辑（必须包含）：**
```javascript
function scaleSlides() {
  const vw = window.innerWidth || document.documentElement.clientWidth || 800;
  const vh = window.innerHeight || document.documentElement.clientHeight || 600;
  const scale = Math.max(0.1, Math.min(vw / 1920, vh / 1080) * 0.92);
  document.documentElement.style.setProperty('--s', scale);
}
window.addEventListener('resize', scaleSlides);
window.addEventListener('load', scaleSlides);
```

这样生成的 HTML 内的所有元素位置和字号都是以固定像素为基准设计的，视觉效果在任何屏幕上完全一致，不会因视口大小变化而产生布局偏移。

### 8. 内容页变体

根据内容类型选择不同布局：

| 变体 | 用途 | 布局 |
|------|------|------|
| content-text | 纯文字说明 | 标题 + 大段文字 |
| content-highlight | 突出关键词 | 标题 + 居中大字(橙色) |
| content-image | 图文混排 | 标题 + 图片居中 + 说明文字 |
| content-comparison | 对比展示 | 标题 + 左右两栏对比 |
| content-list | 列表要点 | 标题 + 结构化列表 |
| content-chart | 数据图表 | 标题 + CSS图表/表格 |
| content-diagram | 流程/架构图 | 标题 + SVG/CSS图示 |
| content-phase-flow | 阶段分组流程图 | 标题 + Phase-Grouped卡片流(见§11) |

### 9. 动效详细规范

**页面级过渡（Morphing）：**
```css
/* 离场：crossfade + 微缩，营造"收缩离开"感 */
.slide.morphing-out {
  animation: morphOut 0.65s cubic-bezier(0.4, 0, 0.2, 1) forwards;
}
/* 入场：crossfade + 从微放回归，营造"展开到位"感 */
.slide.morphing-in {
  animation: morphIn 0.65s cubic-bezier(0.25, 0.1, 0.25, 1) forwards;
}
@keyframes morphOut {
  0%   { opacity: 1; transform: translate(-50%, -50%) scale(var(--s)); }
  100% { opacity: 0; transform: translate(-50%, -50%) scale(calc(var(--s) * 0.92)); }
}
@keyframes morphIn {
  0%   { opacity: 0; transform: translate(-50%, -50%) scale(calc(var(--s) * 1.04)); }
  100% { opacity: 1; transform: translate(-50%, -50%) scale(var(--s)); }
}
```

**内部元素 Stagger 入场：**
```css
/* 子元素带微缩+上移，依次延迟入场 */
.anim-item {
  opacity: 0;
  transform: translateY(16px) scale(0.97);
  transition: opacity 0.5s cubic-bezier(0.25, 0.1, 0.25, 1),
              transform 0.5s cubic-bezier(0.25, 0.1, 0.25, 1);
}
.anim-item:nth-child(1) { transition-delay: 0.08s; }
.anim-item:nth-child(2) { transition-delay: 0.18s; }
.anim-item:nth-child(3) { transition-delay: 0.28s; }

.slide.active .anim-item {
  opacity: 1;
  transform: translateY(0) scale(1);
}
```

**设计原则：**
- 避免方向性位移（不用 translateX），使过渡在任意切换方向下都自然
- 微缩/微放幅度控制在 ±4~8%，视觉上感知为"呼吸"而非跳变
- 内部元素通过 stagger delay 制造层次感，每个子元素间隔 80~120ms
- 使用 cubic-bezier 缓动确保入场有"落定"感

### 10. 输出要求

1. **单文件**：所有CSS/JS内联在一个.html文件中
2. **自包含**：不依赖外部资源（Nexa字体通过base64内嵌，中文使用系统字体栈）
3. **固定画布**：使用固定像素尺寸（1920x1080 或 1440x1080），通过JS scale适配视口
4. **Morphing翻页**：crossfade + scale 切换，支持键盘/鼠标/触摸
5. **可打印**：添加 `@media print` 样式（每页一页）
6. **无障碍**：语义化HTML标签，合理的aria属性

### 11. 流程图变体：Phase-Grouped Flow Chart

当内容涉及**多阶段流程**（通常3-7步）且存在明确的前置/执行/后置分组逻辑时，使用此布局。特别适合：
- 构建路线图（如评测集构建、产品开发pipeline）
- 方法论步骤展示（数据标注流程、模型训练Pipeline）
- 任何需要突出某一"核心执行步骤"的场景

#### 设计要点

1. **三段分组标签**：PRE-EXECUTION（前置）/ EXECUTION（执行）/ POST-EXECUTION（后置），以灰色大写字母标注在卡片上方
2. **彩色左边条卡片**：每张卡片左侧6px竖条标识阶段归属颜色
3. **CORE步骤深色高亮**：流程中**恰好一个**核心步骤使用深色背景(#2A2D3A) + 白色文字 + 白色左边条
4. **箭头连接器**：相邻卡片间用橙色 `›` 符号连接
5. **可选反馈环路**：底部虚线 + "FEEDBACK LOOP" 标签，用于表达迭代过程

#### 配色规则

| 步骤位置 | 左边条颜色 | CSS类名 | 典型含义 |
|----------|-----------|---------|---------|
| Phase 1 (前置) | #E8923A 橙色 | `.bar-orange` | 需求/定义阶段 |
| Phase 2 (前置) | #7FB5B0 青绿 | `.bar-teal` | 准备/采集阶段 |
| Phase 3 (执行·CORE) | #FFFFFF 白色 | `.bar-dark` + `.core-step` | 核心执行步骤 |
| Phase 4 (后置) | #6BBF7B 绿色 | `.bar-green` | 验证/检验阶段 |
| Phase 5 (后置) | #AAAAAA 灰色 | `.bar-gray` | 发布/收尾阶段 |

> 如果流程少于5步，可酌情减少颜色种类，但 CORE 步骤的深色处理必须保留。

#### HTML 模板

```html
<div class="phase-flow anim-item">
  <!-- 阶段分组标签 -->
  <div class="phase-groups">
    <span class="phase-group-label pre" style="flex:2">PRE-EXECUTION</span>
    <span class="phase-group-label exec" style="flex:1">EXECUTION</span>
    <span class="phase-group-label post" style="flex:2">POST-EXECUTION</span>
  </div>

  <!-- 流程卡片 + 箭头 -->
  <div class="phase-cards">
    <div class="phase-card bar-orange">
      <span class="phase-card-label">STEP 01</span>
      <h4 class="phase-card-title">[步骤标题]</h4>
      <p class="phase-card-sub">[简要描述]</p>
    </div>
    <span class="phase-arrow">›</span>

    <div class="phase-card bar-teal">
      <span class="phase-card-label">STEP 02</span>
      <h4 class="phase-card-title">[步骤标题]</h4>
      <p class="phase-card-sub">[简要描述]</p>
    </div>
    <span class="phase-arrow">›</span>

    <div class="phase-card bar-dark core-step">
      <span class="phase-card-label">STEP 03</span>
      <h4 class="phase-card-title">[核心步骤标题]</h4>
      <p class="phase-card-sub">[核心步骤描述]</p>
    </div>
    <span class="phase-arrow">›</span>

    <div class="phase-card bar-green">
      <span class="phase-card-label">STEP 04</span>
      <h4 class="phase-card-title">[步骤标题]</h4>
      <p class="phase-card-sub">[简要描述]</p>
    </div>
    <span class="phase-arrow">›</span>

    <div class="phase-card bar-gray">
      <span class="phase-card-label">STEP 05</span>
      <h4 class="phase-card-title">[步骤标题]</h4>
      <p class="phase-card-sub">[简要描述]</p>
    </div>
  </div>

  <!-- 可选：反馈环路虚线 -->
  <div class="phase-feedback">
    <div class="feedback-line"></div>
    <span class="feedback-label">FEEDBACK LOOP</span>
  </div>
</div>
```

#### CSS 规范

```css
/* === Phase-Grouped Flow Chart === */
.phase-flow {
  display: flex;
  flex-direction: column;
  gap: 18px;
  width: 100%;
}
.phase-groups {
  display: flex;
  align-items: center;
  gap: 0;
  padding: 0 12px;
}
.phase-group-label {
  text-align: center;
  font-size: 13px;
  font-weight: 700;
  letter-spacing: 0.12em;
  color: var(--color-text-light);
  text-transform: uppercase;
  font-family: var(--font-en, 'Nexa', sans-serif);
}
.phase-group-label.pre { color: #E8923A; }
.phase-group-label.exec { color: #333; }
.phase-group-label.post { color: #7FB5B0; }

.phase-cards {
  display: flex;
  align-items: stretch;
  gap: 0;
  width: 100%;
}
.phase-card {
  flex: 1;
  background: #F9F9F9;
  border-radius: 10px;
  padding: 28px 22px;
  border-left: 6px solid #ccc;
  display: flex;
  flex-direction: column;
  gap: 8px;
  min-height: 140px;
  justify-content: center;
}
.phase-card.bar-orange { border-left-color: #E8923A; }
.phase-card.bar-teal   { border-left-color: #7FB5B0; }
.phase-card.bar-dark   { border-left-color: #FFFFFF; }
.phase-card.bar-green  { border-left-color: #6BBF7B; }
.phase-card.bar-gray   { border-left-color: #AAAAAA; }

/* CORE 步骤深色高亮 */
.phase-card.core-step {
  background: #2A2D3A;
  border-left-color: #FFFFFF;
}
.phase-card.core-step .phase-card-label,
.phase-card.core-step .phase-card-title,
.phase-card.core-step .phase-card-sub {
  color: #FFFFFF;
}
.phase-card.core-step .phase-card-sub {
  color: rgba(255,255,255,0.7);
}

.phase-card-label {
  font-size: 16px;
  font-weight: 700;
  letter-spacing: 0.08em;
  color: var(--color-text-light);
  text-transform: uppercase;
  font-family: var(--font-en, 'Nexa', sans-serif);
}
.phase-card-title {
  font-size: 28px;
  font-weight: 700;
  color: var(--color-text);
  margin: 0;
}
.phase-card-sub {
  font-size: 20px;
  color: var(--color-text-secondary);
  margin: 0;
}

/* 箭头连接器 */
.phase-arrow {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 28px;
  color: var(--color-primary);
  font-weight: 700;
  min-width: 32px;
  flex-shrink: 0;
}

/* 反馈环路虚线 */
.phase-feedback {
  position: relative;
  width: 90%;
  margin: 8px auto 0;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 28px;
}
.phase-feedback .feedback-line {
  position: absolute;
  top: 50%;
  left: 0;
  right: 0;
  border-top: 2px dashed var(--color-primary);
  opacity: 0.5;
}
.phase-feedback .feedback-label {
  position: relative;
  background: #fff;
  padding: 0 14px;
  font-size: 12px;
  font-weight: 700;
  letter-spacing: 0.1em;
  color: var(--color-primary);
  font-family: var(--font-en, 'Nexa', sans-serif);
}
```

#### 使用注意

- `flex` 比例分配：PRE 区域包含2张卡，POST 区域包含2张卡，EXECUTION 区域包含1张卡，因此分组标签的 flex 比例为 `2:1:2`（按实际卡片数量调整）
- 如果步骤数不同于5，调整 flex 比例使分组标签与下方卡片对齐
- 反馈环路仅在流程具有迭代性质时添加（如持续改进、版本迭代等场景）
- 此变体应在 **内容页变体表** 中标记为 `content-phase-flow`

---

### 12. 输出路径

将生成的HTML文件保存到用户指定的位置，默认为工作目录的 `outputs/` 文件夹。

文件命名格式：`[主题简称]-presentation.html`
