# Portfolio Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the animated chapter-map hero with an interactive hotel illustration landing page, restructure 6 chapters into 5 named floors, update nav, add photo to About, and merge Certs + Contact into Credentials.

**Architecture:** All changes are in a single file (`index.html`) containing CSS, HTML, and JS. The hotel PNG (`assets/hotel-red.png`) is displayed full-screen; absolutely-positioned overlay divs dim each floor zone; JS cycles through them sequentially on load. The sticky nav uses coloured Roman numeral indicators tied to scroll position via IntersectionObserver.

**Tech Stack:** Vanilla HTML/CSS/JS, no build step. Serve locally with `python3 -m http.server 8765` and open `http://localhost:8765`.

---

## File Map

| File | Changes |
|------|---------|
| `index.html` | All changes — CSS (~lines 46–1200), HTML (~lines 1748–2228), JS (~lines 2230–2627) |
| `assets/hotel-red.png` | Already in place — no changes |
| `assets/svetlana-photo.jpg` | Already in place — no changes |

---

## Task 1: Remove chapter-map overlay CSS + HTML

**Files:**
- Modify: `index.html` — CSS section (lines ~347–498) and HTML (lines ~1763–1798)

- [ ] **Step 1: Remove the chapter-map CSS block**

Find and delete everything between these two comments (inclusive of the comments):
```
/* ────────────── CHAPTER MAP OVERLAY ────────────── */
```
...down to and including `.cm-scroll-cue::after { ... }` (approximately lines 347–498).

This removes: `.hero .fresco` brightness rules, `.chapter-map`, `.chapter-map.dissolving`, `.cm-title`, `.cm-role-line`, `.cm-subtitle-line`, `.cm-items`, `.cm-item`, `.cm-roman`, `.cm-dash`, `.cm-name`, `.cm-dot`, `.cm-concierge`, `.cm-scroll-cue`.

Also remove this line (it hides the signature that no longer exists):
```css
/* Signature permanently hidden — name is shown in chapter map instead */
.hero .hero-signature { display: none; }
/* Hide the original scroll cue while the map is up */
.hero:not(.map-gone) .scroll-cue { opacity: 0; pointer-events: none; }
```

- [ ] **Step 2: Remove the hero section + chapter-map HTML**

Find the block starting at `<!-- ═════════════════════ HERO ═════════════════════ -->` (around line 1762) and delete the entire `<section class="hero" ...>` element, including the chapter-map div, the SVG signature, and the scroll-cue div inside it. Stop before `<!-- ═════════════════════ I — THE CREATIVE TECHNOLOGIST ═════════════════════ -->`.

- [ ] **Step 3: Remove hero CSS rules that are now orphaned**

Delete these CSS blocks which styled the old hero:
```css
section.hero { ... }
.hero .fresco { ... }
.hero .hero-signature { ... }
.hero .hero-signature text { ... }
.hero .hero-signature .sig-stroke { ... }
.hero .hero-signature .sig-fill { ... }
@media (prefers-reduced-motion: reduce) { #heroSignatureStrokeRect ... }
.hero .presents { ... }
.hero h1 { ... }
.hero h1 .ampersand { ... }
.hero .role-line { ... }
.hero .subline { ... }
.hero .buttons { ... }
.hero .scroll-cue { ... }
.hero .scroll-cue::after { ... }
```

- [ ] **Step 4: Remove chapter-map JS block**

Find and delete the entire IIFE labelled `/* ── Chapter Map: animate in, then dissolve on first scroll ── */` (approximately lines 2306–2365). Also delete the signature JS IIFE above it (labelled `/* Signature animation — called by the chapter map JS after the overlay dissolves */`, approximately lines 2262–2304).

- [ ] **Step 5: Verify in browser**

Run: `python3 -m http.server 8765` then open `http://localhost:8765`.
Expected: Page loads directly to the first section (currently `#origins`) with no overlay, no concierge image, no freeze. Scroll works normally.

- [ ] **Step 6: Commit**
```bash
git add index.html
git commit -m "Remove chapter-map overlay and concierge hero"
```

---

## Task 2: Add hotel hero section (HTML + CSS)

**Files:**
- Modify: `index.html` — add CSS block and new `<section class="hotel-hero">` before section#origins

- [ ] **Step 1: Add hotel hero CSS**

Insert this CSS block after the nav CSS and before the `section#origins` background rule (around line 486):

```css
/* ────────────── HOTEL HERO ────────────── */
.hotel-hero {
  position: relative;
  width: 100%;
  min-height: 100vh;
  background: #1a0a0a;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}
.hotel-hero .hotel-img-wrap {
  position: relative;
  width: clamp(320px, 72vw, 900px);
  user-select: none;
}
.hotel-hero .hotel-img-wrap img {
  width: 100%;
  height: auto;
  display: block;
  position: relative;
  z-index: 2;
}
/* Floor overlay divs — positioned over the hotel image */
.hotel-hero .floor-overlay {
  position: absolute;
  left: 4%;
  width: 92%;
  background: rgba(15, 3, 3, 0.78);
  z-index: 3;
  transition: opacity 0.6s ease;
  pointer-events: none;
}
.hotel-hero .floor-overlay.active { opacity: 0; }

/* Floor label that appears beside the active floor */
.hotel-hero .floor-label {
  position: absolute;
  right: calc(100% + 24px);
  top: 50%;
  transform: translateY(-50%);
  text-align: right;
  opacity: 0;
  transition: opacity 0.4s ease;
  pointer-events: none;
  white-space: nowrap;
  z-index: 10;
}
.hotel-hero .floor-label.visible { opacity: 1; }
.hotel-hero .floor-label .fl-roman {
  font-family: 'Playfair Display', Georgia, serif;
  font-style: italic;
  font-size: 13px;
  color: #B83A2C;
  letter-spacing: 0.15em;
  display: block;
  margin-bottom: 4px;
}
.hotel-hero .floor-label .fl-name {
  font-family: 'Jost', sans-serif;
  font-weight: 300;
  font-size: 11px;
  letter-spacing: 0.38em;
  text-transform: uppercase;
  color: rgba(244, 236, 218, 0.75);
  display: block;
}
/* Scroll cue — hidden until animation completes */
.hotel-hero .hotel-scroll-cue {
  position: absolute;
  bottom: 32px;
  left: 50%;
  transform: translateX(-50%);
  font-size: 10px;
  letter-spacing: 0.4em;
  text-transform: uppercase;
  color: rgba(244, 236, 218, 0.5);
  writing-mode: vertical-rl;
  opacity: 0;
  transition: opacity 0.8s ease;
}
.hotel-hero .hotel-scroll-cue.visible { opacity: 1; }
.hotel-hero .hotel-scroll-cue::after {
  content: "";
  display: block;
  width: 1px;
  height: 40px;
  background: rgba(244, 236, 218, 0.4);
  margin: 12px auto 0;
}
```

- [ ] **Step 2: Add hotel hero HTML**

Insert this HTML immediately before `<!-- ═════════════════════ I — THE CREATIVE TECHNOLOGIST ═════════════════════ -->`:

```html
<!-- ═════════════════════ HOTEL HERO ═════════════════════ -->
<section class="hotel-hero" id="hotelHero">
  <div class="hotel-img-wrap" id="hotelImgWrap">
    <img src="assets/hotel-red.png" alt="Portfolio Hotel — Svetlana de Boer" id="hotelImg" draggable="false">

    <!-- Floor overlays: top/height set by JS after image loads -->
    <div class="floor-overlay" id="floorOverlay0">
      <div class="floor-label" id="floorLabel0">
        <span class="fl-roman">I</span>
        <span class="fl-name">About</span>
      </div>
    </div>
    <div class="floor-overlay" id="floorOverlay1">
      <div class="floor-label" id="floorLabel1">
        <span class="fl-roman">II</span>
        <span class="fl-name">Product</span>
      </div>
    </div>
    <div class="floor-overlay" id="floorOverlay2">
      <div class="floor-label" id="floorLabel2">
        <span class="fl-roman">III</span>
        <span class="fl-name">AI Lab</span>
      </div>
    </div>
    <div class="floor-overlay" id="floorOverlay3">
      <div class="floor-label" id="floorLabel3">
        <span class="fl-roman">IV</span>
        <span class="fl-name">Energy</span>
      </div>
    </div>
    <div class="floor-overlay" id="floorOverlay4">
      <div class="floor-label" id="floorLabel4">
        <span class="fl-roman">V</span>
        <span class="fl-name">Credentials</span>
      </div>
    </div>
  </div>

  <div class="hotel-scroll-cue" id="hotelScrollCue">Scroll</div>
</section>
```

- [ ] **Step 3: Verify in browser**

Expected: Full-screen dark background with the hotel image centred. No animation yet — all overlays stack with no height (they'll be positioned by JS in Task 3). The image is visible.

- [ ] **Step 4: Commit**
```bash
git add index.html
git commit -m "Add hotel hero section HTML and CSS"
```

---

## Task 3: Hotel floor animation JS

**Files:**
- Modify: `index.html` — add JS block in `<script>` section

- [ ] **Step 1: Add hotel animation JS**

Insert this IIFE into the `<script>` block, before the closing `})();` of any other IIFE (add it as a new standalone block near the top of the script section, after the nav hide/show IIFE):

```javascript
/* ── Hotel Hero: floor-by-floor reveal animation ── */
(function () {
  const hero     = document.getElementById('hotelHero');
  const img      = document.getElementById('hotelImg');
  const scrollCue = document.getElementById('hotelScrollCue');
  if (!hero || !img) return;

  // Floor zones as [topPercent, heightPercent] of the hotel image height.
  // These are calibrated to the hotel-red.png floor bands.
  // Adjust these values if floor highlights don't align with the image.
  const FLOORS = [
    { top: 14, h: 16 }, // I  — About        (blush rose floor)
    { top: 30, h: 15 }, // II — Product       (crimson floor)
    { top: 45, h: 13 }, // III — AI Lab       (scarlet floor)
    { top: 58, h: 13 }, // IV — Energy        (deep red floor)
    { top: 71, h: 20 }, // V  — Credentials   (burgundy floor)
  ];

  const HOLD_MS  = 1500; // ms each floor stays lit
  const FADE_MS  = 400;  // matches CSS transition

  function positionOverlays() {
    const imgH = img.offsetHeight;
    FLOORS.forEach((f, i) => {
      const overlay = document.getElementById('floorOverlay' + i);
      if (!overlay) return;
      overlay.style.top    = (f.top / 100 * imgH) + 'px';
      overlay.style.height = (f.h   / 100 * imgH) + 'px';
    });
  }

  function runAnimation() {
    positionOverlays();
    let step = 0;

    function activateFloor(i) {
      // dim all
      FLOORS.forEach((_, j) => {
        const ov = document.getElementById('floorOverlay' + j);
        const lb = document.getElementById('floorLabel' + j);
        if (ov) ov.classList.remove('active');
        if (lb) lb.classList.remove('visible');
      });
      if (i >= FLOORS.length) {
        // All done — light up everything
        FLOORS.forEach((_, j) => {
          const ov = document.getElementById('floorOverlay' + j);
          if (ov) ov.classList.add('active');
        });
        scrollCue.classList.add('visible');
        // Unlock scroll and wire scroll-away behaviour
        enableScrollAway();
        return;
      }
      const overlay = document.getElementById('floorOverlay' + i);
      const label   = document.getElementById('floorLabel' + i);
      if (overlay) overlay.classList.add('active');
      setTimeout(() => {
        if (label) label.classList.add('visible');
      }, FADE_MS / 2);
      setTimeout(() => activateFloor(i + 1), HOLD_MS);
    }

    // Start locked — prevent scroll during animation
    document.body.style.overflow = 'hidden';
    // Small delay so page paint settles first
    setTimeout(() => activateFloor(0), 600);
  }

  function enableScrollAway() {
    document.body.style.overflow = '';
    let dismissed = false;
    function dismiss() {
      if (dismissed) return;
      dismissed = true;
      window.removeEventListener('wheel',     dismiss);
      window.removeEventListener('touchmove', dismiss);
      hero.style.transition = 'opacity 0.7s ease';
      hero.style.opacity = '0';
      setTimeout(() => { hero.style.display = 'none'; }, 750);
    }
    window.addEventListener('wheel',     dismiss, { passive: true });
    window.addEventListener('touchmove', dismiss, { passive: true });
  }

  // Wait for image to load before positioning overlays
  if (img.complete) {
    runAnimation();
  } else {
    img.addEventListener('load', runAnimation);
  }

  // Re-position on resize
  window.addEventListener('resize', positionOverlays);
})();
```

- [ ] **Step 2: Verify animation in browser**

Reload `http://localhost:8765`.
Expected:
- Page scroll is locked on load
- Hotel image appears dark except one floor at a time lighting up top → bottom
- Each floor holds for ~1.5s with label visible to its left
- After Floor V: all floors lit, "Scroll" cue appears
- Scrolling (or wheeling) fades the hotel out and reveals the first section beneath

- [ ] **Step 3: Calibrate floor positions**

If floor highlights don't align with the image bands, adjust the `FLOORS` array percentage values. View the hotel image at full size to estimate better values. The roof (~0–14%) is NOT included as a floor — floors start at ~14%.

- [ ] **Step 4: Commit**
```bash
git add index.html
git commit -m "Add hotel floor animation JS — sequential floor reveal on load"
```

---

## Task 4: Update navigation — 5 floors, coloured active states

**Files:**
- Modify: `index.html` — nav HTML (lines ~1748–1758) and nav CSS

- [ ] **Step 1: Replace nav links HTML**

Find the current `<ul>` inside `<nav class="index">` and replace it entirely:

```html
<ul>
  <li><a href="#about"             data-floor="1"><span class="roman">I</span>About</a></li>
  <li><a href="#product-experiment" data-floor="2"><span class="roman">II</span>Product</a></li>
  <li><a href="#lab"               data-floor="3"><span class="roman">III</span>AI Lab</a></li>
  <li><a href="#energy"            data-floor="4"><span class="roman">IV</span>Energy</a></li>
  <li><a href="#certs"             data-floor="5"><span class="roman">V</span>Credentials</a></li>
</ul>
```

- [ ] **Step 2: Add floor colour active state CSS**

After the existing `nav.index ul a:hover { color: var(--red); }` rule, insert:

```css
/* Active floor colour per section */
nav.index ul a[data-floor].floor-active { font-weight: 600; }
nav.index ul a[data-floor="1"].floor-active { color: #C88080; }
nav.index ul a[data-floor="2"].floor-active { color: #CC2200; }
nav.index ul a[data-floor="3"].floor-active { color: #E03030; }
nav.index ul a[data-floor="4"].floor-active { color: #8B1010; }
nav.index ul a[data-floor="5"].floor-active { color: #5C001A; }
```

- [ ] **Step 3: Add active-floor JS (IntersectionObserver)**

Add this IIFE into the `<script>` block:

```javascript
/* ── Active nav floor highlight ── */
(function () {
  const sections = [
    { id: 'about',              floor: 1 },
    { id: 'product-experiment', floor: 2 },
    { id: 'lab',                floor: 3 },
    { id: 'energy',             floor: 4 },
    { id: 'certs',              floor: 5 },
  ];
  const links = Array.from(document.querySelectorAll('nav.index ul a[data-floor]'));

  function setActive(floorNum) {
    links.forEach(a => {
      a.classList.toggle('floor-active', parseInt(a.dataset.floor) === floorNum);
    });
  }

  const io = new IntersectionObserver((entries) => {
    entries.forEach(e => {
      if (e.isIntersecting) {
        const sec = sections.find(s => s.id === e.target.id);
        if (sec) setActive(sec.floor);
      }
    });
  }, { threshold: 0.3 });

  sections.forEach(s => {
    const el = document.getElementById(s.id);
    if (el) io.observe(el);
  });
})();
```

- [ ] **Step 4: Verify in browser**

Scroll through each section. Expected: The correct nav link glows in its floor colour as you enter each section. No sixth "Contact" link visible.

- [ ] **Step 5: Commit**
```bash
git add index.html
git commit -m "Update nav to 5 floors with coloured active states"
```

---

## Task 5: Rename section I → About, add photo layout

**Files:**
- Modify: `index.html` — section#origins HTML and related CSS

- [ ] **Step 1: Rename section id and title**

Change:
```html
<section id="origins" class="chapter" data-screen-label="01 Creative Technologist">
```
To:
```html
<section id="about" class="chapter" data-screen-label="01 About">
```

Change:
```html
<div class="chapter-number">— Chapter I —</div>
<h2>The Creative<br>Technologist</h2>
```
To:
```html
<div class="chapter-number">— Floor I —</div>
<h2>About</h2>
```

- [ ] **Step 2: Update section background CSS reference**

Find:
```css
section#origins { background: var(--ivory); }
```
Change to:
```css
section#about { background: var(--ivory); }
```

- [ ] **Step 3: Add photo + two-column layout CSS**

Insert after the `section#about` background rule:

```css
/* About section — two-column intro */
.about-intro {
  display: grid;
  grid-template-columns: 260px 1fr;
  gap: 48px;
  align-items: start;
  margin-bottom: 40px;
}
.about-photo {
  width: 100%;
  aspect-ratio: 3/4;
  object-fit: cover;
  object-position: center top;
  border: 1px solid var(--hair);
  display: block;
}
.about-text { display: flex; flex-direction: column; gap: 20px; }
.tech-skills-header {
  font-family: var(--f-display);
  font-size: 10px;
  font-weight: 700;
  letter-spacing: 0.45em;
  text-transform: uppercase;
  color: var(--ink-soft);
  margin: 32px 0 0;
}
@media (max-width: 700px) {
  .about-intro { grid-template-columns: 1fr; }
  .about-photo { max-width: 220px; }
}
```

- [ ] **Step 4: Restructure About section HTML**

Replace the current content inside `<div class="chapter-inner">` in section#about (everything from `<span class="po-stamp">` down to just before `<div class="dossier-stack">`):

```html
<div class="about-intro reveal">
  <img src="assets/svetlana-photo.jpg" alt="Svetlana de Boer" class="about-photo">
  <div class="about-text">
    <span class="po-stamp">Product Owner</span>
    <p class="narrative italic role-line">She is also a <span class="rotator" id="roleRotator" aria-live="polite"><span class="rot-word">Builder.</span><span class="rot-word">Technologist.</span><span class="rot-word">Facilitator.</span></span></p>
    <div class="fleuron">✦ ✦ ✦</div>
    <p class="narrative">8+ years taking emerging technology from concept to commercial product. At Shell, I ran the full cycle — from opportunity identification and pen-and-paper PoVs, through 4 POCs, to a €22M+ rNPV product in the 24/7 clean energy domain. Comfortable in deep tech (blockchain, GenAI, IoT, energy systems) and equally focused on the business case behind it. Building AI-native, with hands-on experience in agentic workflows, Claude Code, and MS Copilot.</p>
    <div class="stat-row">
      <div class="stat"><div class="num">8+</div><div class="label">Years</div></div>
      <div class="stat"><div class="num">€2M</div><div class="label">Annual R&amp;D Portfolio</div></div>
      <div class="stat"><div class="num">€22M+</div><div class="label">rNPV</div></div>
      <div class="stat"><div class="num">10+</div><div class="label">Applications</div></div>
    </div>
  </div>
</div>

<div class="tech-skills-header reveal">Technology Skills</div>
```

Keep the existing `<div class="dossier-stack reveal">` unchanged immediately after.

- [ ] **Step 5: Verify in browser**

Expected: About section shows photo on the left, text + stats on the right. Below them, "TECHNOLOGY SKILLS" header then the dossier accordion. No "Chapter I" — shows "Floor I".

- [ ] **Step 6: Commit**
```bash
git add index.html
git commit -m "Rename section I to About, add photo layout and Technology Skills header"
```

---

## Task 6: Rename section II → Product Management, rename proofs

**Files:**
- Modify: `index.html` — section#product-experiment HTML

- [ ] **Step 1: Update section label and title**

Change:
```html
<div class="chapter-number">— Chapter II —</div>
<h2>Six Proofs</h2>
<div class="subtitle">Scroll to reveal each chapter of the work — blocks first, words second. Reverse to return.</div>
```
To:
```html
<div class="chapter-number">— Floor II —</div>
<h2>Product Management</h2>
<div class="subtitle">Product Management · Case Studies — scroll to reveal each project, blocks first, words second.</div>
```

- [ ] **Step 2: Update expand-hero opener label**

Find:
```html
<div class="expand-hero" id="productExpand" data-screen-label="02 Product · Opener">
```
Change `data-screen-label` to `"02 Product Management · Opener"`.

Also update the section:
```html
<section id="product-experiment" class="chapter" data-screen-label="02 Product">
```
To `data-screen-label="02 Product Management"`.

- [ ] **Step 3: Verify in browser**

Expected: Product section title reads "Product Management", subtitle mentions "Case Studies". Chapter number reads "Floor II".

- [ ] **Step 4: Commit**
```bash
git add index.html
git commit -m "Rename section II to Product Management, update subtitle to Case Studies"
```

---

## Task 7: Rename section III → I Built with AI

**Files:**
- Modify: `index.html` — section#lab HTML and expand-hero

- [ ] **Step 1: Update lab section title**

Find:
```html
<div class="chapter-number">— Chapter III —</div>
<h2>The Lab</h2>
```
Change to:
```html
<div class="chapter-number">— Floor III —</div>
<h2>I Built with AI</h2>
```

- [ ] **Step 2: Update data-screen-label**

Find `data-screen-label="03 Lab"` — change to `"03 I Built with AI"`.
Find `data-screen-label="03 Lab · Opener"` — change to `"03 I Built with AI · Opener"`.

- [ ] **Step 3: Commit**
```bash
git add index.html
git commit -m "Rename section III to I Built with AI"
```

---

## Task 8: Rename section IV → Energy Transition

**Files:**
- Modify: `index.html` — section#energy HTML

- [ ] **Step 1: Update energy section title**

Find:
```html
<div class="chapter-number">— Chapter IV —</div>
<h2>Energy</h2>
```
Change to:
```html
<div class="chapter-number">— Floor IV —</div>
<h2>Energy Transition</h2>
```

- [ ] **Step 2: Update data-screen-label**

Change `data-screen-label="04 Energy"` to `"04 Energy Transition"`.

- [ ] **Step 3: Commit**
```bash
git add index.html
git commit -m "Rename section IV to Energy Transition"
```

---

## Task 9: Merge Certifications + Contact → Credentials (Floor V)

**Files:**
- Modify: `index.html` — section#certs and section#contact

- [ ] **Step 1: Update certs section title and id**

Find:
```html
<section id="certs" class="chapter" data-screen-label="05 Certs">
```
Change `data-screen-label` to `"05 Credentials"`.

Find inside it:
```html
<div class="chapter-number">— Chapter V —</div>
<h2>Certifications</h2>
```
Change to:
```html
<div class="chapter-number">— Floor V —</div>
<h2>Credentials</h2>
```

- [ ] **Step 2: Move contact content into section#certs**

Find the entire `<section id="contact" ...>` block (approximately lines 2202–2224) and copy its inner `<div class="chapter-inner">` content. Then paste it inside section#certs, after the closing `</div>` of the certifications fan content and before `</section>`. Add a visual divider:

```html
    <!-- ── Contact ── -->
    <div class="hair reveal" style="margin: 60px 0;"></div>
    <div class="frame reveal">
      <div class="presents" style="color: var(--red); font-family: var(--f-accent); font-style: italic; font-size: 13px; letter-spacing: 0.32em; text-transform: uppercase;">— The Concierge —</div>
      <div class="contact-lines">
        <p>Rotterdam, Netherlands</p>
        <p><a href="mailto:szigalkina@gmail.com">szigalkina@gmail.com</a></p>
        <p><a href="https://www.linkedin.com/in/szigalkina/" target="_blank" rel="noopener">linkedin.com/in/szigalkina</a></p>
      </div>
      <div class="fleuron">✦ ✦ ✦</div>
      <div style="display:flex; gap:12px; justify-content:center; flex-wrap:wrap;">
        <a href="mailto:szigalkina@gmail.com" class="pill">Send a Letter</a>
        <a href="assets/svetlana-zigalkina-cv.pdf" download class="pill">Download CV</a>
      </div>
    </div>
```

- [ ] **Step 3: Delete the standalone contact section**

Delete the entire `<section id="contact" ...>` block (now that its content has been moved into Credentials).

- [ ] **Step 4: Update expand-hero for certs**

Find `data-screen-label="05 Certs · Opener"` — change to `"05 Credentials · Opener"`.

- [ ] **Step 5: Verify in browser**

Expected: Scrolling to Floor V shows "Credentials" with the certifications fan at the top and the contact frame below it. No separate Contact section exists. Footer appears immediately after.

- [ ] **Step 6: Commit**
```bash
git add index.html
git commit -m "Merge Certifications + Contact into single Credentials section (Floor V)"
```

---

## Task 10: Final polish + nav hide behaviour fix

**Files:**
- Modify: `index.html` — nav JS and CSS

- [ ] **Step 1: Fix nav visibility — show after hotel hero dismissed**

The current nav JS shows/hides based on scroll position. It should stay hidden until the hotel hero has been dismissed (scrolled past). Find the nav hide/show IIFE and update the threshold:

```javascript
// Change: if (y > 120 && y > last) nav.classList.add('hidden');
// To ensure nav only appears after user has scrolled past hotel hero:
const hotelH = document.getElementById('hotelHero')?.offsetHeight || window.innerHeight;
if (y > hotelH) nav.classList.remove('hidden');
else nav.classList.add('hidden');
```

Replace the entire nav IIFE with:
```javascript
(function () {
  const nav = document.getElementById('nav');
  let last = window.scrollY;
  let ticking = false;
  window.addEventListener('scroll', () => {
    if (!ticking) {
      requestAnimationFrame(() => {
        const y = window.scrollY;
        const hotelH = document.getElementById('hotelHero')?.offsetHeight || window.innerHeight;
        if (y < hotelH) {
          nav.classList.add('hidden');
        } else if (y > last) {
          nav.classList.add('hidden');
        } else {
          nav.classList.remove('hidden');
        }
        last = y;
        ticking = false;
      });
      ticking = true;
    }
  });
  // Hide on initial load (hotel hero is showing)
  nav.classList.add('hidden');
})();
```

- [ ] **Step 2: Update chapter-map references in CSS**

Search for any remaining `map-gone`, `.hero.map-gone`, or `.cm-` class references in the CSS and delete them if any were missed in Task 1.

Run: `grep -n "map-gone\|cm-\|chapter-map\|hero-signature\|heroSignature" index.html`
Expected output: zero results (or only results inside HTML comments that can stay).

- [ ] **Step 3: Full walkthrough in browser**

Go through the complete page:
1. Load → hotel animation plays, scroll locked ✓
2. After animation → scroll fades hotel → About section visible ✓
3. Nav appears after hotel dismissed, hides on scroll down, shows on scroll up ✓
4. Nav active floor highlights correctly for each of the 5 sections ✓
5. About: photo left, text right, Technology Skills header, dossier ✓
6. Product Management: "Floor II", "Product Management · Case Studies" subtitle ✓
7. I Built with AI: "Floor III", correct title ✓
8. Energy Transition: "Floor IV", correct title ✓
9. Credentials: "Floor V", certs fan + contact frame, no separate contact section ✓
10. Download CV button works ✓

- [ ] **Step 4: Final commit**
```bash
git add index.html
git commit -m "Final polish: nav visibility fix, clean up orphaned CSS references"
```

---

## Notes for implementer

- **Floor zone calibration**: The `FLOORS` array percentages in Task 3 are estimates. After seeing the animation in browser, adjust `top` and `h` values until each overlay aligns with the correct floor band in the hotel image.
- **Photo**: `assets/svetlana-photo.jpg` is a placeholder. Swap it for a final photo later by replacing the file — no code changes needed.
- **No GitHub push** until user confirms the build looks correct in local browser.
