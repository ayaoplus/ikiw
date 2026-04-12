**语言:** 中文 | [English](i18n/en/README.md) | [한국어](i18n/ko/README.md) | [日本語](i18n/ja/README.md) | [Español](i18n/es/README.md)

<p align="center">
  <img src="assets/logo.png" alt="ikiw" width="200">
</p>

# ikiw — I Keep It Wise

> *Wiki, reversed.*

一个基于 LLM 的个人知识库 skill。不依赖任何框架、数据库或向量检索，纯 prompt 驱动，任何能读写文件的 LLM agent 都能使用。

传统 wiki 是你写知识给别人看。ikiw 反过来——LLM 替你整理、维护、综合知识，你只管提问。

灵感来自 [Karpathy 的 LLM Wiki 模式](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)。

## 核心思路

把文章扔进一个目录，LLM 给每篇生成一段摘要，汇总到一个索引文件。查询时 LLM 读索引定位文章，读原文综合回答。就这么简单。

不打标签、不做分类、不建数据库。摘要本身就是最立体的"标签"。

## 知识库结构

```
my-wiki/
├── raw/              # 原始文章，只读
├── summaries.md      # 摘要索引，系统的核心
├── wiki/             # 按需生成的知识页面
└── SCHEMA.md         # 知识库配置（自定义 prompt）
```

## 核心能力

知识库本身提供四个核心命令：

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

## 项目结构

```
ikiw/
├── SKILL.md              # 核心 skill 定义
├── SCHEMA.template.md    # 知识库配置模板
├── plugins/              # 插件目录
│   ├── PLUGINS.md        # 插件开发约定
│   ├── screenshot-export.md
│   ├── poster-generator.md
│   └── style-writer.md
├── designs/              # 视觉样式（33 个 design-md）
├── styles/               # 写作风格模板
├── templates/            # 结构化模板
│   └── poster.md
├── assets/               # 静态资源
└── i18n/                 # 多语言（en/ko/ja/es）
```

## 快速开始

1. 把本仓库作为 skill 加载到你的 LLM agent（Claude Code、Codex、OpenCode 等）
2. 告诉 agent："帮我建一个知识库"
3. agent 会通过对话了解你的文章类型和关注点，自动生成 SCHEMA.md
4. 把文章放入 `raw/` 目录
5. 告诉 agent："帮我生成摘要"
6. 开始查询

## 规模

- 万篇以内：summaries.md 一次性读完，无需额外基础设施
- 超过万篇：接入向量检索做粗筛，摘要数据可直接用于 embedding

## 设计原则

- **不写代码** — 整个系统是 prompt 组合，不是程序
- **不碰原文** — raw/ 目录只读
- **不提前生成** — wiki 按需创建，不浪费
- **不绑平台** — 任何能读写文件的 LLM agent 都能用
- **插件式扩展** — 核心精简，能力通过插件动态增长

## 添加视觉样式

```bash
npx getdesign@latest add <style-name>
# 将生成的 DESIGN.md 重命名后放入 designs/ 目录
```

60+ 可用样式：https://github.com/VoltAgent/awesome-design-md

## 致谢

- [Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — LLM Wiki 模式
- [VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md) — 视觉样式库
