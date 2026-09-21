---
name: image-to-code
description: Generate a design reference image for a section/page before writing its UI code, then implement code that faithfully matches that image instead of drifting into generic AI-default layouts. Use when building a new marketing page, landing page, or UI section from scratch and a stronger visual result than typical scaffolded output is wanted.
metadata:
  origin: ECC
  inspired_by: Leonxlnx/taste-skill (image-to-code-skill) — rewritten, not copied verbatim (unverified personal source)
---

# Image to Code

Generate the design as an image first, study it closely, then implement code that matches it — instead of jumping straight to markup and getting a generic, AI-default layout.

## When to Activate

- building a new marketing/landing page or a distinct UI section from scratch
- the current plan is to write JSX/HTML directly without any visual reference
- previous output for this kind of surface came out looking templated or interchangeable with any other AI-generated page

## Workflow

1. **Generate the image before the code.** Produce one design reference image per section (hero, features, pricing, footer, etc.) rather than a single compressed multi-section board — a compressed board loses the spacing detail that separates a considered layout from a cramped one.
2. **Study the image before implementing.** Look closely at spacing, alignment, type hierarchy, and component boundaries in the generated image before writing any code from it.
3. **Implement with fidelity, not inspiration.** Treat the generated image as a binding spec for spacing, color, and type relationships — not a mood board to loosely riff on. "Design drift" back into a generic template defeats the point of generating the image at all.
4. **Regenerate rather than crop.** If a section's image came out unclear or a new section is needed, generate a fresh standalone image for it rather than cropping it out of an existing composite — cropping tends to lose spacing context.

## Directives for the Generated Images Themselves

- Hero sections: clean, spacious, readable, and legible on a small laptop screen. Headlines capped at 1–3 lines.
- Avoid "cards-inside-cards-inside-cards" nesting and excessive rounded container-in-container patterns.
- Avoid decorative filler copy and pseudo-technical jargon ("seamless," "unleash," "enterprise-grade") unless it's genuinely accurate to the product.
- Prefer more, clearer images over fewer, denser ones — err on the side of generating an extra section image rather than compressing two sections into one.
- Avoid the generic AI default of repeated left-text/right-image alternating blocks for every section; vary the composition.

## Downstream Use

Pair with `emil-kowalski-design` for the typography/spacing/motion system once the image-derived layout is set, and `impeccable-design` as the closing QA pass before calling the UI done.
