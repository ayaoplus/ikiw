# 结构图组件库

`structure-map` 插件的核心。每个组件 = HTML 骨架 + CSS 类名约定 + 适用内容模式 + 强制约束。

> **LLM 必须从此库选择组件，不允许自创**。换 design-md 只换 CSS，不换 HTML 结构。

---

## 通用规则

### 共享类名（所有组件继承）

| 类名 | 用途 |
|------|------|
| `.section` | 通用区块容器（每个组件外层都是 section） |
| `.section-overline` | 区块上方的小标签（"§01 PRINCIPLES" 等） |
| `.section-header` | 区块大标题（中文） |
| `.section-intro` | 区块引言（一句话，可选） |

### 共享视觉规则

- 每个 section 之间 `margin-top: 64px`
- 每个 section-header 之上有 3px 顶部分隔线（受 design-md 控制颜色，huasheng = 橙色，notion = 浅灰）
- 每个 section 内部组件不再独立设置 padding，继承 body safe area

### §XX 编号

**强制要求**：所有 structure-map 的 section-overline 都带 `§01 / §02 / ...` 编号——结构图组件多，编号给读者导航。建议用 CSS counter 自动生成。

```css
body { counter-reset: section; }
.section-overline { counter-increment: section; }
.section-overline::before { content: "§" attr(data-num) "  "; }
/* 或者用 counter() 自动生成 */
```

### 共享文字约束（所有组件必加）

结构图组件多，文字换行问题最容易出现。生成 HTML 时**必须**在 CSS 里包含下面两组规则：

**「1 行」字段**——全部加 `nowrap + ellipsis`：

```css
.source, .section-overline, .section-header,
.principle-name,
.step-number, .step-name,
.typology-tag, .typology-name,
.check-group-title,
.warning-name,
.quote-attribution,
.compare-col-title,
.stat-value, .stat-label,
.phrase-group-title, .phrase-tag,
.footer-source, .footer-meta {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 100%;
}
```

**多行字段**——智能换行：

```css
.subtitle, .section-intro,
.principle-desc, .step-desc, .typology-desc,
.check-text, .warning-desc,
.quote-text, .compare-item, .tldr-content {
  word-break: break-word;
  text-wrap: pretty;
  overflow-wrap: break-word;
}
```

**字数上限见每个组件的「强制约束」节。** 总体原则：**字数约束 > 容器约束 > 视觉约束**。出现 `…` 省略号 → 回提炼层缩字，**不要加宽容器**。

### 字体加载（所有结构图必加）

HTML `<head>` 必须包含 Google Fonts `<link>` 加载 design-md 指定的字体。详细规则 + 33 个 design-md 的加载字符串速查：[`_font-loading.md`](./_font-loading.md)。专有字体用 Google Fonts 近似替代，中文内容加载 `Noto Sans SC`。

---

## 1. principles-list（并列原则/理念列表）

### 触发模式
- 标题含「原则」「理念」「价值观」「心得」「N 条」
- 内容是**并列**的多个独立条目（无顺序、无层级）

### HTML 骨架
```html
<section class="section principles-section">
  <div class="section-overline">PRINCIPLES</div>
  <div class="section-header">{{title}}</div>
  <div class="section-intro">{{intro}}</div>
  <div class="principles-list">
    <div class="principle-item">
      <div class="principle-name">{{name}}</div>
      <div class="principle-desc">{{description}}</div>
    </div>
    <!-- 重复 -->
  </div>
</section>
```

### CSS 类名
- `.principles-list` — 列表容器（建议 `display: grid; grid-template-columns: 1fr;`）
- `.principle-item` — 单条原则容器
- `.principle-name` — 原则名称（≤ 12 字，加粗）
- `.principle-desc` — 原则描述

### 强制约束
- 每条 = 名称 + 描述，**两者都必须有**
- 名称 ≤ 12 字，描述 overview ≤ 50 字 / full ≤ 150 字
- 至少 3 条，建议 4-6 条
- 多于 8 条考虑分组（用多个 principles-list 或 phrase-bank）
- 禁止把多个原则合并成一段散文

### 视觉适配
- 每条 item 用浅卡片（白底 + 1px 边框）或左侧 4px 品牌色 accent
- principle-name 用品牌色或 weight 700

---

## 2. numbered-steps（编号步骤/流程）

### 触发模式
- 标题含「步骤」「流程」「方法」「N 步」「如何 X」
- 内容**有先后顺序**（打乱会语义不通）

### HTML 骨架
```html
<section class="section steps-section">
  <div class="section-overline">WORKFLOW</div>
  <div class="section-header">{{title}}</div>
  <div class="section-intro">{{intro}}</div>
  <div class="numbered-steps">
    <div class="step-item">
      <div class="step-number">1</div>
      <div class="step-body">
        <div class="step-name">{{name}}</div>
        <div class="step-desc">{{description}}</div>
      </div>
    </div>
    <!-- 重复，编号递增 -->
  </div>
</section>
```

### CSS 类名
- `.numbered-steps` — 容器
- `.step-item` — 单步容器（横向 flex：number 在左，body 在右）
- `.step-number` — 大数字徽章（圆形或方形，品牌色背景）
- `.step-body` — 文字容器
- `.step-name` — 步骤名（加粗）
- `.step-desc` — 步骤描述

### 强制约束
- **必须**有数字编号（不允许 emoji、罗马数字、abc 替代）
- 编号从 1 开始递增
- 顺序不能打乱
- 每步描述 overview ≤ 80 字 / full ≤ 250 字
- 通常 3-7 步；超过 7 步考虑拆分或合并

### 视觉适配
- step-number 字号 ≥ 24px，weight 700-900，品牌色背景 + 白字
- step-item 之间 16-20px 间距

---

## 3. typology（分类/原型）

### 触发模式
- 标题含「N 种」「N 类」「N 个原型」「N 种类型」
- 内容是**命名分类**——每个类型有名字、有定义

### HTML 骨架
```html
<section class="section typology-section">
  <div class="section-overline">TYPES</div>
  <div class="section-header">{{title}}</div>
  <div class="section-intro">{{intro}}</div>
  <div class="typology-grid">
    <div class="typology-item">
      <div class="typology-tag">{{tag_text}}</div>
      <div class="typology-name">{{name}}</div>
      <div class="typology-desc">{{description}}</div>
    </div>
    <!-- 重复 -->
  </div>
</section>
```

### CSS 类名
- `.typology-grid` — 网格容器（2 列或 1 列，根据数量决定）
- `.typology-item` — 单个类型容器
- `.typology-tag` — 类型小标签（如 "TYPE 01"，可选）
- `.typology-name` — 类型名称（如"调查实验型"）
- `.typology-desc` — 类型描述

### 强制约束
- 每个类型必须有**名称**和**描述**
- 描述 overview ≤ 60 字 / full ≤ 200 字
- 2-6 个类型；超过 6 个考虑分组
- 禁止只列名称没有描述（那是 phrase-bank 的活）

### 视觉适配
- typology-item 是带边框的卡片
- 2 列网格（数量 ≥ 4）或 1 列（数量 ≤ 3）
- typology-name 用 sub-heading 字号

---

## 4. checklist（清单/自检项，支持分组）

### 触发模式
- 标题含「自检」「清单」「检查」「checklist」「待办」
- 内容是可勾选的离散项

### HTML 骨架
```html
<section class="section checklist-section">
  <div class="section-overline">CHECKLIST</div>
  <div class="section-header">{{title}}</div>
  <div class="section-intro">{{intro}}</div>
  <div class="checklist">
    <!-- 可选分组 -->
    <div class="check-group">
      <div class="check-group-title">{{group_name}}</div>
      <div class="check-group-items">
        <div class="check-item">
          <div class="check-icon">✓</div>
          <div class="check-text">{{item}}</div>
        </div>
        <!-- 重复 -->
      </div>
    </div>
    <!-- 多个分组 -->
  </div>
</section>
```

### CSS 类名
- `.checklist` — 容器
- `.check-group` — 分组容器（可选）
- `.check-group-title` — 分组名（如 "L1 硬性规则"）
- `.check-group-items` — 分组内的项
- `.check-item` — 单项
- `.check-icon` — 勾选图标（"✓" 或方框）
- `.check-text` — 项文字

### 强制约束
- 支持分组（如 L1/L2/L3/L4），也支持无分组扁平列表
- 每项一句话陈述，≤ 40 字
- 不超过 30 项总数；超过则拆为多个 checklist
- 分组的 check-group-title 必须显著区别于子项

### 视觉适配
- check-icon 用品牌色或绿色
- 分组之间 24-32px 间距
- 项之间 8-12px 间距

---

## 5. warning-block（禁忌/反模式）

### 触发模式
- 标题含「禁区」「避免」「不要」「绝对禁忌」「反模式」「AVOID」
- 描述包含负面动词

### HTML 骨架
```html
<section class="section warning-section">
  <div class="section-overline">AVOID</div>
  <div class="section-header">{{title}}</div>
  <div class="section-intro">{{intro}}</div>
  <div class="warning-block">
    <div class="warning-item">
      <div class="warning-icon">⚠</div>
      <div class="warning-body">
        <div class="warning-name">{{name}}</div>
        <div class="warning-desc">{{description}}</div>
      </div>
    </div>
    <!-- 重复 -->
  </div>
</section>
```

### CSS 类名
- `.warning-block` — 容器
- `.warning-item` — 单项（红色调或警告色调）
- `.warning-icon` — 警告图标（⚠ 或 ✗）
- `.warning-name` — 禁忌名称
- `.warning-desc` — 解释为什么禁

### 强制约束
- **视觉必须明显区别于其他 section**：红色边框/背景色/icon
- 至少 2 项
- 每项必须有"为什么禁"的解释
- 禁止用 principles-list 来表达禁忌（视觉上没有警告感）

### 视觉适配
- 红色调（具体值参考 design-md 的 error / warning color）
- warning-item 用 4px 红色左边框 + 浅红背景

---

## 6. quote-block（金句/引用）

### 触发模式
- 原文有突出的金句、关键论断、文档的"灵魂句子"
- 通常放在 hero 之后第一个 section（开篇定调），或某个章节的总结句

### HTML 骨架
```html
<section class="section quote-section">
  <blockquote class="quote-block">
    <div class="quote-text">"{{text}}"</div>
    <div class="quote-attribution">— {{attribution}}</div>
  </blockquote>
</section>
```

### CSS 类名
- `.quote-block` — 容器
- `.quote-text` — 金句正文（大字号、可斜体）
- `.quote-attribution` — 出处

### 强制约束
- 金句 ≤ 80 字
- 必须有引号视觉（不允许只是普通段落）
- attribution 可选（如金句是文档自己的论断则省略）

### 视觉适配
- quote-text 字号 ≥ 24px，可斜体
- 通常用 4px 品牌色左边框 + 加大行距

---

## 7. compare-2col（双列对比）

### 触发模式
- 标题含「对比」「vs」「应该 X 而不是 Y」「推荐 / 反对」「新 / 旧」
- 内容明确分两组

### HTML 骨架
```html
<section class="section compare-section">
  <div class="section-overline">COMPARE</div>
  <div class="section-header">{{title}}</div>
  <div class="section-intro">{{intro}}</div>
  <div class="compare-2col">
    <div class="compare-col compare-good">
      <div class="compare-col-title">{{left.title}}</div>
      <div class="compare-list">
        <div class="compare-item">{{item}}</div>
      </div>
    </div>
    <div class="compare-col compare-bad">
      <div class="compare-col-title">{{right.title}}</div>
      <div class="compare-list">
        <div class="compare-item">{{item}}</div>
      </div>
    </div>
  </div>
</section>
```

### CSS 类名
- `.compare-2col` — 网格容器（2 列均分）
- `.compare-col` — 单栏
- `.compare-good` / `.compare-bad` — 推荐/反对栏（颜色不同）
- `.compare-col-title` — 栏标题
- `.compare-list` / `.compare-item` — 项

### 强制约束
- 左右两栏各 3-7 项，**项数对等或差 1**
- compare-good 用绿色调或品牌色，compare-bad 用红色调或灰色调
- 禁止做成两个独立 list（必须是栅格对照）

### 视觉适配
- 2 列等宽栅格
- 左栏 ✓ 图标，右栏 ✗ 图标

---

## 8. key-stats（关键数字）

### 触发模式
- 内容包含突出数字（百分比、量、效果指标）
- 通常 2-4 个数字组合呈现

### HTML 骨架
```html
<section class="section stats-section">
  <div class="section-overline">STATS</div>
  <div class="section-header">{{title}}</div>
  <div class="key-stats">
    <div class="stat-item">
      <div class="stat-value">{{value}}</div>
      <div class="stat-label">{{label}}</div>
    </div>
    <!-- 重复 2-4 次 -->
  </div>
</section>
```

### CSS 类名
- `.key-stats` — 网格容器（2-4 列均分）
- `.stat-item` — 单个数字卡
- `.stat-value` — 大号数字
- `.stat-label` — 小号说明

### 强制约束
- 2-4 个数字
- 每个数字必须有 label
- stat-value 字号 ≥ 36px

### 视觉适配
- 数字用品牌色 + weight 900
- label 用 secondary text

---

## 9. phrase-bank（短语库/词汇表，支持分组）

### 触发模式
- 标题含「推荐用语」「术语」「高频词」「词组」「Lexicon」
- 内容是**大量短语**，按主题分组

### HTML 骨架
```html
<section class="section phrase-section">
  <div class="section-overline">LEXICON</div>
  <div class="section-header">{{title}}</div>
  <div class="section-intro">{{intro}}</div>
  <div class="phrase-bank">
    <div class="phrase-group">
      <div class="phrase-group-title">{{group_name}}</div>
      <div class="phrase-tags">
        <span class="phrase-tag">{{phrase}}</span>
      </div>
    </div>
    <!-- 重复 -->
  </div>
</section>
```

### CSS 类名
- `.phrase-bank` — 容器
- `.phrase-group` — 分组容器
- `.phrase-group-title` — 分组名
- `.phrase-tags` — 标签横排容器
- `.phrase-tag` — 单个短语标签

### 强制约束
- **必须**分组（无分组就用 principles-list）
- 每组 5-15 个短语
- 短语 ≤ 12 字
- 标签视觉是 chip / pill 样式

### 视觉适配
- phrase-tag 用浅底 + 圆角 + 小内边距
- 分组之间 24px 间距

---

## 10. tldr（开篇摘要）

### 触发模式
- 整篇文档的核心结论、TL;DR、灵魂段落
- 通常放在 hero 之后、第一个内容 section 之前

### HTML 骨架
```html
<section class="section tldr-section">
  <div class="section-overline">TL;DR</div>
  <div class="tldr-content">{{summary}}</div>
</section>
```

### CSS 类名
- `.tldr-section` — 区块（视觉特殊：大号、品牌色背景或边框）
- `.tldr-content` — 摘要正文

### 强制约束
- 1-3 句话
- 通常放在 hero 之后第一个 section
- 可省略
- 字号比普通段落大（≥ 18px）

### 视觉适配
- 用品牌色背景或 4px 品牌色左边框
- 不需要 section-header（overline 已经够了）

---

## 通用 hero（顶部入口）

每张 structure-map 顶部固定有一个 hero（不是 section）：

```html
<header class="hero">
  <div class="source">{{source}}</div>
  <h1>{{title}}</h1>
  <div class="subtitle">{{subtitle}}</div>
</header>
```

- `.source` — 出处标签（同 visual-card）
- `h1` — 文档标题
- `.subtitle` — 一句话总结

约束：
- title 是文档的核心主题或原标题
- subtitle 描述这是关于什么的结构图

---

## 通用 footer（底部）

```html
<footer class="footer">
  <div class="footer-source">{{source_url_or_attribution}}</div>
  <div class="footer-meta">由 ikiw structure-map 生成 · {{date}}</div>
</footer>
```

可省略，但若包含必须紧贴最后 section（不留空白条）。
