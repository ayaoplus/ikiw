# 结构图：竖版（Portrait）

固定宽度 1080px 的竖向长图，高度自适应。**完整覆盖一份结构复杂内容的所有重要节点**——方法论、教程、SKILL 文档、长文骨架、产品规范。

> 与 visual-card 形态相同（1080px 长图），但**目的不同**：visual-card 是浓缩到 5 个点（取舍式），structure-map 是覆盖全部节点（穷举式）。

---

## 工作流（必须按顺序执行）

```
第一步：解析原文骨架（输出结构清单）
   ↓
第二步：组件映射（每个 H2 → 一个组件）
   ↓
第三步：内容提炼（按 depth 决定深度）
   ↓
第四步：HTML 组装（按计划填入组件库）
   ↓
第五步：套用视觉样式（design-md → CSS）
   ↓
第六步：完整性 + 风格 + 内容自查
```

---

## 第一步：解析原文骨架

读完原文后，**先输出结构清单**，再做后续。这一步杜绝"漏内容"——所有 H2 必须有归宿。

输出格式：

```
H1: {{文档标题}}
├─ H2: {{section 标题 1}} (内容形态：N 条 list / 段落 / 表格 / 等)
├─ H2: {{section 标题 2}} (...)
├─ H2: {{section 标题 3}} (...)
...
```

例（卡兹克写作 SKILL）：

```
H1: 卡兹克公众号长文写作
├─ TL;DR: 有见识的普通人在认真聊一件打动他的事
├─ H2: 核心价值观 (4 条 list)
├─ H2: 第一步 理解素材与选题判断 (HKR 三项 + 3 段说明)
├─ H2: 第二步 明确 AI 角色边界 (擅长 vs 必须人来 + 流程图)
├─ H2: 第三步 写作 (5 原型 + 风格内核 16+ 条 + 7 类禁区 + 推荐口语化 6 类词组)
├─ H2: 第四步 四层自检 (L1/L2/L3/L4 各含 4-7 子项)
└─ H2: 参考资料 (引用)
```

---

## 第二步：组件映射

针对每个 H2（或重要 H3），按下表选择**唯一**对应的组件。LLM 不允许凭感觉选——表里有的必须用表里的。

| 原文模式 | 选哪个组件 |
|---------|-----------|
| 标题含「原则 / 理念 / 价值观 / 心得 / N 条」+ 并列项 | `principles-list` |
| 标题含「步骤 / 流程 / 方法 / N 步 / 如何 X」+ 顺序项 | `numbered-steps` |
| 标题含「N 种 / N 类 / N 个原型」+ 命名分类 | `typology` |
| 标题含「自检 / 清单 / 检查 / checklist」 | `checklist` |
| 标题含「禁区 / 避免 / 不要 / 反模式 / AVOID」 | `warning-block` |
| 标题含「vs / 对比 / 这样 vs 那样 / 推荐 vs 反对」 | `compare-2col` |
| 突出的金句、关键引用 | `quote-block` |
| 突出的数字、效果指标 | `key-stats` |
| 标题含「推荐用语 / 术语 / 高频词 / Lexicon」+ 分组词组 | `phrase-bank` |
| 整体性概述 / 摘要 / TL;DR | `tldr` |

### 映射输出

为每个 H2 输出"组件清单"：

```
§01 → tldr (源：开篇定义)
§02 → principles-list (源：核心价值观，4 条)
§03 → numbered-steps (源：4 步流程)
§04 → typology (源：5 种文章原型)
§05 → principles-list (源：风格内核 16+ 条)
§06 → warning-block (源：7 类绝对禁区)
§07 → phrase-bank (源：推荐口语化词组，6 类)
§08 → checklist (源：L1-L4 四层自检)
```

### 模糊情况的兜底

- **完全不匹配任何组件** → 用最接近的，或拆为两段
- **同一 H2 包含多种结构**（如"5 原型 + 风格内核 16 条" 在同一 H2 下）→ **拆成多个 section**，分别用对应组件
- **段落式纯文本** → 短的合并到上一个 section 的 intro，长的自成一个 `tldr` section

---

## 第三步：内容提炼（按 depth 决定深度）

### `--depth overview`（默认）
- 每个组件保留 3-5 个代表项
- 每项描述 ≤ 1 句话
- 总图高度 1500-3000px
- hero 的 subtitle 应注明"OVERVIEW"或"摘要"

### `--depth full`
- **所有子项必须出现**（不许漏）
- 每项描述展开（principles-list ≤ 150 字、numbered-steps ≤ 250 字、typology ≤ 200 字 等）
- 总图高度 3000-8000px+
- hero 的 subtitle 应注明"FULL"或"完整"

---

## 第四步：HTML 组装

按"组件清单"顺序，从 `templates/structure-map-components.md` 取每个组件的 HTML 骨架，填入提炼后的内容。

### 顺序原则

1. **hero**（必含）
2. **tldr**（如有，紧跟 hero 之后）
3. 内容主体（按原文逻辑顺序排列各组件）
4. **footer**（如有）

### 编号规则

每个 section-overline 自动 `§01 §02 ...` 编号（用 CSS counter）。tldr 通常不参与编号。

---

## 第五步：画布契约（强制）

### 绝对禁令（跨模板共性，违反必导致底部留白）

- ❌ **禁止** `body`、`html`、`main`、`#root` 任一元素使用：
  - `min-height: 100vh` / `min-height: 100%`
  - `height: 100vh` / `height: 100%` / `height: <任何固定值>`
  - `display: flex` + `justify-content: space-between` 垂直撑开
  - 任何 ancestor 元素的 `min-height` 超过自身内容高度
- ❌ **禁止** body 设置任何固定 `height`——body 高度**必须**由内容决定
- ❌ **禁止** hero `padding-top < 48px`；source 到 h1 视觉间距 `< 24px`
- ❌ **禁止** 最后一个 section 之后插入任何"撑开高度"的占位 div

> 这些规则优先于任何 design-md 的提示。结构图可能很长，只要违反一条就会在底部出现大块留白。

### 标准约束

| 项 | 约束 |
|----|------|
| 画布宽度 | `body { width: 1080px; margin: 0 auto; }` |
| 画布高度 | `body { height: auto; }`（或完全不设 height）——**由内容决定** |
| 左右安全区 | `body { padding-left: 64px; padding-right: 64px; }` |
| 顶部 padding | `body { padding-top: 56px; }` |
| 底部 padding | `body { padding-bottom: 56px; }` |
| Hero padding | hero 本身 `padding-top ≥ 48px`，source 到 h1 `≥ 24px`，h1 到 subtitle `≥ 16px` |
| Section 垂直间距 | 相邻 section 之间 `margin-top: 64px`（可放大到 80px 给 huasheng 类风格） |
| 子元素继承 | 所有组件不再单独设置左右 padding，继承 body safe area |
| 缺失字段 | 任何组件的字段为空，整个组件直接省略 |
| 图片底部 | 最后一个组件结束后即结束（仅留 body padding-bottom），无大块空白 |

---

## 第六步：套用视觉样式

读 `designs/<style>.md`，按其色彩、字体、间距、组件风格生成 CSS。

### 跨组件统一规则

- 所有 section-header 用同一字号、字重、颜色
- 所有 section-overline 用同一字号、字重、品牌色
- 品牌色只用在：overline、header accent、stat-value、principle-name、step-number、warning-icon、phrase-tag 边框、quote left-border
- 中性色 4 档：primary text / secondary text / tertiary text / border
- 卡片背景：白底 + 1px border（浅色风格）或深底 + 透明 border（深色风格）

### 视觉适配按 design 系列

- **深色系 design**（ferrari、bmw、runwayml）：背景深色，所有组件白字 + 弱边框
- **浅色系 design**（huasheng、notion、mintlify）：背景白/暖白，组件白卡 + 1px 浅边框，**禁止渐变**（除非 design-md 明确允许）
- **混合系 design**（claude、sanity）：hero 深底 + 正文浅底

---

## 第七步：完整性自查（structure-map 特有，**最关键**）

生成完成后，对照"组件清单"逐项核验：

- [ ] **原文每个 H2 是否都对应了一个组件？** 列表里有多少 H2，HTML 里就该有多少个 section
- [ ] **是否有 H2 被遗漏或合并到其他 section？** 不允许
- [ ] full 模式下：每个组件的子项数量是否等于原文子项数量？
- [ ] overview 模式下：每个组件保留 3-5 个代表项，且 subtitle 注明"OVERVIEW"
- [ ] hero 是否包含完整的标题和 subtitle？

## 组件正确性自查

- [ ] "禁区"是否用了 `warning-block` 而**不是** principles-list？
- [ ] "步骤"是否用了 `numbered-steps` 而**不是** principles-list？
- [ ] "对比"是否用了 `compare-2col` 而**不是**两个独立 list？
- [ ] "N 种类型"是否用了 `typology`（带描述）而**不是** phrase-bank（只有名称）？
- [ ] "词组分组"是否用了 `phrase-bank` 而**不是** principles-list？
- [ ] "checklist"是否用了 `checklist` 而**不是** principles-list？

## 空间自查

- [ ] body 是否固定宽度 1080px 并居中？
- [ ] body 是否有 ≥ 40px 的左右 padding？
- [ ] 所有组件是否在 safe area 内（未贴边）？
- [ ] 相邻 section 之间是否 ≥ 56px 垂直间距？
- [ ] 是否存在空白组件占位（有 section 但内容为空）？如有，必须删除整个 section
- [ ] 图片底部是否紧贴最后一个 section（仅留 body padding-bottom）、无大块空白？

## 风格自查

- [ ] 读 design-md 第 7 节 "Do's and Don'ts"，逐条对照
- [ ] 读 design-md 第 1 节 "Key Characteristics"，确认风格"身份标志"是否复现
- [ ] 各 section-header 视觉是否一致（同字号同字重同颜色）？
- [ ] 各 section-overline 视觉是否一致？
- [ ] §XX 编号是否完整且按顺序？

## 换行自查（字数约束验证）

- [ ] 参考 `structure-map-components.md` 顶部的「共享文字约束」，所有「1 行」字段的 CSS 是否都声明了 `white-space: nowrap; overflow: hidden; text-overflow: ellipsis`？
- [ ] 每个组件的"单行字段"（principle-name / step-name / typology-name / warning-name / stat-value / tag 等）是否严格单行？
- [ ] 有没有 `…` 省略号出现？如有 → 回到第三步内容提炼**缩字**，不要加宽容器
- [ ] 段落字段（principle-desc / step-desc / typology-desc 等）行数是否合理（overview ≤ 2 行，full ≤ 5 行）？

## 内容自查

- [ ] 每个组件遵循其 components.md 的字段约束（字数、条数、必填字段）？
- [ ] **是否误用为内容凝练？** 如果原文是文章/笔记（内容只有 5-6 个点），停止 → 提示用户改用 `visual-card`
- [ ] **是否误用为营销海报？** 如果原文是活动/产品 brief（含 CTA、价格），停止 → 提示用户改用 `poster`

---

## 失败模式与修复

| 失败 | 原因 | 修复 |
|------|------|------|
| 整图都是 principles-list | LLM 偷懒没按映射表选 | 重新做映射，强制按"原文模式 → 组件"表 |
| 漏掉部分 H2 | 第一步骨架提取没做或做错 | 重新做骨架提取，列出所有 H2 |
| overview 把所有内容塞下了，没体现"摘要" | depth 参数没遵守 | 重新提炼，每组件保留 3-5 项 |
| full 模式只覆盖了 60% 内容 | LLM 自行取舍 | 强制原文每个子项必须出现 |
| 各 section 视觉风格不统一 | 没按 design-md 跨组件统一规则生成 CSS | 重新生成 CSS，每类元素同一规则 |
| 组件之间有大块空白 | 第五步画布契约没遵守 | 检查 margin-top: 64px |
| 警告区视觉跟普通区一样 | warning-block 没用红色调 | 检查第五步样式映射 |
