# Hidden Patterns

An interactive **art + computing** exhibit built for the **KCS (King's College School) Year 9 Computer Science Curiosity Day '26**.

Live site: **https://djh-kings.github.io/KCS-ART-KS3/**

---

## What it is

Hidden Patterns overlays a live particle simulation on top of three famous paintings and asks students to work out *what algorithm is driving the motion* — then explains it. The particles follow each painting's **actual brushstrokes** (extracted from the image), so the motion genuinely traces the artwork. Students tweak parameters with sliders and feel how each one changes the behaviour.

- **Audience:** KS3 students (~13–14), in a kiosk / classroom setting. Touch supported.
- **Why:** it makes abstract CS ideas — flow fields, Perlin noise, vector fields, wave superposition — tangible and beautiful by hiding them inside artworks students may already know. The "watch → guess → read" flow creates curiosity before the explanation lands.

## The three works & their algorithms

| Painting | Artist | Algorithm | Interaction |
|---|---|---|---|
| **The Starry Night** | Van Gogh, 1889 | Flow field · Perlin noise | Move the cursor to pull the flow |
| **The Great Wave** | Hokusai, 1831 | Directional flow field | Click to place rocks the flow routes around |
| **The Scream** | Munch, 1893 | Wave interference · superposition | Click to send ripples |

### Following the real brushwork
Rather than a generic synthetic flow, the motion is driven by a **structure-tensor orientation field** extracted from each image (the dominant brushstroke direction at each point), baked into the page as a small per-painting grid (`FIELDS` in the script):

- **Starry Night & Great Wave** (particle modes): particles advect *along* the brushstrokes. Stroke orientation is undirected, so each particle keeps the sense matching its heading (momentum) — this reproduces the central swirl and the wave's curl. **Turbulence** blends synthetic noise on top: low = pure painting, high = chaos.
- **The Scream** (band mode): the flowing bands (the wave-superposition visual) are **bent along the field** — flat across the horizontal sky strokes, curving into the funnel toward the figure — while the summed-sine warp still rides on top, preserving the superposition idea.

## The interface

- **Landing** — two columns: an intro (KCS eyebrow, title, tagline, purpose) on the left; the three works as a clickable list on the right. Dark theme by default.
- **Simulation view**
  - Top bar: `← Hidden Patterns` back-link + tagline, theme toggle.
  - Control strip: Artwork opacity, the mode's parameters, colour swatches, **Particle size**, and **Reset** (returns to the painting-matched defaults).
  - Framed stage with the painting + live canvas.
  - **Reading drawer** (docked, always available — optional, not gated): four tabs — **The artist / The painting / The algorithm / Activities**. The first three are ~1000-word reads; Activities holds the student challenges. A "Final questions" section unlocks once all three works have been visited.

### Parameters by mode
- **Flow** (Starry Night): Particles, Turbulence, Cursor pull, Speed, Particle size
- **Wave** (Great Wave): Current speed, Swell, Rock strength, Rock size, Particle size
- **Ripple** (The Scream): Band spacing, Flow speed, Swirl, Particle size

## Technical overview

- **Single file:** the whole exhibit is `index.html`. It uses a lightweight component system (the `<x-dc>` markup + `support.js` runtime) with a `<script type="text/x-dc">` holding the state, the canvas particle engine, the painting data, and the baked flow fields.
- **Images:** embedded directly in `index.html` as base64 data URIs; the original JPEGs live in `assets/images/`.
- **No build step:** open `index.html` in a browser to run it.
- **Deploy:** GitHub Pages serves the `main` branch. Pushing to `main` triggers a rebuild (allow ~1–2 min, then hard-refresh).

### Repository layout
```
index.html                 # the exhibit (single source of truth)
support.js                 # component-system runtime index.html depends on
assets/images/             # original painting JPEGs
archive/                   # superseded design mocks (reference only)
README.md
```

### Editing content
- Reading texts and activities live in the `DATA` array in `index.html` (`artist`, `painting`, `algorithm`, `activities` per work). Paragraph breaks are `\n\n`.
- Brushstroke fields are the `FIELDS` object (per-painting `{w, h, a[], c[]}` — angle grid + coherence). They were generated offline by a structure-tensor analysis of the images and verified with streamline overlays.

## Development log

All work was done as small PRs onto `main`:

1. **#1** — Landing redesign (two-column), docked reading drawer, top-bar back-link + tagline, initial reading copy.
2. **#2** — Archived the superseded design mocks into `archive/`.
3. **#3** — Dark theme default; removed the reveal mechanic; added the Activities tab.
4. **#4** — New landing copy; populated the Activities tab content.
5. **#5** — Removed activity number badges and the landing 1-2-3 steps; promoted the KCS event line to a prominent eyebrow.
6. **#6** — Added the Particle size slider (all three works).
7. **#7** — Particles follow the real brushwork for Starry Night & the Great Wave (baked structure-tensor fields + momentum advection).
8. **#8** — The Scream's bands follow the field (bent bands + superposition warp).

### Rollback point
The stable desktop build (commit `7c4135d`, after #8) is preserved on the branch **`rollback/v1.0-desktop`**. To restore it, reset `main` to that branch. *(Git tags are blocked by the current hosting proxy, so a branch serves as the marker.)*

## Roadmap

- **Mobile & tablet responsive layout** *(in progress)* — one responsive `index.html` with JS-driven breakpoints (phone < 700px, tablet 700–1100px, desktop > 1100px). Phone: single-column landing, full-bleed canvas with a tap-to-expand control sheet, full-screen reading takeover. Tablet: close to desktop with the reading panel as a slide-over drawer. The engine, texts, and data stay shared across all sizes.
- **Activities slider names** — the Great Wave / The Scream activity prompts currently reference sliders from the Starry Night set; copy to be reconciled with each work's actual controls.
