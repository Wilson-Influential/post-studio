# Post Studio

> Internal tool. One piece of copy → on-brand social post variations, exportable as real PNGs.
> Built 2026-06-17 (autonomous evening build). Self-contained single HTML file.

## What it is

The Social Post Generator concept Aleks raised on the 2026-05-28 call: non-designers
(PR, copywriters, account management) paste copy, pick a sector and a placement, and get
several ready-made on-brand post layouts to choose from and export. Lowers the barrier to
design so people can ship on-brand content without booking designer time, and makes it hard
to break the brand rather than relying on gatekeeping.

Follows Shea's "one input, many outputs" pattern (same shape as the Social Safe-Zone Checker)
and is designed to pair with it: generate here, sanity-check placements there.

## How it works

- Single self-contained `index.html`. No build step, no backend, works offline.
- Posts are rendered as **SVG** at native resolution, so previews are crisp and PNG export is
  exact-size and pixel-perfect (SVG → canvas → PNG, all client-side).
- The three real logo PNGs are embedded as base64 (never recreated — brand rule).
- Brand-locked: Effra type with the literal "body = 50% of headline" rule enforced in code,
  the 19 brand colours, sector accents, the gradated-circle motif, navy/yellow/cream system.
- Type auto-fits each layout (wrap + shrink) so any copy length stays inside the safe margins.
- Safe-zone overlay toggle reuses the 80px / ~8% Meta safe margin from the Safe-Zone Checker.

## Fonts caveat

Effra Trial is installed locally on Shea's Mac, so it renders in the browser and in PNG export.
On a phone or a machine without Effra it falls back to system sans, so previews look slightly
off there. Real exports should be done on the Mac. Web licence still pending (same caveat as
all the other artefacts).

## Updates

- **v2 (2026-06-17):** expanded to **9 layouts** (Statement, Tearaway, Spotlight, Split, Quote,
  Frame, Sidebar, Index, Marquee).
- **v3 (2026-06-17):** per Shea, replaced all spheres with the **brand ring** (the Secondary Icon,
  inner/outer 0.59) and converted the soft background blooms to faint partial rings, so the whole
  tool speaks the brand's circle language. Reusable ring assets in `Assets/logos/ring/`.

## Status

Built and verified locally. Not deployed (deploy routes are permission-gated). Delivered to
Shea as the self-contained file. Open question if it goes further: hand to Aleks/James, and
whether to wire AI copy assist later.
