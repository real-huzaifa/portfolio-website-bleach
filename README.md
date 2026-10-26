# Bleach-Inspired Scroll Portfolio

![HTML5](https://img.shields.io/badge/HTML5-000000?logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-000000?logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-000000?logo=javascript&logoColor=white)
![GSAP](https://img.shields.io/badge/GSAP_3.15-000000?logo=greensock&logoColor=white)
![Python](https://img.shields.io/badge/Python_tools-000000?logo=python&logoColor=white)

A cinematic, scroll-driven portfolio website styled after the black-and-white ink of the manga *Bleach*. Every section is a pinned scene with its own animation timeline, so the page reads like turning manga pages: rain falls over the opening panel, a figure drops into frame and lands, paper shards tear away, and rows of work ink in as they reach the centre of the screen.

**Live demo:** https://ahmed-huzaifa-malik-bleach-portfolio.vercel.app/

---

## Project overview

**Goal:** recreate the motion design of a reference "Bleach-inspired anime web design" video (pinned scenes, scrubbed timelines, colour-shift transitions, list rows that fill as they pass) and turn it into a working personal portfolio.

**Constraints**

- Strictly black and white. The reference used red and pink; every transition here is reworked in ink and paper.
- No framework and no build step. One HTML file that deploys anywhere as static files.
- Smooth on phones as well as desktops, and respectful of reduced-motion settings.

**Outcome:** six animated sections, a canvas rain system tuned to the artwork, a small Python pipeline that prepares the manga panels, and nine original illustrations generated in code.

## Features

- **Pinned, scroll-scrubbed scenes** built on GSAP ScrollTrigger. Scrolling drives each timeline forwards and backwards.
- **Canvas rain** in three depth layers that gets heavier as you scroll, angled to match the rain inked in the landing panel.
- **Falling-figure sequence:** a white silhouette spins down and lands upright while the scene fades from ink to paper, then cross-fades into the inked version of the figure.
- **Shard wipe:** paper polygons tear away to reveal the About section.
- **Experience rows** that fill black as they cross the middle of the screen, each with an inverted ink illustration inside.
- **Project cards** that wipe in from below, with parallax inside each card.
- **Section rail and nav** that switch between light and dark text depending on the scene behind them.
- **Film grain, floating dust and a custom cursor** for texture.

## Scenes

| # | Scene | Scroll behaviour |
|---|-------|------------------|
| 01 | Landing | Rain thickens, the camera pushes into the figure's face, blurs and cuts to black |
| 02 | Story | Dust, a white flash, the figure falls and lands, panels slide in, speed lines, "Models that HIT." |
| 03 | About | Shards tear away, the crowned figure fades in beside the summary |
| 04 | Experience | Five rows that ink in one at a time |
| 05 | Work | Four project cards with original illustrations |
| 06 | Contact | Email copy button, LinkedIn and GitHub |

## How it works

### Scroll engine
Lenis smooths the scroll, and its scroll events feed `ScrollTrigger.update` through GSAP's ticker, so pinned scenes and smooth scrolling never drift apart. Each scene is a `gsap.timeline` with `pin: true` and `scrub: 1`. The `end` value (for example `+=680%` for the story scene) sets how much scrolling the scene lasts.

### Rain system
Rain is drawn on a `<canvas>` in three layers (far, mid, near), each with its own speed, length, width and opacity. Every layer is stroked as a single path per frame to keep draw calls low. The scroll progress of the landing scene sets an intensity value that controls drop count, speed and slant. The slant came from the artwork itself: a structure-tensor estimate over the open sky in the panel gave a lean of roughly 7° down and to the left.

### Ink-on-paper compositing
- Panels on paper use `mix-blend-mode: multiply`, so the white of the scan disappears and only the ink stays.
- Panels on black use `filter: invert(1)` with `mix-blend-mode: screen`, so the ink turns into white linework on black.
- Reveals use animated `clip-path: inset()` instead of opacity, so every edge stays hard like a cut panel.

### Asset pipeline (`tools/`)
- `prepare_panels.py gray` converts panels to grayscale, auto-levels them and compresses them to WebP.
- `prepare_panels.py cutout` removes the white background around a figure by flood-filling from the image edges. Enclosed whites such as a face or a skull are kept, and the figure is tinted ink-to-bone.
- `prepare_panels.py silhouette` produces the solid white silhouette (holes filled with SciPy morphology) and the transparent ink layer used in the falling-figure cross-fade.
- `draw_illustrations.py` draws the nine experience and project illustrations from scratch: manga focus lines, halftone dot screens, speed lines and hatching, plus a motif for each entry (ranking bars, boosted trees, a churn crowd, A/B curves, a data pipeline, a gamepad over review scores, a fraud outlier cloud, a risk gauge, a cart with a chi-square tail).

## Challenges and fixes

| Problem | Fix |
|---|---|
| Images inside the experience rows never loaded | Lazy loading skips images inside an element clipped to zero height, so those images load eagerly |
| The multiply blend had no effect on the contact art | A positioned element with `z-index` forms its own stacking context, so the blend mode moved from the image to that layer |
| Pinned scenes drifting against smooth scrolling | Lenis drives `ScrollTrigger.update` from GSAP's ticker, with lag smoothing off |
| Generated rain looked pasted on top of the art | Measured the streak angle in the panel and matched the canvas rain to it |
| Canvases costing CPU while off screen | Rain and dust only animate while their section is in view |
| White figure art leaving hard rectangle edges | Radial and linear CSS masks feather each panel into the background |

## Tech stack

| Area | Tools |
|---|---|
| Markup and styling | HTML5, CSS custom properties, CSS masks, blend modes and clip-path |
| Animation | GSAP 3.15, ScrollTrigger, Lenis 1.3, Canvas 2D |
| Type | Anton, Archivo, IBM Plex Mono (Google Fonts) |
| Asset tooling | Python, Pillow, NumPy, SciPy |
| Testing | Playwright screenshots across the scroll timeline at desktop and phone widths |

## Project structure

```
.
├── index.html                 # markup, styles and all animation code
├── assets/
│   ├── hero-rain.webp         # landing panel
│   ├── fall-white.webp        # falling figure, silhouette
│   ├── fall-ink.webp          # falling figure, ink layer
│   ├── bankai.webp            # story panels
│   ├── profile.webp
│   ├── clash.webp
│   ├── crown-cut.webp         # About figure (cut out)
│   ├── table.webp             # contact panel
│   └── art-*.webp             # generated experience and project illustrations
├── tools/
│   ├── prepare_panels.py      # grayscale, cutout and silhouette processing
│   ├── draw_illustrations.py  # procedural ink illustrations
│   └── requirements.txt
└── docs/                      # README screenshots
```

## Customisation

- **Content:** each scene is a `<section>` with an id (`#intro`, `#story`, `#about`, `#experience`, `#work`, `#contact`).
- **Colours:** one set of CSS variables in `:root` (`--ink`, `--paper`, `--bone` and others).
- **Timing:** each scene's timeline sits in its own commented block in the script. Change the `end` value to lengthen or shorten a scene.
- **Art:** drop a new file into `assets/` under the same name, or run it through `tools/prepare_panels.py` first.

## Accessibility

- `prefers-reduced-motion` turns off smooth scrolling, grain and looping animation, and the rain is drawn as a still frame.
- All content is readable without JavaScript. Hidden starting states are only applied once the script has loaded.
- Real `<a>` and `<button>` elements with visible focus outlines. Decorative images are hidden from screen readers.

## Future improvements

- Drive the falling figure from a rendered image sequence for true 3D rotation.
- Add a case-study page for each project.
- Add a preloader that waits for the panels before the opening animation runs.

## Credits

- *Bleach* manga panels © Tite Kubo / Shueisha, used as fan art in a personal, non-commercial project. Not affiliated with the creators or publishers.
- Experience and project illustrations are original, generated by `tools/draw_illustrations.py`.
- [GSAP](https://gsap.com/) (GSAP Standard License) and [Lenis](https://github.com/darkroomengineering/lenis) (MIT).

## Author

**Ahmed Huzaifa Malik**, Data Analyst and Data Scientist

- Email: ahmedhuzaifamalik@gmail.com
- LinkedIn: [ahmed-huzaifa-malik](https://www.linkedin.com/in/ahmed-huzaifa-malik/)
- GitHub: [real-huzaifa](https://github.com/real-huzaifa)
