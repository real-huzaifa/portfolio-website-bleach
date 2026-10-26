# Ahmed Huzaifa Malik · Portfolio

A scroll-driven, black-and-white portfolio inspired by the manga *Bleach*. Each section is a scene: rain falls over the landing panel, a figure drops into frame and lands, white shards tear away to reveal the About page, and experience rows ink in one at a time as you scroll.

I'm a Data Analyst and Data Scientist, a Data Science undergraduate at PAF-IAST, Pakistan. This site holds my internships, projects and contact details.

![Landing page: rain over a manga panel](docs/preview.jpg)

**Live site:** _add your GitHub Pages link here after deploying (see [Deploy](#deploy))_

---

## Scenes

![Four scenes from the site](docs/scenes.jpg)

| # | Section | What happens as you scroll |
|---|---------|----------------------------|
| 01 | **Landing** | A rain panel with an animated rain layer drawn on a canvas. The rain gets heavier, then the camera pushes into the figure's face, blurs and cuts to black. |
| 02 | **Story** | Dust floats in the dark. A white flash, then a silhouette spins down and lands standing as the screen fades to a blank page under "Every datapoint lands somewhere." Panels slide in, speed lines sweep across and "Models that HIT." takes over. |
| 03 | **About** | White shards tear away to reveal the About page, with the crowned skull figure fading in beside the summary, stack and certifications. |
| 04 | **Experience** | Five internships. The row in the middle of the screen fills black with an ink illustration of that role's work. |
| 05 | **Work** | Four project cards that wipe in from below, each with its own illustration. |
| 06 | **Contact** | Email with a copy button, plus LinkedIn and GitHub. |

![Project cards](docs/work.jpg)

## Built with

- **HTML, CSS and vanilla JavaScript.** One page, no framework, no build step.
- **[GSAP 3.15](https://gsap.com/)** with **ScrollTrigger** for the pinned, scroll-scrubbed timelines.
- **[Lenis 1.3](https://github.com/darkroomengineering/lenis)** for smooth scrolling.
- **Canvas 2D** for the rain and the floating dust.
- **Google Fonts:** Anton (display), Archivo (body), IBM Plex Mono (labels).

## Project structure

```
.
├── index.html          # markup, styles and all animation code
├── assets/
│   ├── hero-rain.webp      # landing panel
│   ├── fall-white.webp     # falling figure, silhouette
│   ├── fall-ink.webp       # falling figure, ink version
│   ├── bankai.webp         # story panels
│   ├── profile.webp
│   ├── clash.webp
│   ├── crown-cut.webp      # About figure
│   ├── table.webp          # contact panel
│   └── art-*.webp          # experience and project illustrations
└── docs/                # README screenshots
```

## Run locally

Serve the folder from a local server so the images and scripts load the same way they will online:

```bash
# Python
python -m http.server 8000

# or Node
npx serve .
```

Then open http://localhost:8000.

## Deploy

**GitHub Pages**

1. Push this folder to a repository, with `index.html` at the root.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, pick `main` and `/ (root)`, then save.
4. After a minute the site is live at `https://<your-username>.github.io/<repo-name>/`.

**Netlify or Vercel:** drag the folder onto the dashboard, or import the repository. No build command is needed.

## Customising

All edits happen in `index.html`.

- **Content:** each section is a labelled `<section>` (`#intro`, `#story`, `#about`, `#experience`, `#work`, `#contact`). Change the text in place.
- **Art:** replace a file in `assets/` with one of the same name, or change the `src`. Panels on light backgrounds use `mix-blend-mode: multiply`, so black ink on a white background works best.
- **Colours:** the palette is defined once as CSS variables in `:root` (`--ink`, `--paper`, `--bone` and so on).
- **Animation:** the script is split into commented blocks (`rain`, `hero`, `story`, `about`, `experience rows`, `work + contact`). Each pinned scene has its own timeline, and the `end: '+=230%'` values set how much scrolling each one takes.

## Accessibility and performance

- Honours `prefers-reduced-motion`: smooth scrolling, film grain and the looping animations switch off, and the rain is drawn as a still frame.
- The rain and dust canvases only animate while their section is on screen.
- All images are compressed WebP, and the experience and project art is lightweight line work.
- Text stays readable without JavaScript, and everything uses real links and buttons with visible focus states.

## Credits

- The *Bleach* manga panels used in the landing, story, About and contact sections are © Tite Kubo / Shueisha. They're used here as fan art in a personal, non-commercial portfolio. This project has no affiliation with the creators or publishers.
- The experience and project illustrations are original.
- GSAP is free under the [GSAP Standard License](https://gsap.com/standard-license/). Lenis is MIT-licensed.

## Contact

- Email: ahmedhuzaifamalik@gmail.com
- LinkedIn: [ahmed-huzaifa-malik](https://www.linkedin.com/in/ahmed-huzaifa-malik/)
- GitHub: [real-huzaifa](https://github.com/real-huzaifa)
