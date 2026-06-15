# Design System — Portfolio Hotel

Wes Anderson / Grand Budapest Hotel register: jewel-box grounds, brass hairlines,
symmetric compositions, ledger typography. The hotel is the information
architecture; every floor (chapter) keeps the same printed-matter grammar.

## Color

| Token | Value | Role |
|---|---|---|
| `--sage` | `#B5C9B0` | page ground |
| `--sage-deep` | `#4A5D45` | Energy ground, footer |
| `--sage-soft` | `#D4DFCE` | About ground |
| `--ivory` | `#F4ECDA` | paper surfaces, light text on dark |
| `--ivory-warm` | `#E8DCC4` | warm paper hover |
| `--pink` | `#F2BFC6` | Credentials ground |
| `--gold` / `--brass` | `#C9A961` / `#A8853E` | brass hairlines, frame rings |
| `--red` | `#B83A2C` | the accent: plaques, drop caps, active states |
| `--red-bold` / `--red-deeper` / `--red-soft` | `#C3271A` / `#7E1A12` / `#D96A55` | case-card tonal range |
| `--ink` / `--ink-soft` | `#2A2620` / `#574E42` | text (ink-soft passes 4.5:1 on all paper grounds) |

Backgrounds are never flat: each ground carries a soft radial vignette
(light at top, deepened foot). Verified contrast: all body text ≥4.5:1.

## Typography

Two families only (footer promise: "Set in Jost & Playfair").

- **Jost** (`--f-display`): chapter titles `--fs-chapter` clamp(42–76px),
  weight 300, tracking `--track-title` 0.14em, uppercase, with matching
  `text-indent` to optically recenter. All labels/plaques: 11px, weight 500,
  tracking `--track-label` 0.28em.
- **Playfair Display** (`--f-accent`/`--f-body`): narrative serif 17–18px /
  1.75–1.85, italic for asides and stamps, red drop cap (`::first-letter`)
  on the bio, big italic numerals on cards.

One label grammar everywhere (plaques): tags, stamps, chapter numbers,
stat labels, statuses all share the same size/tracking/case.

## Components

- **Frames**: double rule = 1px ink outer + brass inner line (or `outline`
  + `outline-offset`). Used by: hero lithograph, portrait, cert cards,
  Concierge frame, hero bookplate border.
- **Buttons** (`.pill`, `.case-btn`): rectangular luggage-label — solid ink
  face (or ghost), offset 1px hairline ring, red on hover. No border-radius.
- **Cards**: left-aligned text always; inner hairline inset frame
  (`::after, inset 7–9px`); soft long shadows `rgba(ink, .12–.28)`.
- **Portrait**: arched cabinet card (`border-radius: 124px 124px 2px 2px`)
  with brass ring.
- **Stats**: hotel-register ledger row — heavy rule + inner hairline top
  and bottom, Playfair red figures.

## Motion

- Hero load: `heroRise` staggered (arch 100ms → hotel 350ms → cue 1400ms),
  cubic-bezier(0.22, 1, 0.36, 1), ~1.1s.
- **Hero parallax (scroll-linked):** one passive scroll listener writes
  `--s` (0→1 across the first viewport) on `#hotelHero`. The CSS turns `--s`
  into compositor-only transforms — `.hero-stage` recedes (`translateY` +
  `scale`) and fades, `.hotel-arch-text` drifts up faster (depth), the
  `.hero-dusk` twilight veil washes in, the bookplate frame + cue fade out.
  Layout reads (vh, doc height) are cached and refreshed on resize/load only,
  so the handler never forces reflow; no rAF gate, so it survives tab restore.
- **Brass reading thread:** `#progressThread` under the nav, width = document
  scroll fraction, same scroll listener.
- **Ledger count-up:** `.stat .num` figures tween 0→value (easeOutQuart, 1.4s)
  on IntersectionObserver enter, landing on the authored text exactly
  (prefix/number/suffix parsed and preserved). **Replays on every re-entry**:
  the observer is never unobserved; exit re-arms the figure to 0, and a
  `pageshow[persisted]` handler resets + replays after a bfcache restore (back
  button / tab restore) so the animation never gets "stuck" at its final value.
  Has a `setTimeout` settle guarantee + a viewport backstop so a figure can
  never stick at "0".
- Scroll reveals: `.reveal` fade-rise; grids stagger children by 110ms steps
  (stats, proofs, lab cards, dossier rows). Hidden state gated behind
  `html.js` (set before first paint) so a JS failure ships the page fully
  visible, never blank; IO has an unsupported-fallback + viewport backstop.
- **Micro-interactions:** nav links center-grow a brass underline; `.pill`
  buttons lift + sweep a diagonal sheen on hover and press down on click;
  front cert card lifts to invite the flip; closed dossier rows nudge their
  arrow; contact links retract-and-redraw their underline.
- Scrub heroes: door videos blob-preloaded, seek-serialized, with revive
  watchdog (see script comments).
- `prefers-reduced-motion: reduce` collapses all animation/transitions,
  forces `.reveal` visible, holds the hero parallax still (`--s` not written),
  and snaps the count-up to final.

## Performance

- LCP = the lobby lithograph: `<link rel="preload" as="image"
  fetchpriority="high">` + `decoding="sync"` on the active frame so it paints
  in the first frame. Hover frames are `fetchpriority="low"` + `decoding="async"`.
- Below-fold images (`about-photo`, concierge mark) are `loading="lazy"`
  `decoding="async"`. All images carry intrinsic `width`/`height` to reserve
  space (zero CLS).

## Layout rules

- `html { overflow-x: clip }` guards the mobile layout viewport against
  absolutely-positioned overhang (cert fan).
- Nav: ivory letterhead bar, monogram (multiply + brightness(1.08) to melt
  the scan's ground), red roman numerals, red center-grow underline = active.
- Mobile (≤600px): nav stacks logo-over-links; fan offsets halved;
  dossier loses its left indent.
