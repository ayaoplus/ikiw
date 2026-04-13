# 字体加载指南（Font Loading Guide）

> 所有 visual-card / poster / structure-map 输出的 HTML 都要引用本文件。生成 HTML 时 **`<head>` 必须包含 Google Fonts `<link>` 标签**，否则 design-md 指定的字体不会被浏览器加载，视觉会偏离预期。

---

## 核心规则（共 5 条）

1. **font-family 只是请求，`<link>` 才是加载**
   在 CSS 里写 `font-family: 'Inter'` 只说明"我想用 Inter"。浏览器能不能真的用上 Inter，**取决于 `<head>` 里有没有对应的 `<link>` 加载声明**。如果没有，字体会静默回退到系统默认（Arial / Helvetica / 中文字体等）。

2. **所有 HTML 输出，`<head>` 必须包含 Google Fonts `<link>`**
   例外：全部用系统字体时可以不加（`-apple-system`、`system-ui`、`PingFang SC` 等）。

3. **字重必须完整匹配 design-md 声明**
   如果 design-md 使用 weight 900，`<link>` 里必须加载 `wght@...900`。否则浏览器会用 **faux bold 模拟**，视觉效果丑陋。

4. **中文内容必须加载 `Noto Sans SC`**
   design-md 的 fallback 链一般会有 `-apple-system` 或 `PingFang SC`，但 Linux/Docker 环境下这些不存在，中文会回退到 DejaVu/Liberation（不支持 CJK），显示为方块。统一加载 Noto Sans SC 最保险。

5. **专有字体不要尝试 Google Fonts 加载**
   Anthropic、SF Pro、Helvetica Neue、Apple Color Emoji 这类专有字体**不在 Google Fonts 目录**。**强行 `<link>` 加载会 404**。使用下面的「近似替代表」选 Google Fonts 里视觉最接近的字体。

---

## 标准加载模板

```html
<head>
  <meta charset="UTF-8">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=<Font1>:wght@<w1>;<w2>&family=<Font2>:wght@<w1>;<w2>&display=swap" rel="stylesheet">
</head>
```

- 字体名中的空格用 `+` 替代：`DM Sans` → `DM+Sans`
- 多字体合并为一个 `<link>` 更高效：用 `&family=` 串联
- `display=swap` 让字体加载期间先用 fallback（避免白屏）

---

## 速查表：每个 design-md 的加载字符串

| design-md | 主字体（推荐） | Google Fonts `<link>` 加载串 |
|---|---|---|
| **airbnb** | Inter + Noto Sans SC（替代 Airbnb Cereal） | `Inter:wght@400;500;600;700&family=Noto+Sans+SC:wght@400;500;700` |
| **airtable** | Inter + Noto Sans SC（替代 Haas） | `Inter:wght@400;500;600;700&family=Noto+Sans+SC:wght@400;500;700` |
| **apple** | Inter + Noto Sans SC（替代 SF Pro） | `Inter:wght@400;500;600;700&family=Noto+Sans+SC:wght@400;500;700` |
| **binance** | Inter + Noto Sans SC | `Inter:wght@400;500;600;700&family=Noto+Sans+SC:wght@400;500;700` |
| **bmw** | Oswald + Noto Sans SC（替代 BMWTypeNext Light） | `Oswald:wght@300;400;500&family=Noto+Sans+SC:wght@300;400;500` |
| **claude** | Fraunces + Inter + JetBrains Mono + Noto Serif SC（替代 Anthropic Serif/Sans/Mono） | `Fraunces:wght@400;500;600&family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@400&family=Noto+Serif+SC:wght@400;500;600` |
| **elevenlabs** | Inter + Noto Sans SC（替代 Waldenburg） | `Inter:wght@400;500;600;700&family=Noto+Sans+SC:wght@400;500;700` |
| **ferrari** | Inter + Noto Sans SC（替代 FerrariSans，一般深色 hero 用粗体） | `Inter:wght@400;500;600;700;900&family=Noto+Sans+SC:wght@400;500;700;900` |
| **hashicorp** | Inter + Noto Sans SC（替代 HashiCorp Sans） | `Inter:wght@400;500;600;700&family=Noto+Sans+SC:wght@400;500;700` |
| **huasheng** | Inter + JetBrains Mono + Noto Sans SC | `Inter:wght@400;500;700;900&family=Noto+Sans+SC:wght@300;400;500;700;900&family=JetBrains+Mono:wght@400` |
| **lamborghini** | Open Sans + Noto Sans SC（替代 LamboType） | `Open+Sans:wght@300;400;500;700;800&family=Noto+Sans+SC:wght@400;500;700` |
| **lovable** | Plus Jakarta Sans + Noto Sans SC（替代 Camera Plain） | `Plus+Jakarta+Sans:wght@400;500;600;700&family=Noto+Sans+SC:wght@400;500;700` |
| **meta** | Inter + Noto Sans SC | `Inter:wght@400;500;600;700&family=Noto+Sans+SC:wght@400;500;700` |
| **minimax** | DM Sans + Outfit + Poppins + Noto Sans SC | `DM+Sans:wght@400;500;600;700&family=Outfit:wght@400;500;600;700&family=Poppins:wght@400;500;600&family=Noto+Sans+SC:wght@400;500;700` |
| **mintlify** | Inter + Geist Mono + Noto Sans SC | `Inter:wght@400;500;600;700&family=Geist+Mono:wght@400;500&family=Noto+Sans+SC:wght@400;500;700` |
| **mongodb** | Source Serif 4 + Source Code Pro + Noto Serif SC（替代 MongoDB Value Serif） | `Source+Serif+4:wght@400;500;600;700&family=Source+Code+Pro:wght@400;500;600&family=Noto+Serif+SC:wght@400;500;700` |
| **notion** | Inter + Noto Sans SC（NotionInter 本质是 Inter） | `Inter:wght@400;500;600;700&family=Noto+Sans+SC:wght@400;500;700` |
| **pinterest** | Inter + Noto Sans SC（替代 Pin Sans） | `Inter:wght@400;500;600&family=Noto+Sans+SC:wght@400;500;700` |
| **posthog** | IBM Plex Sans + Source Code Pro + Noto Sans SC | `IBM+Plex+Sans:wght@100;300;400;500;600;700&family=Source+Code+Pro:wght@400;500&family=Noto+Sans+SC:wght@400;500;700` |
| **raycast** | Inter + Geist Mono + Noto Sans SC（SF Pro → Inter） | `Inter:wght@400;500;600;700&family=Geist+Mono:wght@400;500&family=Noto+Sans+SC:wght@400;500;700` |
| **replicate** | Manrope + Inter + JetBrains Mono + Noto Sans SC（替代 freigeist/basier-square） | `Manrope:wght@400;500;600;700&family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@400&family=Noto+Sans+SC:wght@400;500;700` |
| **runwayml** | Inter + Noto Sans SC（替代 abcNormal） | `Inter:wght@400;500;600;700&family=Noto+Sans+SC:wght@400;500;700` |
| **sanity** | Inter + IBM Plex Mono + Noto Sans SC（替代 waldenburg） | `Inter:wght@400;500;600;700&family=IBM+Plex+Mono:wght@400;500&family=Noto+Sans+SC:wght@400;500;700` |
| **sentry** | Archivo Black + Rubik + Noto Sans SC（替代 Dammit Sans；Monaco 是系统字体） | `Archivo+Black&family=Rubik:wght@400;500;600;700&family=Noto+Sans+SC:wght@400;500;700` |
| **shopify** | Inter + Noto Sans SC（替代 Helvetica Neue） | `Inter:wght@300;400;500;600;700&family=Noto+Sans+SC:wght@400;500;700` |
| **supabase** | Inter + Source Code Pro + Noto Sans SC（替代 Circular） | `Inter:wght@400;500;600;700&family=Source+Code+Pro:wght@400;500&family=Noto+Sans+SC:wght@400;500;700` |
| **superhuman** | Inter + Noto Sans SC（替代 Super Sans VF） | `Inter:wght@300;400;500;600;700&family=Noto+Sans+SC:wght@400;500;700` |
| **tesla** | Inter + Noto Sans SC（替代 Universal Sans） | `Inter:wght@300;400;500;600;700&family=Noto+Sans+SC:wght@400;500;700` |
| **vercel** | Geist + Geist Mono + Noto Sans SC | `Geist:wght@400;500;600;700;900&family=Geist+Mono:wght@400;500&family=Noto+Sans+SC:wght@400;500;700;900` |
| **voltagent** | Inter + Noto Sans SC | `Inter:wght@400;500;600;700&family=Noto+Sans+SC:wght@400;500;700` |
| **warp** | DM Sans + Inter + Geist Mono + Noto Sans SC（替代 Matter） | `DM+Sans:wght@400;500;600;700&family=Inter:wght@400;500&family=Geist+Mono:wght@400;500&family=Noto+Sans+SC:wght@400;500;700` |
| **wise** | Inter + Noto Sans SC（替代 Wise Sans） | `Inter:wght@400;500;600;700&family=Noto+Sans+SC:wght@400;500;700` |
| **x.ai** | Inter + Geist Mono + Noto Sans SC（替代 universalSans） | `Inter:wght@400;500;600;700&family=Geist+Mono:wght@400;500&family=Noto+Sans+SC:wght@400;500;700` |
| **zapier** | Archivo Black + Inter + Fraunces + Noto Sans SC（替代 Degular Display + GT Alpina） | `Archivo+Black&family=Inter:wght@400;500;600;700&family=Fraunces:wght@400;500&family=Noto+Sans+SC:wght@400;500;700` |

---

## 专有字体 → Google Fonts 近似替代

开源项目（MIT License）**不能**打包任何盗版/受限字体文件。商业字体需要用户自行购买授权。我们提供视觉上最接近的 Google Fonts 开源替代：

| 专有字体 | 最佳 Google Fonts 替代 | 为什么 |
|---|---|---|
| Anthropic Serif | **Fraunces** (weight 500) 或 Source Serif 4 | 同为暖色调 serif，字形饱满 |
| Anthropic Sans | **Inter** 或 Manrope | 同为现代 humanist sans |
| Anthropic Mono | **JetBrains Mono** | 开源、易读 |
| SF Pro Display / SF Pro Text | **Inter** | Inter 本身就是受 SF Pro 启发设计 |
| Helvetica Neue | **Inter** 或 Manrope | Inter 是最接近的开源替代 |
| NotionInter | **Inter** | NotionInter 就是 Inter 的修改版 |
| Geist / Geist Mono | 直接用 **Geist / Geist Mono**（Google Fonts 已收录） | 原字体可用 |
| Airbnb Cereal | **Inter** | 同为 humanist sans |
| Haas Grot | **Inter** | 同为 neo-grotesque |
| BMWTypeNext Light | **Oswald** (weight 300) | 窄字身、light weight |
| FerrariSans | **Inter** (weight 500-900) | neo-grotesque，类似 |
| Pin Sans | **Inter** | 工业感 sans |
| HashiCorp Sans | **Inter** | humanist，可互换 |
| Super Sans VF | **Inter** (variable) | variable 字重、humanist |
| Universal Sans (Tesla) | **Inter** | 工业感 geometric |
| Matter | **DM Sans** | 同为 geometric-rounded sans |
| LamboType | **Oswald** 或 **Open Sans** | LamboType 有 condensed 变体，Oswald 最接近 |
| Camera Plain Variable (Lovable) | **Plus Jakarta Sans** 或 **Manrope** | 温暖感 humanist |
| Dammit Sans (Sentry) | **Archivo Black** 或 **Bungee** | 粗体 display 字体 |
| Waldenburg | **Inter** | neo-grotesque |
| Circular | **Inter** 或 Manrope | humanist geometric |
| abcNormal (Runway) | **Inter** | neutral neo-grotesque |
| Wise Sans | **Inter** | Wise Sans 本身就类似 Inter |
| Degular Display (Zapier) | **Archivo Black** | 宽身 display |
| GT Alpina (Zapier) | **Fraunces** | thin-weight 现代 serif |
| universalSans (x.ai) | **Inter** | neo-grotesque |
| MongoDB Value Serif | **Source Serif 4** | 高对比度 serif，适合 headline |
| Dammit Sans | **Archivo Black** | 粗体 display |
| rb-freigeist-neue / basier-square (Replicate) | **Manrope** + **Inter** | 分别对应 display + body |

> **中文 serif** 对应 Noto Serif SC（对应 claude 的 Anthropic Serif 中文部分）；中文 sans 对应 Noto Sans SC（默认）

---

## 示例：完整 HTML `<head>`

以 **minimax** 风格为例：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>...</title>

  <!-- 字体加载（必加） -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600;700&family=Outfit:wght@400;500;600;700&family=Poppins:wght@400;500;600&family=Noto+Sans+SC:wght@400;500;700&display=swap" rel="stylesheet">

  <style>
    body {
      font-family: 'DM Sans', 'Noto Sans SC', -apple-system, "PingFang SC", sans-serif;
      /* ... */
    }
    h1 {
      font-family: 'Outfit', 'Noto Sans SC', sans-serif;
      font-weight: 600;  /* 必须在 <link> 加载的字重范围内 */
    }
  </style>
</head>
```

---

## 如果用户自己购买了商业字体（自托管）

少数高级用户可能合法购买了 Airbnb Cereal、Haas 等商业字体的 Web 授权。支持机制：

1. 把字体文件（.woff2）放到知识库根目录的 `fonts/` 子目录（不在 ikiw skill 仓库，而在**用户的知识库**里）
2. agent 检测到 `fonts/` 存在时，用 `@font-face` 规则自托管：

```css
@font-face {
  font-family: 'Airbnb Cereal';
  src: url('fonts/AirbnbCereal-Book.woff2') format('woff2');
  font-weight: 400;
  font-style: normal;
}
```

3. ikiw 仓库**永远不**打包字体文件——这是法律/版权边界

---

## 自查清单

- [ ] HTML `<head>` 包含 Google Fonts `<link>` 标签
- [ ] `<link>` 里加载的字重覆盖 design-md 声明的所有字重（特别检查 weight 900、weight 300 这些边缘值）
- [ ] 中文内容有 `Noto Sans SC`（或 `Noto Serif SC` for claude/mongodb 这类 serif 风格）
- [ ] 没有尝试加载 Anthropic Serif / SF Pro / Helvetica Neue / Circular 等专有字体（如 design-md 指定，**只用其 fallback 或本文件的替代**）
- [ ] `font-family` 栈以 Google Fonts 字体开头，以系统字体 + `sans-serif` / `serif` 结尾
