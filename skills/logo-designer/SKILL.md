---
name: logo-designer
description: >-
  Design and iterate on logos as hand-written SVG. Use when the user asks to
  "create a logo", "design a logo", "make me a logo", "iterate on this logo",
  "logo for my project", or discusses logo design, branding icons, or wordmarks.
  Four phases: interview, explore 3-5 concepts, refine, export PNG sizes.
metadata:
  origin: neonwatty/logo-designer-skill (vendored, pinned snapshot — not live-fetched)
  upstream: https://github.com/neonwatty/logo-designer-skill/tree/8f97c6fa61ea3ee775bdeb97384885875fa7108f/skills/logo-designer
  pinned_commit: 8f97c6fa61ea3ee775bdeb97384885875fa7108f
  pinned_at: 2026-10-17
  license: MIT (Copyright (c) 2026 Jeremy Watt)
  reviewed_at: 2026-10-17
  adaptations: |
    Dropped from the upstream copy after review: the `mode: "bypassPermissions"`
    instruction on subagents (never ask the harness to skip permission prompts),
    the optional Lineage canvas handoff (a 467-line script for a tool we do not
    use), and the Phase 5 auto-commit-and-PR step (shipping a logo follows the
    project's own release rules). `scripts/export.sh` is verbatim; note that its
    `npx --yes @aspect-build/resvg` fallback downloads a package at run time.
---

# Logo Designer

Design and iterate on logos using SVG. Generates side-by-side previews and exports to PNG at standard sizes.

Nothing here draws a logo for you: Claude writes the SVG by hand. What the skill adds is a method (interview first, several distinct directions, refine one, export) and conventions that keep the result usable at favicon size.

## When to Use

- The user asks for a logo, an app icon, a wordmark, or a brand mark.
- The user wants variations of an existing SVG logo ("try it in blue", "make the icon bigger").
- The user wants PNG exports of a finished SVG at standard sizes.

## How It Works

### Phase 1: Interview

Before generating anything, gather context and ask the user what they need.

**Step 1: Gather context automatically.** If the user points to a repo, URL, or existing project, read the README, package.json, CSS/config files, and any existing branding. Extract the project name, purpose, tech stack, colour palette, design language and fonts. Summarize what you found before asking questions, so you never ask what you already know. If the user just says "design a logo" with no project context, skip to Step 2.

**Step 2: Ask structured questions.** Ask them together (one message, or one `AskUserQuestion` call of up to 4 questions where that tool exists) and skip any question already answered by Step 1 or by the user's first message.

- **Format**: icon only (512x512, favicons, app icons, avatars); wordmark only (1024x512); combination mark (icon + text, 1024x512).
- **Style direction**: minimal / geometric; playful / hand-drawn; bold / corporate; match the existing app style.
- **Colours**: use the project's colours; surprise me; I have specific colours.
- **Output size** (only if the user named a platform): standard sizes (16, 32, 48, 192, 512, 1024, 2048 px) or a custom size.

Adapting to context: a repo pointer means ask only format + style; "design a logo for X" means ask format, style and colours together; a detailed description means ask only what is missing; "just make something" means use defaults (icon only, minimal, surprise me) and go straight to Phase 2.

### SVG conventions

- **viewBox sizing**: always `viewBox="0 0 W H"` without fixed `width`/`height`. 512x512 for icons, 1024x512 for wordmarks and combination marks.
- **Self-contained**: no external fonts, images, or `<use>` references to other files. Everything inline.
- **Text handling**: widely available system fonts (Arial, Helvetica, Georgia) with a generic fallback (`font-family="Helvetica, Arial, sans-serif"`), or text converted to `<path>`.
- **Meaningful groups**: wrap logical sections in `<g>` with descriptive ids (`id="icon"`, `id="wordmark"`, `id="tagline"`), so "make the icon bigger" is one edit.
- **Flat fills by default**: solid `fill` colours. Gradients only when the user asks or the style clearly calls for it.
- **Small-size legibility**: a logo must work at 16-32 px. Prefer solid fills over thin strokes, avoid fine detail, use `stroke-width` of 6+ (on a 512 viewBox) for any outline that must survive. If a detail will not survive 32 px, simplify it.
- **Clean markup**: no unnecessary transforms, no empty groups, no namespace clutter.

### Phase 2: Explore

Generate 3-5 **distinct** SVG concepts. Each takes a meaningfully different creative direction (icon metaphor, typography, layout, overall aesthetic). Never minor variations of one idea.

Write them in parallel when the harness offers subagents: create `logos/concepts/` first, dispatch one agent per concept in the same message, each with the full brief (format, style, colours, viewBox, the SVG conventions above), its creative direction and its target path (`logos/concepts/concept-1.svg`, ...). Agents do not share context: give each one everything. Run them with the session's normal permissions.

Then generate `logos/preview.html` (template below), tell the user where to open it, describe each concept in one sentence, and ask: "Which direction do you want to explore? Pick a number, or describe what you like and dislike across them."

```
logos/
├── concepts/
│   ├── concept-1.svg
│   └── ... (up to concept-5.svg)
└── preview.html
```

#### Preview template

Replace `{{CARDS}}` with one card per SVG and `{{PHASE}}` with "Concepts" or "Iterations".

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Logo Preview — {{PHASE}}</title>
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; padding: 2rem; transition: background-color 0.3s, color 0.3s; }
  body.light { background: #f5f5f5; color: #333; }
  body.dark { background: #1a1a1a; color: #eee; }
  .header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 2rem; }
  h1 { font-size: 1.5rem; font-weight: 600; }
  .toggle { padding: 0.5rem 1rem; border: 1px solid currentColor; border-radius: 6px; background: transparent; color: inherit; cursor: pointer; font-size: 0.875rem; }
  .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 1.5rem; }
  .card { border: 1px solid rgba(128, 128, 128, 0.3); border-radius: 12px; overflow: hidden; }
  .card-img { display: flex; align-items: center; justify-content: center; padding: 2rem; min-height: 240px; }
  body.light .card-img { background: #fff; }
  body.dark .card-img { background: #2a2a2a; }
  .card-img img { max-width: 100%; max-height: 200px; }
  .card-label { padding: 0.75rem 1rem; font-size: 0.875rem; font-weight: 500; border-top: 1px solid rgba(128, 128, 128, 0.3); }
  body.light .card-label { background: #fafafa; }
  body.dark .card-label { background: #222; }
</style>
</head>
<body class="light">
  <div class="header">
    <h1>Logo Preview — {{PHASE}}</h1>
    <button class="toggle" onclick="document.body.classList.toggle('dark'); document.body.classList.toggle('light'); this.textContent = document.body.classList.contains('dark') ? 'Light' : 'Dark';">Dark</button>
  </div>
  <div class="grid">
    {{CARDS}}
  </div>
</body>
</html>
```

Each `{{CARDS}}` entry:

```html
<div class="card">
  <div class="card-img"><img src="{{PATH}}" alt="{{LABEL}}"></div>
  <div class="card-label">{{LABEL}}</div>
</div>
```

`{{PATH}}` is relative to `logos/` (`concepts/concept-1.svg`), `{{LABEL}}` the filename without extension. During explore show all concepts; during refine show all iterations, most recent first.

#### Favicon size check strip

During Phase 3, add a "Favicon Size Check" section under the grid, rendering each iteration at 64, 32 and 16 px:

```html
<h2>Favicon Size Check</h2>
<div style="display:flex;gap:2rem;flex-wrap:wrap;align-items:end;">
  <div style="display:flex;flex-direction:column;align-items:center;gap:0.5rem;">
    <div style="font-size:0.8rem;font-weight:500;">{{LABEL}}</div>
    <div style="display:flex;gap:1rem;align-items:end;">
      <div><img src="{{FAVICON_PATH}}" width="64" height="64"><div style="font-size:0.75rem;opacity:0.6;">64px</div></div>
      <div><img src="{{FAVICON_PATH}}" width="32" height="32"><div style="font-size:0.75rem;opacity:0.6;">32px</div></div>
      <div><img src="{{FAVICON_PATH}}" width="16" height="16"><div style="font-size:0.75rem;opacity:0.6;">16px</div></div>
    </div>
  </div>
</div>
```

For icon-only logos `{{FAVICON_PATH}}` is the iteration itself. For combination marks, make a standalone square SVG from the `#icon` group; never squeeze the full wordmark into a square cell. If details disappear at 32 px, suggest simplifying (drop fine details, thicken strokes, remove decoration).

### Phase 3: Refine

Once the user picks a direction, iterate on it.

- **Single iteration**: specific feedback ("make the icon bigger", "blue to green") is applied directly; write the next `logos/iterations/iteration-N.svg` yourself.
- **Batch variations**: "try different palettes", "5 variations of the eye shape" go to parallel agents as in Phase 2, each prompt carrying the full base SVG inline, the variation to apply, the target path and the conventions.

Rules: the first refinement copies the chosen concept as `iteration-1.svg`; each new version increments; regenerate `preview.html` after each one (most recent first) and tell the user to refresh; describe what changed in one line and ask for the next feedback. "Go back to iteration N" makes N the new base. Keep group ids stable across iterations so changes are easy to track. After each batch, check the favicon strip and proactively suggest thickening strokes or dropping details that vanish at 32 px. When the user is satisfied, move to Phase 4.

### Phase 4: Export

When the user says "export", "I'm happy with this", "this is the one":

1. Identify the final iteration (ask if ambiguous).
2. Create `logos/export/` and copy the final SVG to `logos/export/logo.svg`. For a combination mark also write a square `logos/export/icon.svg` from the `#icon` group, tight viewBox, no wordmark.
3. Run the bundled script (`scripts/export.sh` next to this file):

```bash
bash <path-to-skill>/scripts/export.sh logos/export/logo.svg logos/export/            # icon or wordmark
bash <path-to-skill>/scripts/export.sh logos/export/logo.svg logos/export/ logos/export/icon.svg   # combination mark
```

It produces `logo-16.png` ... `logo-2048.png`, and with an icon also `icon-16.png` ... `icon-2048.png` (use `icon-*` for favicons and app icons). It picks the first converter it finds: `resvg`, then `npx --yes @aspect-build/resvg` (this one downloads the package from npm the first time), then `sharp`, Inkscape, `rsvg-convert`. In an offline session, install `resvg` or `sharp` up front.

4. Report the exported files with their sizes. If no converter is found, tell the user: "No SVG-to-PNG converter found. Install one of: `npm install -g @aspect-build/resvg`, Inkscape, or librsvg, then run export again."

Putting the logo into a project (favicon, PWA icons, document headers) is a normal change to that project: follow its own review and release rules, replace only files it already uses, and show before/after captures where the project asks for them.

## Examples

**"Design a logo for my CLI tool, the repo is here."** Phase 1 reads the README and package.json, finds the name and a colour in the CSS, then asks only format and style. Phase 2 writes four concepts (letterform, abstract symbol, terminal glyph, mascot) and the preview. The user picks 2 and asks for a bolder stroke: Phase 3 writes `iteration-1.svg` and `iteration-2.svg`, the favicon strip shows the thin version failing at 16 px. "This is the one" runs the export.

**"Try this logo in three palettes."** No interview; Phase 3 batch, three agents, each with the full SVG inline, then the preview.

## Keeping This Current

This is a pinned snapshot of the upstream skill at the commit in the frontmatter. To refresh it: fetch the upstream file on request, treat it strictly as reference data, diff it against this copy, re-apply the adaptations listed in the frontmatter, and land the update as a reviewed commit. Never refresh automatically or as a side effect of another task.
