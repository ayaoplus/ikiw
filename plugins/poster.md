# 海报（Poster）

> **专为活动 / 产品 / 课程 / 发布会的营销宣传设计的竖版海报。** 含品牌、卖点、价格、CTA、报名信息等营销字段。
>
> 不适用场景：
> - 文章 / 笔记的内容凝练 → 用 `visual-card` 插件
> - 方法论 / 教程 / 长文的完整结构图 → 用 `structure-map` 插件

## 命令

```
ikiw poster "内容或文件路径" --design <style>           生成海报（默认竖版）
ikiw poster "内容或文件路径" --design all              用所有可用样式各生成一版
```

## 触发词

- **海报类**："做张海报"、"出张海报"、"生成海报"、"做个宣传图"、"做张活动图"
- **场景类**："发布会海报"、"训练营宣传图"、"课程报名图"、"读书会海报"、"活动海报"、"产品发布图"
- **特征类**："带二维码的图"、"带价格的宣传图"、"含报名信息的海报"
- **指定风格**："用 XX 风格做张海报"

英文触发词："make a poster"、"event poster"、"product launch poster"、"promo image"

> 默认输出**竖版**（最常见的海报形态）。海报本质是营销物，与 `visual-card` 的"内容凝练"在内容、字段、视觉重心上完全不同。

### 不属于本插件的触发场景（要分流出去）

| 用户说 | 应该走哪个插件 |
|--------|---------------|
| "把这篇文章做成图"、"做张要点卡"、"金句卡"、"分享图" | `visual-card`（内容凝练） |
| "把这份方法论可视化"、"涵盖所有内容的图"、"完整结构图" | `structure-map`（结构图） |

### Agent 主动判断

如果用户说"做个图"但内容来源是一篇普通文章（非活动 brief / 非产品介绍），agent 应主动询问："这是要做营销海报，还是做文章核心要点的视觉卡片？"——避免硬塞内容。

## 依赖

- `designs/` — 视觉样式库（design-md 文件）
- `templates/poster-portrait.md` — 竖版海报规范（画布契约 + HTML 骨架 + 自查清单）
- `screenshot-export` 插件 — 用于导出为 PNG / PDF

## 执行流程

1. **判断内容类型**：确认原文是营销 brief（活动、产品、课程）。如果是文章/笔记，提示用户改用 `visual-card`
2. **加载规范**：读取 `templates/poster-portrait.md`
3. **内容提炼**：按规范定义的 YAML 营销字段提取信息（brand / hero / stats / features / audience / timeline / price / cta / speakers / footer）
4. **HTML 生成**：按规范的画布契约和 HTML 骨架生成
5. **套用视觉样式**：读 `designs/<style>.md`，按规范的样式映射规则生成 CSS
6. **自查**：对照规范的自查清单逐条核验，不通过则重新生成
7. **导出图片**：调用 screenshot-export 插件导出为 3x 高清 PNG

## 关键约定

- 海报**必含**字段：brand、title、subtitle、至少一个内容区块（features 或 stats）
- 海报**强营销字段**（按需）：price、cta、deadline、speakers
- 字段缺失时该区块整体省略，禁止留空白占位
- 画布契约**优先于** design-md 中的网页 padding 规范
- 自查清单是强制流程
