# 海报：横版（Landscape）

固定 1280×720 的横向营销海报（16:9）。**用于 PPT 嵌入、网站 banner、Twitter/X 配图、活动横幅**——一屏看完的紧凑营销形态。

> 横版海报是**信息密度高的营销物**，不是"极简卡片"。要塞满 720px 的纵向空间，避免大块留白，让转化元素（数据钩子、价格 anchor、行动指令）形成连续的视觉密度。

> 大多数海报场景请用竖版（→ `poster-portrait.md`）。横版只在需要"slide / banner / 横幅"形态时使用。

---

## 第一步：内容提炼

横版**不是极简版竖版**——是横向铺开的营销物，**信息密度要够高**才能填满 720px。提取以下字段：

```yaml
brand: 主办方/品牌名（必填）
event: 活动类型标签（必填）
title: 主标题（10字以内，强营销标题）
subtitle: 副标题（一句话，20字以内）

# Stats 数据卡（强烈推荐，2-3 个）— 横版海报最重要的视觉钩子
stats:
  - value: "21天"
    label: "陪跑训练"
  - value: "全流程"
    label: "拆解到落地"
  - value: "高复制"
    label: "适合个体小团队"

# 卖点（推荐有，4-5 条）
features:
  intro: 一句话总结活动核心（可选）
  items:
    - 卖点1（12字以内）
    - 卖点2
    - 卖点3
    - 卖点4
    - 卖点5（最多 5 条）

# 适合谁（关键词，可选）
audience:
  tags:
    - 标签1（4字以内）
    - 标签2
    - 标签3（最多 3 个）

# Timeline 时间排期（可选，但能显著提升信息密度）
timeline:
  - date: "04.13"
    event: "报名"
  - date: "04.14"
    event: "拉群"
  - date: "04.16-05.11"
    event: "21天训练"
  - date: "05.11"
    event: "结营"

# 价格（强烈推荐）
price:
  current: "499"
  original: "999"
  unit: "¥"
  note: "前 30 名享早鸟价"

# CTA（必填，海报核心）
cta:
  text: "扫码报名"
  qrcode: "/path/to/qr.png"
  deadline: "04.13 24:00 截止"

# Footer
footer:
  brand: 主办方名字
```

### 提炼规则

- **stats 是横版海报的灵魂**：3 个数字钩子能瞬间建立营销张力，**不要省略**（除非原文确实没数据）
- **features 4-5 条最佳**：少于 4 条会显得空，多于 5 条会挤
- **timeline 推荐保留**：横版可以横排展示时间线（4-5 节点），既有信息密度又显得专业
- **action-box 是核心**：price + qrcode + deadline 全部保留，视觉上要做到比其他任何区块都更突出
- **目标是填满 720px 高度，不是留白**——如果生成完发现下半截留白超过 80px，回去加 stats / timeline / 缩窄字号
- **如果原文是文章/方法论而非营销 brief，停止**，提示用户改用 `visual-card` 或 `structure-map`

---

## 第二步：画布契约（强制）

| 项 | 约束 |
|----|------|
| 画布尺寸 | `body { width: 1280px; height: 720px; margin: 0 auto; overflow: hidden; }` |
| 左右安全区 | `body { padding-left: 56px; padding-right: 56px; }` |
| 顶部/底部 padding | `body { padding-top: 48px; padding-bottom: 40px; }` |
| 主体布局 | `display: grid; grid-template-columns: 4fr 8fr; gap: 48px;`（左 hero 4/12，右内容 8/12） |
| 右栏纵向布局 | `display: flex; flex-direction: column;` 让 stats / features / action 三段填满纵向，不留间隙 |
| 内容溢出处理 | 内容塞不下 720px → 减少 features 条数 / 缩短文字，禁止溢出滚动或缩字号兜底 |
| 留白阈值 | 如果右栏底部留白 > 80px，**必须**回到第一步增加 stats 或 timeline，禁止让画面看起来"空旷" |
| Footer | 底部一行，缺失 brand 则整段省略 |

> 横版核心约束：**一屏装下 + 信息密度足够高**。两者要同时满足。

---

## 第二步补：文字约束（字数表 + CSS 换行规则）

横版空间更紧，字数约束比竖版更严。

### 字数硬表

| 字段 | 中文字数 | 允许行数 |
|---|---|---|
| `badge` | ≤ 10 字 | **1 行** |
| `h1`（title） | ≤ 10 字 | ≤ 2 行 |
| `subtitle` | ≤ 20 字 | ≤ 2 行 |
| `tag` | ≤ 4 字 | **1 行** |
| `section-header` | ≤ 8 字 | **1 行** |
| `section-intro` | ≤ 30 字 | ≤ 2 行 |
| `stat-value` | ≤ 4 字符 | **1 行** |
| `stat-label` | ≤ 8 字 | **1 行** |
| `feature-item` | **≤ 12 字** | **1 行** |
| `timeline-date` | 固定格式 `MM.DD` | **1 行** |
| `timeline-event` | ≤ 6 字 | **1 行** |
| `price-current` | ≤ 6 字符 | **1 行** |
| `price-original` | ≤ 8 字符 | **1 行** |
| `cta-text` | ≤ 6 字 | **1 行** |
| `cta-deadline` | ≤ 10 字 | **1 行** |
| `hero-footer` | ≤ 30 字符 | **1 行** |
| `footer-brand` | ≤ 16 字 | **1 行** |

### CSS 硬约束

```css
.badge, .tag, .section-header, .stat-value, .stat-label,
.feature-item, .timeline-date, .timeline-event,
.price-current, .price-original, .cta-text, .cta-deadline,
.hero-footer, .footer-brand, .footer-meta {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 100%;
}
.subtitle, .section-intro {
  word-break: break-word;
  text-wrap: pretty;
  overflow-wrap: break-word;
}
```

### 原则

横版宽度有限，任何字段换行都可能破坏布局。**feature-item 在横版必须 1 行内完成**（这是与竖版最大的差异）。出现 `…` 必回提炼层缩字。

---

## 第三步：HTML 骨架

横版采用**两栏布局**：左 hero（占 4/12）+ 右内容栈（占 8/12，纵向铺满 stats / features / timeline / action）。

### 字体加载（必加）

`<head>` **必须**包含 Google Fonts `<link>` 加载 design-md 指定的字体。详细规则 + 速查表：[`_font-loading.md`](./_font-loading.md)。专有字体用 Google Fonts 近似替代，中文内容加载 `Noto Sans SC`。

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
      <div class="hero-footer">{{footer.brand}}</div>
    </section>

    <!-- 右栏：信息密集型纵向栈 -->
    <section class="content">

      <!-- Stats 横排（推荐有） -->
      <div class="stats-row">
        <div class="stat-item">
          <div class="stat-value">{{value}}</div>
          <div class="stat-label">{{label}}</div>
        </div>
        <!-- 重复 2-3 次 -->
      </div>

      <!-- Features 卖点列表 -->
      <div class="features-section">
        <div class="section-header">项目亮点</div>
        <div class="features-list">
          <div class="feature-item">
            <span class="feature-dot"></span>
            <span>{{feature}}</span>
          </div>
        </div>
      </div>

      <!-- Timeline 时间排期（可选，横排紧凑展示） -->
      <div class="timeline-row">
        <div class="timeline-item">
          <span class="timeline-date">{{date}}</span>
          <span class="timeline-event">{{event}}</span>
        </div>
        <!-- 重复 3-5 次 -->
      </div>

      <!-- Action-box: Price + CTA（核心转化区，视觉最强） -->
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

</body>
</html>
```

### 结构规则

- **两栏栅格**：`.card-grid { display: grid; grid-template-columns: 4fr 8fr; gap: 48px; }`
- **Hero 区**：badge + h1 + subtitle + tags + 底部 brand 字样（hero-footer 紧贴左栏底部，作为品牌签名）
- **右栏内容栈**：stats / features / timeline / action 四段纵向铺满，**不允许大块留白**
- **Action-box 必须视觉最强**：品牌色背景或 2px 品牌色边框，明显区别于 stats 和 features
- **Stats 横排**：3 个数据卡均分宽度，作为右栏顶部的视觉钩子
- **Timeline 横排**：4-5 个时间节点紧凑展示，不要纵向 list 形态（会占太多高度）

### 禁止替换的组件

- 禁止把 features 替换为 `<ul><li>` 或带 emoji 的列表
- 禁止内容溢出后用 `overflow: scroll` 兜底
- 禁止 action-box 平淡处理（必须是页面视觉焦点）
- 禁止省略 cta（横版海报哪怕极简也要保留行动入口）
- **禁止省略 stats**（除非原文真的没数据）——它是横版海报最重要的视觉钩子
- 禁止让右栏出现 ≥ 80px 的底部空白

### CSS 类名约定

| 类名 | 用途 |
|------|------|
| `.card-grid` | 两栏栅格容器 |
| `.hero` | 左栏 hero |
| `.badge` | 品牌/事件标签 |
| `.subtitle` | 副标题 |
| `.tags` / `.tag` | hero 中部关键词 |
| `.hero-footer` | hero 底部品牌签名 |
| `.content` | 右栏内容栈 |
| `.stats-row` / `.stat-item` / `.stat-value` / `.stat-label` | 数据卡横排 |
| `.features-section` / `.section-header` / `.features-list` / `.feature-item` / `.feature-dot` | 卖点列表 |
| `.timeline-row` / `.timeline-item` / `.timeline-date` / `.timeline-event` | 时间线横排 |
| `.action-box` | 营销转化区 |
| `.price-block` / `.price-original` / `.price-current` / `.price-note` | 价格区 |
| `.cta-block` / `.cta-text` / `.cta-qrcode` / `.cta-deadline` | CTA 入口 |

---

## 第四步：套用视觉样式

读取指定的 design-md，按其定义的色彩、字体、间距生成 CSS。

### 样式映射规则

| 海报区块 | 映射到 design-md 的 |
|----------|---------------------|
| `.hero` | Hero / Cover 组件，可整块用品牌色或深色块 |
| `.content` | 通常白底/浅底，承载主信息 |
| `h1` | Display / Hero heading（横版字号建议 ≥ 52px） |
| `.subtitle` | Body Large / Intro 样式 |
| `.stat-value` | 大号数字（28-36px），品牌色加粗 |
| `.stat-label` | Caption / Secondary text |
| `.feature-item` | List item，装饰符号用品牌色 |
| `.timeline-date` | Inter 字体 + 品牌色 + 加粗 |
| `.timeline-event` | Body 短文本 |
| `.action-box` | 视觉最强 box——品牌色背景或 2px 品牌色边框 |
| `.price-current` | 大号 + 品牌色 + 加粗（≥ 40px） |
| `.cta-text` | Button 样式，品牌色背景 + 白字 |
| `.cta-qrcode` | 居中，100-130px |
| `.cta-deadline` | 强调色，制造紧迫感 |

### 视觉适配原则

- **左右色块对比**：横版常用「左栏品牌色块或深色块 + 右栏白底」营造视觉冲击
- **字号比竖版大**：h1 ≥ 52px，feature-item 也要适度放大
- **action-box 占据右栏底部 1/3 高度**：作为视觉锚点
- **stats / features / timeline / action 四段平衡分布**：每段不超过右栏 35% 高度

---

## 第五步：生成后自查（必做）

### 空间自查

- [ ] body 是否固定 1280×720？是否设置 `overflow: hidden`？
- [ ] body 是否有 ≥ 56px 的左右 padding？
- [ ] hero、content、各子区块、footer 是否都在 safe area 内（未贴边）？
- [ ] 内容是否完全装在 720px 内、无溢出？
- [ ] **右栏底部留白是否 ≤ 80px？** 如果 > 80px，必须回到第一步增加 stats / timeline / features 内容
- [ ] 如内容溢出，是否回到第一步缩减字数 / 删条目，而不是缩字号兜底？

### 信息密度自查（横版海报特有）

- [ ] 是否包含 stats 数据卡（除非原文真的没数据）？
- [ ] features 是否 4-5 条（不少于 4）？
- [ ] 是否包含 timeline 或其他能填满纵向空间的信息？
- [ ] 整体看起来是否像"信息丰富的营销海报"，而不是"极简卡片"？

### 营销有效性自查（海报特有）

- [ ] action-box 是否视觉上明显强于 features 区？
- [ ] price-current 是否 ≥ 40px？是否有划线原价制造 anchor？
- [ ] cta-text 是否是明确的行动指令（"扫码报名""立即购买"），而不是"了解更多"？
- [ ] 是否包含至少一种紧迫感元素（deadline / 限定名额 / 早鸟价说明）？

### 换行自查（字数约束验证）

- [ ] 所有 `badge / tag / stat / timeline / cta / price / feature-item` 字段是否严格单行？（横版比竖版更严格，**feature-item 在横版必须 1 行**）
- [ ] 有没有 `…` 省略号出现？如有 → 回到提炼层缩字
- [ ] 所有「1 行」字段的 CSS 是否都声明了 `white-space: nowrap; overflow: hidden; text-overflow: ellipsis`？

### 字体加载自查

- [ ] `<head>` 包含 Google Fonts `<link>`？字重覆盖 design-md 所有声明？
- [ ] 中文加载了 `Noto Sans SC`？
- [ ] 专有字体用了替代？

### 风格自查（对照 design-md）

- [ ] 读 design-md 第 7 节 "Do's and Don'ts"，逐条对照
- [ ] 读 design-md 第 1 节 "Key Characteristics"，确认风格"身份标志"是否复现
- [ ] 横版下 h1 字号 ≥ 52px？

### 内容自查

- [ ] 标题 ≤ 10 字？是营销标题（吸引报名）而非描述标题？
- [ ] features 每条 ≤ 12 字、4-5 条？
- [ ] tag 每条 ≤ 4 字、最多 3 个？
- [ ] 是否误用为内容凝练 / 方法论结构图？如是 → 提示用户改用对应插件
