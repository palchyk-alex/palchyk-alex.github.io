# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository

Personal website for Alex Palchyk (DevOps Engineer), served by GitHub Pages from the `main` branch of the repository named `palchyk-alex.github.io` — the repo name is what makes it live at that URL. There is no build step, package manager, or test suite; every push to `main` is a deploy.

## Preview locally

Any static file server works, e.g. `python3 -m http.server 8000` from the repo root, then open `http://localhost:8000`.

## Architecture

Three hand-written HTML pages that share the same layout skeleton:

- `index.html` — landing page with name, role, socials, and animated infinity SVG
- `certifications.html` — list of Credly-linked badges
- `projects.html` — cards linking to external Git repos (GitHub, GitLab)

The certifications and projects pages are subpages linked from `index.html` via `← Home` back-links; they are not otherwise connected. `projects.html` is not currently linked from the landing page.

Styling comes from [W3.CSS](https://www.w3schools.com/w3css/) loaded from CDN, plus a small inline `<style>` block per page. There is no shared stylesheet — if you change the dark background (`#1b2531`), font (`Secular One`), or card styling, update each page. Icons are either local (`images/icons/*.png`) or hot-linked from `img.icons8.com`; the "Icons by icons8" footer must stay per icons8's attribution requirement.
