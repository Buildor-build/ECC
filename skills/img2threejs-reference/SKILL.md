---
name: img2threejs-reference
description: Pointer to img2threejs — a tool that reconstructs a reference image as a procedural Three.js model (TypeScript factory function + JSON spec, not a mesh file). Use when a task needs a 3D object generated from a single reference photo and rebuilt as editable, diffable code rather than an imported mesh asset.
metadata:
  origin: ECC
  upstream: https://github.com/img2threejs/img2threejs
  reviewed: verified real/active repo (16.5k+ stars, Apache 2.0, Python 3.10+ stdlib for the core forge scripts, no required third-party services) before referencing
---

# img2threejs (reference pointer)

`img2threejs` takes one reference image and rebuilds the object as a procedural Three.js model expressed in code — a TypeScript factory function plus a JSON spec — rather than as an imported mesh file. Because the output is code, it diffs in git and can be hand-edited like any other source file.

This is a **pointer, not a vendored dependency** — it's outside this repo's design-web scope (3D reconstruction, not UI/layout/typography), so nothing from it is bundled here. Install it yourself only if a task genuinely needs this capability.

## When to Activate

- a task needs a 3D object generated from a single reference photo, and the result should be editable/diffable code rather than a binary mesh asset
- existing 2D-design skills in this repo (`design-reference-taste`, `emil-kowalski-design`, `image-to-code`, etc.) don't cover the need because the deliverable is a dimensional object, not a page/section

## Installing (if actually needed)

```bash
git clone https://github.com/img2threejs/img2threejs.git ~/.claude/skills/img2threejs
```

Then invoke per its own docs (e.g. `/img2threejs Rebuild this object as a Three.js model, keep the proportions, angles and colours.`). Review its README yourself at install time — this pointer only vouches for what was true at time of review, not for future changes.

## Notable Behavior

- **Fail-closed by design**: if the requested fidelity can't be reached from the given image, it returns `BLOCKED` and generates nothing rather than shipping a bad model. Give it one object, plain background, good light, whole subject in frame.
- Core forge scripts use Python 3.10+ standard library only — no required pip dependencies for the base workflow. Optional integrations (SAM2, Depth Anything V2, MediaPipe, and paid compute sponsors mentioned in its README) are separate add-ons, not required for basic use — review any of those individually before enabling if you go that route.

## Downstream Use

Independent of the rest of this repo's design-web skill chain (`design-reference-taste` → `emil-kowalski-design` → `impeccable-design`) — use it standalone when the deliverable is a 3D object.
