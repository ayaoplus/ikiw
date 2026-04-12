# 海报生成模板

当用户提供一段活动/项目/产品的原始内容，需要生成海报时，按以下框架处理。

## 第一步：内容提炼

从原始内容中提取以下结构化信息。如果原文没有明确提供某项，合理推断或留空。

```yaml
brand: 品牌/组织名称
event: 活动名称或类型标签（如"快闪行动""训练营""发布会"）
title: 主标题（10字以内，核心卖点）
subtitle: 副标题（一句话，20字以内，补充说明）
tagline: 英文标语（可选，用于装饰）

highlights:
  intro: 一句话总结亮点的核心逻辑（如"不是学工具，而是学落地"）
  items:
    - 亮点1（15字以内）
    - 亮点2
    - 亮点3
    - 亮点4
    - 亮点5
    - 亮点6（最多6条）

tags:
  - 标签1（4字以内，如"人人可操作"）
  - 标签2
  - 标签3
  - 标签4（最多4个）

timeline:
  - date: "日期"
    event: "事件描述"
  - date: "日期"
    event: "事件描述"

footer:
  name: 品牌/作者名
  note: 一行补充信息
```

### 提炼规则

- **标题要短**：砍掉修饰词，留动词和名词。"21天掌握从内容生产到带货落地核心打法" → 标题"全域带货实战营"，副标题"21天掌握内容生产到带货落地核心打法"
- **亮点要并列**：每条亮点是独立的卖点，不要有递进关系。读者扫一眼就能抓住价值
- **标签要极短**：4个字最佳，最多6个字。是情绪标签不是描述："易上手""可复制""低门槛"
- **时间线要简**：日期 + 3-5字事件描述。不要写完整句子

## 第二步：画布规范（强制）

海报是**固定宽度的竖向长图**，不是响应式网页。生成 HTML 时必须遵守以下空间契约：

| 项 | 约束 |
|----|------|
| 画布宽度 | `body { width: 750px; margin: 0 auto; }` 固定 750px 居中 |
| 左右安全区 | `body { padding-left: 48px; padding-right: 48px; }` 所有内容必须落在 safe area 内 |
| 顶部/底部 padding | `body { padding-top: 56px; padding-bottom: 56px; }` |
| 子元素继承 | section、hero、tags、timeline 等区块**不再单独设置左右 padding**，全部继承 body 的 safe area |
| Section 垂直间距 | 相邻 section 之间至少 `margin-top: 48px`（视觉风格允许的话可放大到 64-80px） |
| 卡片/列表内边距 | hero 内的数据卡、highlight-item、tag、timeline-item 等子元素**不允许贴边**，必须落在 safe area 内 |
| Footer 处理 | 如果原文没有 footer 信息，**整个 footer 区块直接省略**，不允许出现空白 footer 占位条 |
| 海报底部 | 最后一个内容区块结束后即结束（加上 body 的 padding-bottom），不允许下方留出大块空白 |

> 这一节的规则**优先于 design-md 中的网页 padding 规范**。design-md 描述的是网页文档，海报是固定画布，二者空间体系不同。

## 第三步：HTML 结构

所有海报统一使用以下 HTML 骨架。design-md 只控制 CSS 样式，不改变结构。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{{brand}} · {{event}}</title>
  <style>
    /* 根据指定的 design-md 生成对应的 CSS */
  </style>
</head>
<body>

  <!-- 1. Hero 区：品牌标签 + 主标题 + 副标题 -->
  <div class="hero">
    <div class="badge">{{brand}} · {{event}}</div>
    <h1>{{title}}</h1>
    <div class="subtitle">{{subtitle}}</div>
  </div>

  <!-- 2. 亮点区：引言 + 列表 -->
  <div class="section highlights-section">
    <div class="section-header">项目亮点</div>
    <div class="section-intro">{{highlights.intro}}</div>
    <div class="highlights-list">
      <!-- 每条亮点一个 item -->
      <div class="highlight-item">
        <span class="highlight-dot"></span>
        <span>{{highlight}}</span>
      </div>
    </div>
  </div>

  <!-- 3. 标签区 -->
  <div class="tags">
    <span class="tag">{{tag}}</span>
  </div>

  <!-- 4. 时间线区 -->
  <div class="section timeline-section">
    <div class="section-header">时间排期</div>
    <div class="timeline">
      <div class="timeline-item">
        <span class="timeline-date">{{date}}</span>
        <span class="timeline-event">{{event}}</span>
      </div>
    </div>
  </div>

  <!-- 5. Footer -->
  <div class="footer">
    <div class="footer-name">{{footer.name}}</div>
    <div class="footer-note">{{footer.note}}</div>
  </div>

</body>
</html>
```

### 结构规则

- **Hero 区必须有**：badge + h1 + subtitle，这是海报的第一视觉焦点
- **亮点区**：section-header + intro（一句话）+ 列表（最多6条）
- **标签区**：紧跟亮点区下方，横向排列，视觉上是亮点的"总结标签"
- **时间线区**：如果没有时间信息，整个区块省略
- **Footer**：品牌名 + 一行注释，收尾

### CSS 类名约定

无论套什么 design-md，HTML 类名保持一致：

| 类名 | 用途 |
|------|------|
| `.hero` | 顶部 hero 区容器 |
| `.badge` | 品牌/事件标签 |
| `.subtitle` | 副标题 |
| `.section` | 通用内容区块 |
| `.section-header` | 区块标题（"项目亮点""时间排期"） |
| `.section-intro` | 区块引言（一句话） |
| `.highlights-list` | 亮点列表容器 |
| `.highlight-item` | 单条亮点 |
| `.highlight-dot` | 亮点前的装饰符号 |
| `.tags` | 标签容器 |
| `.tag` | 单个标签 |
| `.timeline` | 时间线容器 |
| `.timeline-item` | 时间线单条 |
| `.timeline-date` | 日期 |
| `.timeline-event` | 事件描述 |
| `.footer` | 底部 |
| `.footer-name` | 品牌/作者名 |
| `.footer-note` | 补充信息 |

## 第四步：套用视觉样式

读取指定的 design-md 文件（如 `designs/ferrari.md`），按其定义的色彩、字体、间距、组件风格生成 CSS。

### 样式映射规则

| 海报区块 | 映射到 design-md 的 |
|----------|---------------------|
| `.hero` | Hero / Cover 组件的背景、字体、间距 |
| `.badge` | Badge / Overline 的样式 |
| `h1` | Display / Hero heading 的字号、字重、行高 |
| `.subtitle` | Body Large / Intro 的样式 |
| `.section-header` | Section Heading 的样式 |
| `.section-intro` | Caption / Secondary text |
| `.highlight-item` | List item 样式，装饰符号用品牌色 |
| `.tag` | Badge / Label 的样式 |
| `.timeline-date` | 品牌色 + 加粗 |
| `.timeline-event` | Secondary text |
| `.footer` | Footer / Back page 的样式 |

### 视觉适配原则

- **深色系 design（ferrari, bmw, runwayml, sentry）**：hero 用深色底 + 白字，亮点和时间线也保持深色
- **浅色系 design（huasheng, notion, mintlify）**：hero 可用品牌色底或渐变底，正文用白底
- **混合系 design（claude, sanity）**：hero 用深色底，正文切白底或浅色底
- **品牌色只用在**：badge、section-header、highlight-dot、timeline-date、tag。不要大面积铺品牌色

## 第五步：生成后自查（必做）

生成 HTML 后，按以下清单逐条对照。任何一条不通过都必须修正后重新输出，**不允许带瑕疵交付**：

### 空间自查

- [ ] body 是否固定宽度 750px 并居中？
- [ ] body 是否有 ≥ 40px 的左右 padding（safe area）？
- [ ] hero 内的数据卡片、badge、subtitle 是否都在 safe area 内（未贴边）？
- [ ] highlight-item 列表是否在 safe area 内？
- [ ] tag、timeline-item 是否在 safe area 内？
- [ ] 相邻 section 之间是否 ≥ 40px 垂直间距？
- [ ] 是否存在空白 footer 占位（footer 区有背景色但没内容）？如有，必须删除整个 footer 块
- [ ] 海报最底部是否紧贴最后一个内容区块（仅留 body padding-bottom），无大块空白？

### 风格自查（对照 design-md 的 Don't 列表）

- [ ] 读 design-md 第 7 节 "Do's and Don'ts"，逐条对照本次输出是否违反
- [ ] 例：huasheng 禁止 hero 用渐变背景（除 cover/back page）；notion 禁止重阴影；ferrari 禁止浅色 hero 等
- [ ] 例：huasheng 要求 §01 §02 章节编号、3px 橙色顶分隔线、weight 900 主标题——这些是该风格的"身份标志"，缺一不可

### 内容自查

- [ ] 标题是否 ≤ 10 字？
- [ ] highlight 是否每条 ≤ 15 字、无递进关系？
- [ ] tag 是否每条 ≤ 6 字？
- [ ] timeline 的 event 是否 3-5 字短描述（不是完整句子）？
