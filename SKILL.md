---
name: "ikiw"
description: "当用户想要初始化、维护、查询或扩展一个基于 Markdown 文件的个人知识库时使用。适用于创建 ikiw 知识库、生成 summaries.md 摘要索引、查询 raw/ 文章、生成 wiki 页面、处理新文章、检查 stale 派生产物，以及管理多个已注册知识库。不适用于需要数据库、向量检索或通用 RAG 基础设施的场景。"
---

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
# 库管理
ikiw init <path> [--name <alias>]   初始化知识库并自动注册（alias 未指定时用目录名）
ikiw list                            列出所有已注册知识库
ikiw use <name>                      切换默认知识库
ikiw where                           显示当前默认知识库

# 内容操作（全部支持 --lib <name> 指定库；省略时按默认库解析规则）
ikiw summary [--lib <name>]                 批量生成摘要（检测未处理文章）
ikiw summary <filename> [--lib <name>]      为指定文章生成摘要
ikiw query "问题" [--lib <name> | --all]     查询（单库或跨全部已注册库）
ikiw wiki "主题" [--lib <name>]              生成 wiki 页面
ikiw ingest [--lib <name>]                   处理新文章（生成摘要、检查 wiki 更新）
ikiw setup-summary [--lib <name>]            通过对话定义摘要 prompt
ikiw stale [--lib <name>]                    列出可能过期的派生产物
ikiw rebuild <topic|person> [--lib <name>]   重建指定 wiki 或 distill（直接覆盖旧文件）
```

也支持自然语言触发："查知识库"、"搜一下"、"帮我生成摘要"、"建个 wiki 页面"、"哪些 wiki 过期了"、"重建 xx 主题"、"在 A 库和 B 库里都查下"、"切到 xx 库"等。

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

ingest 完成后自动扫一次 wiki/ 和 distill/ 的 frontmatter，提示"可能需要重建"的派生产物——不自动重建，由用户决策。

### 派生产物生命周期

wiki/ 和 distill/ 里的每个文件都是 raw/ 的派生缓存。规范见 SCHEMA.template.md 的"派生产物生命周期"段：

1. **生成时**：每个 wiki / distill 文件必须写入 YAML frontmatter，登记 `type / topic|person / generated_at / sources[]`，wiki 额外记 `schema_prompt_hash`
2. **过期判定**：`ikiw stale` 读各文件的 frontmatter 与当前 raw/ 状态比对——sources 文件 mtime 变动 / 已删除 / wiki 的 schema_prompt_hash 变了 → 标为 stale
3. **重建**：`ikiw rebuild <topic|person>` 按当前 SCHEMA 与 raw/ 直接覆盖旧文件（不保留备份，如需保留由用户自行 git 管理）

过期判定是显式的——只追踪 frontmatter 中登记过的 sources。raw/ 里新增的文章不会让已有 wiki 自动标为 stale；`ikiw ingest` 会给出"可能需要重建"提示，由用户决定要不要重建。

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

一个 ikiw 实例可管理多个知识库，每个有自己的 SCHEMA.md。通过全局注册表统一寻址，支持单库操作、切换默认库、跨库查询。

### 注册表位置与格式

注册表文件路径：**`~/.ikiw/registry.json`**（跨 agent 平台共享，Claude Code / Codex / OpenCode 都读同一份）。

```json
{
  "default": "creator",
  "libraries": {
    "creator": {
      "path": "/Users/erik/wiki/creator",
      "description": "创业 · 增长 · AI",
      "created_at": "2026-04-15T10:30:00"
    },
    "tech": {
      "path": "/Users/erik/wiki/tech",
      "description": "工程深度",
      "created_at": "2026-04-10T09:00:00"
    }
  }
}
```

- `ikiw init <path> [--name]` 自动 upsert 到 `libraries`；首个库自动设为 `default`
- 注册表不存在时按需创建（首次 `init` 或 `list`）
- 用户可手动编辑 `registry.json`（如迁移路径、改描述）

### 默认库解析优先级

agent 执行任何内容命令时，按以下顺序确定目标库：

1. 命令显式带 `--lib <name>` → 用指定库
2. 当前工作目录（cwd）在某个注册库的 `path` 下（含子目录）→ 用该库
3. registry 中的 `default` 字段指向的库 → 用默认库
4. 以上都未命中 → 提示用户"当前无默认库，请先 `ikiw init` 或 `ikiw use <name>`"

### 跨库查询（`--all`）

`ikiw query "问题" --all` 时：

1. 读 registry 中所有 `libraries` 的 `summaries.md`
2. 从所有摘要中综合判断相关文章
3. 读各库原文，综合回答
4. 每条结论明确标注来源库：`[来自 creator]` / `[来自 tech]`
5. 如有跨库矛盾观点，显式对比

跨库查询只做只读汇总，**不**把产出 wiki / distill 写入任何库——写入必须明确指定单库。

### 命名规则

- `name` 用 kebab-case 小写英文（`creator` / `tech-notes` / `reading-list`）
- 不允许重名；`ikiw init --name` 遇到已有 name 时报错并让用户换名或 `ikiw use` 切换
- `ikiw list` 输出格式：`<name>  <path>  <描述>  (default)`

## 规模边界

- **万篇以内：** summaries.md 一次性读完，无需额外基础设施
- **超过万篇：** 需引入向量检索做粗筛，摘要数据可直接用于 embedding
