# Changelog

## Major UI/UX redesign — June 2026

A full front-end redesign of the single-page portfolio in a Wes Anderson /
Grand Budapest Hotel register. All text content was preserved; only UI/UX
changed. Worked entirely in `index.html` (no build step). Supporting docs:
[`PRODUCT.md`](PRODUCT.md) (strategy) and [`DESIGN.md`](DESIGN.md) (visual system).

### Design foundations
- Trimmed unused web fonts (Italianno, Roboto Slab); the page now loads only
  Jost + Playfair Display, matching the footer's "Set in Jost & Playfair".
- Added a type-scale token set and one shared "plaque" label grammar reused by
  every chapter (tags, stamps, chapter numbers, stat labels, statuses).
- Global `prefers-reduced-motion` support: collapses animations/transitions,
  forces reveals visible, holds the hero parallax still, snaps the count-up.
- `html { overflow-x: clip }` to stop the cert fan overhanging the mobile viewport.

### Navigation & footer
- Reworked the nav into an ivory "letterhead" bar with red roman numerals and
  center-grow brass underlines; active floor underlined.
- Removed the "S" monogram logo from the nav and the Credentials contact card
  (per request); replaced the nav brand with a name wordmark, then removed that
  too — the menu is now right-aligned and the name lives on the hotel artwork.
- Added a brass scroll-progress thread beneath the nav (document scroll fraction).
- Footer: gold top-rule, larger tracked type.

### Hero
- Recomposed the establishing shot as a mounted lithograph with a staggered
  load animation and an in-world scroll cue.
- Added scroll-reactive parallax: the stage recedes + fades, the arch title
  drifts faster (depth), and a warm "dusk" veil washes in as you scroll
  (compositor-only transforms via a single rAF-batched `--s` variable).
- Removed the bookplate page-frame and the rectangle/mount around the hotel
  illustration (per request).
- Set the hero background to a flat `#F1E9D4`, sampled from the artwork's own
  ground colour, so the illustration's edges are invisible.

### About
- Arched cabinet-card portrait, sized to match the bio text height; fixed an
  `<img>` `height` attribute that was blowing the portrait up full-height.
- Left-aligned bio with a red Playfair drop cap.
- Hotel-register "ledger" stat row whose figures count up on entering view —
  now replays on every re-entry and on bfcache restore (not just once).

### Product (case studies)
- Replaced the one-at-a-time carousel with a Gallery4-style horizontal
  scroll-snap rail: drag-to-scroll, arrow + keyboard nav, peek of the next card.
- Controls match the live site: ← PREV · 01 / 06 · NEXT → (counter tracks the
  lead card).
- Card sizing matched exactly to the live site (43% of a 960px container,
  20px gap, 46vh min-height, 24/26/22 padding).

### AI Lab, Energy, Credentials
- Specimen-style Lab cards; Energy proof grid; atmospheric (vignetted) grounds.
- Removed leaked editor placeholder captions from the ambient/background slots.
- Polished the Credentials cert-fan cards and the Concierge contact frame;
  rectangular "luggage-label" buttons (sheen sweep on hover, press on click).
- Removed the running count from the cert-fan "Click to flip" hint (the index
  already shows in the top-right counter).

### Motion & micro-interactions
- Staggered scroll reveals (gated behind `html.js` so a JS failure never ships
  a blank page); button lift + brass sheen; cert hover-lift; dossier arrow
  nudge; contact-link underline.

### Performance
- LCP hero image preloaded (`fetchpriority=high`); below-fold images
  `loading=lazy` + `decoding=async`, all with intrinsic `width`/`height` for
  zero layout shift.

### Scroll-scrubbed door videos (reliability)
- Door opener videos scrub frame-by-frame with scroll. Hardened against Safari:
  blob-preloaded into memory, seeks serialised (one in flight, latest queued),
  and a watchdog revives a reclaimed decoder.
- WebKit decoder priming: a muted `play()`→`pause()` warms the decoder so
  paused-video seeks work.
- Fixed energy/credentials/lab doors not opening: only the opener nearest the
  viewport keeps a decoder (deterministic picker), so Safari's
  concurrent-decoder limit is never exceeded; scrolling back re-activates.
- Tuned the scrub so the doors fully open, then HOLD on the open frame before
  the next floor pulls up.

---

_Built with Claude Code. Verified in-browser (desktop + mobile) throughout._
