# GitHub Copilot instructions — AzMeterTaxi

Static single-page **Astro 6** marketing site for "Meter Taxi". TypeScript strict, plain CSS, vanilla JS. No React/Vue/Svelte, no Tailwind, no CSS frameworks. Node >= 22.12.0.

## Commands
- `npm run dev` — dev server
- `npm run build` — production build to `dist/`
- `npm run preview` — preview build

## .astro file conventions
- Four-part order: `---` frontmatter → markup → `<style>` → `<script>`.
- Component files are `PascalCase.astro` in `src/components/`.
- Repeated UI is a typed array of objects in frontmatter rendered with `.map()` (see `apps` in `AppSection.astro`).
- Inline SVG icons live as strings in data and render via `set:html`.
- Use scoped `<style>`; `is:global` only for site-wide utilities.
- Inside `<script>`, guard DOM access: `const el = ...; if (el) { ... }`.

## Styling
- Dark theme: background gradient `#0f0c29 → #302b63 → #24243e`.
- Accent gradient `#818cf8 → #c084fc` (indigo → purple).
- Font `"Inter"`; use `clamp()` for responsive sizes.
- Reuse the existing animation system: `.reveal` / `.reveal-left` / `.reveal-scale` (with `.revealed` via IntersectionObserver) and `.animate-fade-up` etc. with `.delay-N`. Do not invent new animation utilities.
- Mobile-first media queries at `600px`, `768px`, `1024px`, `1400px`.
- Keep `-webkit-` prefixes for `backdrop-filter`.

## Do not
- Add dependencies or UI libraries without asking.
- Introduce a CSS framework or convert existing CSS.
- Reformat unrelated files.
- Change `output`/`base` in `astro.config.mjs` without confirming.

## Accessibility
- One `<header>`/`<main>`/`<footer>`; icon-only links need `title`/`aria-label`; images need `alt`.

Always verify with `npm run build` after changes.
