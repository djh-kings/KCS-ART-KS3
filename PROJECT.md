# Hidden Patterns — Project Description

**Live:** https://djh-kings.github.io/KCS-ART-KS3/

---

## What it is
**Hidden Patterns** is an interactive **art-meets-computing exhibit**: a single web page that overlays a live, physics-style particle simulation on top of three of the most famous paintings in the world, and challenges the viewer to work out the *algorithm* secretly driving the motion — before reading the explanation.

The core idea is misdirection in the best sense. A student looks at *The Starry Night* and sees the sky come alive, swirling exactly the way Van Gogh painted it. Their first thought is "that's beautiful." Their second is "…how is it doing that?" That gap — between wonder and understanding — is where the learning happens.

## Who it's for
- **Primary audience:** **KS3 students, roughly age 13–14** (UK Year 9), encountering algorithms and computational thinking, often for the first time.
- **Built for a specific event:** the **King's College School (KCS) Year 9 Computer Science Curiosity Day '26**.
- **Setting:** a **kiosk / classroom** environment. It's designed to teach itself when no teacher is hovering, and it supports **touch** as well as mouse, so it works on classroom screens and tablets, not just laptops.
- **Secondary audience:** passing teachers, parents, and visitors who can pick it up in seconds without instructions.

## What it does (the experience)
1. **Choose a work.** The landing screen presents three paintings as a clickable list, alongside a short statement of purpose.
2. **Watch.** Open a painting and a particle field comes to life *over* the artwork, tracing its actual movement — Van Gogh's swirls, Hokusai's curling wave, Munch's screaming sky.
3. **Experiment.** A strip of sliders changes the behaviour in real time — number of particles, turbulence, speed, particle size, and painting-specific controls. Students can also interact directly: move the cursor to pull the flow (Starry Night), click to drop rocks the current routes around (The Great Wave), or tap to send ripples (The Scream).
4. **Guess, then read.** A reading panel offers four tabs — **The artist, The painting, The algorithm, Activities** — so a curious student can go as deep as they like, entirely optionally. Nothing is gated; they might read all of it or none of it.
5. **Reset** returns everything to the painting-matched default.

## The three works and the computer science behind them

| Painting | Artist | The hidden algorithm |
|---|---|---|
| **The Starry Night** | Van Gogh, 1889 | **Flow field / Perlin noise** — particles follow an invisible map of directions |
| **The Great Wave** | Hokusai, 1831 | **Directional flow field** — the same idea, routing around obstacles |
| **The Scream** | Munch, 1893 | **Wave interference / superposition** — overlapping waves added together |

Each maps onto a genuine, important CS/maths concept: vector fields and smooth procedural noise (used everywhere in graphics, games, and film), flow around obstacles (the basis of fluid and aerodynamic simulation), and wave superposition (the principle behind optics, sound, and noise-cancellation).

## What makes it special: it follows the real brushwork
This is the heart of the project, and what sets it apart from a generic "pretty particles" demo. The particles don't move in a made-up pattern that merely *resembles* each painting — they follow the painting's **actual brushstrokes**, extracted from the image itself.

Each painting is analysed with a **structure tensor** (a standard computer-vision technique) to find the dominant brushstroke direction at every point, and that orientation field is baked into the page. Then:
- **Starry Night and The Great Wave** advect particles *along* those strokes — which is why the swirl and the wave's curl emerge naturally rather than being faked.
- **The Scream** keeps its flowing-bands "superposition" look but **bends those bands along the field**, so they lie flat across the horizontal sky and curve down into the funnel around the screaming figure.

The **Turbulence** slider then has real meaning: at its lowest, the particles trace the painting faithfully; turn it up and synthetic chaos takes over. So a student can literally *feel* the difference between "structured by an algorithm" and "random."

## Why it works pedagogically
- **Curiosity before content.** The watch-then-explain order creates a question in the student's mind before handing them the answer — far stickier than leading with a definition.
- **Hands-on intuition.** Moving a slider and watching the behaviour change builds the core CS instinct that *a parameter is a knob on a rule*, without any code or jargon.
- **Beauty as the hook.** It hides abstract maths inside artworks students may already recognise, so the concepts arrive attached to something memorable.
- **Self-paced, optional depth.** The ~1000-word reads and the workbook-style **Activities** (challenges like "work out what each slider does," "find the calm spot," "get the simulation as close to the painting as you can") let a class span the full range from a 30-second glance to a full lesson.

## How it's built
- A **single self-contained `index.html`** — no build step; it opens in any browser. It uses a lightweight component system (the `<x-dc>` markup plus a `support.js` runtime) with the state, the canvas particle engine, the painting data, the reading texts, and the baked brushstroke fields all in one script block.
- The **paintings are embedded** directly in the page (base64), with the originals kept in `assets/images/`.
- **Deployed via GitHub Pages** from the `main` branch; every merge redeploys automatically.
- Development runs as small, reviewed pull requests, each documented in the README, with a tagged **rollback point** preserved for safety.

## Current status
The desktop experience is complete: dark theme by default, the two-column landing, the docked reading panel with all four tabs and the real reading copy, the particle-size control, and — most importantly — all three paintings driven by their real brushwork. The project is now being made **responsive for tablets and phones**: the tablet behaviour (a slide-over reading panel) is live, and the phone-specific layout is the next stage.
