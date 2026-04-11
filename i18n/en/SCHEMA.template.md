# Knowledge Base Configuration

This file defines the rules for this knowledge base. The LLM agent should read this file before operating on the knowledge base.

## Basic Information

- **Name:** {{知识库名称}}
- **Description:** {{一句话描述这个知识库的用途}}
- **Directory:** {{知识库根目录的绝对路径}}

## Directory Structure

```
{{知识库根目录}}/
├── raw/              # Original articles, read-only
├── summaries.md      # Summary index
├── wiki/             # Knowledge pages generated on demand
└── SCHEMA.md         # This file
```

## Summary Prompt

Use the following prompt when generating summaries. Generate one summary per article and append it to summaries.md.

```
Read the following article and generate a 3-5 sentence summary. The summary should include:
- Who the author is and their background
- What they did or what methods they shared
- Key metrics (revenue, growth, conversion rates, etc., if available)
- What platforms or domains are involved

The summary should allow someone to judge whether the article is relevant to a query without reading the original.
```

> The above is the default prompt. Users can customize it based on their article types and focus areas.

## summaries.md Format

Each entry consists of a filename heading and a summary body:

```markdown
## {{文件名}}.md
{{3-5 sentence summary}}

## {{文件名}}.md
{{3-5 sentence summary}}
```

- The filename serves as a link; in Obsidian you can navigate via `[[raw/文件名]]`
- New articles are appended to the end of the file
- Everything stays in a single file; do not split into multiple files

## Wiki Prompt

Use the following prompt when generating wiki pages:

```
Based on the following articles, generate a wiki page for the topic "{{主题名}}".

Requirements:
- Organize with a clear framework and hierarchy
- Each knowledge point = one-sentence summary + link back to the source (using [[raw/文件名]] format)
- Synthesize commonalities and differences across articles, not just a simple list
- Do not copy large sections of the original text
- If different articles have contradicting viewpoints, explicitly point them out
```

> The above is the default prompt. Users can customize the structure and style of wiki pages.

## Query Workflow

1. Read summaries.md
2. Determine which articles are relevant to the question based on the summaries
3. Read the relevant original articles
4. Provide a comprehensive answer, citing specific articles
5. If the answer has long-term value, propose saving it as a wiki page

## Styled Output (Optional)

Default output is markdown. For styled HTML output:

- Style files are located in the `designs/` subdirectory of the skill directory
- Built-in styles: notion, claude, linear.app, stripe, vercel, mintlify
- When the user says "output in XX style", read the corresponding style file and render the content as styled HTML
- HTML files are self-contained single files (inline CSS, no external dependencies)

**Adding more styles:** https://github.com/VoltAgent/awesome-design-md
- Install: `npx getdesign@latest add <style-name>`
- Rename the generated DESIGN.md and place it in the skill's `designs/` directory
