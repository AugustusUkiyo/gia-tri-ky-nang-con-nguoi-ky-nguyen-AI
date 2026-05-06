# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A static website (no build step, no server) exploring core human values and skills in the AI era. Two language versions:

- **Vietnamese:** `gia-tri-ky-nang-con-nguoi-ky-nguyen-AI.html` (~5700 lines)
- **English:** `core-values-and-human-skills-in-the-ai-era.html`

Markdown source of truth: `content/gia-tri-ky-nang-con-nguoi-ky-nguyen-AI-final.md` (1674 lines). When HTML and `final.md` differ in factual content, `final.md` is correct.

## Opening the Website

```bash
open gia-tri-ky-nang-con-nguoi-ky-nguyen-AI.html   # macOS
```

No build, no npm, no server needed. Open directly in a browser.

## Architecture: 3-Mode Reading System

The HTML uses `data-mode` on `<body>` to control visibility via CSS:

```css
body[data-mode="summary"]   .only-full, .only-practical { display: none !important; }
body[data-mode="practical"] .only-full, .only-summary   { display: none !important; }
body[data-mode="full"]      .only-summary, .only-practical { display: none !important; }
```

Every content block must carry one of three classes: `only-summary`, `only-full`, or `only-practical`. Headings (`h2`–`h5`) are never tagged — they always display. Untagged `<p>` and `<div>` elements default to visible in all modes.

## Content Structure

The Vietnamese HTML covers five main parts plus appendices, keyed by `id` on `<article>` and `<section>` elements:

- `i-1` → `i-6` — 6 core values (Phần I)
- `ii-1` → `ii-12` — 12 skills (Phần II)
- `iii-1` → `iii-4` — Jagged Frontier / Human–AI collaboration (Phần III)
- `iv-intro`, `iv-1` → `iv-11` — AI Agents (Phần IV)
- `v-1` → `v-5` — Advanced frameworks (Phần V)
- `pa-1` → `pa-3`, `appendix` — Appendices (always visible, no mode tagging needed)

## Design System Constraints

All HTML edits must use the existing CSS variables — never introduce new colors or fonts:

- Colors: `--ink`, `--ink-3`, `--copper`, `--bg-0`, `--bg-1`, `--rule`
- Fonts: Instrument Serif, Fraunces (headings/prose), Geist (UI/code)
- Existing component classes: `tldr only-summary`, `de-card only-practical`, `scenario-card only-practical`, `de-prompt`

Do not touch CSS variables, the color scheme, filter SVGs, or layout structure when adding content.

## JavaScript (single `<script>` block, ~dòng 5502)

The existing script handles: mode switching, TOC drawer, keyboard shortcuts (`ESC`, `Cmd/Ctrl+K`), scroll progress (IntersectionObserver), current section tracker, smooth scroll, reveal-on-scroll, and copy buttons on `.de-prompt` blocks. New JS must be appended to this block — do not create a separate script.

## HTML Templates for New Content

**TLDR card (mode: Summary):**
```html
<div class="tldr only-summary">
  <span class="tldr-label">TL;DR · Tóm tắt</span>
  <p>[1–2 sentences, max 50 words. Stats must be traced to final.md.]
  <strong>Giữ được: [one capability sentence]</strong>.</p>
</div>
```

**Practice card (mode: Practice):**
```html
<div class="de-card only-practical">
  <div class="de-card-head">
    <span class="de-card-tag">[Role · Context]</span>
    <h4>[Skill] — áp dụng ngay</h4>
  </div>
  <div class="de-card-body">
    <p><strong>Bối cảnh:</strong> [2–3 sentence workplace scenario]</p>
    <p><strong>3 bước làm ngay:</strong></p>
    <p>1. [...]<br>2. [...]<br>3. [...]</p>
    <div class="de-prompt">
      <span class="de-prompt-label">Prompt · [name]</span>
      [2–4 sentence Vietnamese prompt, paste-ready for Claude/ChatGPT]
    </div>
  </div>
</div>
```

## Anti-Fabrication Rule

Never introduce new statistics, claims, or citations that aren't already in `final.md`. All numbers in TLDR cards, practice cards, and scenarios must trace back to a specific line in `final.md`.

## Workflow Guidance

- Apply changes one section at a time, verify in browser, then proceed. Do not batch multiple sections in one edit.
- When adding `only-full` to untagged elements: touch `<p>` and `<div>` only — never headings.
- The `plan/` directory contains implementation plans and audit reports. The `archived/` directory holds reference materials (old plans, backups) — do not edit these.
- `test-artifacts/` holds test scripts (`.mjs`) and reports from past audit rounds.
