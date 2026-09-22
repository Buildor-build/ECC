---
name: motion-audit
description: Audit existing UI motion/animation for "AI-slop" patterns (gratuitous pulsing, indiscriminate hover-scale, missing feedback on state changes) and gaps where conditional UI has no animation at all. Use when reviewing an existing codebase's interactions, or after `emil-kowalski-design` has been applied and a verification pass on the motion specifically is needed.
metadata:
  origin: ECC
  inspired_by: kylezantos/design-motion-principles (audit mode) — rewritten, not copied verbatim (unverified personal source); philosophy drawn from the public work of motion designers Emil Kowalski, Jakub Krehel, and Jhey Tompkins
---

# Motion Audit

A focused audit pass for motion/animation specifically — narrower than `impeccable-design`'s general checklist, and meant to run after motion exists (via `emil-kowalski-design` or otherwise) to catch what a general UI pass tends to miss.

## When to Activate

- reviewing an existing codebase's interactions/animations, not building new ones
- after applying `emil-kowalski-design`, as a targeted verification pass
- the user says an interface feels "flat," "dead," or conversely "busy"/"twitchy" and the cause isn't obvious from a general UI review

## Two Failure Modes to Check For

### 1. Motion Gaps — conditional UI with no animation
Find every place the UI's state changes and check whether the transition is animated:
- items entering/leaving a list, a toast appearing/dismissing, a modal opening/closing
- a value changing (counter, progress, price) with no transition at all
- conditional rendering (`{condition && <X/>}`) that pops the element in/out with zero animation
- tab/route changes with an abrupt content swap

A gap here reads as "dead" or unfinished, even if every individual state is styled correctly.

### 2. AI-Slop Motion — animation applied without judgment
Flag animation that exists but was added reflexively rather than deliberately:
- a pulsing/glowing indicator on something that isn't actually live or urgent
- `hover:scale-*` applied to every card/button uniformly, regardless of whether that element is meant to feel "grabbable"
- looping decorative animation with no way to pause/stop it and no `prefers-reduced-motion` guard
- animation duration/easing that's inconsistent across visually-equivalent elements (one card springs, the sibling card eases linearly)
- motion that isn't interruptible — re-triggering it restarts from scratch instead of continuing smoothly

A gap and a slop pattern can coexist: the busy card grid with reflexive hover-scale everywhere, right next to a toast that appears with no transition at all.

## Workflow

1. Render the surface (dev server, Storybook, or an artifact) — do not audit from source code alone; motion timing and interruptibility can't be judged from a diff.
2. Walk every interactive state and every conditional render; note each one against the two failure modes above.
3. For each finding, name what's wrong in one line (gap vs. slop) and the specific fix (e.g. "add opacity+transform exit transition," "remove hover-scale from static info cards, keep it only on draggable ones").
4. Re-render after fixes to confirm the transition reads correctly and respects `prefers-reduced-motion`.

## Downstream Use

Run after `emil-kowalski-design` (which sets the motion vocabulary/tokens) and before or alongside `impeccable-design` (the general polish gate) — this skill is the motion-specific deep pass, not a replacement for either.
