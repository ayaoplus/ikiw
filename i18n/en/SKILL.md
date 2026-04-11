# ikiw — LLM Wiki Knowledge Base

A personal knowledge base management skill powered by LLM. No frameworks, databases, or vector search required — purely prompt-driven, usable by any LLM agent that can read and write files.

Inspired by Karpathy's LLM Wiki pattern.

## Commands

```
ikiw init <path>                    Initialize a knowledge base, generate SCHEMA.md via conversation
ikiw summary                        Batch generate summaries (detect unprocessed articles)
ikiw summary <filename>             Generate summary for a specific article
ikiw query "question"               Query the knowledge base
ikiw wiki "topic"                   Generate a wiki page
ikiw wiki "topic" --style <style>   Generate wiki with a writing style applied
ikiw wiki "topic" --design <design> Generate wiki as styled HTML with a visual design
ikiw write "topic" --style <style>  Write in a style based on knowledge base content
ikiw export "topic" --format png    Export wiki page as a full-page screenshot
ikiw export "topic" --format pdf    Export wiki page as PDF
ikiw ingest                         Process new articles (generate summaries, check wiki updates)
ikiw setup-summary                  Summary assistant, define summary prompt via conversation
```

Also supports natural language triggers:
- "search the knowledge base", "look it up", "any related articles?"
- "generate a summary", "process new articles"
- "create a wiki page", "summarize this topic"
- "what does the knowledge base have on XX?"

## Core Concepts

**A knowledge base = a directory containing three things:**

```
my-wiki/
├── raw/              # Original articles, never modified
├── summaries.md      # Summary index, the system's core
├── wiki/             # Query result cache, generated on demand
└── SCHEMA.md         # Configuration for this knowledge base
```

- `raw/` holds the original source material — read-only, never modified
- `summaries.md` is the LLM's global map — one summary entry per article
- `wiki/` contains on-demand knowledge pages, essentially a query cache
- `SCHEMA.md` defines the rules and custom prompts for this knowledge base

## Capabilities

### 1. Summary Generation

Read an article from raw/, generate a summary using the summary prompt defined in SCHEMA.md, and append it to summaries.md.

**Single article:**
1. Read the article content
2. Generate a summary using the summary prompt
3. Append to the end of summaries.md

**Batch processing:**
1. List all files in raw/
2. Compare against filenames already in summaries.md to find unprocessed articles
3. Generate and append summaries one by one
4. Decide the most efficient processing approach autonomously (sequential, scripted concurrency, batched, etc.)

### 2. Query

The user asks a question with a specific intent; find the answer from the knowledge base.

**Process:**
1. Read summaries.md (the full summary index)
2. Determine which articles are relevant based on the question
3. Read the relevant source articles (typically 10-30)
4. Synthesize an answer, citing specific articles
5. If the answer has long-term reuse value, propose saving it as a wiki page

### 3. Wiki Generation

Synthesize knowledge on a topic into a structured page.

**Process:**
1. Read summaries.md, filter for relevant articles
2. Read the relevant source articles
3. Generate a page using the wiki prompt defined in SCHEMA.md
4. Organize as a framework: each knowledge point = one-sentence summary + link back to the source article
5. Save to the wiki/ directory

**Wiki pages are cache, not authority:**
- They can expire and be rebuilt
- When new articles come in, related wiki pages may need updating
- Don't aim for perfection — aim for usefulness

### 4. Styled Output (Optional)

Default output is markdown. If the user wants better visual presentation, combine with design-md styles to generate styled HTML.

**Usage:**
- User specifies a style: "output in Notion style", "generate a webpage in Claude style"
- Read the corresponding style file from the `designs/` directory
- Render wiki content as styled HTML following the style rules
- Save the HTML file to the knowledge base's `wiki/` directory (same name as the .md file, with .html extension)

**Built-in styles:** Located in this skill's `designs/` directory
- `notion` — Warm minimalism, serif headings, soft surfaces
- `claude` — Terracotta tones, editorial layout, bookish feel
- `linear.app` — Ultra-minimal, precise, purple accents
- `stripe` — Signature purple gradients, light and elegant
- `vercel` — Black-and-white precision, Geist font
- `mintlify` — Green accents, reading-optimized

**Adding more styles:** https://github.com/VoltAgent/awesome-design-md
- Install: `npx getdesign@latest add <style-name>`
- Rename the generated DESIGN.md and place it in the `designs/` directory

### 5. Stylized Writing

Produce articles based on knowledge base content, following a specified writing style template.

**Process:**
1. Obtain the content source (wiki page, query result, or specified source articles)
2. Read the corresponding writing style template from the `styles/` directory
3. Rewrite the content following the style template
4. Output as markdown; optionally layer on design-md to generate styled HTML

**Built-in writing styles:** Located in this skill's `styles/` directory
- Users can add their own style templates by placing them in the `styles/` directory

**Three composable layers:**
- Content layer (wiki prompt) — determines *what* to write
- Style layer (styles/) — determines *how* to write it
- Visual layer (designs/) — determines *how* to present it

When the user says "take the Xiaohongshu product promotion wiki, write an article in WeChat Official Account style, and generate a webpage with Notion styling," the agent reads the wiki content, writing style template, and visual style in sequence, rendering layer by layer.

### 6. Export

Export wiki pages or styled HTML as images or PDF.

**Process:**
1. Confirm the content to export (wiki page, styled HTML)
2. If only markdown is available, first generate styled HTML with the specified design
3. Use Playwright for full-page screenshots:
   - PNG full-page image: `npx playwright screenshot --full-page input.html output.png`
   - PDF: via Playwright's PDF export functionality
4. Save output files to the knowledge base's `wiki/` directory

**Supported formats:**
- `png` — Full-page screenshot, ideal for sharing on social media
- `pdf` — Suitable for printing and archiving

**Usage examples:**
- `ikiw export "topic" --format png` — Export an existing wiki page as a full-page image
- `ikiw wiki "topic" --design notion` then `ikiw export "topic" --format png` — Generate a styled page first, then export

### 7. Ingesting New Articles

Processing flow after new articles are added to raw/:
1. Detect articles in raw/ that are not yet in summaries.md
2. Generate summaries and append to summaries.md
3. Check if any pages in wiki/ might need updating, and notify the user

## Initializing a New Knowledge Base

When the user says "help me set up a knowledge base":

1. Confirm the knowledge base directory path
2. Create the directory structure: raw/, wiki/
3. Learn about the user's article types and focus areas through conversation
4. Generate a custom SCHEMA.md based on the conversation (summary prompt, wiki prompt)
5. Create an empty summaries.md
6. Guide the user to place articles in raw/
7. Begin generating summaries

## Summary Assistant

On first use, help the user define an appropriate summary prompt through conversation:

- "What type of articles do you have?" (tech blogs, business cases, academic papers...)
- "What information matters most when you search?" (methodology, data, people, timelines...)
- "How long should summaries be?" (1-2 sentences ultra-brief, 3-5 sentences standard, a full paragraph detailed)

Generate the summary prompt based on the answers and write it to SCHEMA.md. The user can adjust it at any time.

## Multi-Knowledge-Base Support

A single skill can manage multiple knowledge bases, each with its own SCHEMA.md:

```
~/wikis/生财有术/SCHEMA.md
~/wikis/交易策略/SCHEMA.md
~/wikis/技术笔记/SCHEMA.md
```

Specify the knowledge base when querying, or let the agent automatically determine which one to search based on the question.

## Scale Boundaries

- **Up to 10,000 articles:** summaries.md can be read in one pass — no additional infrastructure needed
- **Beyond 10,000 articles:** Vector search is needed for coarse filtering; summary data can be used directly for embedding
- Don't design for massive scale prematurely — add it when you get there
