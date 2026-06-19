# Post Studio: Carousel Mode (Phase 2)

> Design doc. 2026-06-19. Extends Post Studio from single-post autofill (Phase 1, shipped)
> to a multi-slide carousel built from one piece of copy and one chosen template.

## Goal

The agent agrees a multi-slide carousel of copy with Shea and hands over one link. Opening it
shows the existing 9-template grid with **slide 1 (the cover) rendered in each style**. Shea
picks a style, clicks in, and the tool builds the **whole carousel in that one template**:
slide 1 as a bold cover, slides 2+ as calmer consistent inner slides. He steps through a
filmstrip, edits any slide's copy, and downloads one slide or all of them as PNGs.

## Decisions already settled (do not re-litigate)

1. **One template for the whole carousel.** Not per-slide layout (this overrides the old
   `Studio Autofill/PLAN.md`, which baked a `layout` per post). Carousels read as a set only
   when every slide shares one template. The 9-grid is the style picker, reusing what exists.
2. **Cover + inner variant.** Slide 1 renders as the loud cover treatment. Slides 2+ render as
   a calmer inner treatment of the same template, with a baked slide-progress indicator.
3. **Filmstrip workspace** for the carousel view: large current-slide preview, a clickable
   thumbnail filmstrip of every slide, a per-slide edit panel, download-this / download-all.
4. **Additive only.** The manual single-post editor and the Phase 1 share-link must keep working
   exactly as they do now. A one-post payload behaves like today (no carousel chrome).

## Data model

### Payload (v2)

```json
{
  "v": 2,
  "tool": "post-studio",
  "sector": "digital",
  "placement": "portrait",
  "logo": "main",
  "posts": [
    { "kicker": "string", "head": "string", "body": "string", "cta": "string" }
  ]
}
```

- `posts` is an array of slide copy blocks using the **real** keys the engine already reads:
  `kicker, head, body, cta`. No `layout` key. Sector / placement / logo are carousel-wide.
- **Backward compatibility:** `loadState` accepts both shapes. A v1 payload with a single `c`
  object is read as `posts: [c]`. A v2 payload with `posts.length === 1` is a single post.
- `serialize()` (Copy share link) emits v2 with the `posts` array for any state (1 or many).

### App state

`state.posts` becomes an array of copy blocks. `state.active` is the index of the slide being
edited (0 = cover). `state.template` holds the chosen template id once Shea clicks into a style
(null while in grid mode). The existing `state.c` is replaced by `state.posts[state.active]`
everywhere it is read, so the manual editor edits the active slide.

## Rendering: cover vs inner variant

Each template's `render(ctx)` gains two context fields, both additive and ignored by templates
that do not yet special-case them:

- `ctx.variant`: `'cover'` (slide 1) or `'inner'` (slides 2+).
- `ctx.slide`: `{ index, count }` for the progress indicator.

Phase 2 keeps the variant distinction **shared and light** rather than authoring 9 separate
inner layouts (that is a future refinement):

- **Cover:** current treatment unchanged, plus a small `swipe →` affordance bottom-right when
  `slide.count > 1`.
- **Inner:** the hero headline cap is reduced one step (so inner heads sit below the cover's
  scale), `body` is allowed more room, and a baked **dot + counter** (`● ○ ○  ·  2/4`) sits in
  a consistent brand position. CTA only renders on the final slide.

A shared helper `slideFurniture(ctx)` returns the SVG for the swipe affordance / dots / counter
so every template gets a consistent indicator without per-template code.

## UI flow

### Grid mode (default on load)

- Renders `state.posts[0]` (the cover copy) across all 9 templates as the **cover** variant.
- Caption reflects the set: e.g. `9 layouts · Digital · Portrait · 4-slide carousel · click a
  style to build it`. With one post it reads as today.
- Left control panel edits the **cover** slide's copy and live-updates the grid, as now.

### Carousel view (after clicking a style tile)

Replaces the current single-post modal with a filmstrip workspace:

```
┌──── carousel · Statement ─────────────────────┐
│  ← back to styles                  [⤓ all]    │
│  ┌───────────────┐   Edit slide 2 of 4         │
│  │   SLIDE 2     │   kicker [__________]        │
│  │   preview     │   head   [__________]        │
│  │  (inner)      │   body   [__________]        │
│  │               │   cta    [__________]        │
│  └───────────────┘   [⤓ download this slide]    │
│  [1•][2 ][3 ][4 ]   filmstrip (click to jump)   │
└────────────────────────────────────────────────┘
```

- Thumbnails render each slide in the chosen template with the correct variant (1 = cover).
- Editing a field updates the big preview and that slide's thumbnail live.
- **Download this slide:** existing SVG → canvas → PNG path.
- **Download all:** loops the slides, exporting each PNG in order with a short delay between
  each (no zip dependency). Names: `influential-<sector>-<template>-<placement>-NN.png`.
- **← back to styles** returns to grid mode to try a different template; edits are kept.

If `state.posts.length === 1`, clicking a tile opens the current single-post modal unchanged
(no filmstrip, no variant furniture).

## Skill change (`post-studio`)

- Add carousel planning: agree N slides of copy (cover hook first, then content, optional CTA
  slide last), pick carousel-wide sector / placement / logo, show the per-slide review block,
  build a **v2** link, open it.
- Keep single-post behaviour for one-slide requests.
- Update the "Not yet built" section: carousel mode now exists; per-slide template choice and
  designer-authored inner layouts remain future.
- House rules unchanged: no em dashes, no AI-tell phrases, respect kicker/cta limits.

## Test carousel (for verifying the loop)

Topic Shea asked for: moving from single AI agents / chatbots to AI workflows with subagents
and delegation. Sector `digital`, placement `portrait`, logo `main`, 5 slides.

1. **Cover** — kicker `DELEGATION` · head `Stop prompting one AI. Start running a team of them.`
2. kicker `THE SHIFT` · head `A chatbot waits. A workflow runs itself.` · body `One model in a
   box only does what you type next. A workflow hands each step to whoever is best for it.`
3. kicker `SUBAGENTS` · head `Delegate the work, not just the question.` · body `A lead agent
   splits a job into parts and sends each to a focused subagent. Research, draft, check, in
   parallel, not one slow thread.`
4. kicker `WHY IT WINS` · head `Smaller jobs, sharper output, less lost in the middle.` · body
   `Each subagent holds only what it needs. Cleaner focus, fewer mistakes, and it runs while
   you get on with something else.`
5. **CTA** — kicker `AT INFLUENTIAL` · head `Build the workflow once. Reuse it every week.` ·
   cta `Map your first one`

(Copy gets a humanizer pass before it is treated as final, per house rules.)

## Out of scope (YAGNI for Phase 2)

- Per-slide template choice, designer-authored distinct inner layouts, zip download, a
  clean-vs-creative toggle, auto-splitting one long blob into slides. Noted as future.

## Acceptance

- One v2 link opens grid mode showing slide 1 across 9 styles.
- Clicking a style builds the full carousel in that template (cover + inner variants).
- Filmstrip lets Shea step through, edit any slide, download one or all PNGs in order.
- Single-post payloads and the manual editor behave exactly as before.
