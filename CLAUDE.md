# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository

Personal website for Alex Palchyk (DevOps Engineer), served by GitHub Pages from the `main` branch of the repository named `palchyk-alex.github.io` — the repo name is what makes it live at that URL. There is no build step, package manager, or test suite; every push to `main` is a deploy.

## Preview locally

Any static file server works, e.g. `python3 -m http.server 8000` from the repo root, then open `http://localhost:8000`.

## Architecture

Single-page site: `index.html` is the only page. Sections stack top-to-bottom — intro (typewriter animation over the name + role, photo, socials), about, certifications, footer. All CSS lives in an inline `<style>` block at the top of the file; there is no external stylesheet or CSS framework. Fonts come from Google Fonts (`JetBrains Mono`).

Visual language is a dark terminal: `--bg: #0a0e14`, `--green: #00ff9c` for accents/prompts, `--amber: #ffb454` for hover. Section headers are styled as shell prompts (e.g. `~/about $ cat bio.md`) — keep that pattern if you add sections. The blinking cursor in the intro is a real DOM element (`#cursor`) that the typewriter script moves between `#line1` and `#line2`; the script honors `prefers-reduced-motion` and skips the animation.

Icons are either local (`images/icons/*.png`) or hot-linked from `img.icons8.com`. The "icons by icons8" footer link must stay — it's required by icons8's free-tier attribution terms.
