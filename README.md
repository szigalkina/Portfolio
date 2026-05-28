# Svetlana Zigalkina de Boer — Portfolio

A single-page portfolio built with a Wes Anderson aesthetic — clean geometry, Playfair Display serif, Jost sans-serif, and an Italianno script signature. No frameworks. Pure HTML, CSS, and vanilla JavaScript.

---

## Structure

The portfolio is organised as **six chapters**, each with its own visual identity and interaction:

| Chapter | Section | Theme |
|---------|---------|-------|
| I | The Creative Technologist | Dossier accordion — Blockchain, AI, IoT, 3D |
| II | Product | Full lifecycle product ownership |
| III | The Lab | Experiments and side projects |
| IV | Energy | 6 years of energy domain expertise |
| V | Certifications | Fan-stack card interaction |
| VI | Contact | — |

---

## Key Interactions

- **Chapter Map opening** — Animated landing overlay with staggered Roman numeral chapter list. First scroll dissolves the map and reveals the full concierge image. Page scroll is locked during the overlay so the gesture only dismisses it.
- **SVG signature** — Italianno script revealed via clip-path animation (stroke leads, fill trails 280ms behind).
- **Scroll-scrub video** — Hero videos in Product, Energy and Certifications sections advance frame-by-frame with scroll position.
- **Dossier accordion** — Click-to-expand tech skill rows with monospace spec lines in the Creative Tech section.
- **Certifications fan** — Click-to-advance stacked card interaction.
- **Horizontal gallery** — Drag-to-scroll with arrow navigation, keyboard support and progress bar.

---

## Running Locally

```bash
# Serve from the project root (Python)
python3 -m http.server 8765

# Then open
open http://localhost:8765
```

Or use the VS Code Live Server extension, or any static file server.

---

## Tech Stack

- **HTML / CSS / JavaScript** — no build step, no frameworks
- **Fonts** — Italianno, Jost, Playfair Display (Google Fonts)
- **Assets** — MP4 videos, WebP/PNG images in `/assets`

---

## Assets

| File | Used in |
|------|---------|
| `hero-concierge.png` | Hero landing section |
| `product-hero.mp4` | Product chapter opener |
| `energy-hero.mp4` | Energy chapter opener |
| `certs-hero.mp4` | Certifications chapter opener |

---

*Built with Claude — Anthropic's AI assistant.*
