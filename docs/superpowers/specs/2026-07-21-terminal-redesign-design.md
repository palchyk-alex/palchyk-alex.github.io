# Terminal-Aesthetic Single-Page Redesign

**Date:** 2026-07-21
**Status:** Approved — pending implementation plan

## Purpose

Redesign the personal site into a single page with a terminal / hacker aesthetic. Consolidate `index.html`, `certifications.html`, and `projects.html` into a single `index.html` that reads like a scrollable terminal buffer. Drop the projects section entirely; add a short about/bio section.

## Constraints

- No build step, no package manager, no test suite. GitHub Pages serves the repo root.
- `python3 -m http.server 8000` from the repo root must continue to work as local preview.
- Icons8 attribution must remain (per repo `CLAUDE.md` — attribution is a licensing requirement).
- Existing image assets under `images/` (profile photo, social icons, terraform icon) stay in place and are reused.

## Scope

### In scope
- Rewriting `index.html` end-to-end into a single-page site.
- Deleting `certifications.html` and `projects.html`.
- One inline `<style>` block and one inline `<script>` block in `index.html`. No new external files apart from the Google Font import.

### Out of scope
- New tooling (bundlers, CSS preprocessors, etc.).
- The `images/` assets themselves — reused as-is.
- A projects section (explicitly removed).
- Interactive terminal features (typed commands, `help`, etc.). This is stylistic only.

## Page structure

Long-scroll layout, no nav. Sections in order:

1. **Intro** — fills the viewport
2. **About** — short bio
3. **Certifications** — directory-listing-style list
4. **Footer** — one line, sticks to natural document bottom

### 1. Intro

Vertically centered in a full-viewport container. Content, top to bottom:

- A single "terminal line" block. On page load, a typewriter animation types:
  1. First line: `~ $ whoami` (green, mono)
  2. ~250ms pause
  3. Second line: `Alex Palchyk — DevOps Engineer` (green, mono)
- A green block cursor (`▊`) blinks at 1Hz. It follows the last typed character during animation; after completion it stays blinking at the end of line 2.
- Profile photo (`images/profile.jpg`): 80–90px, `border-radius: 50%`, `object-fit: cover`. Wrapped with a subtle 1px green outline via `box-shadow: 0 0 0 1px rgba(0, 255, 156, 0.4)`. Placed below the terminal line block, centered.
- Social icons: existing GitHub and LinkedIn PNGs (`images/icons/github.png`, `images/icons/linkedin.png`), placed below the photo, ~45×45px each. On hover, apply `filter: drop-shadow(0 0 6px #00ff9c)` for a soft green glow.

**Reduced motion:** if `prefers-reduced-motion: reduce`, skip the char-by-char typing — render both lines immediately. Cursor blink still runs (it's ambient, not motion-triggering per WCAG guidance for the blink cadence used).

### 2. About

Below the intro fold. Structure:

- Section header: `~/about $ cat bio.md` in green mono.
- Thin horizontal rule (`<hr>`) with `border: 1px solid rgba(0, 255, 156, 0.2)` immediately under the header.
- Body: 1–2 short paragraphs of prose. Body color (`#c0c8d0`), monospace, `max-width: 65ch`, `line-height: 1.65`, centered horizontally.
- Generous vertical padding above and below (`~6rem`) so it breathes.

**Bio copy:** placeholder in the initial implementation (`TODO: bio copy — user to provide`). Real copy is a follow-up that can happen before or after the redesign lands. Placeholder should be a short, plausible DevOps bio so the layout is verifiable.

### 3. Certifications

Same padding treatment as About. Structure:

- Section header: `~/certifications $ ls -la` in green mono.
- Thin rule under the header.
- A `<ul>` of certification rows. Each `<li>` is an `<a>` that wraps the entire row and links to the existing Credly URL (`target="_blank"`, `rel="noopener noreferrer"`).
- Row layout (flex): `[icon]  [title]  [spacer]  [→ arrow]`.
  - Icon: existing PNG at 40px.
  - Title: green mono, no underline.
  - Arrow: `→` (or `↗`), muted grey (`#5c6773`), right-aligned.
- Row hover: background `rgba(0, 255, 156, 0.06)`; arrow becomes amber (`#ffb454`); title stays green. No transform / scale.
- Focus (keyboard nav): visible green outline on the whole row.
- Mobile (`<600px`): icon shrinks to ~28px; title stays on one line where possible, truncates with `text-overflow: ellipsis` only if necessary.

The six certifications and their Credly URLs come from the current `certifications.html` — carried over verbatim:

- CKA: Certified Kubernetes Administrator
- AWS Certified Solutions Architect – Associate
- CCNA: Introduction to Networks
- HashiCorp Certified: Terraform Associate (002)
- Linux Foundation Certified Systems Administrator
- MTA: Networking Fundamentals

### 4. Footer

One horizontal line, small (`0.85rem`), muted grey (`#5c6773`), sticks to the natural bottom of the document (not fixed):

```
from apr 2021        icons by icons8         ~ $ █
```

- Left: `from apr 2021` (lowercased).
- Center: `icons by ` + `<a>` linking to `https://icons8.com` (Icons8 attribution).
- Right: a static `~ $ ` with a blinking block cursor.

## Styling system

### Palette

| Token       | Hex                        | Usage                                                          |
| ----------- | -------------------------- | -------------------------------------------------------------- |
| `--bg`      | `#0a0e14`                  | Page background                                                |
| `--green`   | `#00ff9c`                  | Prompts, section headers, cert titles, blinking cursor         |
| `--body`    | `#c0c8d0`                  | Bio prose, general body text                                   |
| `--muted`   | `#5c6773`                  | Footer, arrows, borders (via alpha), secondary text            |
| `--amber`   | `#ffb454`                  | Link/row hover accent only                                     |
| `--green-a` | `rgba(0, 255, 156, 0.2)`   | Section header rules                                           |
| `--green-b` | `rgba(0, 255, 156, 0.06)`  | Cert row hover background                                      |

Defined as CSS custom properties on `:root` so any future palette change is one file, few lines.

### Typography

- **Family:** JetBrains Mono, loaded via Google Fonts (`https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600;700&display=swap`).
- **Fallback stack:** `'JetBrains Mono', ui-monospace, 'SF Mono', Menlo, Consolas, monospace`.
- **Sizes:**
  - Prompt lines (typewriter): `1.5rem` desktop / `1.15rem` mobile
  - Section headers: `1.25rem`
  - Body prose: `1rem`, `line-height: 1.65`
  - Cert row title: `1rem`
  - Footer: `0.85rem`
- Weights: 400 for body/prompts, 600 for section headers, 700 reserved (may not be needed).

### Layout

- `body`: `background: var(--bg); color: var(--body); margin: 0; padding: 0`.
- Sections use a centered container: `max-width: 720px; margin: 0 auto; padding: 6rem 1.5rem`.
- Intro uses `min-height: 100vh; display: flex; flex-direction: column; justify-content: center; align-items: center` so its content lands centered on any viewport.
- Certification rows: flex, `gap: 1rem`, `padding: 0.75rem 1rem`, `border-radius: 4px`.

## Behavior

### Typewriter (JavaScript)

- Small inline `<script>` at the end of `<body>`.
- On `DOMContentLoaded`:
  1. Check `window.matchMedia('(prefers-reduced-motion: reduce)').matches`. If true, insert both strings immediately and skip.
  2. Otherwise, iterate character-by-character over the prompt line, appending to a `<span>`. Base delay ~55ms per character with ±20ms jitter for organic feel.
  3. Pause ~250ms.
  4. Insert a newline element, then iterate character-by-character over the second line.
  5. When done, leave the blinking cursor at the end.
- The cursor is a separate `<span class="cursor">▊</span>` positioned after the current typing target. Its blink is CSS-only (`@keyframes blink` with 1s period).
- Total code budget: ~25 lines of vanilla JS, no libraries.

### Accessibility

- Semantic HTML: `<h1>` for the name (visually rendered by the typewriter output; use `aria-live="polite"` on the typing container so it announces the final content, and hide the animation from AT with `aria-hidden="true"` if the double-announce becomes an issue — will confirm at implementation time).
- `<h2>` for section headers ("About", "Certifications") — the visible text is the styled prompt line, but the underlying element is a real heading for landmark navigation.
- `<ul>` / `<li>` for certifications with `<a>` inside each `<li>`.
- Focus ring: `outline: 2px solid var(--green); outline-offset: 2px` for all interactive elements.
- Alt text preserved from current pages.
- Color contrast: `--green` on `--bg` and `--body` on `--bg` both target WCAG AA at the sizes used. If `--green` fails contrast for the smaller cert-row title, we'll darken the background side rather than the green, to keep the phosphor look.

### Mobile

- Same layout, no separate templates.
- Container padding reduces to `3rem 1rem` under `600px`.
- Typewriter font size drops per the typography table.
- Cert row icon shrinks to 28px; arrow may hide on very narrow widths if the title would truncate.

## Files

### Modified
- `index.html` — rewritten end-to-end.

### Deleted
- `certifications.html`
- `projects.html`

### Unchanged
- `images/` (all assets reused)
- `icon.png` (favicon)
- `CLAUDE.md`

## Validation

After implementation, verify locally:

1. `python3 -m http.server 8000` from repo root → open `http://localhost:8000`.
2. Typewriter runs on first load; refresh replays it.
3. Toggle `prefers-reduced-motion` in dev tools → both lines appear instantly, no per-char animation.
4. All six certification rows link to their Credly URLs, open in a new tab, and show the hover treatment.
5. GitHub and LinkedIn icons link to their URLs and show hover glow.
6. Resize down to ~375px width → layout stays legible, no horizontal scroll.
7. Tab through the page → all links reachable, focus ring visible.
8. Old URLs `/certifications.html` and `/projects.html` return 404 (expected, since they're deleted). No inbound link in `index.html` should reference them.

## Open items to resolve at implementation time

- **Bio copy.** Ships with placeholder; real copy is a follow-up commit.
- **Exact final green shade.** `#00ff9c` is the starting point; may nudge slightly (e.g. `#3dffb3`) if contrast testing suggests it.
- **Screen-reader story for the typewriter `<h1>`.** Will finalize the `aria-live` / `aria-hidden` combination during implementation and confirm with a VoiceOver / NVDA pass.
