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
  <a href="#设计哲学">设计哲学</a> ·
  <a href="#快速开始">快速开始</a> ·
  <a href="#核心能力">核心能力</a> ·
  <a href="#插件系统">插件系统</a> ·
  <a href="#致谢">致谢</a>
</p>

---

## 设计哲学

一直在思考，AI时代的个人知识库应该如何构建？我认为一个方便好用的个人知识库应该是这样的：

- 知识内容不再是人来定期维护了，效率低又不准确。应该由**AI自动来维护、分类、分析。**
- 知识库维度不再应该简单的标签化，而是可以**无限扩展的**。比如我有1000篇关于创业思考的文章，我既可以让AI帮我总结“某个人的创业方法论是什么？”，又可以让AI帮我总结“YouTube上最近最热门的创业项目是什么？”
- **足够简单优雅，**不要随时写复杂的代码来处理，也不要臃肿的RAG。对于大部分人来说，知识库文章不会超过10w篇，完全可以使用简单的逻辑来管理。
- **能力可拓展。**知识库将内容存储，而每个人都有个性化的知识再输出需求，提供插件化的可定制输出能力。

传统知识库是为内容打标签，我们可以也可以让AI自动给内容打标签，但是标签需要预先建分类体系，面临不停加新标签，而且当一篇文章打了8个标签的时候，等于没打。

[Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)提供了一种AI时代的新思路，把所有文章一股脑扔进一个 raw/ 目录，然后把自动整理成不同的wiki。

看上去很好，但是每次新内容增加，都要耗费大量的 token 不断更新 wiki，而且随着内容越来越多，wiki 的可读性也会越来越差。

我的设计思路很简单：

> 把文章扔进一个目录，LLM 给每篇生成一段摘要，汇总到一个索引文件。查询时 LLM 读索引定位文章，读原文综合回答。

这样索引帮助 LLM 缩小了上下文范围，每次只需要在索引中找到相关的内容来处理即可。又让内容的维度变得非常灵活可演进，不用不断维护一个庞大而复杂的标签库。

不打标签、不做分类、不建数据库。摘要本身就是最立体的"标签"，想要wiki也能随时生成。

基于这个构想，我设计了这个**极简知识库Skill**，适用于**1万篇资料内的个人知识库**：

- **不写代码** — 整个系统是 prompt 工程，不是程序
- **不碰原文** — `raw/` 目录只读
- **不提前生成** — wiki 按需创建，不浪费
- **不绑平台** — 任何能读写文件的 LLM agent 都能用
- **插件式扩展** — 核心精简，能力通过插件动态增长
- 万篇以内：`summaries.md` 一次性读完，无需额外基础设施

## 快速开始

1. 把本仓库作为 skill 加载到你的 LLM agent（Claude Code、Codex、OpenCode 等）
2. 告诉 agent："帮我建一个知识库"
3. agent 会通过对话了解你的文章类型和关注点，自动生成 SCHEMA.md
4. 把文章放入 `raw/` 目录
5. 告诉 agent："帮我生成摘要"
6. 开始查询

## 核心能力

知识库本身提供以下核心命令：

| 命令 | 说明 |
|------|------|
| `ikiw init <path>` | 初始化知识库，通过对话生成 SCHEMA.md |
| `ikiw summary` | 批量/单篇生成摘要 |
| `ikiw query "问题"` | 读摘要索引 → 定位文章 → 读原文 → 综合回答 |
| `ikiw wiki "主题"` | 将主题知识综合为结构化页面 |
| `ikiw ingest` | 处理新文章，生成摘要，检查 wiki 更新 |
| `ikiw setup-summary` | 摘要助手，通过对话定义摘要 prompt |

## 插件系统

核心只管知识库。其他能力通过插件扩展——每个插件是一个 `.md` 文件，放到 `plugins/` 目录即可生效。

### 官方插件

| 插件 | 文件 | 命令 | 说明 |
|------|------|------|------|
| 截图导出 | `screenshot-export.md` | `ikiw export` | 将 HTML 导出为 PNG 长图或 PDF |
| 海报生成 | `poster-generator.md` | `ikiw poster` | 从内容自动生成多风格海报 |
| 风格写作 | `style-writer.md` | `ikiw write` | 基于写作风格模板重写内容 |
| 人物蒸馏 | `distill.md` | `ikiw distill` | 从公众人物言论中蒸馏认知框架，生成可激活的思维 Skill |

### 自定义插件

编写一个 `.md` 文件，放入 `plugins/` 目录就是一个插件。约定见 [plugins/PLUGINS.md](plugins/PLUGINS.md)。

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

## 致谢

- [Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — LLM Wiki 模式
- [nuwa-skill](https://github.com/alchaincyf/nuwa-skill) — 人物蒸馏方法论
- [VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md) — 视觉样式库
