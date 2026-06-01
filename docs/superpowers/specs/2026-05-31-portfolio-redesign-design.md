# Portfolio Redesign — Design Spec
**Date:** 2026-05-31  
**Status:** Approved  

---

## Overview

Redesign the portfolio from a 6-chapter Wes Anderson scroll experience into a **5-floor hotel metaphor**, anchored by an illustrated Grand Budapest Hotel-style building. The hotel serves as both the landing page and the persistent navigation system.

---

## 1. Section Structure

Five sections replace the current six chapters. Roman numerals represent floors.

| Floor | Nav label | Full section title | Color band | Current section |
|-------|-----------|-------------------|------------|-----------------|
| I | About | About | Blush rose (top floor) | Creative Technologist |
| II | Product | Product Management | Crimson | Product |
| III | AI Lab | I Built with AI | Bright scarlet | The Lab |
| IV | Energy | Energy Transition | Deep red | Energy |
| V | Credentials | Credentials | Dark burgundy | Certs + Contact (merged) |

---

## 2. Hotel Landing Hero

### Asset
- File: `assets/hotel-red.png` (to be added — red illustrated hotel, transparent background, 5 clearly separated floor bands + charcoal roof)
- The 5 floor bands in the image correspond exactly to the 5 sections above (top to bottom)

### Animation sequence (on page load)
1. Hotel image appears full-screen on ivory/dark background
2. All 5 floor zones start **dimmed** — a dark semi-transparent overlay covers each floor band
3. Floors animate sequentially, **top → bottom**, each holding ~1.5 seconds:
   - Floor overlay lifts → floor glows at full colour
   - Roman numeral + section name fades in to the right: e.g. *"I — About"*
   - Floor dims, next floor activates
4. After Floor V finishes → **all floors light up simultaneously**
5. "Scroll to enter" cue appears below the building
6. User scrolls → hotel fades out → Floor I (About) is revealed

### Implementation approach
- Single PNG image, no additional image files needed
- Dark overlay per floor = absolutely positioned `div` with known `top`/`height` percentages matching each floor band in the image
- Floor percentages (approximate, to be calibrated against final image):
  - Floor I (About): 14% – 30%
  - Floor II (Product): 30% – 45%
  - Floor III (AI Lab): 45% – 58%
  - Floor IV (Energy): 58% – 72%
  - Floor V (Credentials): 72% – 90%
- JavaScript cycles through floors using `setTimeout`, toggling a `.active` class that removes the overlay and shows the label
- No video file required — pure CSS/JS animation

---

## 3. Navigation

### Behaviour
- Hidden until user scrolls past the hotel hero
- Pins to top as a slim horizontal bar
- Always visible once shown
- Highlights the active section floor as user scrolls

### Labels
```
I · About    II · Product    III · AI Lab    IV · Energy    V · Credentials
```

### Active state
- Active section's numeral + label glows in its floor colour
- Matches the floor colour band from the hotel image:
  - I · About → blush rose
  - II · Product → crimson
  - III · AI Lab → bright scarlet
  - IV · Energy → deep red
  - V · Credentials → dark burgundy

### No miniature hotel image in nav
The nav uses coloured Roman numeral indicators only — cleaner, mobile-friendly.

---

## 4. Content Per Section

### Floor I — About
**Layout:** Two-column at top — photo left, text right. Dossier below full-width.

**Content:**
- Photo: `assets/svetlana-photo.jpg` (placeholder — professional headshot, to be replaced with final photo)
- PRODUCT OWNER stamp (red, uppercase, letter-spaced)
- Rotating words: "She is also a *Builder. / Technologist. / Facilitator.*"
- Narrative paragraph (from CV profile)
- Stats row: 8+ Years · €2M Annual R&D · €22M+ rNPV · 10+ Applications
- Sub-header: **"Technology Skills"**
- Dossier accordion: Blockchain · Artificial Intelligence · IoT · 3D & Metaverse

**Removes:** "Chapter I" chapter number label

---

### Floor II — Product Management
**Layout:** Unchanged (scroll-driven video opener + 3×2 proof grid)

**Content changes:**
- Section title: "Product Management"
- "Proofs" renamed to **"Case Studies"** throughout
- Card section label: "Product Management · Case Studies"

**Keeps:** All existing proof card content, video, scroll-scrub behaviour

---

### Floor III — I Built with AI
**Layout:** Unchanged (Lab cards)

**Content changes:**
- Section title: "I Built with AI"

**Keeps:** All existing Lab card content

---

### Floor IV — Energy Transition
**Layout:** Unchanged (video opener + 4 energy boxes)

**Content changes:**
- Section title: "Energy Transition"

**Keeps:** All existing energy content (carriers, storage, optimisation, regulation boxes)

---

### Floor V — Credentials
**Layout:** Certifications fan (top) + Contact info (bottom), single section

**Content changes:**
- Section title: "Credentials"
- Merges current Chapter V (Certifications) and Chapter VI (Contact) into one section
- Contact sub-header: "Get in Touch" or similar

**Keeps:** All existing certifications fan interaction and contact links

---

## 5. What Is Removed

| Removed element | Replaced by |
|----------------|-------------|
| Animated Chapter Map overlay | Hotel landing animation |
| Concierge hero image | Hotel illustration |
| SVG signature animation | — (removed) |
| Chapter VI Contact (standalone) | Merged into Floor V Credentials |
| "Chapter I/II/III…" numbering | Floor I/II/III… Roman numerals |
| Scroll-cue on concierge | "Scroll to enter" below hotel |

---

## 6. Assets Summary

| File | Status | Used in |
|------|--------|---------|
| `assets/hotel-red.png` | ⚠️ To be added | Hero landing + floor animation |
| `assets/svetlana-photo.jpg` | ✅ Ready (placeholder) | About section |
| `assets/product-hero.mp4` | ✅ Exists | Product Management opener |
| `assets/energy-hero.mp4` | ✅ Exists | Energy Transition opener |
| `assets/certs-hero.mp4` | ✅ Exists | Credentials opener |

---

## 7. Out of Scope

- Content edits to energy boxes, dossier text, or certifications
- New photography session (placeholder used)
- Mobile-specific layout changes beyond what responsive CSS handles naturally
- Any new sections or features not listed above
