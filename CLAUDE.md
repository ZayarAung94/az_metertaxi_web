# CLAUDE.md — AzMeterTaxi (Meter Taxi) landing page

Guidance for AI agents (Claude Code and others) working in this repository.

## What this is

A static, single-page **Astro 6** marketing site for "Meter Taxi" (AzMeterTaxi) — a ride-hailing platform with Driver, Channel Owner, Partner, and Passenger apps. Includes a launch countdown (Passenger app launches **April 1, 2027**) and app download cards.

- Output: **static** (`output: "static"` in `astro.config.mjs`)
- Language: **TypeScript strict**
- Styling: **plain CSS** in `.astro` `<style>` blocks — no Tailwind, no CSS framework
- Runtime: **vanilla JS** in component `<script>` blocks — no React/Vue/Svelte
- Node `>=22.12.0`

## Commands

```bash
npm run dev       # dev server (default http://localhost:4321)
npm run build     # build to dist/
npm run preview   # preview production build
npm run astro ... # astro CLI
```

**Always run `npm run build` before finishing** to confirm the change compiles.

## Project structure

```
src/
  components/
    Header.astro            # fixed top nav
    CountdownSection.astro  # hero + launch countdown
    AppSection.astro        # app download cards (data-driven)
    Footer.astro            # footer
  pages/
    index.astro             # page assembly + global styles/animations + SEO meta
public/                     # favicon, static assets
astro.config.mjs            # site: https://kilotaxi-mm.com, base "/", static output
```

## Conventions (follow these)

### Astro components
- Four-part file order: **frontmatter → markup → `<style>` → `<script>`**.
- Component files are `PascalCase.astro`.
- Repeated content is a **typed array of objects in the frontmatter** and rendered with `.map()`. See the `apps` array in `AppSection.astro`.
- Inline SVG icons are strings in the data and rendered with `set:html`.
- `<style>` is scoped unless `is:global` is used for site-wide utilities (`index.astro` global styles + `.reveal` system, section styles).
- Guard DOM access: `const el = document.getElementById(...); if (el) {...}`.
- New design/data live in the component, not hardcoded in `index.astro`.

### Styling
- Dark theme. Background gradient `#0f0c29 → #302b63 → #24243e`.
- Accent gradient `#818cf8 → #c084fc` (indigo → purple), used via `background-clip: text`.
- Font: `"Inter"`. Use `clamp()` for responsive typography.
- Radii: pills `2rem`, cards `0.75–0.875rem`, icon tiles `12–14px`.
- **Reuse the existing reveal/animation system** — don't invent new ones:
  - Scroll reveal: `.reveal`, `.reveal-left`, `.reveal-right`, `.reveal-scale` (+ `.revealed` added by IntersectionObserver in `index.astro`).
  - Entrance: `.animate-fade-up`, `.animate-fade-down`, `.animate-fade-scale`, `.animate-slide-left/right` with `.delay-1..9`.
  - Staggered scroll: `.reveal-delay-1..5`.
- Mobile-first media queries: `600px`, `768px`, `1024px`, `1400px`.
- Keep `-webkit-` prefixes for `backdrop-filter`.

### Content & data
- App entries carry `name`, `description`, `iconSvg`, `apkUrl`, `playStoreUrl`, `appStoreUrl` (and optional `comingSoon`). Use `"#"` for missing URLs.

### Accessibility & SEO
- Preserve one `<header>` / `<main>` / `<footer>`.
- Icon-only links need `title` or `aria-label`.
- Keep the SEO meta block (OG/Twitter/canonical) in `index.astro`; mirror it for new pages.

## Constraints — do NOT
- Add dependencies (beyond `astro`) or frameworks without asking.
- Convert CSS to Tailwind or add a UI kit.
- Reformat or rewrite unrelated files wholesale.
- Change `astro.config.mjs` `output`/`base` without confirming deployment.
- Commit secrets.

## Known placeholders / tech debt (fix only if asked)
- `playStoreUrl` / `appStoreUrl` are mostly `"#"`.
- Only the Driver App has a real `apkUrl`; others are placeholders.
- Typo in `Header.astro` nav link text: "Downlaod" → should be "Download".
- `Footer.astro` uses `rel="stylesheet"` on a company link (should be a normal link, not a stylesheet rel).
