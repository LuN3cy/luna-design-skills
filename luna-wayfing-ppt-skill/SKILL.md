---
name: luna-wayfing-ppt-skill
version: 1.5.0
description: Generate Luna Wayfinding style HTML presentations with road-sign inspired design, flicker-free fade-in transitions, and a unified dark footer nav-bar (dots + page counter). Wayfinding visual metaphor with single-signpost cover/end pages. Supports 16:9 and 4:3 aspect ratios, audience-adaptive detail levels, and concise/detailed/narrative writing styles.
description_zh: 生成 Luna Wayfinding（路标导引）风格的 HTML 演示文稿，灵感来源于道路交通标识系统。v1.5 采用单向 fade-in 切换（永不闪屏）+ 与封面脚栏同色的深色 nav-bar（圆点导航 + 页码，无 brand 文本）。单立柱大路牌封面/尾页结构。支持 16:9/4:3 比例、受众适配详细度、凝练/详细/叙事三种语言风格。
category: content-creation
name_zh: Luna Wayfinding 路标导引演示文稿
---

# Luna-Wayfing-PPT-Skill

当用户需要生成**Luna Wayfinding（路标导引）风格**的HTML演示文稿时使用此技能。输出为单个自包含HTML文件，带 Morphing 翻页动效，支持16:9或4:3比例。

## 设计理念

Wayfinding 风格灵感来源于**道路交通标识系统**，融合了路牌、方向标、停车标志、警告标志等交通元素，将信息传达比喻为"导航"。设计调性为：**清晰直接、功能美学、温暖专业**。

核心隐喻：每一页幻灯片 = 一个路标/站点，整个演示 = 一段旅程。

## 0. 调用前必问参数

通过 AskUserQuestion 工具收集以下三个参数：

| 参数 | 选项 | 说明 |
|------|------|------|
| 比例 | 16:9（默认）/ 4:3 | 画布尺寸 |
| 受众 | 专业人士 / 一般受众 / 学生群体 | 影响信息密度和视觉风格 |
| 风格 | 凝练（默认）/ 详细 / 叙事 | 影响文本量和排版 |

## 1. 色彩系统

### 主色板

| Token | 值 | 用途 |
|-------|------|------|
| --color-primary | #4A8C5C | 主色·交通绿（路牌底色、关键标题背景） |
| --color-primary-light | #5EA872 | 悬浮/渐变辅助 |
| --color-primary-dark | #3A7049 | 深色文字、header底色 |
| --color-accent | #F5C542 | 警示黄·强调色（关键数据、警告标志、高亮元素） |
| --color-accent-warm | #E8923A | 暖橙·辅助强调（数字高亮、装饰元素） |
| --color-danger | #C94040 | 禁止红（禁止标志圆环、重要警告） |
| --color-text | #2A2D3A | 正文主色（深灰接近黑） |
| --color-text-secondary | #777777 | 辅助说明文字 |
| --color-text-light | #AAAAAA | 来源/脚注 |
| --color-bg-light | #FFFFFF | 浅色页面背景（内容页默认） |
| --color-bg-dark | #363636 | 深色页面背景（过渡页、互动页） |
| --color-bg-blue | #4A7CB5 | 蓝天色（封面页渐变背景） |
| --color-border | #E0E0E0 | 分隔线、表格边框 |
| --color-card-bg | #F7F7F7 | 表格/辅助区域背景（路牌卡片不使用此色） |

### 辅助色（Gantt图/时间线/数据可视化）

| 名称 | 值 | 用途 |
|------|------|------|
| gantt-blue | #7FB8D8 | 时间线·阶段1（组队/筹备） |
| gantt-green | #6BBF7B | 时间线·阶段2（策划/执行） |
| gantt-yellow | #F0C75E | 时间线·阶段3（制作/开发） |
| gantt-pink | #E88C9A | 时间线·阶段4（后期/收尾） |
| circle-green | #6BBF7B | 编号圆圈·必选课程 |
| circle-pink | #F0A8B8 | 编号圆圈·选讲课程（柔和粉） |
| circle-blue | #A8CCE8 | 编号圆圈·备选课程（柔和蓝） |

## 2. 字体系统

**英文主字体：Overpass**（Highway Gothic 家族开源替代，交通标识专用字体风格）
**中文辅助字体：系统字体栈**（PingFang SC / Microsoft YaHei / Noto Sans SC）

Overpass 通过 Google Fonts CDN 加载，无需本地安装。它是 FHWA Highway Gothic（美国道路标识专用字体）的开源度量兼容替代，完美契合 Wayfinding 交通标识主题。

```html
<link href="https://fonts.googleapis.com/css2?family=Overpass:wght@100..900&display=swap" rel="stylesheet">
```

```css
:root {
  --font-hwy: 'Overpass', 'Highway Gothic', 'FHWA Series', 'Helvetica Neue', Arial, sans-serif;
  --font-zh: 'PingFang SC', 'Microsoft YaHei', 'Noto Sans SC', 'Heiti SC', sans-serif;
}
```

所有元素默认 `font-family: var(--font-hwy), var(--font-zh);`，英文优先使用 Overpass，中文回退系统字体。

### 字号规范

| 层级 | 用途 | 16:9 字号 | 字重 |
|------|------|---------|------|
| Display | 封面主标题 | 72-80px | 900 (Black) |
| H1 | 页面标题 `[中文 English]` | 48-56px | 900 (Black) |
| H2 | 章节副标题 | 40-48px | 900 |
| H3 | 卡片/区块标题 | 26-32px | 800 (ExtraBold) |
| Body | 正文 | 20-24px | 500 (Medium) |
| Caption | 辅助说明 | 16-20px | 500 |
| Label | 标签/来源 | 14-16px | 500 |

### 特殊字号

| 元素 | 字号 | 字重 | 说明 |
|------|------|------|------|
| 大数字高亮 | 72-96px | 900 | 如 "80%" 单独展示，使用 Overpass |
| 编号圆圈内数字 | 32-40px | 300 (Light) | 圆圈内的步骤序号 |
| 路牌上文字 | 按路牌尺寸适配 | 800 | 封面路牌内标题 |
| 底部信息栏 | 18-22px | 700 | 机构/组织 + 日期信息 |

## 3. 页面标题格式

所有内容页标题遵循**方括号中英双语**格式：

```
[ 中文标题 English Title ]
```

- 方括号 `[` `]` 使用**加粗**
- 中文在前，英文在后，空格分隔
- 标题下方紧跟一条**2px实线分隔线**（颜色 `--color-text`）
- 右上角品牌角标图标为**可选项**，默认不显示。仅当用户主动提供 SVG 或 PNG 图片（路径或 data URI）时才插入

**默认状态（无角标）：**

```html
<header class="slide-header">
  <h1 class="section-title">[ 主讲介绍 <span class="en">About Me</span> ]</h1>
</header>
<div class="header-line"></div>
```

**用户提供图标后：**

```html
<header class="slide-header">
  <h1 class="section-title">[ 主讲介绍 <span class="en">About Me</span> ]</h1>
  <div class="brand-mark"><img src="用户提供的图片路径或dataURI" alt="brand logo"></div>
</header>
<div class="header-line"></div>
```

> **品牌角标规范：** 尺寸建议 36×36px ~ 48×48px，绝对定位于 `header` 右侧（`position:absolute; right:40px; top:50%; transform:translateY(-50%)`）。支持格式：SVG（推荐）、PNG（需透明背景）。若用户未提供图片，则 `<header>` 内不包含 `.brand-mark` 元素。

## 4. 布局系统

### 4.1 固定画布

```css
:root {
  --canvas-w: 1920;
  --canvas-h: 1080;
}
.slide {
  width: calc(var(--canvas-w) * 1px);
  height: calc(var(--canvas-h) * 1px);
  transform: scale(var(--s));
  transform-origin: top left;
  overflow: hidden;
  position: relative;
}
```

### 4.2 间距规范

| Token | 值 | 用途 |
|-------|------|------|
| --pad-page | 80px 100px | 页面内边距（上下 左右） |
| --pad-card | 36px 40px | 卡片内边距 |
| --gap-section | 48px | 章节间距 |
| --gap-card | 24px | 卡片间距 |
| --gap-item | 16px | 列表项间距 |

### 4.3 内容区垂直居中

slide-body 在 slide 内 flex 居中：

```css
.slide-body {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 0 100px 40px;
  gap: var(--gap-section);
}
```

## 5. 组件库

### 5.1 封面页（Cover）—— 单立柱大路牌（Single Signpost）风格

**核心结构：仿真高速公路出口路牌。** 一块大号绿底白边主路牌（位于画布上半部，占宽 1080px 左右） + 紧贴其下的窄副牌（同色或对比黄） + **中央一根细灰色立柱**从副牌底部延伸到画布底部 + **画布底部一条深色 footer 栏**（左品牌 / 中导航点位 / 右副标，与 nav-bar 视觉合并）。背景为天蓝渐变 `linear-gradient(180deg,#7CB1DD 0%,#A8CDE8 55%,#CFE4F2 100%)`。

**视觉清单：**
- 背景：天蓝→淡蓝→近白渐变（自上而下 3 段）
- 主牌 `.main-sign`：约 1080px 宽，绿底 + 10px 白内边框 + 4px 绿外描边 + 18px 圆角；内容居中：中文主标题（≤12 字、≤84px font）/ 英文副标题（letter-spacing:.18em, 36px）/ 60% 宽白色细分隔线 / 副标语句（38px white）
- 副牌 `.sub-sign`：贴在主牌下方 14px 处，绿底白边或黄底深字（结束页用黄底以做首尾对比），单行展示 3 块信息（`<span>` + `.pill` + `<span>`）
- 立柱 `.pole`：18px 宽细灰柱，从副牌底部延伸到 footer 顶部，居中
- 底部 footer `.cover-footer`：120px 高，#1d1f24 深底；左 brand 双行 / 右 info 单行；与 nav-bar 视觉对齐

**严禁出现在封面**：
- 龙门架（gantry-beam + 两侧吊杆）—— 已被废弃
- 云朵装饰、禁止标志、演讲者名牌
- 黄色 session-badge 圆

```html
<section class="slide cover-slide">
  <div class="signpost anim-item">
    <div class="main-sign">
      <h1 class="sign-title-zh">控制系统视角下的 AI 产品</h1>
      <p class="sign-title-en">AI PRODUCTS AS CONTROL SYSTEMS</p>
      <hr class="sign-divider">
      <div class="sign-sub-line">工程思维 · 跨部门汇报</div>
    </div>
    <div class="sub-sign">
      <span>Engineering Mindset</span>
      <span class="pill">2026.06</span>
      <span>Sense · Decide · Act · Verify</span>
    </div>
    <div class="pole"></div>
  </div>
  <div class="cover-footer">
    <div class="cf-brand">主标题<br><span class="cf-brand-sub">English subtitle</span></div>
    <div class="cf-info"><span class="cf-info-zh">中文标签 ·</span>fanzhikang · 2026.06.29</div>
  </div>
</section>
```

```css
.cover-slide{
  background:linear-gradient(180deg,#7CB1DD 0%,#A8CDE8 55%,#CFE4F2 100%);
  position:relative;overflow:hidden;
  justify-content:flex-start;align-items:center;
  padding:140px 0 168px; /* 顶 140px 留白，底 168px = 120 footer + 48 nav-bar */
}
.signpost{
  position:relative;display:flex;flex-direction:column;align-items:stretch;
  width:1080px;z-index:3;flex:1; /* flex:1 让 .pole 可拉伸到 footer */
}
.signpost .main-sign{
  width:100%;background:var(--color-primary);
  border:10px solid #fff;outline:4px solid var(--color-primary);
  border-radius:18px;padding:68px 60px 56px;
  color:#fff;text-align:center;
  box-shadow:0 18px 36px rgba(0,0,0,.18);
}
.signpost .main-sign .sign-title-zh{font-size:84px;font-weight:900;line-height:1.1;white-space:nowrap}
.signpost .main-sign .sign-title-en{font-family:var(--font-hwy);font-size:36px;font-weight:700;color:rgba(255,255,255,.92);letter-spacing:.18em;margin-top:14px}
.signpost .main-sign .sign-divider{width:60%;border:none;border-top:3px solid rgba(255,255,255,.55);margin:38px auto 32px}
.signpost .main-sign .sign-sub-line{font-size:38px;font-weight:800;color:#fff}
.signpost .sub-sign{
  margin-top:14px;background:var(--color-primary);
  border:8px solid #fff;outline:3px solid var(--color-primary);
  border-radius:14px;padding:18px 56px;
  color:#fff;font-family:var(--font-hwy);font-size:24px;font-weight:700;
  display:flex;align-items:center;justify-content:center;gap:28px;
  box-shadow:0 8px 18px rgba(0,0,0,.14);
}
.signpost .sub-sign .pill{background:rgba(255,255,255,.92);color:var(--color-primary-dark);padding:4px 14px;border-radius:6px;font-weight:900}
.signpost .pole{
  width:18px;flex:1;align-self:center;margin-top:-6px;min-height:280px;
  background:linear-gradient(90deg,#9aa0a8,#d5d9de 50%,#9aa0a8);
  border-radius:0 0 4px 4px;
  box-shadow:inset -2px 0 0 rgba(0,0,0,.08),inset 2px 0 0 rgba(255,255,255,.4);
}
.cover-footer{
  position:absolute;left:0;right:0;bottom:48px;height:120px;background:#1d1f24;
  color:#fff;display:flex;align-items:center;justify-content:space-between;
  padding:0 80px;z-index:4;font-family:var(--font-hwy);
}
.cover-footer .cf-brand{font-size:22px;font-weight:900;letter-spacing:.08em;line-height:1.3}
.cover-footer .cf-brand .cf-brand-sub{display:block;color:var(--color-accent);font-size:22px;font-weight:900}
.cover-footer .cf-info{font-size:20px;font-weight:700;color:rgba(255,255,255,.78);letter-spacing:.06em}
```

**关键约束（避坑要点）：**
- 中文标题字数 > 12 字时，必须把 `.sign-title-zh` 的 font-size 从 84 → 72/64，否则会撑破白色内边框
- `.signpost { flex:1 }` 是 `.pole { flex:1 }` 能延伸的前提；漏掉 flex:1 → 立柱不长
- `.cover-slide` 的 `padding-bottom:120px` 必须等于 footer 高，否则立柱要么撞穿要么悬空
- 主牌 padding 推荐 68/60/56（上/左右/下）；padding 太大会让中英文挤在中间留太多绿边

**色彩可换：** 整体换 `--color-primary` 为深蓝/酒红，主牌跟随；天空渐变同步换 hue。

### 5.1.1 封面 vs 结束页色彩差异

结束页**复用全部 cover-slide 结构**，仅副牌颜色切换以做首尾呼应：
- 封面 `.sub-sign`：绿底白字（同主牌色）
- 结束 `.end-slide .sub-sign`：黄底深字（`var(--color-accent)`），`.pill` 颜色反相

### 5.2 内容页 —— 浅色底

白色背景，顶部标题行（方括号格式 + 品牌角标），标题下2px黑色分隔线。

**布局变体表：**

| 变体 | 用途 | 布局 |
|------|------|------|
| content-text | 纯文字说明 | 标题 + 大段正文 |
| content-highlight | 突出关键数据 | 标题 + 居中大数字/图标 + 说明 |
| content-two-col | 双栏对比 | 标题 + 左右两栏，各有子标题+内容 |
| content-schedule | 课表/时间线 | 左栏图标列表 + 右栏编号网格 |
| content-table | 数据表格 | 标题 + 结构化表格（分隔线风格） |
| content-flow | 流程步骤 | 标题 + 横向箭头连接的步骤 |
| content-gantt | 甘特图/时间线 | 标题 + 横向彩色条带 |
| content-list-num | 编号列表 | 标题 + 带数字编号的列表项 |
| content-cards-sign | 路牌卡片 | 标题 + 交通标识牌风格的彩色卡片网格 |

### 5.3 过渡页 —— 深色底

深灰/深色背景 (#363636)，居中展示大号文字（白色 + 绿色高亮局部字）。用于章节过渡、互动环节、停顿。

**对比度铁律：深色底页（`.transition-slide` / 任何 `--color-bg-dark` 背景）下，所有正文 / h1 / h2 / p 必须显式声明浅色字号（`color:#fff` 或 `rgba(255,255,255,.X)`）。** 因为 `.slide` 全局默认 `color:var(--color-text)` 是接近黑（#2A2D3A），如果不覆盖，整段文字会与背景同化、几乎不可见。这是 thesis / core judgement 收束页的高频翻车点。

划掉旧概念 → 引出新概念的"strike + 替换"模式如下：

```html
<section class="slide transition-slide">
  <div class="thesis-block">
    <div class="lead-tag">CORE JUDGEMENT · N / N</div>
    <h2>
      AI 产品的本质，不是<br>
      做一个更聪明的<em class="strike">聊天机器人</em>，<br>
      而是 <em class="hi">设计一个能在不确定性中稳定达成目标的控制系统</em>。
    </h2>
    <p class="more"><b>AI 模型是概率性执行器，AI 产品是控制系统。</b>……</p>
    <div class="qline-list">
      <div class="qline">目标值是什么？</div>……
    </div>
  </div>
</section>
```

```css
.transition-slide{
  background:var(--color-bg-dark);
  display:flex;align-items:center;justify-content:center;
}
.transition-slide .big-text{font-size:72px;font-weight:800;color:#fff;text-align:center}
.transition-slide .big-text .highlight{color:var(--color-primary-light)}
.transition-slide .sub-text{font-size:28px;color:rgba(255,255,255,.55);margin-top:16px}

/* === Thesis / Core judgement 收束页（深色底） === */
.thesis-block{padding:0 100px;text-align:left;max-width:1620px;margin:0 auto}
.thesis-block .lead-tag{
  display:inline-block;background:var(--color-accent);color:var(--color-text);
  padding:7px 18px;border-radius:4px;
  font-family:var(--font-hwy);font-size:15px;font-weight:800;letter-spacing:.14em;
  margin-bottom:28px;
}
/* ⚠️ 必须显式白色 —— 不依赖 .slide 默认 color */
.thesis-block h2{
  font-size:54px;font-weight:900;line-height:1.22;
  color:#fff;letter-spacing:-.005em;
}
.thesis-block h2 em{font-style:normal;color:var(--color-accent);position:relative}
/* 划掉的旧概念：暗色 + 红斜线 */
.thesis-block h2 em.strike{
  color:rgba(255,255,255,.42);
  text-decoration:line-through;
  text-decoration-color:var(--color-danger);
  text-decoration-thickness:5px;
}
/* 高亮的新概念：暖黄强调 */
.thesis-block h2 em.hi{color:var(--color-accent)}
.thesis-block .more{
  margin-top:30px;font-size:21px;line-height:1.7;
  color:rgba(255,255,255,.78);max-width:1480px;
}
.thesis-block .more b{color:var(--color-accent);font-weight:800}
.qline-list{
  display:grid;grid-template-columns:repeat(3,1fr);gap:14px;margin-top:30px;
}
.qline{
  background:rgba(255,255,255,.06);border:1.5px solid rgba(255,255,255,.18);
  border-radius:8px;padding:16px 22px;
  font-size:20px;font-weight:700;color:#fff;
  display:flex;align-items:center;gap:14px;
}
.qline::before{content:"?";color:var(--color-accent);font-weight:900;font-size:28px;flex-shrink:0}
```

**避坑速查：** 如果 deck 里有任何 thesis / 论断收束页用了 `--color-bg-dark` 背景，**先检查所有文字 color 是否都被显式覆盖到 `#fff` 或 `rgba(255,255,255,.X)`**。常见漏网之鱼：未加 `em` 包裹的中文连接词（"不是"/"而是"/"但是"）会留下黑色不可见文本。

### 5.4 互动投票页 —— 深色底

深色背景，左侧粗体问题（白色，斜体感），右侧圆形单选按钮 + 选项。

```html
<div class="slide transition-slide poll-slide">
  <div class="poll-grid">
    <div class="poll-question">专业?</div>
    <div class="poll-options">
      <label><span class="radio-circle"></span> 数媒</label>
      <label><span class="radio-circle"></span> 教技</label>
      <label><span class="radio-circle"></span> 其他</label>
    </div>
    <!-- 重复多行问题 -->
  </div>
</div>
```

### 5.5 结束页 —— 与封面同构（同一 signpost）

结束页**必须复用封面的 signpost 结构**，确保首尾视觉呼应，并彻底杜绝"尾页空白"。区别只在文字与副牌底色：

```html
<section class="slide cover-slide end-slide">
  <div class="signpost anim-item">
    <div class="main-sign">
      <h1 class="sign-title-zh">感谢观看</h1>
      <p class="sign-title-en">THANK YOU · Q &amp; A</p>
      <hr class="sign-divider">
      <div class="sign-sub-line">主标题再次出现</div>
    </div>
    <div class="sub-sign">
      <span>作者名</span>
      <span class="pill">日期</span>
      <span>主题标签</span>
    </div>
    <div class="pole"></div>
  </div>
  <div class="cover-footer">
    <div class="cf-brand">主标题<br><span class="cf-brand-sub">English subtitle</span></div>
    <div class="cf-info"><span class="cf-info-zh">中文标签 ·</span>作者 · 日期</div>
  </div>
</section>
```

CSS 仅一行差异化即可：
```css
.end-slide .signpost .sub-sign{background:var(--color-accent);outline-color:var(--color-accent);color:var(--color-text)}
.end-slide .signpost .sub-sign .pill{background:var(--color-text);color:#fff}
```

**严禁：** 结束页只放一句"感谢观看"在画布正中、或只留 `.end-sign` 单个空白绿框 —— 必须复用 signpost 全套（main-sign 三行 + sub-sign 三段 + pole + footer），否则会出现"尾页一片空白"的 bug。

### 5.6 编号圆圈组件

用于课表、步骤等序号展示。细线空心圆 + 内部大号数字（Light字重）。

```css
.num-circle {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  border: 2px solid var(--circle-color, #6BBF7B);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 32px;
  font-weight: 300;
  color: var(--circle-color, #6BBF7B);
}
```

颜色变体：绿色（默认/必修）、粉色（选修）、蓝色（备选）。

### 5.7 双栏带副标题组件

标题行右侧附加灰色辅助说明（如"说人话""跟着大家进度走"等口语化短语）。

```html
<div class="section-header">
  <h2 class="section-title-inner">课程简析</h2>
  <span class="section-aside">说人话</span>
</div>
<div class="section-divider"></div>
```

### 5.8 图标+描述列表组件

左侧圆形线框图标 + 右侧标题+说明文字，竖向堆叠。

```html
<div class="icon-desc-item">
  <div class="icon-circle"><!-- SVG icon --></div>
  <div class="icon-content">
    <h3>60%拍片+40%设计</h3>
    <p>主要是艺术设计类竞赛经验</p>
  </div>
</div>
```

### 5.9 路牌卡片组件

模拟交通标识牌外观：彩色面板底色 + 白色粗边框（全包围） + 外侧一圈细描边（与面板底色一致） + 轻微圆角。

```css
.lbar-card {
  --sign-color: #2E7D42;           /* 面板底色，通过变体class覆盖 */
  background: var(--sign-color);
  border-radius: 6px;
  padding: 24px;
  border: 6px solid #fff;          /* 白色粗边框 */
  outline: 3px solid var(--sign-color); /* 外圈细描边，颜色跟随底色 */
  box-shadow: 0 2px 8px rgba(0,0,0,.15);
  color: #fff;
}
.lbar-card h4 { font-size: 28px; font-weight: 800; color: #fff; }
.lbar-card .card-sub { font-size: 17px; color: rgba(255,255,255,.75); }
.lbar-card .card-stat { font-size: 32px; font-weight: 900; color: #fff; }
```

颜色变体通过覆盖 `--sign-color`、`background` 和 `outline-color` 实现：

| 变体 class | 底色 | 用途举例 |
|-----------|------|---------|
| .bar-green | #2E7D42 | 默认/积极信息 |
| .bar-blue | #37648B | 技术/中性信息 |
| .bar-orange | #C56A1D | 警示/商业 |
| .bar-red | #A03030 | 风险/关键 |
| .bar-yellow | #8C6D12 | 高亮/洞察 |
| .bar-teal | #2B7A72 | 补充/生态 |
| .bar-purple | #5E4580 | 创新/差异化 |
| .bar-pink | #8C3A4A | 社交/用户 |
| .bar-gray | #4A4A4A | 次要/存档 |

**卡片内标签 (.pill)** 使用白色半透明底 + 深色文字，确保在深色路牌背景上可读：

```css
.pill { background: rgba(255,255,255,.92); border-radius: 16px; padding: 5px 16px; font-weight: 700; }
```

**独立信息路牌**（如"趋势观察"、"关键洞察"框）使用相同结构，inline style 写法示例：

```html
<div style="background:#2E7D42;border-radius:6px;padding:20px 28px;border:4px solid #fff;outline:3px solid #2E7D42">
  <span style="color:#fff">内容文字</span>
</div>
```

### 5.10 Gantt 时间线组件

横向彩色条带，竖向按项目分行，横轴按月份。

```css
.gantt-chart { position: relative; padding: 20px 0; }
.gantt-row { display: flex; align-items: center; margin: 20px 0; }
.gantt-label { width: 140px; font-size: 22px; font-weight: 700; }
.gantt-track { flex: 1; position: relative; height: 24px; }
.gantt-bar {
  position: absolute;
  height: 24px;
  border-radius: 12px;
  top: 0;
}
.gantt-bar.team { background: var(--gantt-blue); }
.gantt-bar.plan { background: var(--gantt-green); }
.gantt-bar.build { background: var(--gantt-yellow); }
.gantt-bar.post { background: var(--gantt-pink); }
```

### 5.11 竖向柱状图组件（Bar Chart）

底部对齐的彩色柱体，柱顶上方浮动数值标签 `bar-value`（绝对定位 `top:-32px`），柱底下方挂年份/分类标签 `bar-label`。

```css
.bar-chart{
  display:flex;align-items:flex-end;gap:28px;
  height:320px;                   /* 容器总高 */
  padding:40px 40px 0;            /* 顶部≥40px 给 bar-value 让位 */
  border-bottom:3px solid var(--color-text);
  position:relative;
}
.bar-chart .bar-col{flex:1;display:flex;flex-direction:column;align-items:center;gap:10px}
.bar-chart .bar{width:100%;border-radius:8px 8px 0 0;position:relative}
.bar-chart .bar-label{
  font-family:var(--font-hwy);font-size:18px;
  color:var(--color-text-secondary);text-align:center;
  margin-top:10px;font-weight:600;
}
.bar-chart .bar-value{
  position:absolute;top:-32px;left:50%;transform:translateX(-50%);
  font-family:var(--font-hwy);font-size:19px;font-weight:800;
  white-space:nowrap;
}
```

**关键约束（避坑要点）：**

由于 `*{box-sizing:border-box}` 全局生效，`.bar-chart` 的有效柱体可用高度 = `height − padding-top`。

- 最高柱的 `height` **必须 ≤ 容器 `height − 40px`**（默认 320 − 40 = 280px 上限）。
- 单根柱的 `bar-value` 位于柱顶上方 32px，因此预留的 40px 顶部内边距同时容纳了标签 + 8px 安全间距。
- **禁止用 inline `style="height:XXX"` 强行压缩 `.bar-chart` 高度**——会导致最高柱顶溢出到上方分隔线/标题区。若必须自定义高度，inline 必须同时设置 `padding-top:40px`，或者直接在 `.bar-chart` 上用更大的 height（如 360/400px）。
- 数值/年份比例：单组建议 5 根以内；柱体 `height` 之间保持视觉跳跃比（如 50→95→145→175→260），避免最高与次高过于接近。
- 浅色柱使用 `color:var(--color-text)` 的深色 bar-value，深色柱使用 `color:#fff` 的白色 bar-value。

**HTML 示例：**

```html
<div class="bar-chart"><!-- 不要写 inline height -->
  <div class="bar-col">
    <div class="bar" style="height:50px;background:rgba(74,140,92,.3)">
      <span class="bar-value" style="color:var(--color-text)">~$2.5亿</span>
    </div>
    <span class="bar-label">2023</span>
  </div>
  <!-- ... -->
  <div class="bar-col">
    <div class="bar" style="height:260px;background:var(--color-accent)">
      <span class="bar-value" style="color:var(--color-text)">$33.5亿</span>
    </div>
    <span class="bar-label">2034E</span>
  </div>
</div>
```

### 5.12 交通标志装饰元素

在适当位置使用 SVG 交通标志作为装饰，增强 wayfinding 主题：

| 标志 | SVG元素 | 使用场景 |
|------|---------|---------|
| 禁止标志 | 红色圆环 + 斜线 | 封面装饰、强调"不要做某事" |
| 警告标志 | 黄色三角 + 感叹号 | 重要提醒、注意事项 |
| 停车标志 | 绿底白P方块 | 暂停/中场休息提示 |
| 方向箭头 | 绿色 → | 流程连接、步骤指引 |
| 信息标志 | 蓝底白字 | 人员/角色信息 |

## 6. HTML 骨架结构

### 5.13 底部分页指示器（.nav-bar）—— 必备组件（v1.5：纯色实底，与封面脚栏视觉合并）

**目的：** 用户浏览 PPT 时必须能随时看到「当前页 / 总页数」、可视化进度、并可**点击任意页跳转**，同时鼠标悬停某个 dot 时弹出该页的标题预览气泡。

**结构（v1.5）：** **居中的 dot 导航**（每页一个圆点，active 拉宽为药丸） + **右侧绝对定位页码** + 顶部 2px 进度条。**没有左侧 brand 文本**（v1.4 的 `<div class="nav-brand">` 已删除）；**没有 .on-dark 变体**（颜色不再随幻灯片底色切换）。

**关键决策：**
- nav-bar 用 **纯色实底 `#1d1f24`**，与 `.cover-footer` 颜色完全一致；封面/尾页底栏 + nav-bar 在视觉上合并成一条 168px 高的深色页脚。
- **不用渐变、不用 `backdrop-filter:blur`**——过去试过 `linear-gradient + blur`，浅底页面看起来"灰雾"模糊，深底页面又会出现 banding；纯色干净。
- 所有 dot / pageno / progress 颜色按"深底"默认，不再因当前幻灯片明暗切换。

```css
.nav-bar{
  /* 关键：position:absolute 挂在 #deck 内部，跟随画布缩放；避免全屏 letterbox 时落到黑底里被吞 */
  position:absolute;left:0;right:0;bottom:0;z-index:9999;height:48px;
  display:flex;align-items:center;justify-content:center;
  padding:0 56px;font-family:var(--font-hwy);
  background:#1d1f24;            /* 纯色实底，与 cover-footer 同色 */
  pointer-events:auto;
}
.nav-progress{
  position:absolute;left:0;right:0;bottom:0;height:2px;
  background:rgba(255,255,255,.08);
}
.nav-progress-fill{
  height:100%;width:0%;
  background:var(--color-accent);     /* 累计进度用警示黄一抹细线，不抢戏 */
  transition:width .55s cubic-bezier(.4,0,.2,1);
}
.nav-dots{display:flex;align-items:center;gap:9px;pointer-events:auto}
.nav-dot{
  width:7px;height:7px;border-radius:50%;
  background:rgba(255,255,255,.28);cursor:pointer;
  transition:all .25s cubic-bezier(.4,0,.2,1);position:relative;
}
.nav-dot:hover{background:#fff;transform:scale(1.4)}
.nav-dot.active{width:22px;border-radius:4px;background:var(--color-accent)}
/* hover 时弹出预览气泡（页码 + 章节标题） */
.nav-dot .preview{
  position:absolute;left:50%;bottom:calc(100% + 12px);
  transform:translateX(-50%) translateY(4px);
  background:#0f1115;color:#fff;font-family:var(--font-hwy);
  font-size:12px;font-weight:700;letter-spacing:.06em;
  padding:6px 12px;border-radius:4px;white-space:nowrap;
  pointer-events:none;opacity:0;
  transition:opacity .18s ease, transform .18s ease;
  box-shadow:0 4px 14px rgba(0,0,0,.4);z-index:10;
  border:1px solid rgba(255,255,255,.08);
}
.nav-dot .preview::after{
  content:"";position:absolute;left:50%;top:100%;transform:translateX(-50%);
  border:5px solid transparent;border-top-color:#0f1115;
}
.nav-dot:hover .preview{opacity:1;transform:translateX(-50%) translateY(0)}
.nav-dot .preview .pn{color:var(--color-accent);font-weight:900;margin-right:6px}
/* 页码：绝对定位贴右，让 dots 自然居中而不被推偏 */
.nav-pageno{
  position:absolute;right:56px;top:50%;transform:translateY(-50%);
  font-size:14px;font-weight:700;color:rgba(255,255,255,.55);
  letter-spacing:.12em;pointer-events:auto;
  font-variant-numeric:tabular-nums;
}
.nav-pageno .np-cur{color:#fff;font-weight:900}
```

**配套 JS 注入（v1.5）：**

```js
function titleOf(s,i){
  if(s.classList.contains('cover-slide')&&!s.classList.contains('end-slide'))return '封面 Cover';
  if(s.classList.contains('end-slide'))return '尾页 Thank You';
  const t=s.querySelector('.section-title');
  if(t)return (t.textContent||'').replace(/[\[\]【】]/g,'').trim().slice(0,28);
  return 'Slide '+(i+1);
}
const dotsHtml=slides.map((s,i)=>{
  const tt=titleOf(s,i).replace(/"/g,'&quot;');
  return '<div class="nav-dot" data-idx="'+i+'"><div class="preview"><span class="pn">'+String(i+1).padStart(2,'0')+'</span>'+tt+'</div></div>';
}).join('');
/* v1.5：不再有 nav-brand */
navBar.innerHTML=
  '<div class="nav-dots">'+dotsHtml+'</div>'+
  '<div class="nav-pageno"><span class="np-cur">01</span> &nbsp;/&nbsp; <span class="np-tot">'+String(total).padStart(2,'0')+'</span></div>'+
  '<div class="nav-progress"><div class="nav-progress-fill"></div></div>';
deck.appendChild(navBar);   // ★ 必须 deck，不能是 document.body
// dot 点击跳转
navBar.querySelectorAll('.nav-dot').forEach(d=>d.addEventListener('click',e=>{
  e.stopPropagation();
  const idx=parseInt(d.dataset.idx);
  if(idx!==cur)go(idx);
}));
/* v1.5：updateNav 不再 toggle .on-dark —— nav-bar 永远是同一种深色 */
function updateNav(){
  fill.style.width=((cur+1)/total*100)+'%';
  npCur.textContent=String(cur+1).padStart(2,'0');
  dotEls.forEach((d,i)=>d.classList.toggle('active',i===cur));
}
```

**关键约束（避坑要点）：**
- **必须 `deck.appendChild(navBar)`，不能 `document.body.appendChild`。** 全屏播放 (e.g. 21:9 显示器看 16:9 deck) 会出现上下黑色 letterbox，fixed 定位的 nav-bar 会落到黑底里。挂在 deck 内并用 `position:absolute` 让 nav-bar 跟着画布缩放，永远在画布内。
- 既然挂在 1920×1080 画布内会被整体缩放，字号要相应放大（14-15px），padding 也放大（56px 而非 28px）。
- `.cover-footer` 必须 `bottom:48px`（脚栏高度 = nav-bar 高度），同时 `.cover-slide { padding-bottom: 168px }`（120 footer + 48 nav），让 pole 仍精确顶到脚栏顶端。
- 页码 `.nav-pageno` **必须 `position:absolute; right:56px`**，否则会随 flex 把 dots 推离视觉中心。
- 章节标题取自 `.section-title.textContent`，**自动剥离方括号** `[ ] 【】`；超过 28 字截断。
- 封面/尾页用语义化名称（"封面 Cover"/"尾页 Thank You"），不显示页面标题。
- 点击 dot 时 **必须 `e.stopPropagation()`**，否则会被 deck-level 的 click 监听（左右半屏翻页）吞掉。
- **历史教训（已废弃）：** v1.0 的"渐变+blur"底色 + v1.4 的"on-dark 切换"双方案在不同明暗页面上都不耐看；v1.5 改纯色实底 + 永远深色 + 去掉左侧 brand 标签后视觉最干净。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[演示标题]</title>
  <style>
    /* === 全局变量 === */
    :root {
      --canvas-w: 1920;
      --canvas-h: 1080;
      --color-primary: #4A8C5C;
      --color-primary-light: #5EA872;
      --color-primary-dark: #3A7049;
      --color-accent: #F5C542;
      --color-accent-warm: #E8923A;
      --color-danger: #C94040;
      --color-text: #2A2D3A;
      --color-text-secondary: #777777;
      --color-text-light: #AAAAAA;
      --color-bg-light: #FFFFFF;
      --color-bg-dark: #363636;
      --color-bg-blue: #4A7CB5;
      --color-border: #E0E0E0;
      --color-card-bg: #F7F7F7;
      --font-main: 'MiSans', 'PingFang SC', 'Microsoft YaHei', 'Noto Sans SC', sans-serif;
      --font-en: 'MiSans', 'Helvetica Neue', Arial, sans-serif;
    }
    
    /* === Reset + 画布 === */
    * { margin: 0; padding: 0; box-sizing: border-box; }
    html, body { background: #1a1a1a; overflow: hidden; width: 100%; height: 100%; }
    body { display: flex; align-items: center; justify-content: center; }
    
    .deck { position: relative; width: calc(var(--canvas-w)*1px); height: calc(var(--canvas-h)*1px); transform: scale(var(--s)); transform-origin: center center; }
    
    .slide {
      position: absolute; inset: 0;
      width: calc(var(--canvas-w)*1px);
      height: calc(var(--canvas-h)*1px);
      opacity: 0; pointer-events: none; z-index: 0;
      display: flex; flex-direction: column;
      font-family: var(--font-main);
      color: var(--color-text);
      background: var(--color-bg-light);
      overflow: hidden;
      transition: opacity .55s ease;     /* v1.5：唯一动画属性 */
    }
    .slide.active { opacity: 1; pointer-events: auto; z-index: 2; }
    .slide.prev   { opacity: 1; z-index: 1; transition: none; }   /* 旧层兜底 */
    
    /* === 页面标题 === */
    .slide-header {
      display: flex; justify-content: space-between; align-items: flex-start;
      padding: 52px 100px 0;
    }
    .section-title {
      font-size: 42px; font-weight: 700;
      color: var(--color-text);
      font-family: var(--font-main);
    }
    .section-title .en {
      font-family: var(--font-en);
      font-weight: 400;
    }
    .brand-mark {
      font-size: 36px; font-weight: 900; color: var(--color-text);
      /* C-logo mark placeholder */
    }
    .header-line {
      margin: 20px 100px 0;
      border-top: 2px solid var(--color-text);
    }
    
    .slide-body {
      flex: 1;
      display: flex; flex-direction: column;
      justify-content: center;
      padding: 0 100px 40px;
      gap: 48px;
    }
    
    /* ... 其他核心CSS见各组件规范 ... */
  </style>
</head>
<body>
<div class="deck" id="deck">
  <!-- Slide 1: Cover -->
  <!-- Slide 2-N: Content -->
  <!-- Slide N+1: End -->
</div>
<script>
  /* === Scale + Navigation ===
     必备四类交互：键盘 / 鼠标点击 / 触摸滑动 / 滚轮翻页。
     底部 .nav-bar 固定指示：进度条 + "n / N" 页码。
  */
  (function(){
    const W = parseInt(getComputedStyle(document.documentElement).getPropertyValue('--canvas-w'));
    const H = parseInt(getComputedStyle(document.documentElement).getPropertyValue('--canvas-h'));
    const deck = document.getElementById('deck');
    function resize(){
      const s = Math.min(innerWidth/W, innerHeight/H);
      deck.style.setProperty('--s', s);
      deck.style.width = W+'px';
      deck.style.height = H+'px';
      deck.style.transform = 'scale('+s+')';
    }
    addEventListener('resize', resize); resize();

    const slides = Array.from(deck.querySelectorAll('.slide'));
    const total = slides.length;
    let cur = 0;

    /* 注入底部导航条 —— 详见 §5.13；务必 deck.appendChild，不能 document.body */
    const navBar = document.createElement('div');
    navBar.className = 'nav-bar';
    /* navBar.innerHTML 参见 §5.13 v1.5：dots + pageno + progress，no brand */
    deck.appendChild(navBar);
    /* ... 略，详见 §5.13 ... */

    function animItems(slide, delay){
      slide.querySelectorAll('.anim-item').forEach((el,i)=>{
        el.style.opacity='0'; el.style.transform='translateY(18px)';
        setTimeout(()=>{ el.style.transition='opacity .5s ease, transform .5s ease'; el.style.opacity='1'; el.style.transform='translateY(0)'; }, delay+i*90);
      });
    }
    /* v1.5 单向 fade-in 切换：不闪屏，详见 §7 */
    function go(n){
      if(n<0||n>=total||n===cur) return;
      const prev = slides[cur];
      const next = slides[n];
      cur = n;
      prev.classList.remove('active');
      prev.classList.add('prev');
      next.classList.remove('prev');
      void next.offsetWidth;             /* ★ reflow，必须 */
      next.classList.add('active');
      setTimeout(()=>prev.classList.remove('prev'), 600);
      animItems(next, 260);
      updateNav();
    }
    slides[0].classList.add('active');
    animItems(slides[0], 400);
    updateNav();

    /* —— 键盘 —— */
    addEventListener('keydown', e=>{
      if(e.key==='ArrowRight'||e.key==='ArrowDown'||e.key===' '||e.key==='PageDown'||e.key==='Enter') go(cur+1);
      if(e.key==='ArrowLeft'||e.key==='ArrowUp'||e.key==='PageUp'||e.key==='Backspace') go(cur-1);
    });
    /* —— 点击：左右半屏 —— */
    addEventListener('click', e=>{
      if(e.target.closest('a,button,input,label,select,textarea,.nav-bar')) return;
      if(e.clientX > innerWidth*0.5) go(cur+1); else go(cur-1);
    });
    /* —— 触摸 —— */
    let tx=0;
    addEventListener('touchstart', e=>{ tx=e.touches[0].clientX; });
    addEventListener('touchend', e=>{
      const dx = e.changedTouches[0].clientX - tx;
      if(Math.abs(dx)>40) dx<0 ? go(cur+1) : go(cur-1);
    });
    /* —— 滚轮 / 触控板（带节流，避免一次惯性滑动连翻多页）—— */
    let wheelLock = false;
    addEventListener('wheel', e=>{
      e.preventDefault();
      if(wheelLock) return;
      if(Math.abs(e.deltaY) < 20 && Math.abs(e.deltaX) < 20) return;
      wheelLock = true;
      if(e.deltaY > 0 || e.deltaX > 0) go(cur+1); else go(cur-1);
      setTimeout(()=>{ wheelLock = false; }, 750);
    }, { passive:false });
  })();
</script>
</body>
</html>
```

## 7. 核心 CSS 模板（v1.5 单向 fade-in 切换，永不闪屏）

**关键问题（历史教训）：** v1.4 双向 `morphOut + morphIn` 同步动画 —— 即使加了 `visibility:visible` 兜底，只要切换瞬间「两层都处于 opacity<1」就会出现明显的 brightness 下降（用户视感为"闪屏"）。任何"出 + 进"对称 crossfade 都无法根除这个问题。

**v1.5 修复方案（必须遵守）：**
1. **单向 fade-in，不做 fade-out**。旧层在整个过渡期内保持 `opacity:1` 不动，新层从 `opacity:0` 淡入到 `opacity:1` 覆盖在上方。
2. 切换流程（三步走）：
   - `prev.classList.remove('active'); prev.classList.add('prev')` → 旧层维持完全不透明，z-index 降为 1。
   - `next.classList.remove('prev'); void next.offsetWidth; next.classList.add('active')` → 强制 reflow 后触发 transition，新层从 0 淡入到 1，z-index = 2 覆盖在上。
   - 等待 600ms 转场结束后再 `prev.classList.remove('prev')` → 此时新层已完全覆盖，移除旧层 visibility 时人眼无感。
3. **不要 `visibility:hidden`**。基础 `.slide` 只用 `opacity:0; pointer-events:none` 来隐藏。

```css
.slide{
  position:absolute;inset:0;
  width:calc(var(--canvas-w)*1px);height:calc(var(--canvas-h)*1px);
  opacity:0;pointer-events:none;z-index:0;
  display:flex;flex-direction:column;
  font-family:var(--font-hwy),var(--font-zh);
  color:var(--color-text);background:var(--color-bg-light);overflow:hidden;
  transition:opacity .55s ease;   /* 唯一动画：opacity */
}
.slide.active{opacity:1;pointer-events:auto;z-index:2}
.slide.prev{opacity:1;z-index:1;transition:none}   /* 旧层保留满不透明在下层 */
```

```js
function go(n){
  if(n<0||n>=total||n===cur)return;
  const prev=slides[cur];
  const next=slides[n];
  cur=n;
  /* 1. 旧层降为 .prev：保持完全不透明在下层 */
  prev.classList.remove('active');
  prev.classList.add('prev');
  /* 2. 新层从 opacity:0 开始（强制 reflow 后再加 .active 才能触发 transition） */
  next.classList.remove('prev');
  void next.offsetWidth;                  /* ★ reflow trick：确保 transition 生效 */
  next.classList.add('active');
  /* 3. transition 结束后再去掉 .prev —— 此刻新层已 opacity:1 完全覆盖，旧层瞬间归 0 无感 */
  setTimeout(()=>prev.classList.remove('prev'),600);
  animItems(next,260);
  updateNav();
}
```

**严禁：**
- 不要写 `@keyframes morphOut/morphIn` 双向动画。
- 不要在基础 `.slide` 上写 `visibility:hidden`。
- 不要省略 `void next.offsetWidth` —— 浏览器会把 `remove('prev')` 与 `add('active')` 批量成同一帧，transition 不会触发，会变成"瞬切"。

## 8. 页面模板规范

### 8.1 Cover（封面页）

- 背景：CSS线性渐变 蓝 → 淡蓝 → 白
- 底部：CSS绘制云朵（多个白色椭圆叠加 border-radius）
- 中央路牌结构：绿底圆角矩形 + 白色边框 + 灰色立柱
- 路牌内：主标题(白字) + 英文副标题(白字) + 绿色分隔线 + 黄色session编号徽章 + 分支标题(白字)
- 路牌下方：白底绿边小牌（地点·日期·时间）
- 左侧装饰：禁止标志（可定制内容）
- 左下：演讲者蓝色名牌
- 底栏：灰底黄色文字，左机构、右活动

### 8.2 Content（内容页）

- 白色背景
- 标题格式：`[ 中文 English ]` + 右上角品牌标
- 标题下2px黑色分隔线
- 内容区：flex居中，根据变体使用不同布局
- 底部可选灰色脚注

### 8.3 Transition（过渡页）

- 深灰背景 #363636
- 居中大号文字，关键字绿色高亮
- 英文副标题灰色
- 也可配合道路标志图标（如停车P标志 → "暂停一下"）

### 8.4 End（结束页）

- 与封面相同的蓝天+云朵+路牌结构
- 路牌内"感谢观看" + 结束标识
- 底栏保持一致

## 9. 动效详细规范

**页面级过渡（v1.5 单向 fade-in，永不闪屏）：**
- 唯一动画属性是 `opacity`，时长 0.55s，缓动 `ease`。
- 旧层（`.prev`）整个过渡期 `opacity:1` 不动，仅 z-index 降到 1，留作"底色兜底"。
- 新层（`.active`）从 `opacity:0` 淡入到 `opacity:1`，z-index = 2 覆盖在上。
- 600ms 后 JS 才 `prev.classList.remove('prev')`，让旧层瞬间归零（此刻新层已满覆盖，肉眼无感）。
- **没有 fade-out、没有 scale、没有 visibility 切换** —— 任何对称 crossfade 都会在中间帧出现亮度下降，被用户感知为"闪屏"。
- JS 内必须 `void next.offsetWidth` 强制 reflow，否则浏览器合并 class 操作 → transition 不触发 → 变瞬切。

**内部元素 stagger 入场：**
- `.anim-item` 元素按序入场，首张延迟 400ms，后续切换延迟 260ms
- 项间隔 90ms
- 初始 `opacity:0` + `translateY(18px)`
- 过渡 0.5s ease

**设计原则：**
- 避免方向性位移（不用 `translateX`），使过渡在任意切换方向下都自然
- **页面级动画只动 opacity**，不做 scale / blur / translate（一切位移和形变都极易暴露"两层错位"）
- 内部 `.anim-item` 可保留 `translateY(18px)` 的轻微落定感，这是单层动画，不影响整体亮度

## 10. 比例适配

### 16:9 → 4:3 适配

```css
/* 4:3 适配 */
:root {
  --canvas-w: 1440;
  --canvas-h: 1080;
}
```

4:3模式下的调整：
- 页面内边距：80px → 60px
- 标题字号：42px → 36px
- 正文字号：22px → 20px
- 卡片间距：24px → 18px
- 路牌宽度适当缩小

## 11. 内容变体详细规范

### 11.1 content-highlight（数据高亮）

左侧大型图标/图形（如圆形百分比环）+ 右侧大号数据文字 + 说明。

```html
<div class="slide-body">
  <div class="highlight-row">
    <div class="highlight-visual">
      <!-- 如 SVG 圆形进度环 80% -->
    </div>
    <div class="highlight-text">
      <h2 class="big-number">出勤率 ≥ <span class="accent">80%</span></h2>
      <p class="highlight-desc">获评"合格学员"</p>
    </div>
  </div>
</div>
```

### 11.2 content-flow（横向流程）

步骤之间用 `→` 箭头连接，可附加时间/说明。

```html
<div class="flow-row">
  <span class="flow-step">组队</span>
  <span class="flow-arrow">→</span>
  <span class="flow-step">分工</span>
  <span class="flow-arrow">→</span>
  <span class="flow-step">选题</span>
  <!-- ... -->
</div>
```

### 11.3 content-two-col（双栏并列）

每栏有独立标题 + 辅助说明 + 下方内容，标题下有分隔线。

### 11.4 content-gantt（甘特图）

横向时间线，纵向项目列表，彩色圆角条带表示阶段。右上角放置图例。

## 12. 装饰规范

### 交通标志SVG模板

**禁止标志：**
```svg
<svg viewBox="0 0 100 100" width="120" height="120">
  <circle cx="50" cy="50" r="45" fill="none" stroke="#C94040" stroke-width="8"/>
  <line x1="20" y1="80" x2="80" y2="20" stroke="#C94040" stroke-width="8"/>
  <text x="50" y="58" text-anchor="middle" font-size="36" font-weight="700" fill="#2A2D3A">急</text>
</svg>
```

**警告标志：**
```svg
<svg viewBox="0 0 100 100" width="100" height="100">
  <polygon points="50,8 95,88 5,88" fill="#F5C542" stroke="#2A2D3A" stroke-width="4"/>
  <text x="50" y="72" text-anchor="middle" font-size="48" font-weight="900" fill="#2A2D3A">!</text>
</svg>
```

**停车标志：**
```svg
<svg viewBox="0 0 80 80" width="80" height="80">
  <rect x="4" y="4" width="72" height="72" rx="10" fill="#4A8C5C" stroke="#fff" stroke-width="4"/>
  <text x="40" y="56" text-anchor="middle" font-size="44" font-weight="700" fill="#fff">P</text>
</svg>
```

**角落文档装饰（右下角文件夹图标）：**
用于"资源分享""文件说明"类页面右下角，CSS/SVG绘制一个略倾斜的文件+文件夹，附加手写体小字。

## 13. 输出要求

1. **单文件**：所有CSS/JS内联在一个.html文件中
2. **自包含**：不依赖外部资源（字体使用系统字体栈，SVG装饰内联）
3. **固定画布**：使用固定像素尺寸（1920x1080 或 1440x1080），通过JS scale适配视口
4. **Morphing翻页**：crossfade + scale 切换，支持键盘/鼠标/触摸
5. **可打印**：添加 `@media print` 样式（每页一页）
6. **无障碍**：语义化HTML标签，合理的aria属性
7. **深色过渡页**：在内容节奏需要时插入深色过渡页
8. **路标装饰**：封面必须使用路牌风格，正文中适当使用交通标志装饰

## 14. 输出路径

将生成的HTML文件保存到用户指定的位置，默认为工作目录的 `outputs/` 文件夹。

文件命名格式：`[主题简称]-presentation.html`
