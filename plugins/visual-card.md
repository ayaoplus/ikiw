# 视觉卡片（Visual Card）

> **把一篇文章/笔记的核心观点凝练成一张可分享的卡片。** 极简、聚焦、社交友好——只讲核心 3-5 个点，不讲完整骨架。
>
> 不适用场景：
> - 活动/产品宣传 → 用 `poster` 插件
> - 完整结构知识图、方法论全景 → 用 `structure-map` 插件

## 命令

```
ikiw card "内容或文件路径" --design <style>                    生成竖版（默认）
ikiw card "内容或文件路径" --layout portrait --design <style>   竖版长图
ikiw card "内容或文件路径" --layout landscape --design <style>  横版单页
ikiw card "内容或文件路径" --design all                        用所有可用样式各生成一版
```

## 触发词

绝大多数用户不会使用"视觉卡片"这个准确术语。agent 应识别以下口语化表达并触发本插件：

- **图片类**："把这篇做成图"、"配张图"、"画张图"、"做个图发朋友圈"
- **卡片类**："做张卡片"、"出张卡片"、"做张视觉卡片"、"金句卡"、"要点卡"
- **图文类**："做个分享图"、"配个封面图"
- **指定版式**："竖版卡片/图"（→ portrait）、"横版卡片/图""封面图"（→ landscape）
- **指定风格**："用 XX 风格出张卡片"、"做个 notion 风的封面"

英文触发词："make a card"、"visual card"、"share card"、"key takeaways card"

> 默认输出**竖版**（portrait），除非用户明确说"横版""封面图""一屏"等。

### 不属于本插件的触发场景（要分流出去）

| 用户说 | 应该走哪个插件 |
|--------|---------------|
| "做张活动海报"、"产品发布海报"、"训练营报名图" | `poster`（营销海报） |
| "把这篇方法论做成完整知识图"、"全景结构图"、"涵盖所有内容的图" | `structure-map`（结构图） |
| "把整本书做成图"、"把这份 SKILL 文档完整可视化" | `structure-map` |

## 何时用哪个版式

| 场景 | 推荐版式 |
|------|----------|
| 微信朋友圈、小红书、即刻 | portrait |
| Twitter/X 配图、博客封面、PPT 嵌入、Slack 分享 | landscape |
| 一篇文章浓缩为 4-6 个要点 | portrait |
| 单一观点、数据卡、金句、对比 | landscape |
| 默认 | portrait |

## 依赖

- `designs/` — 视觉样式库（design-md 文件）
- `templates/visual-card-portrait.md` — 竖版规范（画布契约 + HTML 骨架 + 自查清单）
- `templates/visual-card-landscape.md` — 横版规范
- `screenshot-export` 插件 — 用于导出为 PNG/PDF

## 执行流程

1. **判断版式**
   - 用户显式指定 `--layout` → 直接用
   - 未指定 → 按上面的「何时用哪个版式」表自动判断；不确定时默认 portrait
2. **加载规范**：读取对应的 `templates/visual-card-{layout}.md`
3. **内容提炼**：按规范定义的 YAML 结构提取信息
4. **HTML 生成**：按规范的画布契约和 HTML 骨架生成
5. **套用视觉样式**：读 `designs/<style>.md`，按规范的样式映射规则生成 CSS
6. **自查**：对照规范的自查清单逐条核验，不通过则重新生成
7. **导出图片**：调用 screenshot-export 插件导出为 3x 高清 PNG

## 关键约定

- 两种版式共享同一套 design-md 视觉风格，但各自有独立的画布契约和 HTML 骨架
- 换风格只换 CSS，不换 HTML 结构
- 画布契约**优先于** design-md 中的网页 padding 规范——design-md 描述网页文档，视觉卡片是固定画布
- 自查清单是强制流程，不允许带瑕疵交付
