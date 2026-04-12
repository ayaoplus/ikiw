# 视觉卡片：横版单页（Landscape）

固定 1280×720 的横向画面（16:9）。适合 Twitter/X 配图、博客封面、PPT 嵌入、Slack 分享——"一屏看完"的信息密度。

---

## 第一步：内容提炼

横版只能装下一屏信息，比竖版更克制。提取的字段更少：

```yaml
brand: 品牌/组织名称
event: 事件/类型标签（可选）
title: 主标题（10字以内）
subtitle: 副标题（一句话，20字以内）

# 二选一：核心内容
mode: highlights | quote | stats | compare

# mode = highlights：4 条以内亮点
highlights:
  intro: 一句话引言（可选）
  items:
    - 亮点1（12字以内）
    - 亮点2
    - 亮点3
    - 亮点4（最多 4 条）

# mode = quote：金句卡
quote:
  text: 一句金句（30字以内）
  attribution: 出处（人名/书名/文章名）

# mode = stats：数据卡（2-3 个数字）
stats:
  - value: "21天"
    label: "陪跑训练"
  - value: "5步"
    label: "完整流程"
  - value: "1对1"
    label: "贴身辅导"

# mode = compare：对比卡
compare:
  left:
    title: 左侧标题
    items: ["要点1", "要点2", "要点3"]
  right:
    title: 右侧标题
    items: ["要点1", "要点2", "要点3"]

tags:
  - 标签1（4字以内）
  - 标签2
  - 标签3（最多 3 个）

footer:
  name: 品牌/作者名（横版 footer 是底部一行，省略 note）
```

### 模式选择规则

- 内容是**多个并列亮点** → `highlights`（默认）
- 内容是**一句话精华** → `quote`
- 内容包含**几个关键数字** → `stats`
- 内容是**两种方案/前后对比** → `compare`

### 提炼规则（与竖版的差异）

- **更短**：横版字段比竖版少 30-40%。亮点 12 字内，标签 3 个内
- **不放时间线**：横版装不下时间线，有时间信息也压缩到 quote 或 stats
- **footer 单行**：只放 brand name，省略 note。视觉上是底部品牌行

---

## 第二步：画布契约（强制）

横版是**固定尺寸**的单屏画面，不滚动。

| 项 | 约束 |
|----|------|
| 画布尺寸 | `body { width: 1280px; height: 720px; margin: 0 auto; overflow: hidden; }` 16:9 固定 |
| 左右安全区 | `body { padding-left: 64px; padding-right: 64px; }` |
| 顶部/底部 padding | `body { padding-top: 56px; padding-bottom: 48px; }` |
| 主体布局 | 用 `display: flex` 或 `display: grid` 把 hero 和内容主体分两栏（左 5: 右 7 或上下分），不允许内容溢出底部 |
| 内容溢出处理 | 如果内容塞不下 720px，**减少 highlight 条数** 或 **缩短文字**，禁止允许溢出滚动 |
| Footer 处理 | 横版 footer 固定在底部一行，原文无 brand 时整段省略 |

> 横版的核心约束是「**一屏装下，绝不溢出**」。如果内容多到装不下，必须让 LLM 二次提炼缩减，而不是缩小字号或溢出。

---

## 第三步：HTML 骨架

横版采用**两栏布局**（左 hero / 右内容）作为默认结构。design-md 只控制 CSS，不允许改结构。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>{{brand}} · {{title}}</title>
  <style>
    /* 根据指定的 design-md 生成对应的 CSS */
  </style>
</head>
<body>

  <main class="card-grid">

    <!-- 左栏：Hero -->
    <section class="hero">
      <div class="badge">{{brand}} · {{event}}</div>
      <h1>{{title}}</h1>
      <div class="subtitle">{{subtitle}}</div>

      <!-- 标签紧贴 hero 底部 -->
      <div class="tags">
        <span class="tag">{{tag}}</span>
      </div>
    </section>

    <!-- 右栏：内容主体（按 mode 选一种） -->
    <section class="content">

      <!-- mode = highlights -->
      <div class="highlights-section">
        <div class="section-intro">{{highlights.intro}}</div>
        <div class="highlights-list">
          <div class="highlight-item">
            <span class="highlight-dot"></span>
            <span>{{highlight}}</span>
          </div>
        </div>
      </div>

      <!-- mode = quote -->
      <blockquote class="quote">
        <div class="quote-text">"{{quote.text}}"</div>
        <div class="quote-attribution">— {{quote.attribution}}</div>
      </blockquote>

      <!-- mode = stats -->
      <div class="stats-grid">
        <div class="stat-item">
          <div class="stat-value">{{value}}</div>
          <div class="stat-label">{{label}}</div>
        </div>
      </div>

      <!-- mode = compare -->
      <div class="compare-grid">
        <div class="compare-col compare-left">
          <div class="compare-title">{{compare.left.title}}</div>
          <ul>
            <li>{{item}}</li>
          </ul>
        </div>
        <div class="compare-col compare-right">
          <div class="compare-title">{{compare.right.title}}</div>
          <ul>
            <li>{{item}}</li>
          </ul>
        </div>
      </div>

    </section>

  </main>

  <!-- Footer：底部品牌单行（原文无则省略） -->
  <footer class="footer">
    <div class="footer-name">{{footer.name}}</div>
  </footer>

</body>
</html>
```

### 结构规则

- **两栏栅格**：`.card-grid { display: grid; grid-template-columns: 5fr 7fr; gap: 48px; }`（hero 占 5/12，content 占 7/12）
- **Hero 区**：badge + h1 + subtitle + tags（标签紧贴 hero 底部，不再独立成区）
- **Content 区**：按 mode 选一种内容结构
- **Footer**：独立于栅格之外，固定在 body 底部一行

### 禁止替换的组件

- 禁止把横版做成「上下三段式」长图（那是竖版做法）
- 禁止 stat-item 用图标 emoji 替代数字
- 禁止 quote 不带引号视觉
- 禁止内容溢出后用 `overflow: scroll` 兜底

### CSS 类名约定

| 类名 | 用途 |
|------|------|
| `.card-grid` | 主栅格容器（两栏） |
| `.hero` | 左栏 hero |
| `.badge` | 品牌/事件标签 |
| `.subtitle` | 副标题 |
| `.tags` / `.tag` | hero 底部的标签组 |
| `.content` | 右栏内容容器 |
| `.highlights-section` / `.highlights-list` / `.highlight-item` / `.highlight-dot` | highlights 模式 |
| `.quote` / `.quote-text` / `.quote-attribution` | quote 模式 |
| `.stats-grid` / `.stat-item` / `.stat-value` / `.stat-label` | stats 模式 |
| `.compare-grid` / `.compare-col` / `.compare-title` | compare 模式 |
| `.footer` / `.footer-name` | 底部品牌单行 |

---

## 第四步：套用视觉样式

读取指定的 design-md 文件，按其定义的色彩、字体、间距、组件风格生成 CSS。

### 样式映射规则

| 卡片区块 | 映射到 design-md 的 |
|----------|---------------------|
| `.hero`（左栏） | Hero / Cover 组件，可整块用品牌色或深色块 |
| `.content`（右栏） | 通常白底/浅底，承载主信息 |
| `h1` | Display / Hero heading 的字号、字重、行高 |
| `.subtitle` | Body Large / Intro 的样式 |
| `.highlight-item` | List item 样式，装饰符号用品牌色 |
| `.quote-text` | Display / Pull-quote 样式（大字号、可斜体） |
| `.stat-value` | 大号数字（48-72px），品牌色或深色 |
| `.stat-label` | Caption / Secondary text |
| `.compare-title` | Sub-heading 样式 |
| `.tag` | Badge / Label 样式 |
| `.footer` | Footer 单行，secondary text 颜色 |

### 视觉适配原则

- **左右色块对比**：横版常用「左栏品牌色块 + 右栏白底」营造视觉冲击。但**禁止使用渐变**（除非 design-md 明确允许）
- **字号比竖版大**：横版屏占比小，h1 字号要更大（建议 56-72px），保证缩略图也能看清
- **内容区垂直居中**：`.content` 内容用 `align-content: center` 垂直居中，避免上下失衡

---

## 第五步：生成后自查（必做）

生成 HTML 后逐条对照清单。任何不通过必须重新生成，**不允许带瑕疵交付**。

### 空间自查

- [ ] body 是否固定 1280×720？是否设置 `overflow: hidden`？
- [ ] body 是否有 ≥ 56px 的左右 padding？
- [ ] hero 内的 badge、h1、subtitle、tags 是否都在 safe area 内（未贴边）？
- [ ] content 区是否在 safe area 内？
- [ ] 内容是否完全装在 720px 内、无溢出？（在浏览器/截图工具里渲染验证）
- [ ] 如果内容溢出，是否回到「第一步」再次缩减字数 / 删条目，而不是缩小字号兜底？
- [ ] footer 是否单行紧贴底部？无 brand 时是否整段省略？

### 风格自查（对照 design-md）

- [ ] 读 design-md 第 7 节 "Do's and Don'ts"，逐条对照
- [ ] 读 design-md 第 1 节 "Key Characteristics"，确认风格"身份标志"是否复现
- [ ] 横版下 h1 字号是否足够大（≥ 56px）？

### 内容自查

- [ ] 标题是否 ≤ 10 字？
- [ ] highlight 每条 ≤ 12 字、最多 4 条？
- [ ] quote ≤ 30 字？
- [ ] stat 最多 3 个？
- [ ] tag 最多 3 个、每条 ≤ 4 字？
