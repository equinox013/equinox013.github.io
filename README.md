# equinox013.github.io — Portfolio Source

Personal portfolio site for **Nicholas Dale** — Data Scientist / Data Analyst based in Melbourne, AU. Hosted on GitHub Pages at [equinox013.github.io](https://equinox013.github.io).

---

## Architecture

The entire site is a **single-file static build** — one `index.html` containing all HTML structure, embedded CSS, and embedded JavaScript. No framework, no build step, no bundler. It deploys directly on GitHub Pages with zero configuration.

```
equinox013.github.io/
├── index.html          # Everything — markup, styles, behaviour
├── assets/
│   ├── logo.png        # Brand mark (618×617 PNG)
│   ├── profile.jpg     # Hero carousel — photo 1
│   ├── profile2.jpg    # Hero carousel — photo 2
│   └── profile3.jpg    # Hero carousel — photo 3
├── DEPLOY.md           # Step-by-step GitHub Pages deployment guide
└── README.md           # This file
```

---

## HTML

The document is structured as a series of `<section>` elements with consistent `id` attributes used by the nav and Intersection Observer:

| Section | `id` | Purpose |
|---|---|---|
| Hero | `#hero` | Name, title, photo carousel, quote rotator |
| About | `#about` | Bio, Tableau embeds |
| Projects | `#projects` | DS / DA tab switcher with HuggingFace embeds |
| Skills | `#skills` | Categorised tech stack grid |
| Experience | `#experience` | Timeline — education and work history |
| Footer | `<footer>` | Social links, motto, easter egg |

**Floating glass pill nav** (`#float-nav`) — visible at ≥1300px viewport width, sits fixed on the left edge. Contains logo, section links, dark mode toggle, music toggle, and the Spotify popup as a child element. At <1300px it is hidden and replaced by `#mobile-nav`, a conventional top bar.

**Image protection** — all `<img>` tags carry `draggable="false"` and a global `contextmenu` listener blocks right-click on all images. No assets link to a downloadable CV.

---

## CSS

All styles are embedded in a `<style>` block in `<head>`. The system is token-based using CSS custom properties.

### Design tokens

```css
:root {
  --bg:          #F5F0E8;   /* Cream — primary background */
  --text:        #1A1A1A;   /* Near-black — all body copy */
  --text-muted:  #6B6560;   /* Secondary text */
  --border:      rgba(26,26,26,0.12);
  --border-bold: rgba(26,26,26,0.35);
  --tag-bg:      rgba(26,26,26,0.06);
  --glass-bg:    rgba(245,240,232,0.72);
  --glass-border:rgba(26,26,26,0.12);
  --glass-shadow:0 8px 32px rgba(0,0,0,0.10);
  --nav-bg:      rgba(245,240,232,0.88);
  --transition:  0.25s ease;
}
```

Dark mode overrides are applied via `[data-theme="dark"]` on the `<html>` element:

```css
[data-theme="dark"] {
  --bg:          #0D1A38;
  --text:        #F5F0E8;
  --glass-bg:    rgba(13,26,56,0.78);
  /* ... */
}
```

The footer always uses a hardcoded dark background (`#111111` / `#07101f` in dark mode) to prevent the cream background from inverting incorrectly when the theme switches — footer link colours are hardcoded to `#F5F0E8` for the same reason.

### Typography

| Role | Font | Source |
|---|---|---|
| Headers / UI labels | Poppins (800–900) | Google Fonts |
| Body / narrative | Raleway (300–600) | Google Fonts |
| Motto / script | Momo Signature | Google Fonts |
| Japanese text | Hachi Maru Pop | Google Fonts |

The `.script` utility class applies Momo Signature. Japanese fun-fact text is detected at runtime and the `.jp-text` class is toggled to apply Hachi Maru Pop automatically.

### Key layout patterns

**Glass pill nav** — `backdrop-filter: blur(22px) saturate(1.8)` with `border-radius: 48px`. The pill has `overflow: visible` so the Spotify popup child can extend below it without clipping.

**Hero layout** — CSS Grid, two columns. Left column: branding + quote rotator. Right column: photo carousel. The quote rotator uses a fixed `height: 132px` with all `.quote-item` elements `position: absolute` — this prevents layout shift when quotes change.

**Skills grid** — `repeat(auto-fill, minmax(200px, 1fr))` with one `.full` class item spanning all columns for the domain expertise row.

**Scroll reveal** — `.reveal` elements start at `opacity: 0; transform: translateY(24px)` and transition to visible once they enter the viewport via Intersection Observer. Delay variants (`.reveal-delay-1` through `-3`) stagger sibling cards.

**Spotify popup** — `position: absolute` child of `#float-nav`, anchored at `top: calc(100% + 10px); left: 0`. This aligns its left edge with the pill's left edge, forming an L-shape. Slides in via `translateY` transition.

---

## JavaScript

All JS is in a single `<script>` block at the end of `<body>`. No external libraries or dependencies.

### Dark mode

Theme preference is stored in `localStorage` under the key `theme`. On load, the stored value (or `prefers-color-scheme` if no stored value) is applied by setting `document.documentElement.dataset.theme`. Both the pill toggle (`#themeToggleFloat`) and mobile toggle (`#themeToggleMobile`) call the same handler and stay in sync.

### Project tab switcher

```js
tabBtns.forEach(btn => btn.addEventListener('click', () => {
  const target = btn.dataset.tab;
  // toggles .active on buttons and .tab-panel elements
}));
```

Two panels: `data-tab="ds"` (Data Science) and `data-tab="da"` (Data Analytics). HuggingFace iframes within each panel are lazy-loaded — they only receive a `src` on first click of the "Launch Demo" overlay button.

### Photo carousel

Three photos (`profile.jpg`, `profile2.jpg`, `profile3.jpg`) with auto-advance every 4000ms. Dot indicators are rendered dynamically and sync on manual click. A `MutationObserver` is not used — the carousel tracks its own index in state.

### Quote rotator

Five testimonial quotes cycle every 5200ms with a CSS `opacity` fade. The active quote fades out first (opacity → 0), then text swaps, then fades back in. All items stay `position: absolute` throughout; only the `active` class controls visibility, never layout.

### HuggingFace lazy loader

```js
const HF = {
  housing: 'https://equinox013-melbourne-housing-predictor.hf.space',
  waste:   'https://equinox013-smart-waste-classifier.hf.space',
  mental:  'https://equinox013-mental-health-transformer.hf.space',
  hikari:  'https://equinox013-hikari-complaint-classifier.hf.space',
};
```

Each project card has a click-to-load overlay. On click, the iframe `src` is set from the `HF` map and the overlay is hidden. This avoids loading all four Spaces on initial page load.

### Spotify toggle

Clicking `#musicToggle` in the pill toggles the `#spotifyPopup` child element (`.open` class). The iframe `src` is only injected on first open — it is not loaded at page init.

### Easter egg

Three decorative `✦` stars in the footer bottom row (`#easterTrigger`) trigger the fun fact rotator on click. When activated:

- The footer motto is hidden and replaced by `#footerEaster`
- A `setInterval` at **5000ms** cycles through the `funFacts` array via `showFact(idx)`
- A `starDance` keyframe animation (staggered per star) plays on the trigger group
- `showFact` detects Japanese characters via regex and toggles `.jp-text` to apply Hachi Maru Pop

Clicking the stars again deactivates the rotator and restores the motto.

### Scroll reveal

A single `IntersectionObserver` watches all `.reveal` elements. When 15% of the element enters the viewport, the `visible` class is added, triggering the CSS transition. The observer disconnects from each element after it has been revealed (fires once).

---

## Deployment

The repo is named `equinox013.github.io` — GitHub Pages auto-serves `index.html` from the `main` branch root with no configuration required. See `DEPLOY.md` for the full push workflow.

---

## AI Acknowledgement

This portfolio was built with substantial assistance from **Claude** (Anthropic), operating within a structured, spec-driven development workflow.

The human-in-the-loop — Nicholas Dale — drove every meaningful decision throughout the process:

- **Initial direction and brand identity** — the two-tone cream/near-black aesthetic, font selections, 4-point star motifs, and the "Data Wizard" narrative were defined upfront and held as non-negotiable constraints throughout.
- **Content and narrative architecture** — all personal content (biography, career history, testimonials, skills, project descriptions, fun facts) was authored and curated by Nicholas, with AI used only to structure and place it within the layout.
- **Specification-driven prompting** — features were requested with specific, precise intent: "glass pill nav with backdrop blur," "fixed-height quote rotator to prevent layout shift," "lazy-load HuggingFace iframes on click," "Spotify popup as an L-shape below the pill." The AI translated these specs into working code.
- **Iterative review and correction** — every output was reviewed visually in-browser. Bugs (dark mode footer inversion, carousel dot clipping, pandas in the wrong skill category, quote layout shift) were caught by the human and reported back for targeted fixes. No change shipped without explicit approval.
- **Taste and restraint** — calls to keep the site minimal, refuse parallax effects, remove personal contact details, reject downloadable CVs, and preserve the two-tone discipline were all human decisions made against what a default AI output might have produced.

Claude contributed code generation, debugging, and implementation velocity. The vision, the brand, and the judgment were Nicholas's throughout.

---

*Built with intention. Shipped with precision.*
