---
name: frontend-slides
description: Create Securitas-branded HTML presentations from scratch or by converting PowerPoint files. Use when the user wants a clean internal slide deck that stays inside the Securitas visual system.
---

# Frontend Slides

Create zero-dependency, Securitas-branded HTML presentations that run entirely in the browser.

## Core Principles

1. **Zero Dependencies** — Output a single HTML file with inline CSS and JavaScript.
2. **Brand First** — Keep decks inside the Securitas visual system by default.
3. **Viewport Safe** — Every slide must fit within `100vh` with no internal scrolling.
4. **Practical Output** — Favor clear internal communication over novelty.

## Brand Mode

This repo is Securitas-first. Do not explore unrelated visual styles unless the user explicitly asks to break the brand.

Always:

- Read [html-template.md](html-template.md) first because it now contains the Securitas brand defaults and structural starter
- Read [viewport-base.css](viewport-base.css) and include its full contents in every presentation
- Use [animation-patterns.md](animation-patterns.md) only for restrained motion

Default visual behavior:

- Deep marine background
- Keep the background flat navy by default on standard content slides; do not introduce gradients unless the user asks for a more expressive treatment
- White typography
- Restrained red accents
- Use the official Securitas logotype or symbol assets when branding is needed; never draw a fake logo badge
- Support a real logotype lockup on cover slides instead of a generic framed logo panel
- Use top-right three-dot marker and strongly left-aligned layouts mainly for supporting/content slides, not as a mandatory title-slide treatment
- Subtle 180-320ms reveal motion

## Viewport Fitting Rules

These are non-negotiable:

- Every `.slide` must use `height: 100vh; height: 100dvh; overflow: hidden;`
- All font sizes and spacing must use `clamp()`
- Content containers need max-height constraints
- Images must use `max-height: min(50vh, 400px)`
- Include height breakpoints for 700px, 600px, and 500px
- Include `prefers-reduced-motion`

If content does not fit, split it into more slides. Never cram and never allow scrolling inside a slide.

## Content Density Limits

| Slide Type | Maximum Content |
| --- | --- |
| Title slide | 1 heading + 1 subtitle + optional tagline |
| Content slide | 1 heading + 4-6 bullet points or 2 short paragraphs |
| Feature grid | 1 heading + 6 cards maximum |
| Image slide | 1 heading + 1 image |
| Quote slide | 1 quote + attribution |

If the requested content exceeds these limits, automatically split it across additional slides.

## Title Slide Pattern

When the user asks for a presentation cover or title slide, do not default to a generic split-screen corporate hero.

Prefer this cover-slide composition unless the user asks otherwise:

- one dominant title set large, with the title acting as the hero element
- centered or near-centered headline placement when it improves impact and matches the user reference
- small supporting text such as date, subtitle, or context anchored to an outer edge of the grid rather than stacked directly under the title by default
- the real Securitas logotype used as a brand sign-off, not a placeholder badge inside a card
- minimal chrome on the cover: no accent orb, no boxed logo panel, no utility-card treatment unless explicitly requested
- brand expression can come from composition, typography, and official background assets, not from extra interface furniture

Brand-specific title-slide rules:

- If licensed brand assets are available, prefer `Securitas Pro Bold` for the title and `Securitas Pro Regular` for supporting text; if not, use the closest clean fallback already available in the project
- For dark backgrounds, use the official Red + White logotype version from the Securitas brand portal
- Never alter, redraw, recolor, or typeset the logotype manually; use the original asset as provided
- If the user wants a more expressive cover, use the official Perspective Dot Space as a background or lower-field motif because the brand portal explicitly calls it appropriate for presentation title pages and expressive backdrops
- When using Dot Space behind text, maintain a calm area for legibility and avoid placing the headline over the densest part of the pattern
- Follow the brand system’s grid logic using multiples of three; for presentation layouts, prefer a 12-column structure

Avoid these title-slide mistakes:

- framed mock-logo cards
- bottom-right red callout circles on cover slides
- treating the cover like a content slide with kicker, title, and paragraph all in one left column by default
- adding decorative UI panels that compete with the title
- centering everything mechanically; match the reference composition instead of forcing the generic template

## Closing Slide Pattern

For the final slide, prefer a clean brand sign-off rather than another content layout.

Default closing-slide behavior:

- flat navy background with no gradient treatment
- centered official Securitas logotype as the primary and usually only element
- no mock logo badge, no container card, and no extra chrome around the mark
- no heading, subheading, or thank-you sentence unless the user explicitly asks for copy on the final slide
- hide or remove supporting UI such as navigation dots, progress bars, and corner branding when the final sign-off slide is in view

Brand-specific closing-slide rules:

- use the official logotype asset from the Securitas brand portal or other approved brand source
- on dark backgrounds, use the official Red + White logotype version
- keep generous clearspace around the mark and size it for calm prominence rather than maximum scale

## Standard Content Slide Pattern

For regular one-column and two-column slides, use the slide width as a true working canvas rather than building everything inside a narrow left rail.

Prefer this for standard content slides unless the user asks otherwise:

- the three-dot brand marker in the top-right as the default branding treatment
- size the top-right dot mark with enough presence to feel intentional, roughly matching the scale a small corner logotype would have occupied
- do not place a Securitas wordmark or full logotype in the top-left on standard content slides unless the user explicitly asks for it
- one heading plus one optional subheading
- no eyebrow or kicker text above the heading unless the user explicitly asks for section labels
- flat navy background without gradients by default
- content blocks aligned to a 12-column grid with wide usable text spans

One-column slide rules:

- allow the main text area to stretch broadly across the page while preserving comfortable side margins
- do not cap the one-column body to a narrow 55-65% text column by default
- use full-width paragraphs, bullets, or panels when the reference indicates a wide reading line
- keep plenty of vertical air between heading, subheading, and body content

Two-column slide rules:

- the two columns should collectively span most of the slide width
- maintain clear outer margins and a strong gutter between columns
- avoid clustering both columns into the left half of the slide
- balance column widths unless the content clearly demands asymmetry

Avoid these content-slide mistakes:

- section-kicker text like “ONE-SECTION LAYOUT” above the heading by default
- top-left Securitas wordmarks or logotypes added automatically on standard content slides
- content shells that stop halfway across the slide without a reason
- oversized decorative panels that make the layout feel cramped
- decorative gradients on standard business slides when the user asked for a navy field
- tiny top-right dots that look accidental, or oversized branding that competes with the content

## Phase 0: Detect Mode

Determine which workflow applies:

- **New Presentation** — create from scratch
- **PPT Conversion** — convert an existing `.pptx`
- **Enhancement** — modify an existing HTML deck

## Phase 1: Content Discovery

For new presentations, gather:

- presentation purpose
- approximate length
- whether content is complete or rough
- whether images are involved
- whether the user wants in-browser editing support

If the user already gave clear slide content, do not over-question them. Move directly into structure and generation.

## Phase 1.2: Image Evaluation

If the user provides images:

1. Scan the image files
2. Review them visually
3. Decide which are usable
4. Build the outline around the best assets

Keep screenshots and diagrams framed as utility panels, not decorative hero art.

## Phase 2: Structure The Deck

Before writing the final HTML:

- outline the slide flow
- keep each slide within density limits
- choose layouts that match the Securitas brand system
- give the opening slide its own composition rules instead of reusing a standard content-slide shell
- treat one-column and two-column slides as full-width grid layouts, not narrow text islands
- decide the editable content schema before writing slide markup
- favor two-column business layouts, utility panels, dividers, and one clear callout when needed

## Content Management Pattern

Default to a self-contained editing model that lets users update copy without regenerating the whole deck.

Preferred approach:

- keep the presentation as a single HTML file
- place one inline JSON content block near the top of the HTML, typically inside `<head>` or immediately after `<body>`
- use a block such as `<script id="deck-data" type="application/json">...</script>` for all editable copy
- keep slide HTML focused on layout and placeholders, not hardcoded copy
- use JavaScript to read the inline JSON and populate the relevant text nodes on load

Why this is the default:

- users can update one content block instead of hunting through the document
- the deck stays portable because there is still only one file
- it avoids browser issues that often come with fetching local external JSON files from `file://`
- layout, styling, and content stay clearly separated

Implementation rules:

- every generated deck should have a clearly labeled editable content block near the top of the file
- use stable keys grouped by slide, for example `title`, `oneColumn`, `twoColumn`, `closing`
- arrays are appropriate for repeating items such as cards, bullets, text boxes, or column entries
- bind content into the DOM through predictable selectors or `data-*` attributes
- keep fallback text minimal and clearly identifiable as placeholders
- do not scatter editable copy throughout the slide markup when the same content can live in the JSON block

Avoid by default:

- external JSON files loaded with `fetch()` for ordinary local decks
- requiring users to edit multiple parts of the document for one slide
- mixing structural HTML and editable copy in a way that makes manual editing risky
- storing most text only inside JavaScript template strings when a clean JSON block would be easier to edit

## Phase 3: Generate Presentation

Before generating, read:

- [html-template.md](html-template.md)
- [viewport-base.css](viewport-base.css)
- [animation-patterns.md](animation-patterns.md)

Requirements:

- single self-contained HTML file
- inline CSS and JavaScript
- one inline JSON content block near the top of the file for editable copy unless the user explicitly prefers hardcoded markup
- full `viewport-base.css` contents embedded in the `<style>` block
- webfont loading only when needed
- clear comments for major sections

Implementation rules:

- use licensed `Securitas Pro` assets when they are available in the project; otherwise use the closest approved or user-accepted fallback already established for the deck
- keep motion subtle and fast
- avoid neon, pastel, glassmorphism, playful rounded UI, and experimental motifs
- use red as a signal color, not a dominant canvas
- when the user provides a visual reference, match the composition first and only then fill in the copy structure
- for standard slides, prefer flat navy backgrounds and wide horizontal layouts before adding decorative treatments
- separate content from layout by reading slide copy from the inline JSON content block when feasible

## Phase 4: PPT Conversion

When converting PowerPoint files:

1. Run `python scripts/extract-pptx.py <input.pptx> <output_dir>`
2. Present the extracted slide titles, content, images, and notes to the user
3. Rebuild the deck in the Securitas HTML style
4. Preserve slide order and images from `assets/`

## Phase 5: Delivery

After generating:

1. Clean up temporary preview files if any were created
2. Open the resulting HTML file in the browser
3. Tell the user:
   - where the file is
   - how many slides it contains
   - how to navigate it
   - which content block or keys to edit if they want to customize text without changing layout

## Phase 6: Export (Optional)

If the user wants a PDF:

1. Run:

   ```bash
   bash scripts/export-pdf.sh <path-to-html> [output.pdf]
   ```

2. Explain that:
   - the PDF is a static snapshot
   - animations and interactivity are not preserved
   - the file is suitable for email, Slack, printing, and documents

If the PDF is too large, re-run with:

```bash
bash scripts/export-pdf.sh <path-to-html> [output.pdf] --compact
```

## Supporting Files

| File | Purpose | When to Read |
| --- | --- | --- |
| [viewport-base.css](viewport-base.css) | Mandatory responsive CSS | During generation |
| [html-template.md](html-template.md) | HTML scaffold, interaction patterns, and Securitas brand defaults | During generation |
| [animation-patterns.md](animation-patterns.md) | Motion reference | During generation |
| [scripts/extract-pptx.py](scripts/extract-pptx.py) | PPT extraction helper | PPT conversion only |
| [scripts/export-pdf.sh](scripts/export-pdf.sh) | PDF export helper | Export only |
