# 海报：竖版（Portrait）

固定宽度 750px、高度自适应的竖向营销海报。**用于活动、产品、课程、发布会的宣传**——含品牌、卖点、价格、CTA 等营销字段。

> 不是视觉卡片（→ `visual-card`），不是结构图（→ `structure-map`）。海报的核心目的是**让读者看完之后产生行动**（报名、扫码、咨询、购买）。

---

## 第一步：内容提炼

从营销 brief 中提取以下结构化信息。所有字段都是可选的（除了 brand + title + subtitle），缺失时该区块整体省略。

```yaml
# 必填
brand: 主办方/品牌名
event: 活动类型标签（"训练营""发布会""读书会""课程""挑战赛"等）
title: 主标题（10字以内，最有吸引力的卖点或活动名）
subtitle: 副标题（一句话，20-30字，补充说明卖点和价值）

# 数据卡（可选，2-3 个）— 最直观的亮点数字，紧贴 hero 下方
stats:
  - value: "21天"
    label: "陪跑训练"
  - value: "全流程"
    label: "拆解到落地"
  - value: "高复制"
    label: "适合个体与小团队"

# 卖点（推荐有，3-6 条）
features:
  intro: 一句话总结这场活动的核心逻辑（"不是学工具，而是学落地"）
  items:
    - 卖点1（15字以内，独立的卖点）
    - 卖点2
    - 卖点3
    - 卖点4
    - 卖点5（最多 6 条）

# 适合谁（可选）
audience:
  intro: 一句话描述目标人群
  tags:
    - 标签1（4字以内，"人人可操作""易复制""低门槛"）
    - 标签2
    - 标签3
    - 标签4（最多 4 个）

# 时间排期（可选）
timeline:
  - date: "04月13日"
    event: "报名"
  - date: "04月14日"
    event: "拉群"
  - date: "04月15日"
    event: "晚上直播讲解项目 SOP"

# 主讲/嘉宾（可选）
speakers:
  - name: "张三"
    title: "AI 创业者 / 公众号 X 万粉"
    avatar: "/path/to/avatar.jpg"  # 可选
  - name: "李四"
    title: "前 Y 公司 CTO"

# 价格（可选）
price:
  current: "499"
  original: "999"          # 划线原价（可选）
  unit: "元"               # 单位（默认"元"，可改为"$"等）
  note: "前 30 名享早鸟价"  # 价格说明（可选）

# CTA（可选但强烈推荐）
cta:
  text: "扫码报名"          # 按钮文案
  qrcode: "/path/to/qr.png" # 二维码图片路径或 URL（可选）
  contact: "微信 xxx 备注「报名」"  # 联系方式（可选）
  deadline: "2026-04-13 24:00 截止" # 截止时间（可选，制造紧迫感）

# Footer
footer:
  brand: 主办方名字
  contact: 联系方式或免责声明
```

### 提炼规则

- **title 是营销标题，不是文章标题**：要"吸引点击/报名"，不是"准确描述"。"AI 时代如何运营公众号" → "公众号变现陪跑营"
- **stats 是视觉钩子**：3 个数字强卖点（"21天""全流程""高复制"），不要超过 3 个
- **features 是并列卖点**：每条独立的"为什么报名"，不要有顺序/递进
- **audience tags 是情绪标签**："人人可操作""低门槛""可复制"，不是冷冰冰的描述
- **price 是视觉焦点**：原价划线 + 现价大字，制造 anchor 效应
- **cta 是行动入口**：必须明确"做什么""去哪做""什么时候之前做"
- **deadline 制造紧迫感**：截止时间、限定名额能显著提升转化
- **如果原文没有营销意图（是文章/笔记/方法论），停下并提示用户改用 `visual-card` 或 `structure-map`**

---

## 第二步：画布契约（强制）

竖版海报是固定宽度的长图。空间约束：

| 项 | 约束 |
|----|------|
| 画布宽度 | `body { width: 750px; margin: 0 auto; }` |
| 左右安全区 | `body { padding-left: 48px; padding-right: 48px; }` |
| 顶部 padding | `body { padding-top: 56px; }` |
| 底部 padding | `body { padding-bottom: 56px; }` |
| 子元素继承 | section、hero、stats、features、tags、timeline 等区块**不再单独设置左右 padding**，全部继承 body 的 safe area |
| Section 垂直间距 | 相邻 section 之间至少 `margin-top: 48px`（视觉风格允许的话可放大到 64-80px） |
| Stats / CTA 子元素内边距 | stat-item、price-block、cta-button 等子元素**不允许贴边**，必须落在 safe area 内 |
| 缺失字段 | 任何一个区块的字段为空，**整个区块直接省略**，禁止出现空白占位 |
| 海报底部 | 最后一个内容区块结束后即结束（仅留 body 的 padding-bottom），禁止下方出现大块空白 |

> 这一节优先于 design-md 的网页 padding 规范——design-md 描述网页文档，海报是固定画布。

---

## 第三步：HTML 骨架

按以下顺序组装区块。每个区块如果数据缺失就**整段省略**。design-md 只控制 CSS 样式，不允许改变结构。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{{brand}} · {{event}} · {{title}}</title>
  <style>
    /* 根据指定的 design-md 生成对应的 CSS */
  </style>
</head>
<body>

  <!-- 1. Hero：品牌标签 + 主标题 + 副标题 -->
  <section class="hero">
    <div class="badge">{{brand}} · {{event}}</div>
    <h1>{{title}}</h1>
    <div class="subtitle">{{subtitle}}</div>
  </section>

  <!-- 2. Stats（可选，紧贴 hero） -->
  <section class="stats-section">
    <div class="stat-item">
      <div class="stat-value">{{value}}</div>
      <div class="stat-label">{{label}}</div>
    </div>
    <!-- 重复 2-3 次 -->
  </section>

  <!-- 3. Features（卖点） -->
  <section class="section features-section">
    <div class="section-header">项目亮点</div>
    <div class="section-intro">{{features.intro}}</div>
    <div class="features-list">
      <div class="feature-item">
        <span class="feature-dot"></span>
        <span>{{feature}}</span>
      </div>
    </div>
  </section>

  <!-- 4. Audience（适合谁，可选） -->
  <section class="section audience-section">
    <div class="section-header">适合谁</div>
    <div class="section-intro">{{audience.intro}}</div>
    <div class="tags">
      <span class="tag">{{tag}}</span>
    </div>
  </section>

  <!-- 5. Timeline（时间排期，可选） -->
  <section class="section timeline-section">
    <div class="section-header">时间排期</div>
    <div class="timeline">
      <div class="timeline-item">
        <span class="timeline-date">{{date}}</span>
        <span class="timeline-event">{{event}}</span>
      </div>
    </div>
  </section>

  <!-- 6. Speakers（主讲/嘉宾，可选） -->
  <section class="section speakers-section">
    <div class="section-header">主讲</div>
    <div class="speakers-list">
      <div class="speaker-item">
        <img class="speaker-avatar" src="{{avatar}}" alt="{{name}}">
        <div class="speaker-info">
          <div class="speaker-name">{{name}}</div>
          <div class="speaker-title">{{title}}</div>
        </div>
      </div>
    </div>
  </section>

  <!-- 7. Price + CTA（价格 + 报名入口，强营销区，建议合并为一个 box） -->
  <section class="section action-section">
    <div class="action-box">
      <!-- Price -->
      <div class="price-block">
        <span class="price-original">{{price.unit}}{{price.original}}</span>
        <span class="price-current">{{price.unit}}{{price.current}}</span>
        <span class="price-note">{{price.note}}</span>
      </div>
      <!-- CTA -->
      <div class="cta-block">
        <div class="cta-text">{{cta.text}}</div>
        <img class="cta-qrcode" src="{{cta.qrcode}}" alt="二维码">
        <div class="cta-contact">{{cta.contact}}</div>
        <div class="cta-deadline">{{cta.deadline}}</div>
      </div>
    </div>
  </section>

  <!-- 8. Footer -->
  <footer class="footer">
    <div class="footer-brand">{{footer.brand}}</div>
    <div class="footer-contact">{{footer.contact}}</div>
  </footer>

</body>
</html>
```

### 结构规则

- **Hero 必含**：badge + h1 + subtitle，海报第一视觉焦点
- **Stats**：紧贴 hero 下方（不加 section margin），视觉上是 hero 的一部分。最多 3 个，横向并排
- **Features**：3-6 条卖点，禁止超过 6 条
- **Audience**：可选，独立成区
- **Timeline / Speakers**：可选，按需出现
- **Action（Price + CTA）**：放在最后一个内容区块（footer 之前），合并为一个明显的 box——这是海报最重要的转化区
- **Footer**：单行收尾，缺失则省略

### 禁止替换的组件

- 禁止把 `.feature-item` 替换为 `<ul><li>` 或带 emoji 的列表
- 禁止把 `.timeline-item` 替换为编号步骤组件（圆圈数字 + 描述）
- 禁止 stat-value 用图标 emoji 替代数字
- 禁止 cta-block 没有任何视觉强化（必须用品牌色块、边框或阴影突出）
- 禁止额外加 hero 上方的"装饰条""页眉"

### CSS 类名约定

无论套什么 design-md，类名保持一致：

| 类名 | 用途 |
|------|------|
| `.hero` | 顶部 hero 区容器 |
| `.badge` | 品牌/事件标签 |
| `.subtitle` | 副标题 |
| `.stats-section` | 数据卡区容器 |
| `.stat-item` | 单个数据卡 |
| `.stat-value` | 数据值（大号数字） |
| `.stat-label` | 数据标签（小号说明） |
| `.section` | 通用内容区块 |
| `.section-header` | 区块标题 |
| `.section-intro` | 区块引言（一句话） |
| `.features-list` / `.feature-item` / `.feature-dot` | 卖点列表 |
| `.tags` / `.tag` | 标签组 |
| `.timeline` / `.timeline-item` / `.timeline-date` / `.timeline-event` | 时间线 |
| `.speakers-list` / `.speaker-item` / `.speaker-avatar` / `.speaker-name` / `.speaker-title` | 主讲嘉宾 |
| `.action-section` / `.action-box` | 营销转化区 |
| `.price-block` / `.price-original` / `.price-current` / `.price-note` | 价格区 |
| `.cta-block` / `.cta-text` / `.cta-qrcode` / `.cta-contact` / `.cta-deadline` | CTA 入口 |
| `.footer` / `.footer-brand` / `.footer-contact` | 底部 |

---

## 第四步：套用视觉样式

读取指定的 design-md 文件（如 `designs/huasheng.md`），按其定义的色彩、字体、间距、组件风格生成 CSS。

### 样式映射规则

| 海报区块 | 映射到 design-md 的 |
|----------|---------------------|
| `.hero` | Hero / Cover 组件的背景、字体、间距 |
| `.badge` | Badge / Overline 的样式 |
| `h1` | Display / Hero heading 的字号、字重、行高 |
| `.subtitle` | Body Large / Intro 的样式 |
| `.stat-value` | 大号数字（48-72px），品牌色或深色 |
| `.stat-label` | Caption / Secondary text |
| `.section-header` | Section Heading 的样式 |
| `.section-intro` | Caption / Secondary text |
| `.feature-item` | List item 样式，装饰符号用品牌色 |
| `.tag` | Badge / Label 的样式 |
| `.timeline-date` | 品牌色 + 加粗 |
| `.timeline-event` | Secondary text |
| `.speaker-avatar` | 圆形头像，48-64px |
| `.action-box` | 视觉最强的 box——品牌色背景或 2px 品牌色边框，明显不同于其他区块 |
| `.price-original` | 划线 + 灰色 |
| `.price-current` | 大号 + 品牌色 + 加粗 |
| `.cta-text` | Button 样式，品牌色背景 + 白字 |
| `.cta-qrcode` | 居中，120-160px |
| `.cta-deadline` | 强调色（如红色或品牌色），制造紧迫感 |
| `.footer` | Footer / Back page 的样式 |

### 视觉适配原则

- **深色系 design**（ferrari、bmw、runwayml、sentry）：hero 用深色底 + 白字，action-box 用品牌色块
- **浅色系 design**（huasheng、notion、mintlify）：hero 可用品牌色块或浅色块，action-box 用品牌色背景或 2px 品牌色边框
- **混合系 design**（claude、sanity）：hero 用深色底，正文切白底，action-box 切回深色
- **品牌色用在**：badge、stat-value、section-header、feature-dot、timeline-date、tag、price-current、cta-text 背景。其他地方克制使用

---

## 第五步：生成后自查（必做）

生成 HTML 后，按以下清单逐条对照。任何一条不通过都必须修正后重新输出，**不允许带瑕疵交付**。

### 空间自查

- [ ] body 是否固定宽度 750px 并居中？
- [ ] body 是否有 ≥ 40px 的左右 padding（safe area）？
- [ ] hero、stats、features、audience、timeline、speakers、action 各区块是否都在 safe area 内（未贴边）？
- [ ] stat-item、tag、timeline-item、speaker-item 等子元素是否在 safe area 内？
- [ ] 相邻 section 之间是否 ≥ 40px 垂直间距？（stats 紧贴 hero 例外）
- [ ] 是否存在空白区块占位？如有，必须删除整个区块
- [ ] 海报最底部是否紧贴最后一个内容区块（仅留 body padding-bottom）、无大块空白？

### 营销有效性自查（海报特有）

- [ ] action-box（价格 + CTA 区）是否视觉上明显比其他区块更突出？
- [ ] price-current 是否够大（≥ 48px）？是否有划线原价制造 anchor 效应？
- [ ] cta-text 是否是明确的行动指令（"扫码报名""立即购买"），而不是模糊词（"了解更多""查看详情"）？
- [ ] 是否包含至少一种紧迫感元素（deadline / 限定名额 / 早鸟价说明）？

### 风格自查（对照 design-md）

- [ ] 读 design-md 第 7 节 "Do's and Don'ts"，逐条对照本次输出是否违反
- [ ] 读 design-md 第 1 节 "Key Characteristics"，确认本次输出是否复现了该风格的"身份标志"
- [ ] 例：huasheng 必须有 §01 §02 章节编号、3px 橙色顶分隔线、weight 900 主标题、零渐变；notion 必须克制无重阴影；ferrari 必须深色 hero

### 内容自查

- [ ] 标题是否 ≤ 10 字？是否是营销标题（吸引报名）而非描述标题？
- [ ] feature 是否每条 ≤ 15 字、无递进关系？
- [ ] tag 是否每条 ≤ 6 字？是情绪/特征标签而非分类？
- [ ] timeline 的 event 是否 3-5 字短描述（不是完整句子）？
- [ ] **是否误用为内容凝练？** 如果原文是文章/笔记而非营销 brief → 停止，提示用户改用 `visual-card` 插件
- [ ] **是否误用为方法论结构图？** 如果原文是教程/方法论的全景结构 → 停止，提示用户改用 `structure-map` 插件
