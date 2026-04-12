# ikiw — LLM Wiki 知识库

一个基于 LLM 的个人知识库管理 skill。不依赖任何框架、数据库或向量检索，纯 prompt 驱动，任何能读写文件的 LLM agent 都能使用。

灵感来自 Karpathy 的 LLM Wiki 模式。

## 命令

```
ikiw init <path>                    初始化知识库，通过对话生成 SCHEMA.md
ikiw summary                        批量生成摘要（检测未处理文章）
ikiw summary <filename>             为指定文章生成摘要
ikiw query "问题"                    查询知识库
ikiw wiki "主题"                     生成 wiki 页面
ikiw wiki "主题" --style <style>     生成 wiki 并按写作风格输出
ikiw wiki "主题" --design <design>   生成 wiki 并按视觉样式输出 HTML
ikiw write "主题" --style <style>    基于知识库内容进行风格写作
ikiw export "主题" --format png      导出 wiki 页面为长图（默认 3x 高清）
ikiw export "主题" --format pdf      导出 wiki 页面为 PDF
ikiw export "主题" --format png --width 520 --dpr 3   自定义视口宽度和像素密度
ikiw ingest                         处理新文章（生成摘要、检查 wiki 更新）
ikiw setup-summary                  摘要助手，通过对话定义摘要 prompt
```

也支持自然语言触发：
- "查知识库"、"搜一下"、"有没有相关文章"
- "帮我生成摘要"、"处理新文章"
- "建个 wiki 页面"、"总结一下这个主题"
- "知识库里关于 XX 的内容"

## 核心概念

**知识库 = 一个目录，包含三样东西：**

```
my-wiki/
├── raw/              # 原始文章，不修改
├── summaries.md      # 摘要索引，系统的核心
├── wiki/             # 查询结果缓存，按需生成
└── SCHEMA.md         # 本知识库的配置
```

- `raw/` 是原始素材，只读，永远不修改
- `summaries.md` 是 LLM 的全局地图，每篇文章一条摘要
- `wiki/` 是按需生成的知识页面，本质是查询缓存
- `SCHEMA.md` 定义本知识库的规则和自定义 prompt

## 能力

### 1. 摘要生成

读一篇 raw/ 中的文章，按 SCHEMA.md 中定义的摘要 prompt 生成摘要，追加到 summaries.md。

**单篇处理：**
1. 读取文章内容
2. 按摘要 prompt 生成摘要
3. 追加到 summaries.md 末尾

**批量处理：**
1. 列出 raw/ 中所有文件
2. 对比 summaries.md 中已有的文件名，找出未处理的文章
3. 逐篇生成摘要并追加
4. 自行决定最高效的处理方式（直接逐篇、写脚本并发、分批等）

### 2. 查询

用户带着具体意图提问，从知识库中找到答案。

**流程：**
1. 读取 summaries.md（全量摘要索引）
2. 根据问题从摘要中判断哪些文章相关
3. 读取相关原文（通常 10-30 篇）
4. 综合回答，引用具体文章
5. 如果回答有长期复用价值，提议存为 wiki 页面

### 3. Wiki 生成

将某个主题的知识综合为一个结构化页面。

**流程：**
1. 读取 summaries.md，筛选相关文章
2. 读取相关原文
3. 按 SCHEMA.md 中定义的 wiki prompt 生成页面
4. 框架式组织：每个知识点 = 一句话摘要 + 链回原文
5. 存入 wiki/ 目录

**Wiki 是缓存，不是权威：**
- 可以过期、可以重建
- 新文章进来后，相关 wiki 可能需要更新
- 不追求完美，追求有用

### 4. 样式输出（可选）

默认输出 markdown。如果用户需要更好的视觉呈现，可以结合 design-md 样式生成 styled HTML。

**使用方式：**
- 用户指定样式："用 Notion 风格输出"、"用 Claude 风格生成网页"
- 读取 `designs/` 目录下对应的样式文件
- 按样式规则将 wiki 内容渲染为 styled HTML 页面
- 将 HTML 文件存入知识库的 `wiki/` 目录（与 .md 同名，后缀 .html）

**内置样式：** 位于本 skill 的 `designs/` 目录
- `notion` — 温暖极简，衬线标题，柔和表面
- `claude` — 赤土色调，编辑式布局，书卷气
- `linear.app` — 超极简，精确，紫色点缀
- `stripe` — 标志性紫色渐变，轻盈优雅
- `vercel` — 黑白精准，Geist 字体
- `mintlify` — 绿色点缀，阅读优化

**添加更多样式：** https://github.com/VoltAgent/awesome-design-md
- 安装：`npx getdesign@latest add <style-name>`
- 将生成的 DESIGN.md 重命名后放入 `designs/` 目录

### 5. 风格写作

基于知识库内容，按指定的写作风格模板输出文章。

**流程：**
1. 获取内容来源（wiki 页面、查询结果、或指定的原文）
2. 读取 `styles/` 目录下对应的写作风格模板
3. 按风格模板重写内容
4. 输出为 markdown，可叠加 design-md 生成 styled HTML

**内置写作风格：** 位于本 skill 的 `styles/` 目录
- 用户可自行添加风格模板，放入 `styles/` 目录即可

**三层可组合：**
- 内容层（wiki prompt）→ 决定写什么
- 风格层（styles/）→ 决定怎么写
- 视觉层（designs/）→ 决定怎么呈现

用户说"把小红书带货的 wiki 用公众号风格写篇文章，用 Notion 样式生成网页"，agent 依次读取 wiki 内容、写作风格模板、视觉样式，逐层渲染输出。

### 6. 导出

将 wiki 页面或 styled HTML 导出为图片或 PDF。

**流程：**
1. 确认要导出的内容（wiki 页面、styled HTML）
2. 如果只有 markdown，先按指定 design 生成 styled HTML
3. 使用 Playwright 截图导出（通过 Node API 设置 deviceScaleFactor）
4. 输出文件存入知识库的 `wiki/` 目录或用户指定的路径

**默认参数：**
- `--width 520` — 视口宽度，内容紧凑无多余背景
- `--dpr 3` — 3 倍像素密度（实际输出 1560px 宽），高清适合社交媒体和打印
- `--format png` — 默认输出 PNG

**所有参数：**

| 参数 | 默认值 | 说明 |
|------|--------|------|
| `--format` | `png` | 输出格式：`png` 或 `pdf` |
| `--width` | `520` | 视口宽度（px），内容区宽度，不含多余背景 |
| `--dpr` | `3` | 设备像素比，1 = 标清，2 = 视网膜，3 = 超高清 |
| `--output` | 知识库 `wiki/` 目录 | 输出文件路径 |

**截图实现：**

使用 Playwright Node API，核心代码：

```javascript
const context = await browser.newContext({
  viewport: { width: WIDTH, height: 800 },
  deviceScaleFactor: DPR
});
const page = await context.newPage();
await page.goto(url);
await page.screenshot({ path: output, fullPage: true });
```

agent 根据参数自行生成并执行截图脚本。

**组合使用示例：**
- `ikiw export "小红书带货" --format png` — 默认 520px 宽、3x 高清导出
- `ikiw export "小红书带货" --width 375 --dpr 3` — 手机宽度导出
- `ikiw export "小红书带货" --width 1200 --dpr 2` — 宽屏导出
- `ikiw wiki "小红书带货" --design notion` 然后 `ikiw export "小红书带货" --format png` — 先生成样式页面再导出

### 7. 新文章入库

新文章加入 raw/ 后的处理流程：
1. 检测 raw/ 中未出现在 summaries.md 里的文章
2. 生成摘要，追加到 summaries.md
3. 检查 wiki/ 中是否有相关页面可能需要更新，提示用户

## 初始化新知识库

当用户说"帮我建一个知识库"时：

1. 确认知识库目录路径
2. 创建目录结构：raw/、wiki/
3. 通过对话了解用户的文章类型和关注点
4. 基于对话生成自定义的 SCHEMA.md（摘要 prompt、wiki prompt）
5. 创建空的 summaries.md
6. 引导用户将文章放入 raw/
7. 开始生成摘要

## 摘要助手

首次使用时，通过对话帮用户定义合适的摘要 prompt：

- "你的文章是什么类型的？"（技术博客、商业案例、学术论文…）
- "你查询时最关心什么信息？"（方法论、数据、人物、时间线…）
- "摘要大概多长合适？"（1-2 句极简、3-5 句标准、一段话详细）

根据回答生成摘要 prompt 写入 SCHEMA.md。用户随时可以调整。

## 多知识库支持

一个 skill 可以管理多个知识库，每个有自己的 SCHEMA.md：

```
~/wikis/生财有术/SCHEMA.md
~/wikis/交易策略/SCHEMA.md
~/wikis/技术笔记/SCHEMA.md
```

查询时指定知识库，或让 agent 根据问题自动判断该查哪个。

## 规模边界

- **万篇以内：** summaries.md 一次性读完，无需额外基础设施
- **超过万篇：** 需要引入向量检索做粗筛，摘要数据可直接用于 embedding
- 不要提前为超大规模设计，到了再加
