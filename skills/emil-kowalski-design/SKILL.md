---
name: emil-kowalski-design
description: Apply Emil Kowalski-style interface craft — restrained, physics-based motion, disciplined typography, and generous spacing — so UI work stops reading as a generic AI-generated layout. Use when building or reviewing web/app UI, choosing type scales, spacing, or animation timing.
metadata:
  origin: ECC
---

# Emil Kowalski Design

Interface craft modeled on Emil Kowalski's public writing and shipped work (Vercel, Sonner, Vaul): quiet typography, deliberate spacing, and motion that feels physical rather than decorative.

## When to Activate

- building or reviewing a web/app UI, landing page, or component library
- choosing a type scale, spacing system, or animation timing
- the current layout looks flat, generic, or "AI-website" — evenly spaced boxes, default shadows, no rhythm
- adding transitions, toasts, dialogs, drawers, or any interactive micro-interaction

## Typography

- Use a single type family for UI unless a display face is deliberately chosen for headlines.
- Tighten letter-spacing slightly on large headings (-0.01em to -0.03em); leave body text at default tracking.
- Line-height scales inversely with size: tight (1.1–1.2) for large headings, generous (1.5–1.7) for body copy.
- Limit the type scale to 5–7 sizes total. Avoid arbitrary one-off font sizes.
- Font weight does the work contrast usually does — prefer weight shifts (400/500/600) over color shifts for hierarchy.

## Spacing

- Build spacing from a single base unit (commonly 4px) and stick to its multiples everywhere.
- Prefer more whitespace over more borders/dividers to separate sections.
- Optical alignment beats mathematical alignment — nudge elements by eye when the grid makes something look off-center.
- Avoid symmetric padding by default; asymmetric padding (e.g. more top than bottom near a heading) often reads more intentional.

## Motion

- Every animation needs a physical justification: what is entering, leaving, or being manipulated, and from where.
- Default to spring-based easing (e.g. Framer Motion / CSS spring approximations) over linear or generic ease-in-out for anything the user directly manipulates (drag, dismiss, toggle).
- Keep durations short: 100–200ms for micro-interactions (hover, toggle), 200–350ms for entrances/exits, never above ~500ms.
- Interruptible motion: if a user re-triggers an animation mid-flight, it should continue smoothly from its current state, not restart or jump.
- Use opacity + transform together (never transform alone) for entrances so content doesn't pop in unnaturally.
- Reference implementations: Sonner (toasts) and Vaul (drawers) for interruptible, gesture-driven motion patterns.

## Anti-Patterns to Remove

- Default browser/framework shadows (`box-shadow: 0 4px 6px rgba(0,0,0,0.1)` boilerplate) — replace with a considered, subtle shadow or none at all.
- Centered-everything layouts with no asymmetry.
- Ease-in-out on everything, same duration on everything.
- Purple-to-blue gradient backgrounds and generic hero sections — a hallmark of unreviewed AI output; deliberately avoid unless explicitly requested.

## Workflow

1. Before writing UI code, decide the type scale and spacing unit explicitly; write them down as constants/tokens.
2. Build static layout first, get spacing and typography right, then add motion last.
3. For any new animation, state in one line what physical action it represents before implementing it.
4. Screenshot or preview the result (see `run` skill / Playwright if available) before declaring the UI done — do not judge UI from code alone.

## Downstream Use

Pair with `design-reference-taste` (for pulling real reference structure) and `impeccable-design` (for a polish/QA pass) — this skill supplies the typography/spacing/motion vocabulary those two rely on.
