# 焦橙主题设计系统（Burnt Orange Design System）

> 本文档是一份完整的 UI Kit / 设计规范。  
> **用途**：当你拿到一篇纯 Markdown 文章后，请严格参照本文档将其渲染为精美的单文件 HTML 页面。  
> **核心风格**：书籍级排版、焦橙色点睛、大量留白、清晰的信息层级。

---

## 一、总体设计理念

| 维度 | 原则 | 说明 |
|------|------|------|
| 色调 | **暖白 + 焦橙** | 底色为纯白，用焦橙色（`#C2410C`）作为唯一的强调色贯穿全局 |
| 排版 | **书籍级** | 模拟实体书阅读体验，A4 尺寸，充足的上下左右边距 |
| 字体 | **中西双栈** | 中文用 Noto Sans SC，英文用 Inter，代码用 JetBrains Mono |
| 密度 | **宽松** | 行高 1.8，段落间距充足，内容不拥挤 |
| 组件 | **功能卡片** | 根据内容语义选用不同样式的卡片组件（提示/警告/对比/步骤等） |
| 装饰 | **极简** | 不用阴影（仅尾页二维码有微阴影），不用渐变（仅封面/尾页有极淡暖色渐变），不用圆角按钮以外的圆角 |

---

## 二、色彩系统

### 2.1 CSS 变量定义

```css
:root {
  /* 基础 */
  --bg: #FFFFFF;                /* 页面背景 */
  --surface: #FFFFFF;           /* 内容表面 */
  --text: #1A1A1A;              /* 主文本色 */
  --text-secondary: #6B6B6B;   /* 辅助文本色 */

  /* 品牌色 / 强调色 */
  --accent: #C2410C;            /* 焦橙 — 核心强调色 */
  --accent-light: #FFF7ED;      /* 焦橙浅底 — 用于强调区块背景 */

  /* 边界 & 分隔 */
  --border: #E5E5E5;            /* 通用边框/分隔线 */
  --section-line: #C2410C;      /* 章节顶部分隔线（与强调色一致） */

  /* 代码 */
  --code-bg: #F5F5F0;           /* 代码块背景（微暖灰） */

  /* 功能色 — 提示 */
  --tip-bg: #F0FDF4;            /* 绿底 */
  --tip-border: #22C55E;        /* 绿边框 */

  /* 功能色 — 警告 */
  --warn-bg: #FEF2F2;           /* 红底 */
  --warn-border: #EF4444;       /* 红边框 */

  /* 功能色 — 高亮 */
  --highlight: #FFFBEB;         /* 黄底 — 用于 callout */
}
```

### 2.2 色彩使用规则

| 场景 | 色值 | 说明 |
|------|------|------|
| 正文文字 | `#1A1A1A` | 接近纯黑但不刺眼 |
| 辅助文字（日期、英文副标题、描述等） | `#6B6B6B` | 明显弱化 |
| 极弱文字（声明、版权） | `#9CA3AF` | 最弱层级 |
| 强调色使用场景 | `#C2410C` | 章节编号、链接、badge、步骤圆圈、流程图边框、比较框标题 |
| 封面/尾页渐变 | `linear-gradient(135deg, #FFFBF5 0%, #FFF5EB 25%, #FFFFFF 100%)` | 极淡的暖色渐变，仅用于封面和尾页 |

---

## 三、字体系统

### 3.1 字体栈

```css
/* 引入 */
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=Noto+Sans+SC:wght@300;400;500;600;700;900&family=JetBrains+Mono:wght@400;500&display=swap');

/* 主字体（中文为主） */
font-family: 'Noto Sans SC', 'Inter', -apple-system, system-ui, sans-serif;

/* 英文/数字专用场景 */
font-family: 'Inter', sans-serif;

/* 代码 */
font-family: 'JetBrains Mono', monospace;
```

### 3.2 字号层级

| 元素 | 字号 | 字重 | 补充说明 |
|------|------|------|---------|
| 封面标题 | 36pt | 900 (Black) | 行高 1.15，字间距 -1px |
| 章节大标题 (h2) | 20pt | 700 | 上方有 3px 焦橙色分隔线 |
| 章节英文副标题 | 10pt | 400 italic | 颜色 `#9CA3AF`，Inter 字体 |
| 章节引言 | 11.5pt | 300 | 颜色 `--text-secondary` |
| 三级标题 (h3) | 13pt | 600 | 可附带英文注释（9.5pt, `#9CA3AF`） |
| 四级标题 (h4) | 11pt | 600 | — |
| 正文 | 10.5pt | 400 | 行高 1.8 |
| 表格正文 | 9.5pt | 400 | — |
| 表头 | 8.5pt | 600 | 颜色 `--text-secondary`，字间距 0.5px |
| 代码块 | 9pt | 400 | JetBrains Mono，行高 1.6 |
| 行内代码 | 9pt | 400 | 背景 `--code-bg`，padding 1px 4px |
| 功能卡片标签 | 8.5pt | 700 | 字间距 0.5px |
| 功能卡片正文 | 10pt | 400 | — |
| 页脚声明 | 8.5pt | 400 | 颜色 `--text-secondary` |

---

## 四、间距与布局

### 4.1 页面尺寸

```css
@page {
  size: A4;
  margin: 18mm 0;  /* 上下 18mm，左右由内容区 padding 控制 */
}
```

### 4.2 内容区

```css
.content {
  padding: 0 55px;  /* 左右各 55px */
}
```

### 4.3 关键间距

| 元素 | 间距 | 说明 |
|------|------|------|
| 段落 (p) | `margin-bottom: 10px` | — |
| 章节标题 (h2) | `margin-top: 45px; margin-bottom: 6px; padding-top: 18px` | 上方有充足呼吸空间 |
| 三级标题 (h3) | `margin-top: 24px; margin-bottom: 8px` | — |
| 四级标题 (h4) | `margin-top: 16px; margin-bottom: 6px` | — |
| 功能卡片 (tip/warning/callout) | `margin: 14px 0; padding: 12px 16px` | — |
| 代码块 (pre) | `margin: 10px 0 14px; padding: 12px 16px` | — |
| 表格 | `margin: 14px 0` | — |
| 列表 | `margin: 6px 0 12px 22px` | 列表项间 `margin-bottom: 3px` |
| 步骤组件 | `margin: 14px 0; gap: 12px` | 步骤编号与内容之间 |

---

## 五、组件库

以下是本设计系统中所有可用的组件。**渲染 Markdown 时，请根据内容语义自动选用合适的组件。**

### 5.1 封面 (Cover)

**何时使用**：文章的第一页，展示标题、副标题、作者信息。

```html
<div class="cover">
  <div class="cover-badge">日期 · 版本</div>
  <h1><em>强调词</em><br>主标题</h1>
  <p class="cover-subtitle">副标题描述</p>
  <p class="cover-en">英文副标题（可选）</p>
  <div class="cover-meta">
    <strong>字段名：</strong>字段值<br>
  </div>
  <div class="cover-author"><strong>作者名</strong><br>其他信息</div>
  <div class="cover-disclaimer">免责声明（可选）</div>
</div>
```

样式要点：
- 背景：极淡暖色渐变
- 左上角有两条装饰线（80px 焦橙 + 30px 浅橙 `#FDBA74`）
- badge 为焦橙底白字小标签，圆角 3px
- 标题中 `<em>` 标签内的文字显示为焦橙色
- 垂直居中布局

### 5.2 目录 (TOC)

**何时使用**：文章内容较长（3 章以上）时自动生成。

```html
<div class="toc">
  <h2>目录</h2>
  <div class="toc-sub">CONTENTS</div>
  <div class="toc-group">Part 1: 分组名</div>
  <div class="toc-item">
    <a href="#id"><span class="num">§01</span><span class="title">标题</span></a>
  </div>
</div>
```

样式要点：
- 顶部有 3px 焦橙色线
- 分组标题用焦橙色，带底部边框
- 条目之间用点状虚线分隔
- 编号用 Inter 字体、焦橙色、加粗

### 5.3 章节标题 (Section Title)

**何时使用**：Markdown 的 `##` 二级标题。

```html
<h2 class="section-title page-break" id="唯一ID">
  <span class="num">§01</span> 标题文字
</h2>
<p class="section-en">English Subtitle（可选）</p>
<p class="section-intro">本章引言，一两句话概述本章内容。</p>
```

样式要点：
- 上方 3px 焦橙色实线
- 编号用 Inter 字体、焦橙色
- 英文副标题斜体、灰色
- 引言用 11.5pt、300 字重、灰色
- 自动分页（`page-break-before: always`）

### 5.4 提示卡片 (Tip)

**何时使用**：核心建议、最佳实践、推荐做法。Markdown 中标记为 `> [!TIP]` 或内容带有"建议""推荐""核心"等关键词的引用块。

```html
<div class="tip">提示内容文字</div>
```

样式要点：
- 背景：浅绿 `#F0FDF4`
- 左边框：4px 绿色 `#22C55E`
- 自动显示标签"核心建议"（通过 `::before` 伪元素）
- 右侧圆角 8px

### 5.5 警告卡片 (Warning)

**何时使用**：注意事项、常见陷阱、风险提醒。Markdown 中标记为 `> [!WARNING]` 或内容带有"注意""警告""小心""不要"等关键词。

```html
<div class="warning">警告内容文字</div>
```

样式要点：
- 背景：浅红 `#FEF2F2`
- 左边框：4px 红色 `#EF4444`
- 自动显示标签"注意"
- 右侧圆角 8px

### 5.6 高亮卡片 (Callout)

**何时使用**：经验分享、要点总结、引用名言、重要补充说明。Markdown 中标记为 `> [!NOTE]` 或以"经验""总结""花叔的经验"等开头的引用块。

```html
<div class="callout"><strong>标题：</strong>内容文字</div>
```

样式要点：
- 背景：浅黄 `#FFFBEB`
- 边框：1px `#FDE68A`
- 圆角：8px（四边）
- `<strong>` 文字显示为焦橙色

### 5.7 步骤组件 (Step)

**何时使用**：有明确先后顺序的操作步骤。Markdown 中的有序列表，如果每一项包含**标题 + 详细说明**的结构（而非简单的一行列表项），应渲染为步骤组件。

```html
<div class="step">
  <div class="step-num">1</div>
  <div class="step-content">
    <h4>步骤标题</h4>
    <p>步骤说明</p>
  </div>
</div>
```

样式要点：
- 编号：26px 圆形、焦橙底、白字、Inter 字体
- 与内容水平排列（flexbox），间距 12px
- 避免分页打断（`page-break-inside: avoid`）

### 5.8 流程图 (Flow)

**何时使用**：展示线性流程、演进路径、工作流。当 Markdown 内容描述一个从 A→B→C 的递进关系时使用。

```html
<div class="flow">
  <div class="flow-item">步骤A</div>
  <div class="flow-arrow">→</div>
  <div class="flow-item">步骤B</div>
  <div class="flow-arrow">→</div>
  <div class="flow-item">步骤C</div>
</div>
```

样式要点：
- 水平排列，居中
- 每个节点：白底、2px 焦橙边框、圆角 8px、焦橙色文字
- 箭头：浅灰 `#D1D5DB`、18pt
- 支持换行（`flex-wrap: wrap`）

### 5.9 对比框 (Compare)

**何时使用**：推荐 vs 不推荐、好 vs 坏、Before vs After 的对比展示。

```html
<div class="compare">
  <div class="compare-good">
    推荐的做法或内容
  </div>
  <div class="compare-bad">
    不推荐的做法或内容
  </div>
</div>
```

样式要点：
- 两栏等宽网格布局（`grid-template-columns: 1fr 1fr`）
- 推荐侧：浅绿底 + 绿边框，自动标签"推荐"
- 不推荐侧：浅红底 + 红边框，自动标签"不推荐"
- 内部可嵌套代码块（`<pre>`），内部代码块字号缩小到 8.5pt

### 5.10 代码块 (Code Block)

**何时使用**：Markdown 的围栏代码块（` ``` `）。

```html
<pre><code>代码内容</code></pre>
```

样式要点：
- 背景：`#F5F5F0`（微暖灰）
- 边框：1px `#E5E5E5`
- 圆角：8px
- 字体：JetBrains Mono, 9pt，行高 1.6
- 内边距：12px 16px

### 5.11 行内代码 (Inline Code)

**何时使用**：Markdown 的行内代码（`` ` ``）。

```html
<code>代码片段</code>
```

样式要点：
- 背景：`#F5F5F0`
- padding：1px 4px
- 圆角：3px
- 字体：JetBrains Mono, 9pt

### 5.12 表格 (Table)

**何时使用**：Markdown 的表格语法。

```html
<table>
  <thead>
    <tr><th>表头1</th><th>表头2</th></tr>
  </thead>
  <tbody>
    <tr><td>内容1</td><td>内容2</td></tr>
  </tbody>
</table>
```

样式要点：
- 全宽（`width: 100%`）
- 表头：`#F5F5F0` 背景、8.5pt、600 字重、`--text-secondary` 色、底部 2px 边线
- 单元格：9px 12px padding、底部 1px 边线
- 最后一行无底部边线
- 正文 9.5pt

### 5.13 文件树 (File Tree)

**何时使用**：展示项目目录结构。

```html
<div class="file-tree">
project/<br>
├── src/<br>
│&nbsp;&nbsp; ├── index.ts<br>
│&nbsp;&nbsp; └── utils.ts<br>
└── package.json
</div>
```

样式要点：
- 与代码块相同的背景和边框
- JetBrains Mono 字体，9pt，行高 1.7
- 用 `<br>` 换行，用 `&nbsp;` 缩进

### 5.14 图表占位符 (Diagram)

**何时使用**：需要图表但无法直接渲染时的占位。

```html
<div class="diagram">
  <div class="label">图表标题</div>
  图表描述文字
</div>
```

样式要点：
- 背景：`#F9FAFB`
- 边框：2px 虚线 `#D1D5DB`
- 圆角：12px
- 文字居中，灰色

### 5.15 尾页 (Back Page)

**何时使用**：文章最后一页，展示作者信息、联系方式等。

样式要点：
- 与封面相同的暖色渐变背景
- 内容垂直水平居中
- 可包含二维码图片（圆角包裹，微阴影 `0 8px 30px rgba(0,0,0,0.08)`）
- CTA 按钮：焦橙底白字、圆角 8px

---

## 六、Markdown → HTML 语义映射规则

这是最核心的部分。渲染 Markdown 时，需要根据以下规则判断内容该使用哪种组件。

### 6.1 标题映射

| Markdown | HTML 组件 | 附加规则 |
|----------|-----------|---------|
| `#` (h1) | 封面标题 | 仅出现一次，渲染为封面 |
| `##` (h2) | `section-title` | 自动编号 `§01` `§02` ...；自动分页 |
| `###` (h3) | 普通 `<h3>` | — |
| `####` (h4) | 普通 `<h4>` | — |

### 6.2 引用块映射

| Markdown 语法 | 渲染组件 | 判断条件 |
|---------------|---------|---------|
| `> [!TIP]` 或内容含"建议""推荐""技巧""最佳实践" | **Tip（提示卡片）** | 绿色左边框 |
| `> [!WARNING]` 或内容含"注意""警告""小心""不要""避免" | **Warning（警告卡片）** | 红色左边框 |
| `> [!NOTE]` 或内容含"经验""总结""一句话" | **Callout（高亮卡片）** | 黄色边框 |
| 其他普通引用 | **Callout（高亮卡片）** | 默认使用 callout |

### 6.3 列表映射

| Markdown | 渲染方式 | 判断条件 |
|----------|---------|---------|
| 有序列表（每项含粗体标题 + 详细说明） | **Step（步骤组件）** | 列表项结构为"标题 + 段落" |
| 有序列表（简单的一行描述） | 普通 `<ol>` | — |
| 无序列表 | 普通 `<ul>` | — |

### 6.4 特殊内容识别

| 内容模式 | 渲染组件 | 识别方式 |
|----------|---------|---------|
| A → B → C 形式的流程 | **Flow（流程图）** | 含"→"箭头的文本行，或明确描述阶段递进 |
| 推荐 vs 不推荐 | **Compare（对比框）** | 段落中出现"推荐/不推荐"对比结构 |
| 目录结构描述 | **File Tree（文件树）** | 含 `├──` `└──` `│` 的文本块 |
| 表格 | **Table** | Markdown 表格语法 |
| 代码块 | **Pre > Code** | 围栏代码块 |

---

## 七、完整 CSS 模板

渲染时请直接使用以下完整 CSS 作为 `<style>` 标签内容。

```css
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=Noto+Sans+SC:wght@300;400;500;600;700;900&family=JetBrains+Mono:wght@400;500&display=swap');

:root {
  --bg: #FFFFFF;
  --surface: #FFFFFF;
  --text: #1A1A1A;
  --text-secondary: #6B6B6B;
  --accent: #C2410C;
  --accent-light: #FFF7ED;
  --border: #E5E5E5;
  --code-bg: #F5F5F0;
  --tip-bg: #F0FDF4;
  --tip-border: #22C55E;
  --warn-bg: #FEF2F2;
  --warn-border: #EF4444;
  --section-line: #C2410C;
  --highlight: #FFFBEB;
}

* { margin: 0; padding: 0; box-sizing: border-box; }

@page {
  size: A4;
  margin: 18mm 0;
}

@page cover-page {
  size: A4;
  margin: 0;
}

@page back-page {
  size: A4;
  margin: 0;
}

body {
  font-family: 'Noto Sans SC', 'Inter', -apple-system, system-ui, sans-serif;
  font-size: 10.5pt;
  line-height: 1.8;
  color: var(--text);
  background: var(--bg);
}

/* ===== 封面 ===== */

.cover {
  page: cover-page;
  page-break-after: always;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 60px 55px;
  background: linear-gradient(135deg, #FFFBF5 0%, #FFF5EB 25%, #FFFFFF 100%);
  position: relative;
}

.cover::before {
  content: '';
  position: absolute;
  top: 40px;
  left: 50px;
  width: 80px;
  height: 5px;
  background: var(--accent);
  border-radius: 3px;
}

.cover::after {
  content: '';
  position: absolute;
  top: 40px;
  left: 140px;
  width: 30px;
  height: 5px;
  background: #FDBA74;
  border-radius: 3px;
}

.cover-badge {
  display: inline-block;
  background: var(--accent);
  color: white;
  font-size: 10px;
  font-weight: 600;
  letter-spacing: 2px;
  text-transform: uppercase;
  padding: 5px 14px;
  border-radius: 3px;
  margin-bottom: 24px;
  font-family: 'Inter', sans-serif;
}

.cover h1 {
  font-size: 36pt;
  font-weight: 900;
  line-height: 1.15;
  letter-spacing: -1px;
  color: var(--text);
  margin-bottom: 16px;
}

.cover h1 em {
  font-style: normal;
  color: var(--accent);
}

.cover-subtitle {
  font-size: 14pt;
  font-weight: 300;
  color: var(--text-secondary);
  margin-bottom: 16px;
  max-width: 520px;
  line-height: 1.6;
}

.cover-en {
  font-family: 'Inter', sans-serif;
  font-size: 11pt;
  color: #9CA3AF;
  font-weight: 400;
  font-style: italic;
  margin-bottom: 50px;
}

.cover-meta {
  font-size: 9.5pt;
  color: var(--text-secondary);
  border-top: 1px solid var(--border);
  padding-top: 20px;
  line-height: 2;
}

.cover-meta strong { color: var(--text); font-weight: 600; }

.cover-author {
  margin-top: 20px;
  font-size: 10pt;
  color: var(--text-secondary);
  line-height: 2;
}

.cover-author strong {
  font-size: 12pt;
  color: var(--text);
  display: block;
  margin-bottom: 2px;
}

.cover-disclaimer {
  margin-top: 14px;
  font-size: 8.5pt;
  color: #9CA3AF;
  line-height: 1.8;
}

/* ===== 目录 ===== */

.toc {
  page-break-after: always;
  padding: 10px 55px 0;
}

.toc h2 {
  font-size: 22pt;
  font-weight: 700;
  margin-bottom: 8px;
  color: var(--text);
}

.toc-sub {
  font-size: 10pt;
  color: var(--text-secondary);
  margin-bottom: 30px;
}

.toc-group {
  margin-top: 20px;
  margin-bottom: 6px;
  font-size: 10pt;
  font-weight: 600;
  color: var(--accent);
  letter-spacing: 0.5px;
  padding-bottom: 4px;
  border-bottom: 1px solid var(--border);
}

.toc-item {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  padding: 8px 0;
  border-bottom: 1px dotted #D4D4D4;
  font-size: 10.5pt;
}

.toc-item .num {
  color: var(--accent);
  font-weight: 700;
  font-family: 'Inter', sans-serif;
  margin-right: 10px;
  min-width: 24px;
  font-size: 10pt;
}

.toc-item .title { font-weight: 500; }

.toc-item a {
  color: inherit;
  text-decoration: none;
  display: flex;
  align-items: baseline;
  gap: 8px;
}

/* ===== 内容区 ===== */

.content {
  padding: 0 55px;
}

h2.section-title {
  font-size: 20pt;
  font-weight: 700;
  color: var(--text);
  margin-top: 45px;
  margin-bottom: 6px;
  padding-top: 18px;
  border-top: 3px solid var(--section-line);
  page-break-after: avoid;
}

h2.section-title .num {
  color: var(--accent);
  font-family: 'Inter', sans-serif;
  margin-right: 6px;
  font-size: 18pt;
}

.section-en {
  font-family: 'Inter', sans-serif;
  font-size: 10pt;
  color: #9CA3AF;
  font-style: italic;
  margin-bottom: 6px;
}

.section-intro {
  font-size: 11.5pt;
  color: var(--text-secondary);
  margin-bottom: 20px;
  font-weight: 300;
}

h3 {
  font-size: 13pt;
  font-weight: 600;
  color: var(--text);
  margin-top: 24px;
  margin-bottom: 8px;
  page-break-after: avoid;
}

h3 .en {
  font-family: 'Inter', sans-serif;
  font-size: 9.5pt;
  color: #9CA3AF;
  font-weight: 400;
  margin-left: 6px;
}

h4 {
  font-size: 11pt;
  font-weight: 600;
  color: var(--text);
  margin-top: 16px;
  margin-bottom: 6px;
}

p { margin-bottom: 10px; }

/* ===== 组件 ===== */

.tip {
  background: var(--tip-bg);
  border-left: 4px solid var(--tip-border);
  padding: 12px 16px;
  margin: 14px 0;
  border-radius: 0 8px 8px 0;
  font-size: 10pt;
  page-break-inside: avoid;
}

.tip::before {
  content: '核心建议';
  display: block;
  font-weight: 700;
  font-size: 8.5pt;
  letter-spacing: 0.5px;
  color: #16A34A;
  margin-bottom: 3px;
}

.warning {
  background: var(--warn-bg);
  border-left: 4px solid var(--warn-border);
  padding: 12px 16px;
  margin: 14px 0;
  border-radius: 0 8px 8px 0;
  font-size: 10pt;
  page-break-inside: avoid;
}

.warning::before {
  content: '注意';
  display: block;
  font-weight: 700;
  font-size: 8.5pt;
  letter-spacing: 0.5px;
  color: #DC2626;
  margin-bottom: 3px;
}

.callout {
  background: var(--highlight);
  border: 1px solid #FDE68A;
  border-radius: 8px;
  padding: 14px 16px;
  margin: 14px 0;
  font-size: 10pt;
  page-break-inside: avoid;
}

.callout strong { color: var(--accent); }

pre {
  background: var(--code-bg);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 12px 16px;
  overflow-x: auto;
  font-family: 'JetBrains Mono', monospace;
  font-size: 9pt;
  line-height: 1.6;
  margin: 10px 0 14px;
  page-break-inside: avoid;
}

code {
  font-family: 'JetBrains Mono', monospace;
  font-size: 9pt;
  background: var(--code-bg);
  padding: 1px 4px;
  border-radius: 3px;
}

pre code { background: none; padding: 0; }

table {
  width: 100%;
  border-collapse: collapse;
  margin: 14px 0;
  font-size: 9.5pt;
  page-break-inside: avoid;
}

thead th {
  background: #F5F5F0;
  font-weight: 600;
  text-align: left;
  padding: 9px 12px;
  border-bottom: 2px solid var(--border);
  font-size: 8.5pt;
  letter-spacing: 0.5px;
  color: var(--text-secondary);
}

td {
  padding: 9px 12px;
  border-bottom: 1px solid var(--border);
  vertical-align: top;
}

tr:last-child td { border-bottom: none; }

.step {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  margin: 14px 0;
  page-break-inside: avoid;
}

.step-num {
  flex-shrink: 0;
  width: 26px;
  height: 26px;
  background: var(--accent);
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
  font-size: 12px;
  font-family: 'Inter', sans-serif;
  margin-top: 2px;
}

.step-content { flex: 1; }
.step-content h4 {
  font-weight: 600;
  margin-top: 0;
  margin-bottom: 5px;
  font-size: 10.5pt;
  line-height: 1.6;
}

ul, ol { margin: 6px 0 12px 22px; }
li { margin-bottom: 3px; }

em { font-style: italic; color: var(--text-secondary); }
strong { font-weight: 600; }

.page-break { page-break-before: always; }

.file-tree {
  background: var(--code-bg);
  border: 1px solid var(--border);
  border-radius: 8px;
  padding: 14px 18px;
  font-family: 'JetBrains Mono', monospace;
  font-size: 9pt;
  line-height: 1.7;
  margin: 10px 0 14px;
  page-break-inside: avoid;
}

.flow {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  margin: 20px 0;
  flex-wrap: wrap;
}

.flow-item {
  background: white;
  border: 2px solid var(--accent);
  border-radius: 8px;
  padding: 8px 16px;
  font-weight: 600;
  font-size: 10pt;
  color: var(--accent);
}

.flow-arrow {
  color: #D1D5DB;
  font-size: 18pt;
  font-weight: 300;
}

.compare {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
  margin: 14px 0;
  page-break-inside: avoid;
}

.compare-good, .compare-bad {
  border-radius: 8px;
  padding: 12px 14px;
  font-size: 9.5pt;
}

.compare-good {
  background: var(--tip-bg);
  border: 1px solid #BBF7D0;
}

.compare-good::before {
  content: '推荐';
  display: block;
  font-weight: 700;
  font-size: 8.5pt;
  color: #16A34A;
  margin-bottom: 4px;
}

.compare-bad {
  background: var(--warn-bg);
  border: 1px solid #FECACA;
}

.compare-bad::before {
  content: '不推荐';
  display: block;
  font-weight: 700;
  font-size: 8.5pt;
  color: #DC2626;
  margin-bottom: 4px;
}

.compare-good pre, .compare-bad pre {
  margin: 6px 0 0;
  font-size: 8.5pt;
}

.diagram {
  background: #F9FAFB;
  border: 2px dashed #D1D5DB;
  border-radius: 12px;
  padding: 30px;
  margin: 20px 0;
  text-align: center;
  color: #9CA3AF;
  font-size: 9pt;
  page-break-inside: avoid;
}

.diagram .label {
  font-weight: 600;
  color: var(--text-secondary);
  margin-bottom: 8px;
  font-size: 10pt;
}

.footer-note {
  margin-top: 50px;
  padding-top: 18px;
  border-top: 1px solid var(--border);
  font-size: 8.5pt;
  color: var(--text-secondary);
  text-align: center;
  line-height: 1.8;
}

/* ===== 尾页 ===== */

.back-page {
  page: back-page;
  page-break-before: always;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 50px 55px 40px;
  background: linear-gradient(135deg, #FFFBF5 0%, #FFF5EB 25%, #FFFFFF 100%);
  text-align: center;
}

.back-page-title {
  font-size: 22pt;
  font-weight: 700;
  color: var(--text);
  margin-bottom: 6px;
}

.back-page-subtitle {
  font-size: 12pt;
  font-weight: 400;
  color: var(--text-secondary);
  margin-bottom: 28px;
}

.back-page-link {
  display: inline-block;
  margin-top: 16px;
  padding: 9px 24px;
  background: var(--accent);
  color: white;
  font-size: 10pt;
  font-weight: 600;
  border-radius: 8px;
  text-decoration: none;
}

.back-page-footer {
  margin-top: 28px;
  font-size: 8.5pt;
  color: #B0B0B0;
  line-height: 1.8;
  letter-spacing: 0.5px;
}
```

---

## 八、渲染工作流指南

当你收到一篇 Markdown 文章和本设计规范文档时，请按以下流程渲染：

### Step 1：分析文章结构

1. 识别文章标题（`#`）→ 生成封面
2. 统计所有 `##` 章节 → 如果 ≥3 章则生成目录
3. 扫描所有引用块、列表、代码块、表格 → 预判组件使用

### Step 2：生成 HTML 骨架

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>文章标题</title>
  <style>
    /* 粘贴第七节的完整 CSS */
  </style>
</head>
<body>
  <!-- 封面 -->
  <div class="cover">...</div>

  <!-- 目录（可选） -->
  <div class="toc">...</div>

  <!-- 正文章节，每章一个 .content 容器 -->
  <div class="content">...</div>

  <!-- 尾页（可选） -->
  <div class="back-page">...</div>
</body>
</html>
```

### Step 3：逐节渲染内容

- 按第六节的映射规则，将 Markdown 元素转换为对应的 HTML 组件
- 章节自动编号（§01, §02, §03...）
- 引用块根据内容语义自动选择 tip / warning / callout
- 复杂有序列表自动转为步骤组件
- 递进关系自动转为流程图
- 对比内容自动转为对比框

### Step 4：输出最终文件

- 输出为单个 `.html` 文件
- 所有样式内联在 `<style>` 标签中（不依赖外部 CSS 文件）
- 字体通过 Google Fonts CDN 引入
- 确保 `lang="zh-CN"` 和正确的 `<meta charset="UTF-8">`

---

## 九、风格原则速查

| 规则 | 说明 |
|------|------|
| **唯一强调色** | 全局只用焦橙 `#C2410C`，不引入其他亮色 |
| **四级灰度文字** | 主文本 `#1A1A1A` → 辅助 `#6B6B6B` → 弱化 `#9CA3AF` → 极弱 `#B0B0B0` |
| **功能色只用三种** | 绿（提示）、红（警告）、黄（高亮），不扩展 |
| **不用阴影** | 唯一例外：尾页二维码包裹器的微阴影 |
| **不用渐变** | 唯一例外：封面和尾页的极淡暖色渐变 |
| **圆角统一** | 卡片/代码块/表格内用 8px；行内代码/badge 用 3px；步骤编号用 50%（圆形） |
| **分隔线** | 章节间用 3px 焦橙实线；其他位置用 1px `#E5E5E5` |
| **打印友好** | 所有卡片组件设置 `page-break-inside: avoid`；章节标题设置 `page-break-after: avoid` |
| **中文为主** | 正文用 Noto Sans SC；英文标注、编号用 Inter |
| **代码等宽** | 所有代码场景统一用 JetBrains Mono |

---

## 十、自定义提示词模板

当你需要将一篇 Markdown 文章渲染为本风格的 HTML 时，可以使用以下提示词：

```
请将以下 Markdown 文章渲染为单文件 HTML 页面。

**设计规范**：请严格遵循 DESIGN_SYSTEM.md 中定义的焦橙主题设计系统。

**要求**：
1. 根据文章标题生成封面（cover）
2. 若章节数 ≥ 3，生成目录（toc）
3. 使用第六节的语义映射规则，自动选用合适的组件
4. 所有样式内联，输出单个 .html 文件
5. 保持书籍级排版质量

**Markdown 内容**：
（粘贴你的文章）
```

---

*本设计系统提取自一份书籍级 PDF 排版的静态 HTML 页面，经过完整分析和结构化整理。*
