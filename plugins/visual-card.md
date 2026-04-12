# 视觉卡片（Visual Card）

> 把一段内容（文章、活动、产品、笔记）浓缩为一张视觉化总结。两种版式：**竖版长图**（适合社交分享）+ **横版单页**（适合截图嵌入）。

## 命令

```
ikiw card "内容或文件路径" --design <style>                    生成竖版（默认）
ikiw card "内容或文件路径" --layout portrait --design <style>   竖版长图
ikiw card "内容或文件路径" --layout landscape --design <style>  横版单页
ikiw card "内容或文件路径" --design all                        用所有可用样式各生成一版
```

## 触发词

"做张视觉卡片"、"出张卡片"、"把这篇做成卡片"、"竖版卡片"、"横版卡片"、"用 XX 风格做卡片"

## 何时用哪个版式

| 场景 | 推荐版式 |
|------|----------|
| 微信朋友圈、小红书、即刻、公众号封面 | portrait |
| Twitter/X 配图、博客封面、PPT 嵌入、Slack 分享 | landscape |
| 完整流程、时间线、教程、长摘要 | portrait |
| 单一观点、数据卡、对比、金句、亮点摘要 | landscape |
| 用户没指定，且内容信息量大 | portrait |
| 用户没指定，且内容只有一两个核心点 | landscape |

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
