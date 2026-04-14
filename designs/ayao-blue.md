# Design System Inspired by Ayao (Royal Blue Edition)

## 1. Visual Theme & Atmosphere

Ayao's design system is a book rendered as a webpage — composed, spacious, and quietly authoritative. The entire experience is built on a pure white canvas (`#FFFFFF`) with a single accent color: Royal Blue (`#1E40AF`), used with discipline as the sole chromatic element across the entire system. Where most design systems rely on multiple accent colors, Ayao commits to one — and that commitment creates an unmistakable identity.

The typography is dual-stack: Noto Sans SC for Chinese text and Inter for Latin characters, with JetBrains Mono for code. Display headlines run at 36pt weight 900 (Black) with tight letter-spacing (-1px), creating dense, commanding title blocks. Body text breathes at 10.5pt with an unusually generous 1.8 line-height — closer to a printed book than a typical web page. This relaxed density is the signature feel: content never crowds itself.

What makes Ayao distinctive is its radical restraint. No shadows anywhere (one micro-exception for QR codes). No gradients (one subtle cool gradient reserved for cover and back pages only). No rounded corners beyond 8px. The depth comes entirely from the interplay between the Royal Blue accent, four tiers of gray text, and generous whitespace. Functional cards — tips (green), warnings (red), callouts (yellow) — provide semantic color, but the brand expression is always monochromatic blue on white.

**Key Characteristics:**
- Pure white canvas with Royal Blue (`#1E40AF`) as the sole accent color
- Dual-stack typography: Noto Sans SC (Chinese) + Inter (Latin) + JetBrains Mono (code)
- Book-grade spacing: 1.8 line-height, generous margins, A4-inspired proportions
- Weight 900 (Black) for display titles — maximum density, maximum impact
- 3px Royal Blue section dividers as the primary structural element
- Decorative cover lines: 80px royal blue + 30px light blue (`#93C5FD`) top-left corner
- Zero shadows, zero gradients (except cover/back page cool gradient)
- Section numbering with `#01` `#02` format in Inter font, Royal Blue color
- Print-friendly: `page-break-inside: avoid` on all card components

## 2. Color Palette & Roles

### Primary
- **Royal Blue** (`#1E40AF`): The core and only accent color — used for section dividers, badges, step numbers, links, flow chart borders, and comparison headers. Deliberately deep and confident.
- **Pure White** (`#FFFFFF`): Page background and content surface.
- **Near Black** (`#1A1A1A`): Primary text color — close to pure black but slightly softened for comfortable reading.

### Text Hierarchy
- **Primary Text** (`#1A1A1A`): Main body text and headings.
- **Secondary Text** (`#6B6B6B`): Dates, descriptions, subtitles, and de-emphasized content.
- **Tertiary Text** (`#9CA3AF`): English subtitles, disclaimers, and weakest text tier.
- **Quaternary Text** (`#B0B0B0`): Copyright notices, back page footer — the absolute minimum presence.

### Surface & Background
- **Cover Gradient**: `linear-gradient(135deg, #F5F8FF 0%, #EBF2FF 25%, #FFFFFF 100%)` — an extremely subtle cool blue tint, used only on cover and back pages.
- **Accent Light** (`#EFF6FF`): Light blue tint for highlighted content blocks.
- **Code Background** (`#F5F5F0`): Warm-gray surface for code blocks and file trees.

### Semantic Colors
- **Tip Green** (`#22C55E`): Left border for tip/advice cards. Background: `#F0FDF4`.
- **Warning Red** (`#EF4444`): Left border for warning cards. Background: `#FEF2F2`.
- **Highlight Yellow** (`#FDE68A`): Border for callout cards. Background: `#FFFBEB`.

### Borders
- **Default Border** (`#E5E5E5`): Standard dividers, table borders, card borders.
- **Section Line** (`#1E40AF`): 3px top border on section headings — the primary structural accent.
- **Decorative Light Blue** (`#93C5FD`): Secondary cover decoration line.

## 3. Typography Rules

### Font Family
- **Primary (Chinese)**: `Noto Sans SC`, with fallback: `Inter, -apple-system, system-ui, sans-serif`
- **Latin/Numbers**: `Inter`, with fallback: `sans-serif`
- **Code**: `JetBrains Mono`, with fallback: `monospace`

### Hierarchy

| Role | Font | Size | Weight | Line Height | Letter Spacing | Notes |
|------|------|------|--------|-------------|----------------|-------|
| Cover Title | Noto Sans SC | 36pt | 900 (Black) | 1.15 | -1px | Maximum impact, book-title density |
| Section Heading (h2) | Noto Sans SC | 20pt | 700 | default | normal | 3px Royal Blue top border |
| Section Number | Inter | 18pt | 700 | default | normal | `#01` format, Royal Blue color |
| English Subtitle | Inter | 10pt | 400 italic | default | normal | Color: `#9CA3AF` |
| Section Intro | Noto Sans SC | 11.5pt | 300 | default | normal | Color: `#6B6B6B` |
| Sub-heading (h3) | Noto Sans SC | 13pt | 600 | default | normal | Optional English annotation at 9.5pt |
| Sub-heading Small (h4) | Noto Sans SC | 11pt | 600 | default | normal | — |
| Body | Noto Sans SC | 10.5pt | 400 | 1.8 | normal | The signature relaxed reading experience |
| Table Body | Noto Sans SC | 9.5pt | 400 | default | normal | — |
| Table Header | Noto Sans SC | 8.5pt | 600 | default | 0.5px | Color: `#6B6B6B` |
| Code Block | JetBrains Mono | 9pt | 400 | 1.6 | normal | Background: `#F5F5F0` |
| Inline Code | JetBrains Mono | 9pt | 400 | default | normal | Background: `#F5F5F0`, padding: 1px 4px |
| Card Label | Noto Sans SC | 8.5pt | 700 | default | 0.5px | Functional card type labels |
| Card Body | Noto Sans SC | 10pt | 400 | default | normal | — |
| Footer | Noto Sans SC | 8.5pt | 400 | default | normal | Color: `#6B6B6B` |

### Principles
- **Weight 900 only for cover titles**: The heaviest weight in the system creates a singular moment of maximum density. All other headings use 600-700.
- **1.8 body line-height is non-negotiable**: This generous spacing is the core of the book-grade reading experience. Do not compress it.
- **Inter for all numbers and Latin**: Even within Chinese text, numbers and Latin characters should render in Inter for optical consistency.
- **Section numbering in Inter**: The `#01` format uses Inter bold in Royal Blue — creating a typographic anchor that ties structure to brand.

## 4. Component Stylings

### Section Dividers
- 3px solid Royal Blue (`#1E40AF`) top border on every h2
- The primary visual rhythm element — every new section announces itself with a royal blue line

### Cover
- Background: subtle cool blue gradient
- Top-left decoration: two horizontal lines (80px royal blue + 30px light blue)
- Badge: Royal Blue background, white text, 3px radius
- Title `<em>` renders in Royal Blue for emphasis
- Vertically centered layout

### Tip Cards
- Background: `#F0FDF4` (light green)
- Left border: 4px solid `#22C55E`
- Right border-radius: 8px
- Auto-label via `::before` pseudo-element

### Warning Cards
- Background: `#FEF2F2` (light red)
- Left border: 4px solid `#EF4444`
- Right border-radius: 8px
- Auto-label via `::before` pseudo-element

### Callout Cards
- Background: `#FFFBEB` (light yellow)
- Border: 1px solid `#FDE68A`
- Border-radius: 8px (all sides)
- `<strong>` text renders in Royal Blue

### Step Components
- Number: 26px circle, Royal Blue background, white text, Inter font
- Horizontal layout (flexbox) with 12px gap
- `page-break-inside: avoid`

### Flow Charts
- Horizontal centered layout with flex-wrap
- Nodes: white background, 2px Royal Blue border, 8px radius, Royal Blue text
- Arrows: light gray `#D1D5DB`, 18pt, weight 300

### Comparison Boxes
- Two-column grid (1fr 1fr)
- Good side: green background + border, auto-label
- Bad side: red background + border, auto-label

### Tables
- Full width, collapsed borders
- Header: `#F5F5F0` background, 8.5pt, 600 weight, 2px bottom border
- Cells: 9px 12px padding, 1px bottom border
- Last row: no bottom border

### Code Blocks
- Background: `#F5F5F0` (warm gray)
- Border: 1px `#E5E5E5`
- Border-radius: 8px
- JetBrains Mono, 9pt, line-height 1.6

## 5. Layout Principles

### Page Dimensions
- A4-inspired proportions
- Content padding: 0 55px (left/right)
- Print margins: 18mm top/bottom

### Spacing System

| Element | Spacing | Notes |
|---------|---------|-------|
| Paragraph | `margin-bottom: 10px` | — |
| Section heading (h2) | `margin-top: 45px; margin-bottom: 6px; padding-top: 18px` | Generous breathing room above |
| Sub-heading (h3) | `margin-top: 24px; margin-bottom: 8px` | — |
| Sub-heading small (h4) | `margin-top: 16px; margin-bottom: 6px` | — |
| Cards | `margin: 14px 0; padding: 12px 16px` | — |
| Code blocks | `margin: 10px 0 14px; padding: 12px 16px` | — |
| Tables | `margin: 14px 0` | — |
| Lists | `margin: 6px 0 12px 22px` | List items: `margin-bottom: 3px` |

### Whitespace Philosophy
- **Book-grade pacing**: Every section heading gets 45px top margin — creating distinct "chapter" breaks.
- **Content never crowds**: The 1.8 line-height and generous card margins ensure no element feels compressed.
- **Print-first thinking**: All card components use `page-break-inside: avoid` for clean PDF output.

## 6. Depth & Elevation

| Level | Treatment | Use |
|-------|-----------|-----|
| Flat (Level 0) | No shadow, no border | Page background, body text |
| Contained (Level 1) | `1px solid #E5E5E5` | Tables, code blocks, file trees |
| Semantic (Level 2) | `4px left border` in semantic color | Tip/warning cards |
| Accent (Level 3) | `3px top border` in Royal Blue | Section headings |
| Structural (Level 4) | `2px solid #1E40AF` | Flow chart nodes, comparison boxes |

**Shadow Philosophy**: Ayao uses **zero shadows**. Depth is communicated entirely through borders, background color shifts, and whitespace. The only exception is a micro-shadow (`0 8px 30px rgba(0,0,0,0.08)`) on QR code wrappers in back pages. This no-shadow discipline creates a flat, print-like quality that reinforces the book metaphor.

## 7. Do's and Don'ts

### Do
- Use Royal Blue (`#1E40AF`) as the only accent color — no exceptions
- Maintain 1.8 line-height for all body text
- Use 3px Royal Blue top borders for section headings
- Use Inter font for all numbers, Latin text, and section numbering
- Use weight 900 only for cover titles — weight 700 for section headings, 600 for sub-headings
- Keep generous whitespace between sections (45px+ above h2)
- Apply `page-break-inside: avoid` on all card components for print quality
- Use semantic colors only for functional cards: green (tips), red (warnings), yellow (callouts)

### Don't
- Don't introduce additional accent colors — the single-color discipline is the identity
- Don't use shadows — depth comes from borders and whitespace
- Don't use gradients except on cover and back pages
- Don't compress line-height below 1.6 for any text element
- Don't use border-radius larger than 8px (except step number circles at 50%)
- Don't use bold (800+) weight for anything other than cover titles
- Don't mix Noto Sans SC into contexts where Inter should be used (numbers, section numbers)
- Don't add decorative elements — the design is reductive by nature

## 8. Responsive Behavior

### Breakpoints
| Name | Width | Key Changes |
|------|-------|-------------|
| Mobile | <480px | Single column, reduced padding (20px), title scales to 24pt |
| Tablet | 480-768px | Content padding 35px, title scales to 30pt |
| Desktop | 768px+ | Full A4-inspired layout, 55px padding, 36pt titles |

### Collapsing Strategy
- **Comparison boxes**: 2-column grid → stacked single column on mobile
- **Flow charts**: Horizontal → wrapped with flex-wrap
- **Step components**: Maintain horizontal layout, reduce gap
- **Tables**: Horizontal scroll on mobile
- **Cover decoration lines**: Hide on mobile

## 9. Agent Prompt Guide

### Quick Color Reference
- Brand Accent: "Royal Blue (#1E40AF)"
- Page Background: "Pure White (#FFFFFF)"
- Primary Text: "Near Black (#1A1A1A)"
- Secondary Text: "Gray (#6B6B6B)"
- Tertiary Text: "Light Gray (#9CA3AF)"
- Section Divider: "3px solid #1E40AF"
- Code Background: "Warm Gray (#F5F5F0)"
- Cover Gradient: "linear-gradient(135deg, #F5F8FF 0%, #EBF2FF 25%, #FFFFFF 100%)"

### Example Component Prompts
- "Create a cover page with cool blue gradient background, 80px+30px royal blue decoration lines top-left, title at 36pt weight 900, Royal Blue badge, and vertically centered layout."
- "Create a section heading with 3px Royal Blue top border, #01 numbering in Inter bold royal blue, 20pt Noto Sans SC weight 700, and optional italic English subtitle in #9CA3AF."
- "Create a tip card with #F0FDF4 background, 4px green left border, 8px right border-radius, and auto-generated label."
- "Create a price display with Inter font, 72px weight 800, Royal Blue color, centered on white background with section divider above."
- "Create a step component with 26px Royal Blue circle number, white text, Inter font, horizontal flex layout with 12px gap."
