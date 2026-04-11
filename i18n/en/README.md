**Language:** English | [中文](../../README.md) | [한국어](../ko/README.md) | [日本語](../ja/README.md) | [Español](../es/README.md)

# ikiw — I Keep It Wise

> *Wiki, reversed.*

A personal knowledge base skill powered by LLM. No frameworks, databases, or vector search required — purely prompt-driven, usable by any LLM agent that can read and write files.

Traditional wiki is where you write knowledge for others to read. ikiw flips it — the LLM organizes, maintains, and synthesizes knowledge for you. You just ask.

Inspired by [Karpathy's LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

## Core Idea

Drop articles into a directory. The LLM generates a summary for each one and compiles them into an index file. When querying, the LLM reads the index to locate relevant articles, reads the originals, and synthesizes an answer. That's it.

No tags, no categories, no databases. The summaries themselves are the richest "tags" you can get.

## Knowledge Base Structure

```
my-wiki/
├── raw/              # Original articles, read-only
├── summaries.md      # Summary index, the core of the system
├── wiki/             # Knowledge pages generated on demand
└── SCHEMA.md         # Knowledge base config (custom prompts)
```

## Capabilities

| Command | Description |
|---------|-------------|
| Summarize | Read an article, generate a summary, append to summaries.md |
| Batch Summarize | Detect unprocessed articles and summarize them in bulk |
| Query | Read summary index → locate articles → read originals → synthesize answer |
| Wiki Generation | Synthesize topic knowledge into structured pages, created on demand |
| Styled Writing | Output articles based on content + writing style templates |
| Styled Output | Render to styled HTML using design-md |
| Ingest New Article | Generate summary, check for wiki updates |
| Initialize | Create a knowledge base, generate SCHEMA.md through conversation |
| Summary Assistant | Help users define summary prompts through conversation |

## Three Output Layers

The same content can be progressively enhanced with different templates:

1. **Content Layer** — Wiki prompts in SCHEMA.md determine *what* to write
2. **Style Layer** — Writing style templates in `styles/` determine *how* to write
3. **Visual Layer** — design-md files in `designs/` determine *how* to present

## Quick Start

1. Load this repo as a skill in your LLM agent (Claude Code, Codex, OpenCode, etc.)
2. Tell the agent: "Help me create a knowledge base"
3. The agent will learn about your article types and focus areas through conversation, then auto-generate SCHEMA.md
4. Place articles in the `raw/` directory
5. Tell the agent: "Generate summaries for me"
6. Start querying

## Project Structure

```
ikiw/
├── SKILL.md              # Skill definition (agent entry point)
├── SCHEMA.template.md    # Knowledge base config template
├── designs/              # Visual styles (design-md)
│   ├── notion.md
│   ├── claude.md
│   ├── linear.app.md
│   ├── stripe.md
│   ├── vercel.md
│   └── mintlify.md
└── styles/               # Writing style templates (user-provided)
```

## Scale

- Up to 10,000 articles: summaries.md can be read in one pass — no additional infrastructure needed
- Beyond 10,000: Integrate vector search for rough filtering — summary data can be used directly for embeddings

## Design Principles

- **No code** — The entire system is a combination of prompts, not a program
- **No touching originals** — The raw/ directory is read-only
- **No pre-generation** — Wiki pages are created on demand, nothing wasted
- **No platform lock-in** — Works with any LLM agent that can read and write files
- **No over-engineering** — Add features only when needed

## Adding Visual Styles

```bash
npx getdesign@latest add <style-name>
# Rename the generated DESIGN.md and place it in the designs/ directory
```

60+ available styles: https://github.com/VoltAgent/awesome-design-md

## Acknowledgments

- [Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — LLM Wiki pattern
- [VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md) — Visual style library
