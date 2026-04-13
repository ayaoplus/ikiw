# 结构图：横版（Landscape）

固定宽度 1280px 的横向长图，高度自适应。**采用 2 列布局**，把组件横向排开，适合桌面浏览、A4 横向打印、PPT 多页拼接。

> 大多数结构图场景请用竖版（→ `structure-map-portrait.md`）。横版用于：A4 横向打印、桌面大屏阅读、PPT 多页拼接。
> **横版默认使用 `--depth overview`**（每节点 3-5 项摘要），因为 full 模式在横版下信息密度过高。

---

## 工作流

与 portrait 完全一致：解析骨架 → 组件映射 → 内容提炼 → HTML 组装 → 套样式 → 自查。
区别只在**画布契约**和**HTML 组装顺序**（双列分布）。

详细规则参考 [`structure-map-portrait.md`](structure-map-portrait.md) 第一至第三步。

---

## 画布契约（强制）

| 项 | 约束 |
|----|------|
| 画布宽度 | `body { width: 1280px; margin: 0 auto; }` |
| 左右安全区 | `body { padding-left: 56px; padding-right: 56px; }` |
| 顶部 padding | `body { padding-top: 56px; }` |
| 底部 padding | `body { padding-bottom: 56px; }` |
| Hero | 全宽（横跨 2 列） |
| TL;DR | 全宽（横跨 2 列） |
| 主体 | 双列网格 `display: grid; grid-template-columns: 1fr 1fr; gap: 32px;` |
| Footer | 全宽 |
| Section 垂直间距 | 同列内组件间 `margin-top: 56px`；列与列之间靠 grid gap 控制 |
| 组件溢出 | 单个组件不允许跨列；如内容过多，拆分为两个 section 各占一列 |

---

## HTML 组装顺序

```
┌─────────────────────────────────────────┐
│  hero（全宽）                              │
├─────────────────────────────────────────┤
│  tldr（全宽，可选）                         │
├──────────────────┬──────────────────────┤
│  §01 组件         │  §02 组件             │
├──────────────────┼──────────────────────┤
│  §03 组件         │  §04 组件             │
├──────────────────┼──────────────────────┤
│  §05 组件         │  §06 组件             │
├──────────────────┼──────────────────────┤
│  §07 组件         │  §08 组件             │
├─────────────────────────────────────────┤
│  footer（全宽，可选）                       │
└─────────────────────────────────────────┘
```

### 组件分配规则

- **单列宽组件**（默认）：principles-list、numbered-steps、typology、warning-block、key-stats、quote-block、phrase-bank（每组一行 chip）
- **必须全宽组件**：hero、tldr、footer
- **可全宽也可单列**：compare-2col（建议全宽以保证两栏空间）、checklist（如分组多则全宽）

### 组件高度平衡

横版 2 列布局会出现"左列高右列空"或反之的情况。处理：
- 默认按 §XX 顺序左 → 右 → 左 → 右 ...
- 如果某 section 明显比邻居高，调整顺序让两列高度大致平衡
- 不允许某列空白超过 100px

---

## HTML 骨架

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>{{title}}</title>
  <style>/* 根据 design-md 生成 */</style>
</head>
<body>

  <header class="hero">
    <div class="source">{{source}}</div>
    <h1>{{title}}</h1>
    <div class="subtitle">{{subtitle}}</div>
  </header>

  <!-- TL;DR 全宽 -->
  <section class="section tldr-section full-width">
    <div class="section-overline">TL;DR</div>
    <div class="tldr-content">{{summary}}</div>
  </section>

  <!-- 主体 2 列网格 -->
  <main class="map-grid">
    <!-- 组件按 §01 §02 §03 ... 顺序填入，CSS 自动 2 列分布 -->
    <section class="section">...</section>
    <section class="section">...</section>
    <!-- ... -->
  </main>

  <footer class="footer">...</footer>

</body>
</html>
```

CSS 关键：

```css
.hero { width: 100%; }
.full-width { grid-column: 1 / -1; }
.map-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 32px 40px;  /* row-gap 32, column-gap 40 */
}
.map-grid .section { margin-top: 0; }  /* gap 已经处理间距 */
```

---

## 套用视觉样式

完全复用 portrait 的样式映射规则（参见 [structure-map-portrait.md](structure-map-portrait.md) 第六步）。横版下：

- 字号比 portrait 小一档（h1: 48px 而非 56px；section-header: 22px 而非 26px）
- 卡片间距更紧凑（gap 12px 而非 16px）
- compare-2col 优先全宽，让两栏有足够空间

---

## 完整性 + 风格 + 内容自查

完全复用 portrait 的自查清单（含换行自查）。**横版特有补充**：

- [ ] 是否使用了 2 列网格布局（`grid-template-columns: 1fr 1fr`）？
- [ ] hero / tldr / footer 是否全宽（横跨 2 列）？
- [ ] 单个 section 是否在单列宽度内（除明确全宽的）？
- [ ] 左右两列高度差是否 ≤ 100px？
- [ ] 横版下是否使用了 `--depth overview`（不建议 full）？
- [ ] 字号是否按横版规则缩小一档？
- [ ] **横版列宽只有竖版的 45%**，所有单行字段的字数**再砍 30%**，避免省略号：principle-name ≤ 8 字、step-name ≤ 8 字、typology-name ≤ 8 字、warning-name ≤ 8 字、compare-col-title ≤ 6 字
