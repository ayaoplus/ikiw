**语言:** 中文 | [English](i18n/en/README.md) | [한국어](i18n/ko/README.md) | [日本語](i18n/ja/README.md)

# ikiw

一个基于 LLM 的个人知识库 skill。不依赖任何框架、数据库或向量检索，纯 prompt 驱动，任何能读写文件的 LLM agent 都能使用。

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

## 能力

| 命令 | 说明 |
|------|------|
| 摘要生成 | 读文章，生成摘要，追加到 summaries.md |
| 批量摘要 | 检测未处理文章，批量生成 |
| 查询 | 读摘要索引 → 定位文章 → 读原文 → 综合回答 |
| Wiki 生成 | 将主题知识综合为结构化页面，按需创建 |
| 风格写作 | 基于内容 + 写作风格模板输出文章 |
| 样式输出 | 用 design-md 渲染为 styled HTML |
| 新文章入库 | 生成摘要、检查 wiki 更新 |
| 初始化 | 创建知识库、通过对话生成 SCHEMA.md |
| 摘要助手 | 通过对话帮用户定义摘要 prompt |

## 三层输出

同一份内容可以逐层叠加不同模板：

1. **内容层** — SCHEMA.md 中的 wiki prompt 决定写什么
2. **风格层** — `styles/` 目录下的写作风格模板决定怎么写
3. **视觉层** — `designs/` 目录下的 design-md 决定怎么呈现

## 快速开始

1. 把本仓库作为 skill 加载到你的 LLM agent（Claude Code、Codex、OpenCode 等）
2. 告诉 agent："帮我建一个知识库"
3. agent 会通过对话了解你的文章类型和关注点，自动生成 SCHEMA.md
4. 把文章放入 `raw/` 目录
5. 告诉 agent："帮我生成摘要"
6. 开始查询

## 项目结构

```
ikiw/
├── SKILL.md              # skill 定义（agent 的入口）
├── SCHEMA.template.md    # 知识库配置模板
├── designs/              # 视觉样式（design-md）
│   ├── notion.md
│   ├── claude.md
│   ├── linear.app.md
│   ├── stripe.md
│   ├── vercel.md
│   └── mintlify.md
└── styles/               # 写作风格模板（用户自行添加）
```

## 规模

- 万篇以内：summaries.md 一次性读完，无需额外基础设施
- 超过万篇：接入向量检索做粗筛，摘要数据可直接用于 embedding

## 设计原则

- **不写代码** — 整个系统是 prompt 组合，不是程序
- **不碰原文** — raw/ 目录只读
- **不提前生成** — wiki 按需创建，不浪费
- **不绑平台** — 任何能读写文件的 LLM agent 都能用
- **不过度设计** — 到了需要的时候再加功能

## 添加视觉样式

```bash
npx getdesign@latest add <style-name>
# 将生成的 DESIGN.md 重命名后放入 designs/ 目录
```

60+ 可用样式：https://github.com/VoltAgent/awesome-design-md

## 致谢

- [Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — LLM Wiki 模式
- [VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md) — 视觉样式库
