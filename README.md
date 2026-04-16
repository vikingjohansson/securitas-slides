# Securitas Slides

A lightweight Claude Code skill for creating Securitas-branded HTML presentations and converting PowerPoint decks into the same format.

This repo is maintained at `git@github.com:vikingjohansson/securitas-slides.git`.

Credit: this project builds on the original `frontend-slides` work by [zarazhangrui](https://github.com/zarazhangrui).

## What This Repo Is

This repo is a focused internal starter for:

- Securitas-branded HTML slide decks
- PPTX-to-HTML conversion
- viewport-safe, dependency-free presentations
- optional PDF export

## What Stays In The Repo

- `SKILL.md` — the Claude skill workflow and generation rules
- `viewport-base.css` — the mandatory viewport-fitting CSS
- `html-template.md` — the structural HTML/CSS/JS starter
- `animation-patterns.md` — restrained motion guidance
- `scripts/extract-pptx.py` — PowerPoint extraction helper
- `scripts/export-pdf.sh` — optional PDF export helper

## Installation

### Clone This Repo

```bash
git clone git@github.com:vikingjohansson/securitas-slides.git
cd securitas-slides
```

### Install In Codex From GitHub

```bash
python ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo vikingjohansson/securitas-slides \
  --path . \
  --name securitas-slides
```

Then restart Codex and use `/securitas-slides`.

### Install In Claude Code Manually

```bash
mkdir -p ~/.claude/skills/securitas-slides/scripts

cp SKILL.md viewport-base.css html-template.md animation-patterns.md ~/.claude/skills/securitas-slides/
cp scripts/extract-pptx.py scripts/export-pdf.sh ~/.claude/skills/securitas-slides/scripts/
```

Then use `/securitas-slides` in Claude Code.

## Usage

### Create a New Deck

```text
/securitas-slides

> Create a Securitas-branded internal presentation about AI enablement
```

The skill will gather the content, keep the design inside the Securitas brand system, and generate a single self-contained HTML presentation.

### Convert a PowerPoint

```text
/securitas-slides

> Convert presentation.pptx to a Securitas-branded HTML slideshow
```

The skill will extract text, images, and notes from the PPTX, then rebuild the deck as a branded HTML presentation.

### Export to PDF

```bash
bash scripts/export-pdf.sh ./presentation.html
bash scripts/export-pdf.sh ./presentation.html ./presentation.pdf
```

## Repo Structure

| File | Purpose |
| --- | --- |
| `SKILL.md` | Core Claude workflow and rules |
| `viewport-base.css` | Required responsive CSS for 100vh slides |
| `html-template.md` | HTML scaffold plus Securitas brand defaults |
| `animation-patterns.md` | Brand-safe animation reference |
| `scripts/extract-pptx.py` | PPTX extraction to JSON + assets |
| `scripts/export-pdf.sh` | Render slides to PDF |

## Requirements

- Claude Code
- For PPT conversion: Python + `python-pptx`
- For PDF export: Node.js

## License

MIT
