# 截图导出

> 将 HTML 页面导出为 PNG 长图或 PDF，支持高清输出。

## 首次使用：安装 Playwright（必需）

ikiw 核心是纯 prompt，但图片导出功能依赖 Node.js 浏览器自动化。首次触发任何图片生成（视觉卡片、海报、结构图）前，agent 必须提醒用户执行：

```bash
cd <ikiw skill 目录> && npm init -y && npm i playwright && npx playwright install chromium
```

未安装时 agent 应先询问用户是否授权执行安装，再继续生成；用户拒绝安装时降级为输出 HTML 文件（见下方"降级策略"）。

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

## 关键约定（避免底部留白）

- **必须** `fullPage: true`——让截图高度由内容决定，不锁死视口高度
- viewport.height 只作为渲染**最小**高度，不是输出最终高度
- 如果输出图底部仍有大块空白，**不是 screenshot 的问题，是 HTML 侧有 `min-height: 100vh` / `height: 100vh` 之类**——回到模板层修复，不要在这里 workaround
- 竖版长图（poster / visual-card / structure-map）：body 必须 `height: auto`，由 fullPage 自动展开
- 横版固定画布（landscape 类）：body 有固定 `width + height`，截图用 `fullPage: false` 取精确视口

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
