---
name: cli-terminal-report
description: 生成命令行/终端风格的单文件 HTML 汇报文档。模仿 Reisinger Studio 极简等宽字体美学——纯排版无边框、[+][-][•][↗] 符号树状导航、boot sequence 打字动画、CRT 微光扫描线、blinking cursor 状态栏。当用户要求"终端风格""命令行风格""CLI 风格""极简等宽""monospace 排版""复古终端"的汇报/报告/笔记 HTML 时使用。
version: 1.0.0
---

# CLI Terminal Report

按 Reisinger Studio 风格生成等宽字体、纯排版、ASCII 符号驱动的汇报 HTML。

## 启动 SPEC（每次必问）

在动笔之前，**先用 AskUserQuestion 收齐 4 个字段**，它们既是顶部导航栏内容，也是文件名/作者元数据的唯一来源。除非用户在最初指令里已经一次性给齐，否则不要凭记忆/默认值替代。

| 字段 | 用途 | 默认值（仅当用户明确"沿用默认"时使用） |
|---|---|---|
| `DOC` 文档名称 | 顶栏第 1 行 + 文件名 slug | 由当前任务主题转写 |
| `DATE` 生成日期 | 顶栏 + 文件名后缀 | 当日 `YYYY-MM-DD`（用 `date` 校准） |
| `AUTHOR` 作者 | 顶栏署名 | `fanzhikang`（仅在用户曾默认时） |
| `VER` 版本号 | 顶栏 + 文件名 | `1.0.0` 起步，后续手动迭代 |

顶部 Banner **固定只有这 4 行**，顺序：`DOC → DATE → AUTHOR → VER`，不再夹塞副标题/STUDIO 标语等装饰行。每行用 typewriter 串行打出（80ms 间隔）。

## 语言一致性

侧栏 `CONTENTS` 项的语言、章节锚点标题、`# SECTION_NAME` 大标题、ASCII 流程图节点文字，全部跟随**正文主体语言**：

- 正文中文 → 目录/锚点/流程图节点全用中文（编号可保留 ASCII，如 `00 序章`、`01 第一时代 · HDR 时代`）；不允许目录英文 + 正文中文这种割裂。
- 正文英文 → 全英文。
- 双语正文（如术语原文）→ 目录用主语言 + 必要时英文括注。

Banner 的 4 个字段名（`DOC / DATE / AUTHOR / VER`）保持英文不变，它们是 spec 元数据不是内容。

## 视觉令牌

```
--bg:     #f4efe6   (warm paper)        | dark: #0c0c0c
--fg:     #1a1a1a                       | dark: #d9d4c7
--muted:  #6e6a5f                       | dark: #8a8576
--accent: #c8462c   terracotta（默认 RED，可切换）
--rule:   #c9c1ad                       | dark: #2a2820
```

强调色（accent）支持运行时切换，详见 `## 强调色切换器`。

字体：`'JetBrains Mono', 'IBM Plex Mono', ui-monospace, monospace`
正文 14px / 行高 1.7 / 标题与正文同字体同字号家族（最大 18px）。

**禁止**：边框、阴影、圆角、渐变、彩色 icon、emoji、sans-serif、伪 stats 卡片。

## 符号词汇表

| 符号 | 语义 | 交互 |
|---|---|---|
| `[+]` | collapsed | 点击展开变 `[-]` |
| `[-]` | expanded | 点击折叠变 `[+]` |
| `[ ]` | 默认导航项 | 点击跳转锚点；不切换为 `[•]` |
| `[•]` | **当前正在阅读的章节**（scrollspy 自动标记） | 用户不直接操作，由滚动位置决定 |
| `[↗]` | external link | target=_blank |
| `>>` | block quote prefix |   |
| `$` / `>` | prompt prefix |   |
| `█` | cursor | `animation: blink 1s steps(2) infinite` |
| `─` × N | rule | 装饰横线 |

**重要**：`[•]` 不再表示"已读 / 已点击"。它是阅读位置指示器——同一时刻有且仅有一个章节项处于 `[•]` 状态，其余保持 `[ ]`。实现方式：IntersectionObserver 监听 `<main>` 中各 section，进入视口时把对应侧栏行切换为 `[•]`（其它行复位为 `[ ]`），同时给该行加 `accent` 文字色以强化"你在这里"。

缩进 2 空格。可用大写字母 A/B/C 前缀分组（参考 Reisinger 截图 tag 树）。

## 版式

- ≥960px：左 280–320px 侧栏 + 右内容区（max-width 820px）
- <960px：纵向堆叠，侧栏顶部置顶可折叠
- 全页 padding 40–60px，背景纯色，**无任何边框线**
- 区段之间用 `─` × 60 ASCII 线分隔，不用 `<hr>` 默认样式

## 内容结构

1. **Banner**（顶部 4 行，固定字段）：`DOC` / `DATE` / `AUTHOR` / `VER`，逐行 typewriter 出现，不允许加副标题或 STUDIO 标语等额外行。
2. **Sidebar**（左）：**只保留一个 `CONTENTS` 组**，按正文章节顺序铺列锚点项。**禁止再加** `PLATFORM` / `SEARCH BY TAGS` / `ORDER` 等装饰组——它们只是视觉噪声、对正文导航无贡献。如果存在外链参考资料，统一以正文最后一节 `# REFERENCES` 的形式呈现（节内用 `[↗]` 列出），同时它也是 CONTENTS 的最后一个锚点项。
3. **Main**（右）：每段以 `# SECTION_NAME` + ASCII rule 起头；正文 prose 为主，**禁止 `<ul><li>`**，列举一律用 `<pre>` ASCII 树或表格。
4. **Footer prompt**：`$ cat doc.md` + 闪烁 `█`。

## 流程图（ASCII Diagram）

流程图、对比矩阵、时间线一律用 `<pre>` 写的 ASCII 字符画，与正文同字体同色板，**禁止**引入 SVG / Mermaid / canvas / 图片。

字符词汇表：

| 字符 | 语义 |
|---|---|
| `┌ ┐ └ ┘ ─ │ ├ ┤ ┬ ┴ ┼` | 方框 / 分支 |
| `→ ← ↑ ↓ ↗ ↘ ↖ ↙` | 单向流 |
| `⇄ ⇅ ⇆ ⇋` | 双向流 / 协同 |
| `═ ║ ╔ ╗ ╚ ╝` | 强调路径（用于关键链路） |
| `· ┄ ┈` | 弱化 / 旁路 / 可选 |
| `[label]` 或 `< label >` | 单节点（方括号=组件、尖括号=输入/输出） |
| `(n)` | 阶段编号，与正文 numbered takeaway 对齐 |

布局规则：

- 每张图前置一行 `# 图名` 标题，下方一行 `─` × 60 分隔；图后用 1 段 prose 解释，不堆砌另一张图。
- 节点宽度优先用空格对齐到列；不要为了对齐塞奇怪 Unicode 空格——纯 ASCII 半角空格 + box-drawing 字符即可。
- 横向流向左→右；纵向流向上→下；分支用 `├ ┴ ┬` 收束。
- 关键路径用双线 `═ ║`，次要旁路用 `┄ ┈`，颜色仍由 `.hit / .muted` 类承担，不引入额外颜色。
- 单图行宽 ≤ 72 字符，避免横向滚动；超大流程拆成多张图，用编号衔接。

参考骨架：

```
# 图名
────────────────────────────────────────────────────────────
   ┌────────────┐      ┌────────────┐      ┌────────────┐
   │  输入帧 X  │ ───► │  对齐 + 融合 │ ───► │  端上 ML 输出 │
   └────────────┘      └─────┬──────┘      └────────────┘
                             │
                             ▼
                       ┌────────────┐
                       │  ProRAW 元数据 │
                       └────────────┘
```

## 强调色切换器

页面顶部右侧提供一个 ASCII 风格的切换器，提供三档强调色，用户随时切换、不需要重载：

```
ACCENT [•]RED  [ ]BLUE  [ ]GREEN
```

色板（写死，不允许用户自定义十六进制）：

| 档位 | hex | 语义 |
|---|---|---|
| `RED`（默认） | `#c8462c` | Reisinger terracotta，原始风格 |
| `BLUE` | `#2c6fc8` | Information / 冷调汇报 |
| `GREEN` | `#3a8a4a` | Terminal / 经典绿屏 |

实现约束：

- **位置：内联在 banner 同一行的右端**，跟随正文一起滚动——**禁止使用 `position: fixed` / `sticky`**，否则会遮挡正文。具体实现：把切换器放在 `<header class="banner">` 内的右上角（`position: absolute; top: 0; right: 0;` 相对 banner），banner 自身保留 `position: relative`；切换器与 4 行 spec 同高，往下滚动后跟着 banner 一起离开视口。
- 字号与正文一致 14px / JetBrains Mono；**禁止**加边框、背景色、阴影。
- 三个色档以纯 ASCII `[•] / [ ]` 表达选中态，hover 时 `color: var(--accent); transform: translateX(2px)`，与侧栏 hover 同步。
- 切换通过修改 `:root` 上的 `--accent` CSS 变量完成，所有用到 accent 的元素（hover 状态、`.hit`、`.row.active`、scrollspy `[•]`）即时跟随；不需要刷新页面。
- 默认值 `RED`；切换只在内存中生效（不写 localStorage——遵循 in-memory only 约束）。
- 切换器自身的 `[•]` 标记和当前激活色档同步（点击 BLUE 后 `[•]` 移到 BLUE 前）。
- Banner 内字段（DOC/DATE/AUTHOR/VER）的字段名不染色；强调色只作用于交互态和 `.hit` 高亮。
- 移动端（<960px）：banner 转纵向时切换器换行到 banner 末尾，仍随滚动消失；不切回 fixed。

参考骨架（结构 + 行为）：

```html
<header class="banner" style="position:relative">
  <pre class="line l1">DOC     : ...</pre>
  <pre class="line l2">DATE    : ...</pre>
  <pre class="line l3">AUTHOR  : ...</pre>
  <pre class="line l4">VER     : ...</pre>
  <div class="accent-switch" role="group" aria-label="accent color">
    <span class="label">ACCENT</span>
    <a class="opt active" data-accent="#c8462c"><span class="mark">[•]</span>RED</a>
    <a class="opt"        data-accent="#2c6fc8"><span class="mark">[ ]</span>BLUE</a>
    <a class="opt"        data-accent="#3a8a4a"><span class="mark">[ ]</span>GREEN</a>
  </div>
</header>
<style>
  .banner{position:relative;}
  .accent-switch{position:absolute;top:0;right:0;display:flex;gap:10px;}
</style>
<script>
  document.querySelectorAll('.accent-switch .opt').forEach(function(o){
    o.addEventListener('click', function(){
      document.documentElement.style.setProperty('--accent', o.dataset.accent);
      document.querySelectorAll('.accent-switch .mark').forEach(function(m){ m.textContent='[ ]'; });
      o.querySelector('.mark').textContent='[•]';
    });
  });
</script>
```

## 必备动效

| 动效 | 实现 | 时序 |
|---|---|---|
| Boot typewriter | `clip-path: inset(0 100% 0 0) → inset(0 0 0 0)` + `steps(40)` | banner 每行 80ms delay 串行 |
| Cursor blink | `animation: blink 1s steps(2) infinite` | 永久 |
| Sidebar 展开/折叠 | `max-height` 0 ↔ scrollHeight，`transition: 300ms ease-out`；同时 `[+]/[-]` 符号互换 | 300ms |
| Scrollspy `[•]` | IntersectionObserver 监听 main 内 sections，当前可见 section 对应侧栏行符号置为 `[•]` 并加 accent 色，其它行回 `[ ]` | 即时 |
| Hover nav | `color: var(--accent)` + `transform: translateX(4px)` | 150ms |
| CRT 扫描线 | `body::before` 固定层 `linear-gradient(transparent 50%, rgba(0,0,0,0.025) 50%)` size `100% 3px`，`mix-blend-mode: multiply`，`pointer-events:none` | 静态 |
| Section 滚入 | IntersectionObserver 添加 `.visible`：`opacity 0→1` + `translateY(12px→0)`，400ms | 一次性 |

## 实现约束

- 单 `.html` 文件自包含；CSS 内联 `<style>`，JS 末尾 `<script>`
- 外部依赖仅 Google Fonts（JetBrains Mono）
- 不使用 localStorage（in-memory only）
- 文件 ≤ 80 KB
- 可访问性：导航项 `tabindex="0"`，Enter/Space 触发 toggle
- 打印样式可选：隐藏侧栏、关闭 CRT 层、单列

## 输出路径

`<workspace>/outputs/<slug>-cli-report.html`，完成后以 `file://` 链接呈现。

## 反模式（拒绝清单）

- ❌ 任何 border / box-shadow / border-radius
- ❌ sans-serif 字体
- ❌ Lucide / FontAwesome icons
- ❌ 渐变背景或多色块
- ❌ 伪 stats 卡片（无真实数字时禁用 metric grid）
- ❌ emoji
- ❌ `<ul><li>` 项目符号（一律 ASCII 树形 `<pre>` 替代）
- ❌ 任何带阴影的 hover/focus 高亮（只用颜色 + 位移）

## 验证

生成后用 headless Chrome 截 1920×1080 首屏，确认：banner typewriter 动画顺序、侧栏树对齐、prompt 闪烁、无任何边框/阴影/sans-serif。
