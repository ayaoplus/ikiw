# 海报生成

> 从原始内容自动提炼结构化信息，按统一骨架 + 可换视觉样式生成海报。

## 命令

```
ikiw poster "内容或文件路径" --design <style>       生成指定风格海报
ikiw poster "内容或文件路径" --design all            用所有可用样式各生成一版
```

## 触发词

"生成海报"、"做个海报"、"出一版海报"、"用 XX 风格做海报"

## 依赖

- `designs/` — 视觉样式文件
- `templates/poster.md` — 海报结构模板（内容提炼规则 + HTML 骨架 + 样式映射）
- `screenshot-export` 插件 — 用于导出为 PNG

## 执行流程

1. 读取 `templates/poster.md` 获取结构模板
2. **内容提炼**：从原始内容中提取标准化 YAML 结构（brand、title、highlights、tags、timeline、footer）
3. **HTML 生成**：按模板定义的统一骨架和 CSS 类名约定生成 HTML
4. **套用视觉样式**：读取 `designs/<style>.md`，按样式映射规则生成 CSS
5. **导出图片**：调用 screenshot-export 插件导出为 3x 高清 PNG

## 关键约定

- 所有海报使用相同的 HTML 类名（`.hero`、`.badge`、`.highlight-item`、`.tag`、`.timeline-item`、`.footer` 等）
- 换风格只换 CSS，不换 HTML 结构
- 内容提炼规则见 `templates/poster.md`
