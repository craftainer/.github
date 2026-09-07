# Brand assets

Source files for the craftainer mark, palette, and type system.

## Logo

- `avatar.svg` — ink-plate version for the org avatar. GitHub's avatar
  upload only accepts raster images (PNG/JPG), so export this at
  **460×460** before uploading it under org Settings → Profile picture.
- `mark-light.svg` — transparent background, for use on light surfaces.
- `mark-dark.svg` — transparent background, for use on dark surfaces.

The mark is a crate seen face-on: a 3×3 grid of slats with one slot
emptied out (dashed) and its piece popped out and tinted papaya — "the
piece you swap." The artwork already accounts for GitHub's circular
avatar crop (mentions, notifications, third-party integrations) — every
line sits inside the inscribed circle, so don't rescale the crate
relative to its canvas without re-checking that margin.

Don't recolor the popped piece. Papaya is the one accent color in the
mark, and it's what carries the "swappable" idea — everything else stays
neutral ink/paper.

## Palette

| Token           | Light     | Dark      |
|-----------------|-----------|-----------|
| Accent — Papaya | `#FF7A29` | `#FF9752` |
| Teal — panels   | `#3E7C74` | `#6FB3A8` |
| Ink             | `#1B2024` | `#E9EBE9` |
| Paper           | `#EDEFEE` | `#14181B` |

The logo files use a single fixed papaya (`#FF7A29` / `#B84A00` outline)
regardless of theme — only the neutral ink/paper flips between the
light and dark mark variants.

## Type

- **Display — Big Shoulders Stencil.** Used sparingly: the wordmark and
  nothing else. A stencil cut, reading like a mark stamped on a
  shipping crate.
- **Body — Hanken Grotesk.** Everything else — headings, running text.
- **Mono / code — Martian Mono.** Code snippets, repo names, labels.

All three are free, open-source Google Fonts (SIL OFL 1.1 / Apache
2.0) — no royalties or attribution required for commercial or branding
use.
