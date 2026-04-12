# 截图导出

> 将 HTML 页面导出为 PNG 长图或 PDF，支持高清输出。

## 命令

```
ikiw export "主题" --format png                    导出为长图（默认 3x 高清）
ikiw export "主题" --format pdf                    导出为 PDF
ikiw export "主题" --width 520 --dpr 3             自定义视口宽度和像素密度
```

## 触发词

"导出图片"、"生成长图"、"截图"、"导出 PDF"

## 依赖

- `designs/` — 如果需要先生成 styled HTML
- Playwright — Node.js 浏览器自动化工具

## 默认参数

| 参数 | 默认值 | 说明 |
|------|--------|------|
| `--format` | `png` | `png` 或 `pdf` |
| `--width` | `520` | 视口宽度（px），内容紧凑无多余背景 |
| `--dpr` | `3` | 设备像素比，3 = 超高清（输出 1560px 宽） |
| `--output` | 知识库 `wiki/` 目录 | 输出路径 |

## 执行流程

1. 确认要导出的 HTML 文件路径
2. 如果只有 markdown，先按指定 design 生成 styled HTML
3. 使用 Playwright Node API + file:// 协议截图：

```javascript
import { chromium } from 'playwright';
const browser = await chromium.launch();
const context = await browser.newContext({
  viewport: { width: WIDTH, height: 800 },
  deviceScaleFactor: DPR
});
const page = await context.newPage();
await page.goto('file:///absolute/path/to/page.html', {
  waitUntil: 'domcontentloaded',
  timeout: 60000
});
await page.waitForTimeout(2000);
await page.screenshot({ path: output, fullPage: true });
await browser.close();
```

4. agent 自行生成并执行截图脚本

## 降级策略

1. **Playwright 可用** → 直接截图
2. **Playwright 未安装** → 执行 `npm install playwright && npx playwright install chromium`，安装后重试
3. **安装失败** → 输出 HTML 文件，提示："HTML 已生成，请在浏览器中打开。如需导出图片，可安装 Playwright"

原则：永远给用户一个结果。
