# 海报：横版（Landscape）

固定 1280×720 的横向营销海报（16:9）。**用于 PPT 嵌入、网站 banner、Twitter/X 配图、活动横幅**——一屏看完的紧凑营销形态。

> 大多数海报场景请用竖版（→ `poster-portrait.md`）。横版只在需要"slide / banner / 横幅"形态时使用。

---

## 第一步：内容提炼

横版只能装下一屏，比竖版更克制。优先保留：brand / hero / features / action（price + cta），**省略 stats、speakers、timeline**（如必须保留 timeline，最多 3 项压缩展示）。

```yaml
brand: 主办方/品牌名
event: 活动类型标签
title: 主标题（10字以内）
subtitle: 副标题（一句话，20字以内）

# 核心卖点（3-4 条最佳，最多 5 条）
features:
  intro: 一句话总结活动核心
  items:
    - 卖点1（12字以内）
    - 卖点2
    - 卖点3
    - 卖点4（最多 5 条）

# 适合谁（关键词，可选）
audience:
  tags:
    - 标签1（4字以内）
    - 标签2
    - 标签3（最多 3 个）

# 价格（可选）
price:
  current: "499"
  original: "999"
  unit: "元"
  note: "前 30 名享早鸟价"

# CTA（强烈推荐）
cta:
  text: "扫码报名"
  qrcode: "/path/to/qr.png"
  deadline: "2026-04-13 24:00 截止"

# Footer
footer:
  brand: 主办方名字
```

### 提炼规则

- **更短**：features 最多 5 条、每条 ≤ 12 字
- **不放 stats / speakers / timeline**——横版装不下，删
- **action-box 是核心**：横版即使内容少，价格 + CTA 也必须保留
- **如果原文是文章/方法论而非营销 brief，停止**，提示用户改用 `visual-card` 或 `structure-map`

---

## 第二步：画布契约（强制）

| 项 | 约束 |
|----|------|
| 画布尺寸 | `body { width: 1280px; height: 720px; margin: 0 auto; overflow: hidden; }` |
| 左右安全区 | `body { padding-left: 64px; padding-right: 64px; }` |
| 顶部/底部 padding | `body { padding-top: 56px; padding-bottom: 48px; }` |
| 主体布局 | `display: flex` 或 `display: grid`，左 hero / 右内容 + action（约 5fr : 7fr） |
| 内容溢出处理 | 内容塞不下 720px → **减少 features 条数 / 缩短文字**，禁止溢出滚动或缩字号 |
| Footer | 底部一行，缺失 brand 则整段省略 |

> 横版核心约束：**一屏装下，绝不溢出**。

---

## 第三步：HTML 骨架

横版采用**两栏布局**（左 hero / 右 features+action）作为默认结构。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>{{brand}} · {{event}} · {{title}}</title>
  <style>
    /* 根据指定的 design-md 生成对应的 CSS */
  </style>
</head>
<body>

  <main class="card-grid">

    <!-- 左栏：Hero + 关键词 -->
    <section class="hero">
      <div class="badge">{{brand}} · {{event}}</div>
      <h1>{{title}}</h1>
      <div class="subtitle">{{subtitle}}</div>
      <div class="tags">
        <span class="tag">{{tag}}</span>
      </div>
    </section>

    <!-- 右栏：Features + Action -->
    <section class="content">
      <!-- Features -->
      <div class="features-section">
        <div class="section-intro">{{features.intro}}</div>
        <div class="features-list">
          <div class="feature-item">
            <span class="feature-dot"></span>
            <span>{{feature}}</span>
          </div>
        </div>
      </div>

      <!-- Action: Price + CTA（合并为一个明显的 box） -->
      <div class="action-box">
        <div class="price-block">
          <span class="price-original">{{price.unit}}{{price.original}}</span>
          <span class="price-current">{{price.unit}}{{price.current}}</span>
          <span class="price-note">{{price.note}}</span>
        </div>
        <div class="cta-block">
          <div class="cta-text">{{cta.text}}</div>
          <img class="cta-qrcode" src="{{cta.qrcode}}" alt="二维码">
          <div class="cta-deadline">{{cta.deadline}}</div>
        </div>
      </div>
    </section>

  </main>

  <!-- Footer -->
  <footer class="footer">
    <div class="footer-brand">{{footer.brand}}</div>
  </footer>

</body>
</html>
```

### 结构规则

- **两栏栅格**：`.card-grid { display: grid; grid-template-columns: 5fr 7fr; gap: 56px; }`（hero 5/12，content 7/12）
- **Hero 区**：badge + h1 + subtitle + tags（关键词紧贴 hero 底部）
- **Content 区**：上半 features，下半 action-box
- **Action-box 必须视觉最强**：品牌色背景或 2px 品牌色边框，明显不同于 features
- **Footer**：底部一行，缺失 brand 则整段省略

### 禁止替换的组件

- 禁止把 `.feature-item` 替换为 `<ul><li>` 或带 emoji 的列表
- 禁止内容溢出后用 `overflow: scroll` 兜底
- 禁止 action-box 平淡处理（必须是页面视觉焦点）
- 禁止省略 cta（横版海报哪怕极简也要保留行动入口）

### CSS 类名约定

| 类名 | 用途 |
|------|------|
| `.card-grid` | 两栏栅格容器 |
| `.hero` | 左栏 hero |
| `.badge` | 品牌/事件标签 |
| `.subtitle` | 副标题 |
| `.tags` / `.tag` | hero 底部关键词 |
| `.content` | 右栏内容容器 |
| `.features-section` / `.features-list` / `.feature-item` / `.feature-dot` | 卖点列表 |
| `.action-box` | 营销转化区 |
| `.price-block` / `.price-original` / `.price-current` / `.price-note` | 价格区 |
| `.cta-block` / `.cta-text` / `.cta-qrcode` / `.cta-deadline` | CTA 入口 |
| `.footer` / `.footer-brand` | 底部 |

---

## 第四步：套用视觉样式

读取指定的 design-md，按其定义的色彩、字体、间距生成 CSS。

### 样式映射规则

| 海报区块 | 映射到 design-md 的 |
|----------|---------------------|
| `.hero` | Hero / Cover 组件，可整块用品牌色或深色块 |
| `.content` | 通常白底/浅底，承载主信息 |
| `h1` | Display / Hero heading（横版字号建议 ≥ 56px） |
| `.subtitle` | Body Large / Intro 样式 |
| `.feature-item` | List item，装饰符号用品牌色 |
| `.action-box` | 视觉最强 box——品牌色背景或 2px 品牌色边框 |
| `.price-current` | 大号 + 品牌色 + 加粗（≥ 40px） |
| `.cta-text` | Button 样式，品牌色背景 + 白字 |
| `.cta-qrcode` | 居中，100-140px |
| `.cta-deadline` | 强调色，制造紧迫感 |
| `.footer` | Footer 单行，secondary text 颜色 |

### 视觉适配原则

- **左右色块对比**：横版常用「左栏品牌色块 + 右栏白底」营造视觉冲击
- **字号比竖版大**：h1 ≥ 56px，feature-item 也要适度放大
- **action-box 垂直居中或紧贴底部**：作为转化区视觉锚点

---

## 第五步：生成后自查（必做）

### 空间自查

- [ ] body 是否固定 1280×720？是否设置 `overflow: hidden`？
- [ ] body 是否有 ≥ 56px 的左右 padding？
- [ ] hero、content、action-box、footer 是否都在 safe area 内（未贴边）？
- [ ] 内容是否完全装在 720px 内、无溢出？
- [ ] 如内容溢出，是否回到第一步缩减字数 / 删条目，而不是缩字号兜底？

### 营销有效性自查（海报特有）

- [ ] action-box 是否视觉上明显强于 features 区？
- [ ] price-current 是否 ≥ 40px？是否有划线原价制造 anchor？
- [ ] cta-text 是否是明确的行动指令（"扫码报名""立即购买"），而不是"了解更多"？
- [ ] 是否包含至少一种紧迫感元素（deadline / 限定名额 / 早鸟价说明）？

### 风格自查（对照 design-md）

- [ ] 读 design-md 第 7 节 "Do's and Don'ts"，逐条对照
- [ ] 读 design-md 第 1 节 "Key Characteristics"，确认风格"身份标志"是否复现
- [ ] 横版下 h1 字号 ≥ 56px？

### 内容自查

- [ ] 标题 ≤ 10 字？是营销标题（吸引报名）而非描述标题？
- [ ] features 每条 ≤ 12 字、最多 5 条？
- [ ] tag 每条 ≤ 4 字、最多 3 个？
- [ ] 是否误用为内容凝练 / 方法论结构图？如是 → 提示用户改用对应插件
