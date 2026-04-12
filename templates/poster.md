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

## 第二步：HTML 结构

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

## 第三步：套用视觉样式

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
