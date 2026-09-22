---
name: impeccable-design
description: Final polish pass for UI work — a rigorous checklist covering contrast, alignment, states, responsiveness, and consistency so output reads as studio-quality rather than a first draft. Use before declaring any UI task done, or when reviewing someone else's UI.
metadata:
  origin: ECC
---

# Impeccable Design

A closing quality gate for UI work. Run this after layout and motion are in place (see `emil-kowalski-design`) and before calling any UI task finished.

> Not to be confused with `impeccable-plugin-reference`, this repo's pointer to the unrelated third-party `pbakaus/impeccable` Claude Code plugin. This skill here is a static, hand-written checklist — no hooks, no external install, no binaries.

## When to Activate

- right before marking a UI/frontend task complete
- reviewing an existing component, page, or PR for visual quality
- the user says something looks "off," "amateur," or "not quite right" but can't pinpoint why

## Checklist

### Hierarchy & Contrast
- One clear primary action per screen/section; everything else is visually subordinate.
- Text contrast meets WCAG AA at minimum (4.5:1 body, 3:1 large text) — verify, don't eyeball.
- Disabled, hover, focus, and active states are all defined, not just default and hover.

### Alignment & Grid
- Every edge lines up with another edge somewhere on the page (an invisible grid), not just "close enough."
- Icon and text baselines align; icons are optically, not mathematically, centered against adjacent text.
- No orphaned single words wrapping alone on a line in headings/buttons.

### Consistency
- Corner radii, shadow depth, border weight, and icon stroke width are each drawn from one small set of values used everywhere.
- Interactive elements of the same semantic role (all primary buttons, all links) look and behave identically across the surface.
- Empty states, loading states, and error states exist for every data-driven view — not just the happy path.

### Responsiveness
- Verify at minimum: mobile (~375px), tablet (~768px), desktop (~1440px) widths.
- Text never overflows its container; touch targets are ≥44px on mobile.
- Nothing that matters is hidden below the fold on common viewport heights without a scroll affordance.

### Motion & Feedback
- Every user action that changes state gives immediate visual feedback (button press, form submit, toggle).
- No layout shift caused by async content loading in without a reserved space/skeleton.

### Copy
- No placeholder/lorem ipsum text left in a "finished" surface.
- Button and label copy is verb-first and specific ("Save changes," not "Submit").

## Workflow

1. Do not run this checklist from reading code alone — render the UI (dev server, Storybook, or an artifact) and look at it.
2. If Playwright or a browser tool is available, screenshot each breakpoint and diff visually against the checklist above.
3. Log failing items as a short punch list, fix the highest-hierarchy issues first (contrast/hierarchy before micro-spacing).
4. Re-check after fixes — a polish pass that isn't re-verified isn't a polish pass.

## Downstream Use

Run after `emil-kowalski-design` (structure/motion) and `design-reference-taste` (reference-informed layout). This skill is the gate, not the generator — it catches what those two produced but didn't verify.
