<p align="center">
  <img src="assets/logo.png" alt="ikiw" width="200">
</p>
<p align="center">
  <em>WIKI mirrored, Wisdom discovered.</em><br>
  <strong>可能是最适合AI时代的个人知识库的构建模式</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License: MIT">
  <img src="https://img.shields.io/badge/Claude%20Code-Skill-8A2BE2" alt="Claude Code Skill">
  <img src="https://img.shields.io/badge/skills.sh-Compatible-9ACD32" alt="skills.sh Compatible">
</p>

<br>

<p align="center">
  没有任何代码、数据库或 RAG，让 agent 快速构建和读取你的 AI 知识库<br>
  把所有文章一股脑扔进目录即可，这个 skill 会自动帮你整理、维护、分析、输出<br>
  可扩展插件设计，在知识库基础上随意扩展各种能力：分析报告、二创写作、格式化卡片、海报……
</p>

<p align="center">
  <a href="#为什么用-ikiw">为什么用 ikiw？</a> ·
  <a href="#skill-特点">Skill 特点</a> ·
  <a href="#快速开始">快速开始</a> ·
  <a href="#目录结构">目录结构</a> ·
  <a href="#核心能力">核心能力</a> ·
  <a href="#插件系统">插件系统</a> ·
  <a href="#致谢">致谢</a>
</p>

---

## 为什么用 ikiw？

**AI 时代的个人知识库应该如何构建？**

- 知识内容不再是人来定期维护了，效率低又不准确。应该由**AI自动来维护、分类、分析。**
- 知识库维度不再应该简单的标签化，而是可以**无限扩展的**。既可以让AI帮我总结“某个人的创业方法论是什么？”，又可以让AI帮总结“YouTube上最近最热门的创业项目是什么？”
- **足够简单优雅，** 不要随时写复杂的代码来处理，也不要臃肿的RAG。对于大部分人来说，知识库文章不会超过10w篇，完全可以使用简单的逻辑来管理。

**ikiw的设计哲学是：**

> 把文章扔进一个目录，LLM 给每篇生成一段摘要，汇总到一个索引文件。查询时 LLM 读索引定位文章，读原文综合回答。

索引帮助 LLM 缩小了上下文范围，每次只需要在索引中找到相关的内容来处理即可。又让内容的维度变得非常灵活可演进，不用不断维护一个庞大而复杂的标签库。

**不打标签、不做分类、不建数据库。摘要本身就是最立体的"标签"，想要 wiki 也能随时生成。**

---

## Skill 特点

基于上述构想，ikiw 是一个**极简知识库 Skill**，适用于**1万篇资料内的个人知识库**：

- **不写代码** — 整个系统是 prompt 工程，不是程序
- **不碰原文** — `raw/` 目录只读
- **不提前生成** — wiki 按需创建，不浪费 token
- **不绑平台** — 任何能读写文件的 LLM agent 都能用（Claude Code、Codex、OpenCode 等）
- **插件式扩展** — 核心精简，能力通过插件动态增长
- **零基础设施** — 万篇以内 `summaries.md` 一次性读完，无需向量库

---

## 快速开始

### 1. 安装 Skill

直接告诉你的 agent：

> 帮我把 https://github.com/ayaoplus/ikiw 这个 skill 安装上

agent 会自动 clone 仓库到 skill 目录并加载（Claude Code 一般是 `~/.claude/skills/`，Codex/OpenCode 同理）。

也可以手动 clone：

```bash
git clone https://github.com/ayaoplus/ikiw.git ~/.claude/skills/ikiw
```

### 2. 初始化知识库

告诉 agent：

> 帮我在 ~/Documents/my-wiki 建一个 ikiw 知识库，主要存创业、增长、AI 相关的文章

agent 会通过对话了解你的文章类型和关注点，自动生成 `SCHEMA.md`（包含定制化的摘要 prompt、wiki prompt、查询规则）。

### 3. 投喂文章并生成摘要

把文章（markdown 文件）丢进 `raw/` 目录，然后告诉 agent：

> 帮我把新文章生成摘要

agent 会自动检测未处理的文章，按 `SCHEMA.md` 中的 prompt 生成摘要，追加到 `summaries.md`。

### 4. 查询知识库

直接用自然语言问：

> 查一下知识库里有哪些讲 cold email 增长的案例，列出关键数据

agent 会读 `summaries.md` → 定位相关文章 → 读原文 → 综合回答，并标注引用来源。

### 5. 生成 Wiki 页面

当某个主题被反复查询，可以沉淀成 wiki 页面：

> 帮我把知识库里关于"创业方法论"的内容整理成一个 wiki 页面

agent 会综合多篇相关文章，生成框架式的结构化页面（每个知识点带原文链接），存入 `wiki/` 目录。Wiki 是缓存，可以随时重建。

---

## 目录结构

知识库的目录组织非常简单，全部由一个 `SCHEMA.md` 配置串起来：

```
my-wiki/
├── SCHEMA.md          # 知识库配置：摘要 prompt、wiki prompt、查询规则
├── raw/               # 原始文章（只读，由你投喂）
│   ├── article-1.md
│   └── article-2.md
├── summaries.md       # 摘要索引（系统的核心，由 agent 维护）
└── wiki/              # 按需生成的主题 wiki 页面
    └── topic-a.md
```

Skill 仓库自身的结构：

```
ikiw/
├── SKILL.md           # skill 入口定义
├── SCHEMA.template.md # 知识库配置模板
├── plugins/           # 插件（每个 .md 文件即一个插件）
├── designs/           # 视觉样式（design-md，供截图、海报插件使用）
├── styles/            # 写作风格模板（供 style-writer 插件使用）
└── templates/         # 结构化模板（如海报骨架）
```

---

## 核心能力

知识库本身提供以下核心命令，每个命令都支持自然语言触发：

| 命令 | 自然语言触发 | 说明 |
|------|--------------|------|
| `ikiw init <path>` | "帮我建一个知识库"、"初始化一个 wiki" | 通过对话生成 SCHEMA.md |
| `ikiw summary` | "生成摘要"、"处理一下新文章" | 批量/单篇生成摘要 |
| `ikiw query "问题"` | "查知识库"、"搜一下…"、"知识库里有没有…" | 读摘要索引 → 定位文章 → 读原文 → 综合回答 |
| `ikiw wiki "主题"` | "建个 wiki 页面"、"把 X 主题整理成 wiki" | 将主题知识综合为结构化页面 |
| `ikiw ingest` | "处理新文章"、"入库" | 生成摘要 + 检查 wiki 更新 |
| `ikiw setup-summary` | "调整一下摘要规则" | 摘要助手，通过对话定义摘要 prompt |

> agent 会根据上下文自动判断意图，不必死记命令。命令形式更适合写脚本或精确控制。

---

## 插件系统

核心只管知识库。其他能力通过插件扩展——每个插件是一个 `.md` 文件，放到 `plugins/` 目录即可生效。

### 官方插件

<table>
  <thead>
    <tr><th>插件</th><th>说明</th><th>详细文档</th></tr>
  </thead>
  <tbody>
    <tr>
      <td nowrap>截图导出</td>
      <td>将 HTML 导出为 PNG 长图或 PDF</td>
      <td nowrap><a href="plugins/screenshot-export.md">screenshot-export.md</a></td>
    </tr>
    <tr>
      <td nowrap>海报生成</td>
      <td>从内容自动生成多风格海报</td>
      <td nowrap><a href="plugins/poster-generator.md">poster-generator.md</a></td>
    </tr>
    <tr>
      <td nowrap>风格写作</td>
      <td>基于写作风格模板重写内容</td>
      <td nowrap><a href="plugins/style-writer.md">style-writer.md</a></td>
    </tr>
    <tr>
      <td nowrap>人物蒸馏</td>
      <td>从公众人物言论中蒸馏认知框架，生成可激活的思维 Skill</td>
      <td nowrap><a href="plugins/distill.md">distill.md</a></td>
    </tr>
  </tbody>
</table>

### 自定义插件

编写一个 `.md` 文件，放入 `plugins/` 目录就是一个插件。

约定见 [plugins/PLUGINS.md](plugins/PLUGINS.md)。

### 资源目录

插件可引用以下共享资源：

| 目录 | 内容 | 被谁引用 |
|------|------|----------|
| `designs/` | 33 个视觉样式（design-md） | screenshot-export、poster-generator |
| `styles/` | 写作风格模板 | style-writer |
| `templates/` | 结构化模板（海报骨架等） | poster-generator |

### 三层输出

插件组合可实现三层叠加输出：

1. **内容层** — SCHEMA.md 的 prompt 决定写什么
2. **风格层** — `styles/` 决定怎么写
3. **视觉层** — `designs/` 决定怎么呈现

---

## 致谢

- [Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — LLM Wiki 模式
- [nuwa-skill](https://github.com/alchaincyf/nuwa-skill) — 人物蒸馏方法论
- [VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md) — 视觉样式库
