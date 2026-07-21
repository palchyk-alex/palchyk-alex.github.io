# Single-Screen Landing Page — Design

## Goal

Fit the entire `index.html` landing page — intro, about, certifications, footer — within one viewport on desktop (~1080p, usable ≈ 900px vertical) so the visitor does not need to scroll. Mobile continues to scroll naturally.

## Non-goals

- Restructuring content or removing sections. All four current sections stay.
- Changing the visual language (colors, font, terminal-prompt aesthetic).
- Modifying `certifications.html` or `projects.html`. Scope is `index.html` only.
- Adjusting the typing animation behavior.

## Current problem

`.intro` uses `min-height: 100vh`, which forces the intro block to fill the viewport. About and Certifications then stack below it, guaranteeing scroll. Certifications is also vertically expensive: 6 rows in a single column at ~60px each.

## Approach

Tight single column. Remove the full-viewport intro, tighten spacing globally, and reflow certifications into a 2-column grid.

### 1. Intro block (target ≈ 180px)

In the `.intro` selector:
- Remove `min-height: 100vh`.
- Change `padding: 2rem 1.5rem` → `padding: 1.25rem 1.5rem`.
- Change `gap: 1.75rem` → `gap: 1rem`.

In `.photo`:
- Width/height `90px` → `72px`.

In `.socials img`:
- Width/height `45px` → `36px`.

`.terminal-line` `min-height: 4.5rem` stays as-is — it reserves space so the typing animation doesn't cause layout shift.

### 2. About section (target ≈ 110px)

In the `.section` selector:
- Change `padding: 1.5rem 1.5rem` → `padding: 1rem 1.5rem`.

In `.section hr`:
- Change `margin: 0.75rem 0 2rem` → `margin: 0.5rem 0 1rem`.

In `.about-body p`:
- Change `line-height: 1.65` → `1.55`.
- Change bottom margin `0 auto 1rem` → `0 auto 0.5rem`.

Content (both paragraphs) stays unchanged.

### 3. Certifications section (target ≈ 200px)

Convert `.cert-list` from a stacked list into a 2-column grid:

```css
.cert-list {
  list-style: none;
  margin: 0;
  padding: 0;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0.25rem 1rem;
}
```

In `.cert-row`:
- Change `padding: 0.75rem 1rem` → `padding: 0.5rem 0.75rem`.
- Change `gap: 1rem` → `0.75rem`.
- Add `font-size: 0.9rem;` so the longest title ("HashiCorp Certified: Terraform Associate (002)") fits comfortably inside a ~340px half-column without wrapping or ellipsis.

In `.cert-icon`:
- Width/height `40px` → `28px`.

`.cert-title` needs no changes on desktop — the smaller row font is enough to keep titles on one line. Do **not** add `overflow: hidden` / ellipsis on desktop.

Markup change: none required. The existing `<ul class="cert-list">` becomes a grid container via CSS alone.

### 4. Footer (target ≈ 40px)

In `.footer`:
- Change `padding: 1.5rem` → `padding: 0.75rem 1.5rem`.

## Mobile behavior (≤600px)

Extend the existing `@media (max-width: 600px)` block:
- Collapse `.cert-list` back to a single column: `grid-template-columns: 1fr;`.
- Keep the existing mobile cert-icon size (`28px`) and mobile title-ellipsis rule.

Page will scroll on mobile — acceptable per the design decision.

## Vertical budget (desktop 1080p, ~900px usable)

| Section         | Target height |
|-----------------|---------------|
| Intro           | ~180px        |
| About           | ~110px        |
| Certifications  | ~200px        |
| Footer          | ~40px         |
| Section margins | ~150px        |
| **Total**       | **~680px**    |

Leaves ~200px of headroom for browser chrome variance.

## Files touched

- `index.html` — `<style>` block only. No markup, no JS changes.

## Testing

- Serve locally (`python3 -m http.server 8000`) and confirm no scrollbar appears on a 1080p desktop viewport in the browser.
- Confirm the terminal typing animation still runs without layout shift.
- Confirm at ≤600px width the cert grid collapses to a single column and content wraps as expected.
- Confirm all cert links still open in a new tab and reach their Credly badges.
