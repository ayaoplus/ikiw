# 视觉卡片：竖版长图（Portrait）

固定宽度 1080px、高度自适应的竖向长图。**用于把一篇文章/笔记的核心观点凝练成一张可分享的卡片**——只讲核心，不讲完整骨架。

> 不是用来宣传活动/产品（那是 `poster` 插件），也不是用来呈现完整方法论结构（那是 `structure-map` 插件）。

---

## 第一步：内容提炼

从原文中提取以下结构化信息。**面向"内容凝练"**——抽出文章想表达的核心观点和支撑要点，不是套用营销字段。

```yaml
source: 出处（书名/文章名/作者/平台，可选；用作 hero 上方的小标签）
title: 主标题（10字以内，文章的核心观点或最锋利的一句话）
subtitle: 副标题（一句话，20-30字，补充展开 title）

key_points:
  intro: 一句话引言（点出这些要点的共同逻辑，可选）
  items:
    - 要点1（15字以内，独立的核心点）
    - 要点2
    - 要点3
    - 要点4
    - 要点5（最多 5 条）

tags:
  - 关键词1（4字以内，是观点标签或情绪标签）
  - 关键词2
  - 关键词3
  - 关键词4（最多 4 个）

footer:
  source: 引用出处（"《XX》P.42""@作者 / 公众号"）
  link: 原文链接（可选）
```

### 提炼规则

- **标题要锋利**：是文章中"最值得被记住"的那一句话或核心论点，不是流水账标题。"如何构建个人知识库" → "AI 时代的知识库不再需要标签"
- **要点要并列**：每条独立支撑标题，没有递进/嵌套关系。读者扫一眼能记住
- **关键词不是分类**：是读者读完后会留在脑子里的"调子"——"反共识""有温度""第一性原理"，不是"AI/工具/方法"
- **如果原文是活动/产品宣传，停下并提示用户改用 `poster` 插件**——视觉卡片不为营销服务，硬塞会两败俱伤
- **如果原文是结构复杂的方法论/教程，提示用户改用 `structure-map`**——视觉卡片只能放 5 个点，方法论塞不下
- **footer 可选**：原文没有出处信息就整段省略，禁止留空

---

## 第二步：画布契约（强制）

竖版是固定宽度的长图，不是响应式网页。生成 HTML 时必须遵守以下空间约束：

### 绝对禁令（跨模板共性，违反必导致底部留白）

- ❌ **禁止** `body`、`html`、`main`、`#root` 任一元素使用：
  - `min-height: 100vh` / `min-height: 100%`
  - `height: 100vh` / `height: 100%` / `height: <任何固定值>`
  - `display: flex` + `justify-content: space-between` 垂直撑开
  - 任何 ancestor 元素的 `min-height` 超过自身内容高度
- ❌ **禁止** body 设置任何固定 `height`——body 高度**必须**由内容决定（即 `height: auto` 或不写 height）
- ❌ **禁止** hero `padding-top < 48px`；badge/source 到 h1 视觉间距 `< 24px`
- ❌ **禁止** 最后一个 section 之后插入任何"撑开高度"的占位 div

> 这些规则优先于任何 design-md 的提示。只要违反一条，输出图就会在底部出现大块留白。

### 标准约束

| 项 | 约束 |
|----|------|
| 画布宽度 | `body { width: 1080px; margin: 0 auto; }` 固定 1080px 居中 |
| 画布高度 | `body { height: auto; }`（或完全不设 height）——**由内容决定** |
| 左右安全区 | `body { padding-left: 64px; padding-right: 64px; }` 所有内容必须落在 safe area 内 |
| 顶部 padding | `body { padding-top: 56px; }` |
| 底部 padding | `body { padding-bottom: 56px; }` |
| Hero padding | hero 本身 `padding-top ≥ 48px`，badge/source 到 h1 `≥ 24px`，h1 到 subtitle `≥ 16px` |
| 子元素继承 | section、hero、tags 等区块**不再单独设置左右 padding**，全部继承 body 的 safe area |
| Section 垂直间距 | 相邻 section 之间至少 `margin-top: 48px`（视觉风格允许的话可放大到 64–80px） |
| 卡片/列表内边距 | hero 内的数据卡、key-point-item、tag 等子元素**不允许贴边**，必须落在 safe area 内 |
| Footer 处理 | 原文没有 footer 信息时**整个 footer 区块直接省略**，禁止出现空白 footer 占位条 |
| 卡片底部 | 最后一个内容区块结束后即结束（仅留 body 的 padding-bottom），禁止下方出现大块空白 |

> 这一节优先于 design-md 的网页 padding 规范——design-md 描述网页文档，视觉卡片是固定画布。

---

## 第三步：HTML 骨架

所有竖版卡片统一使用以下 HTML 结构。design-md 只控制 CSS 样式，**不允许改变结构**。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{{title}} · {{source}}</title>
  <style>
    /* 根据指定的 design-md 生成对应的 CSS */
  </style>
</head>
<body>

  <!-- 1. Hero 区：出处标签 + 主标题 + 副标题 -->
  <section class="hero">
    <div class="source">{{source}}</div>
    <h1>{{title}}</h1>
    <div class="subtitle">{{subtitle}}</div>
  </section>

  <!-- 2. 核心要点区 -->
  <section class="section key-points-section">
    <div class="section-header">核心要点</div>
    <div class="section-intro">{{key_points.intro}}</div>
    <div class="key-points-list">
      <!-- 每条要点一个 item -->
      <div class="key-point-item">
        <span class="key-point-dot"></span>
        <span>{{key_point}}</span>
      </div>
    </div>
  </section>

  <!-- 3. 关键词区 -->
  <section class="tags">
    <span class="tag">{{tag}}</span>
  </section>

  <!-- 4. Footer：出处归属（原文无则整段省略） -->
  <footer class="footer">
    <div class="footer-source">{{footer.source}}</div>
    <div class="footer-link">{{footer.link}}</div>
  </footer>

</body>
</html>
```

### 结构规则

- **Hero 区必须有**：source（可缺）+ h1 + subtitle，是卡片的第一视觉焦点
- **核心要点区**：section-header + intro（可选一句话）+ 列表（**最多 5 条**）
- **关键词区**：紧跟要点区下方，横向排列，最多 4 个
- **Footer**：原文有出处/链接才显示，否则整段省略

### 禁止替换的组件

LLM 容易"自由发挥"换组件，下列禁止：

- 禁止把 `.key-point-item` 替换为 `<ul><li>` 或带 emoji 的列表
- 禁止额外加 hero 上方的"装饰条""页眉"
- 禁止 footer 加渐变背景或大块色块
- **禁止加 timeline、CTA、价格、报名按钮等营销字段**——这些属于 `poster` 插件，视觉卡片只做内容凝练

### CSS 类名约定

无论套什么 design-md，类名保持一致：

| 类名 | 用途 |
|------|------|
| `.hero` | 顶部 hero 区容器 |
| `.source` | 出处标签（在 hero 顶部） |
| `.subtitle` | 副标题 |
| `.section` | 通用内容区块 |
| `.section-header` | 区块标题（"核心要点"） |
| `.section-intro` | 区块引言（一句话） |
| `.key-points-list` | 要点列表容器 |
| `.key-point-item` | 单条要点 |
| `.key-point-dot` | 要点前的装饰符号 |
| `.tags` | 关键词容器 |
| `.tag` | 单个关键词 |
| `.footer` | 底部 |
| `.footer-source` | 出处归属 |
| `.footer-link` | 原文链接 |

---

## 第四步：套用视觉样式

读取指定的 design-md 文件（如 `designs/huasheng.md`），按其定义的色彩、字体、间距、组件风格生成 CSS。

### 样式映射规则

| 卡片区块 | 映射到 design-md 的 |
|----------|---------------------|
| `.hero` | Hero / Cover 组件的背景、字体、间距 |
| `.badge` | Badge / Overline 的样式 |
| `h1` | Display / Hero heading 的字号、字重、行高 |
| `.subtitle` | Body Large / Intro 的样式 |
| `.section-header` | Section Heading 的样式 |
| `.section-intro` | Caption / Secondary text |
| `.highlight-item` | List item 样式，装饰符号用品牌色 |
| `.tag` | Badge / Label 的样式 |
| `.footer` | Footer / Back page 的样式 |

> 已废弃的映射（不再使用）：`.badge`（→ 改为 `.source`）、`.timeline-*`（视觉卡片不放时间线）。

### 视觉适配原则

- **深色系 design**（ferrari、bmw、runwayml、sentry）：hero 用深色底 + 白字，要点区也保持深色
- **浅色系 design**（huasheng、notion、mintlify）：hero 可用品牌色块或浅色块，正文用白底；**禁止使用渐变背景**（除非 design-md 明确允许）
- **混合系 design**（claude、sanity）：hero 用深色底，正文切白底或浅色底
- **品牌色只用在**：source、section-header、key-point-dot、tag。不要大面积铺品牌色

---

## 第五步：生成后自查（必做）

生成 HTML 后，按以下清单逐条对照。任何一条不通过都必须修正后重新输出，**不允许带瑕疵交付**。

### 空间自查

- [ ] body 是否固定宽度 1080px 并居中？
- [ ] body 是否有 ≥ 40px 的左右 padding（safe area）？
- [ ] hero 内的 source、h1、subtitle 是否都在 safe area 内（未贴边）？
- [ ] key-point-item 列表是否在 safe area 内？
- [ ] tag 是否在 safe area 内？
- [ ] 相邻 section 之间是否 ≥ 40px 垂直间距？
- [ ] 是否存在空白 footer 占位（footer 区有背景色但没内容）？如有，必须删除整个 footer 块
- [ ] 卡片最底部是否紧贴最后一个内容区块（仅留 body padding-bottom）、无大块空白？

### 风格自查（对照 design-md）

- [ ] 读 design-md 第 7 节 "Do's and Don'ts"，逐条对照本次输出是否违反
- [ ] 读 design-md 第 1 节 "Key Characteristics"，确认本次输出是否复现了该风格的"身份标志"
- [ ] 例：huasheng 必须有 §01 §02 章节编号、3px 橙色顶分隔线、weight 900 主标题、零渐变；notion 必须克制无重阴影；ferrari 必须深色 hero

### 内容自查

- [ ] 标题是否 ≤ 10 字？是否是文章中"最锋利的一句"？
- [ ] key_points 是否 ≤ 5 条、每条 ≤ 15 字、无递进关系？
- [ ] tag 是否 ≤ 4 个、每条 ≤ 4 字、是观点/情绪标签而非分类标签？
- [ ] **是否误用为活动/产品营销？** 如有 CTA、价格、报名按钮、时间地点 → 停止，告诉用户改用 `poster` 插件
- [ ] **是否误用为方法论结构？** 如有大量层级、原本应该有"步骤""清单""对比"等结构 → 停止，告诉用户改用 `structure-map` 插件
