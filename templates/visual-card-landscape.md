# 视觉卡片：横版单页（Landscape）

固定 1280×720 的横向画面（16:9）。适合 Twitter/X 配图、博客封面、PPT 嵌入、Slack 分享——"一屏看完"的信息密度。

> 与竖版同样**面向"内容凝练"**——只浓缩一篇文章/笔记的核心观点，不为活动营销服务（→ `poster`）、不为完整结构服务（→ `structure-map`）。

---

## 第一步：内容提炼

横版只能装下一屏信息，比竖版更克制。

```yaml
source: 出处（书名/文章名/作者/平台，可选；显示为 hero 顶部小标签）
title: 主标题（10字以内，文章最锋利的一句）
subtitle: 副标题（一句话，20字以内）

# 四选一：核心内容形态
mode: key_points | quote | stats | compare

# mode = key_points：要点卡（默认，最常用）
key_points:
  intro: 一句话引言（可选）
  items:
    - 要点1（12字以内）
    - 要点2
    - 要点3
    - 要点4（最多 4 条）

# mode = quote：金句卡
quote:
  text: 一句金句（30字以内）
  attribution: 出处（人名/书名/文章名）

# mode = stats：数据卡（2-3 个数字）
stats:
  - value: "70%"
    label: "用户留存提升"
  - value: "3x"
    label: "查询速度"
  - value: "0"
    label: "代码改动"

# mode = compare：对比卡
compare:
  left:
    title: 左侧标题
    items: ["要点1", "要点2", "要点3"]
  right:
    title: 右侧标题
    items: ["要点1", "要点2", "要点3"]

tags:
  - 关键词1（4字以内）
  - 关键词2
  - 关键词3（最多 3 个）

footer:
  source: 出处归属（"《XX》""@作者"，可选）
```

### 模式选择规则

- 内容是**多个并列要点** → `key_points`（默认，覆盖 80% 场景）
- 内容是**一句话精华** → `quote`
- 内容包含**几个关键数字** → `stats`
- 内容是**两种方案/前后对比** → `compare`

### 提炼规则

- **更短**：横版字段比竖版少 30-40%。要点 12 字内，关键词 3 个内
- **不放时间线、CTA、价格、报名**——这些是营销字段，应该用 `poster` 插件
- **footer 单行**：只放 source（出处归属），原文无则整段省略

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

## 第二步补：文字约束（字数表 + CSS 换行规则）

横版空间有限，字数约束比竖版更严。

### 字数硬表

| 字段 | 中文字数 | 允许行数 |
|---|---|---|
| `source` | ≤ 12 字 | **1 行** |
| `h1`（title） | ≤ 10 字 | ≤ 2 行 |
| `subtitle` | ≤ 25 字 | ≤ 2 行 |
| `tag` | ≤ 4 字 | **1 行** |
| `content-overline` / `section-overline` | ≤ 20 字符 | **1 行** |
| `content-header` / `section-header` | ≤ 10 字 | **1 行** |
| `section-intro` | ≤ 30 字 | ≤ 2 行 |
| `key-point-item`（mode=key_points） | ≤ 12 字 | **1 行** |
| `quote-text`（mode=quote） | ≤ 30 字 | ≤ 3 行 |
| `quote-attribution` | ≤ 15 字 | **1 行** |
| `stat-value`（mode=stats） | ≤ 4 字符 | **1 行** |
| `stat-label` | ≤ 8 字 | **1 行** |
| `compare-col-title`（mode=compare） | ≤ 8 字 | **1 行** |
| `compare-item` | ≤ 15 字 | **1 行** |
| `footer-source` | ≤ 20 字 | **1 行** |

### CSS 硬约束

```css
.source, .tag, .content-overline, .content-header, .section-header,
.key-point-item, .quote-attribution,
.stat-value, .stat-label, .compare-col-title,
.footer-source, .footer-meta {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 100%;
}
.subtitle, .section-intro, .quote-text, .compare-item {
  word-break: break-word;
  text-wrap: pretty;
  overflow-wrap: break-word;
}
```

### 原则

横版一屏装下，任何字段多占一行都可能挤溢出。出现 `…` 省略号必回提炼层缩字。

---

## 第三步：HTML 骨架

横版采用**两栏布局**（左 hero / 右内容）作为默认结构。design-md 只控制 CSS，不允许改结构。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>{{title}} · {{source}}</title>
  <style>
    /* 根据指定的 design-md 生成对应的 CSS */
  </style>
</head>
<body>

  <main class="card-grid">

    <!-- 左栏：Hero -->
    <section class="hero">
      <div class="source">{{source}}</div>
      <h1>{{title}}</h1>
      <div class="subtitle">{{subtitle}}</div>

      <!-- 关键词紧贴 hero 底部 -->
      <div class="tags">
        <span class="tag">{{tag}}</span>
      </div>
    </section>

    <!-- 右栏：内容主体（按 mode 选一种） -->
    <section class="content">

      <!-- mode = key_points -->
      <div class="key-points-section">
        <div class="section-intro">{{key_points.intro}}</div>
        <div class="key-points-list">
          <div class="key-point-item">
            <span class="key-point-dot"></span>
            <span>{{key_point}}</span>
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

  <!-- Footer：底部出处单行（原文无则省略） -->
  <footer class="footer">
    <div class="footer-source">{{footer.source}}</div>
  </footer>

</body>
</html>
```

### 结构规则

- **两栏栅格**：`.card-grid { display: grid; grid-template-columns: 5fr 7fr; gap: 48px; }`（hero 占 5/12，content 占 7/12）
- **Hero 区**：source + h1 + subtitle + tags（关键词紧贴 hero 底部，不再独立成区）
- **Content 区**：按 mode 选一种内容结构
- **Footer**：独立于栅格之外，固定在 body 底部一行；原文无 source 则整段省略

### 禁止替换的组件

- 禁止把横版做成「上下三段式」长图（那是竖版做法）
- 禁止 stat-item 用图标 emoji 替代数字
- 禁止 quote 不带引号视觉
- 禁止内容溢出后用 `overflow: scroll` 兜底
- **禁止加营销字段**（CTA、价格、报名、时间地点）——属于 `poster` 插件

### CSS 类名约定

| 类名 | 用途 |
|------|------|
| `.card-grid` | 主栅格容器（两栏） |
| `.hero` | 左栏 hero |
| `.source` | 出处标签（hero 顶部） |
| `.subtitle` | 副标题 |
| `.tags` / `.tag` | hero 底部的关键词组 |
| `.content` | 右栏内容容器 |
| `.key-points-section` / `.key-points-list` / `.key-point-item` / `.key-point-dot` | key_points 模式 |
| `.quote` / `.quote-text` / `.quote-attribution` | quote 模式 |
| `.stats-grid` / `.stat-item` / `.stat-value` / `.stat-label` | stats 模式 |
| `.compare-grid` / `.compare-col` / `.compare-title` | compare 模式 |
| `.footer` / `.footer-source` | 底部出处单行 |

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
| `.key-point-item` | List item 样式，装饰符号用品牌色 |
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
- [ ] hero 内的 source、h1、subtitle、tags 是否都在 safe area 内（未贴边）？
- [ ] content 区是否在 safe area 内？
- [ ] 内容是否完全装在 720px 内、无溢出？（在浏览器/截图工具里渲染验证）
- [ ] 如果内容溢出，是否回到「第一步」再次缩减字数 / 删条目，而不是缩小字号兜底？
- [ ] footer 是否单行紧贴底部？无 source 时是否整段省略？

### 风格自查（对照 design-md）

- [ ] 读 design-md 第 7 节 "Do's and Don'ts"，逐条对照
- [ ] 读 design-md 第 1 节 "Key Characteristics"，确认风格"身份标志"是否复现
- [ ] 横版下 h1 字号是否足够大（≥ 56px）？

### 换行自查（字数约束验证）

- [ ] `source / tag / key-point-item / stat-value / stat-label / compare-col-title` 严格单行？
- [ ] 有没有 `…` 省略号出现？如有 → 回到提炼层缩字
- [ ] `quote-text` 是否 ≤ 3 行？`compare-item` 是否每条 1 行？
- [ ] 所有「1 行」字段的 CSS 是否都声明了 `white-space: nowrap; overflow: hidden; text-overflow: ellipsis`？

### 内容自查

- [ ] 标题是否 ≤ 10 字？是否是文章中"最锋利的一句"？
- [ ] key_points 每条 ≤ 12 字、最多 4 条？
- [ ] quote ≤ 30 字？
- [ ] stat 最多 3 个？
- [ ] tag 最多 3 个、每条 ≤ 4 字、是观点/情绪标签而非分类标签？
- [ ] **是否误用为活动/产品营销？** 如有 CTA、价格、报名按钮 → 停止，提示用户改用 `poster`
- [ ] **是否误用为方法论结构？** 如有大量层级、原本应有"步骤""清单""对比" → 停止，提示用户改用 `structure-map`
