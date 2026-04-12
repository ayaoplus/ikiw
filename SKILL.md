# ikiw — I Keep It Wise

纯 prompt 驱动的个人知识库 skill，支持插件扩展。任何能读写文件的 LLM agent 都能使用。

## 知识库结构

```
my-wiki/
├── raw/              # 原始文章，只读
├── summaries.md      # 摘要索引，系统的核心
├── wiki/             # 按需生成的知识页面
└── SCHEMA.md         # 本知识库的配置
```

## 核心命令

```
ikiw init <path>              初始化知识库，通过对话生成 SCHEMA.md
ikiw summary                  批量生成摘要（检测未处理文章）
ikiw summary <filename>       为指定文章生成摘要
ikiw query "问题"              查询知识库
ikiw wiki "主题"               生成 wiki 页面
ikiw ingest                   处理新文章（生成摘要、检查 wiki 更新）
ikiw setup-summary            摘要助手，通过对话定义摘要 prompt
```

也支持自然语言触发："查知识库"、"搜一下"、"帮我生成摘要"、"建个 wiki 页面"等。

## 核心能力

### 摘要生成

读 raw/ 中的文章，按 SCHEMA.md 中定义的摘要 prompt 生成摘要，追加到 summaries.md。
支持单篇和批量处理。批量时自动检测未处理的文章。

### 查询

1. 读取 summaries.md（全量摘要索引）
2. 从摘要中判断哪些文章相关
3. 读取相关原文（通常 10-30 篇）
4. 综合回答，引用具体文章
5. 如有长期价值，提议存为 wiki 页面

### Wiki 生成

将某个主题的知识综合为结构化页面，存入 wiki/ 目录。
框架式组织：每个知识点 = 一句话摘要 + 链回原文。
Wiki 是缓存，不是权威——可以过期、可以重建。

### 新文章入库

检测 raw/ 中未出现在 summaries.md 里的文章，生成摘要，检查 wiki 是否需要更新。

## 插件系统

ikiw 的核心只管知识库（摘要、查询、wiki）。其他能力通过插件扩展。

### 插件目录

插件位于 `plugins/` 目录。每个插件是一个 `.md` 文件，定义了命令、触发词和执行流程。
agent 读取 SKILL.md 后，扫描 `plugins/` 目录加载可用插件。

### 官方插件

| 插件 | 文件 | 说明 |
|------|------|------|
| 截图导出 | `plugins/screenshot-export.md` | 将 HTML 页面导出为 PNG/PDF 长图 |
| 视觉卡片 | `plugins/visual-card.md` | 把文章/笔记凝练为核心要点卡（竖版长图 / 横版单页） |
| 海报 | `plugins/poster.md` | 活动/产品/课程营销海报（含价格、CTA、报名信息） |
| 结构图 | `plugins/structure-map.md` | 把方法论/SKILL/长文完整可视化（覆盖所有节点的知识结构图） |
| 风格写作 | `plugins/style-writer.md` | 基于写作风格模板重写内容 |
| 人物蒸馏 | `plugins/distill.md` | 从公众人物言论中蒸馏认知框架，生成思维 Skill |

### 自定义插件

用户可以自行编写插件，放入 `plugins/` 目录即可生效。
插件开发约定见 `plugins/PLUGINS.md`。

### 资源目录

插件可引用以下共享资源：

| 目录 | 内容 | 被谁引用 |
|------|------|----------|
| `designs/` | 视觉样式文件（design-md） | screenshot-export、visual-card、poster、structure-map |
| `templates/` | 视觉卡片 / 海报 / 结构图版式模板（含画布契约、HTML 骨架、字段规范、组件库） | visual-card、poster、structure-map |
| `styles/` | 写作风格模板 | style-writer |

## 多知识库

一个 ikiw 实例可管理多个知识库，每个有自己的 SCHEMA.md。
查询时指定知识库，或让 agent 根据问题自动判断。

## 规模边界

- **万篇以内：** summaries.md 一次性读完，无需额外基础设施
- **超过万篇：** 需引入向量检索做粗筛，摘要数据可直接用于 embedding
