# 知识库配置

本文件定义这个知识库的规则。LLM agent 在操作本知识库时应先读取此文件。

## 基本信息

- **名称：** {{知识库名称}}
- **描述：** {{一句话描述这个知识库的用途}}
- **目录：** {{知识库根目录的绝对路径}}

## 目录结构

```
{{知识库根目录}}/
├── raw/              # 原始文章，只读
├── summaries.md      # 摘要索引
├── wiki/             # 按需生成的知识页面
└── SCHEMA.md         # 本文件
```

## 摘要 Prompt

生成摘要时使用以下 prompt。每篇文章生成一条摘要，追加到 summaries.md。

```
阅读以下文章，生成 3-5 句话的摘要。摘要应包含：
- 作者是谁、什么身份背景
- 核心做了什么事或分享了什么方法
- 关键数据（收入、增长、转化率等，如果有的话）
- 涉及什么平台或领域

摘要应该让人不读原文也能判断这篇文章是否与某个查询相关。
```

> 以上是默认 prompt，用户可根据自己的文章类型和关注点自定义。

## summaries.md 格式

每条记录由文件名标题和摘要正文组成：

```markdown
## {{文件名}}.md
{{3-5 句话的摘要}}

## {{文件名}}.md
{{3-5 句话的摘要}}
```

- 文件名即链接，在 Obsidian 中可通过 `[[raw/文件名]]` 跳转
- 新文章追加到文件末尾
- 不分文件，始终维护为一个文件

## Wiki Prompt

生成 wiki 页面时使用以下 prompt：

```
基于以下文章，为主题"{{主题名}}"生成一个 wiki 页面。

要求：
- 框架式组织，分清层级
- 每个知识点 = 一句话摘要 + 链回原文（使用 [[raw/文件名]] 格式）
- 综合多篇文章的共性和差异，不是简单罗列
- 不大段复制原文
- 如果不同文章有矛盾的观点，明确指出
```

> 以上是默认 prompt，用户可自定义 wiki 页面的结构和风格。

## 派生产物生命周期（强制 frontmatter）

wiki/ 和 distill/ 里的每个文件都是 raw/ 的派生缓存。为了能自动识别过期、支持重建，**每个派生文件必须包含 YAML frontmatter**。

### wiki 文件 frontmatter 格式

```yaml
---
type: wiki
topic: {{主题名}}
generated_at: 2026-04-15T10:30:00
schema_prompt_hash: {{SCHEMA.md 中 wiki prompt 片段的前 4 字符哈希，如 a3f9}}
sources:
  - path: raw/article-1.md
    mtime: 2026-04-01T12:00:00
  - path: raw/article-2.md
    mtime: 2026-04-10T09:15:00
---
```

### distill 文件 frontmatter 格式

```yaml
---
type: distill
person: {{人名}}
generated_at: 2026-04-15T10:30:00
sources:
  local:
    - path: raw/naval-almanack.md
      mtime: 2026-03-20T08:00:00
  web:
    - url: https://nav.al/rich
      fetched_at: 2026-04-15T10:25:00
---
```

### 过期判定（三条任一命中即标为 stale）

1. 任一 `sources[].path` 在 raw/ 中的 mtime > 该 frontmatter 记录的 mtime
2. 任一 `sources[].path` 已从 raw/ 删除
3. wiki 文件的 `schema_prompt_hash` ≠ 当前 SCHEMA.md 中对应 prompt 的哈希

### 不做的事

- **不**自动重建过期产物——由 `ikiw stale` 列出候选，用户决策
- **不**追踪未在 sources 中登记的 raw/ 新增文件——sources 是显式依赖，新增文章的"隐式可能影响"由 `ikiw ingest` 完成后提示
- **不**给 distill 的 web 来源做过期判定（URL 内容变化无法感知），仅作为溯源信息

## 查询流程

1. 读取 summaries.md
2. 从摘要中判断哪些文章与问题相关
3. 读取相关原文
4. 综合回答，引用具体文章
5. 如有长期价值，提议存为 wiki 页面

## 样式输出（可选）

默认输出 markdown。如需 styled HTML 输出：

- 样式文件位于 skill 目录的 `designs/` 子目录
- 内置样式：notion、claude、linear.app、stripe、vercel、mintlify
- 用户说"用 XX 风格输出"时，读取对应样式文件，将内容渲染为 styled HTML
- HTML 文件为自包含的单文件（内联 CSS，无外部依赖）

**添加更多样式：** https://github.com/VoltAgent/awesome-design-md
- 安装：`npx getdesign@latest add <style-name>`
- 将生成的 DESIGN.md 重命名后放入 skill 的 `designs/` 目录
