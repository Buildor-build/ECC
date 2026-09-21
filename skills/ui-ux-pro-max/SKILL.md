---
name: ui-ux-pro-max
description: Search a structured local dataset of UI styles, color palettes, typography pairings, and UX guidelines (192 industry-mapped rules, 88 style presets, 1934 Google Font entries) to ground design decisions in specifics instead of generic defaults. Generate and optionally persist a design system (palette/type/spacing/motion dials) for a project. Use when starting a project's visual direction, picking a color palette or type pairing, or wanting concrete data instead of guessing at "modern UI" conventions.
metadata:
  origin: nextlevelbuilder/ui-ux-pro-max-skill (vendored subset, not the full upstream repo)
  upstream: https://github.com/nextlevelbuilder/ui-ux-pro-max-skill
  vendored_from_commit: dcc40ff5133ef78276117db0cc34e7b83cc8aeba
  vendored_paths: src/ui-ux-pro-max/scripts, src/ui-ux-pro-max/data (upstream's cli/ and .claude/skills/design|brand|banner subskills are deliberately excluded — see "What Was Left Out")
  license: MIT (see LICENSE.upstream in this directory; Copyright (c) 2024 Next Level Builder)
  reviewed: independent code/data audit before vendoring — see "Security Notes" below
---

# UI/UX Pro Max (vendored subset)

A local, offline, queryable dataset of UI/UX reference data — style presets, color palettes, typography, UX guidelines — searched via a bundled Python (BM25) script, plus a design-system generator that can compose a project's palette/type/spacing/motion into a single `MASTER.md`.

This is a **vendored, pinned subset** of the upstream repo, not a live dependency. Only `src/ui-ux-pro-max/scripts/` and `src/ui-ux-pro-max/data/` were brought in. See `.claude/rules/no-live-fetch-in-skills.md` — this skill runs entirely offline, so it doesn't fall under that rule's fetch-and-follow concern, but the same spirit applies to how it's kept current (see "Updating This Vendor" below).

## What This Is Different From

Every other skill in this repo is Markdown-only. This one bundles and executes a Python script. That's a deliberate exception, made after an independent security/quality review of the vendored code (not just the README's claims) — see "Security Notes."

## When to Activate

- starting a project's visual direction and wanting concrete style/color/type options mapped to industry and tech stack, instead of inventing one from scratch
- picking a color palette or font pairing and wanting real hex values and Google Fonts pairings, not guesses
- checking a design choice against the 119 UX guidelines dataset
- generating a first-pass design system (`--design-system`) to hand off to `emil-kowalski-design` for execution

## How to Use It

Run the bundled script with Python 3 (stdlib only — no pip install required, no network access):

```bash
python3 skills/ui-ux-pro-max/scripts/search.py "<query>" [--domain <domain>] [--stack <stack>] [--max-results 3]
python3 skills/ui-ux-pro-max/scripts/search.py "<query>" --design-system [-p "Project Name"]
python3 skills/ui-ux-pro-max/scripts/search.py "<query>" --design-system --persist -p "Project Name" --output-dir "<project-root>"
```

- Domains: `style, color, chart, landing, product, ux, typography, google-fonts, icons, gsap, react, web`
- Stacks: `react, nextjs, vue, svelte, astro, swiftui, react-native, flutter, nuxtjs, nuxt-ui, html-tailwind, shadcn, jetpack-compose, threejs, angular, laravel`
- `--persist` writes `design-system/<project-slug>/MASTER.md` under `--output-dir` (or CWD). It refuses to overwrite an existing `MASTER.md` without `--force`.

Running this will trigger a normal Bash permission prompt for users on stricter permission settings — that's expected, not a bug in the skill.

## Downstream Use

Feed the resulting style/palette/type choices into `emil-kowalski-design` for execution and motion, `design-reference-taste`/`design-md-references` for structural grounding, and close with `impeccable-design` / `motion-audit`.

## What Was Left Out

The upstream repo also ships:
- `cli/` — an npm installer (`ui-ux-pro-max-cli`). Not needed here; this skill is used directly from the repo path.
- `.claude/skills/design/`, `brand/`, `banner/` — subskills that make **outbound network calls** to AI image-generation APIs (`GEMINI_API_KEY` / `MUAPI_API_KEY`). Deliberately excluded to keep this skill fully offline and free of external API dependencies. If you want AI-generated design/brand/banner images, get them from the upstream repo directly and review its API-key handling yourself first.

## Security Notes (from independent review before vendoring)

- `search.py`/`core.py`/`design_system.py`: no `eval`/`exec`/`subprocess`/`os.system`/network calls. File reads are confined to this skill's own `data/` directory. Writes only happen behind an explicit `--persist` flag, scoped to `--output-dir` (or CWD), and refuse to clobber an existing `MASTER.md` without `--force`.
- Dataset row counts were spot-checked against the README's claims and matched (not padded).
- Publisher (`nextlevelbuilder` / npm maintainer `mrgoonie`) is an established OSS contributor, MIT-licensed, with a real test suite and CI in the upstream repo.

## Updating This Vendor

This is a pinned snapshot (`vendored_from_commit` above), not tracked against upstream `main` — the upstream repo moves fast (multiple commits/day from many contributors). To refresh: on explicit request, re-run the same review (code read, not just README trust), re-copy only `scripts/` and `data/`, update the pinned commit hash, and land it as a normal reviewed commit — never an automatic sync.
