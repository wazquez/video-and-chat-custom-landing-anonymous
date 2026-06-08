# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Single-file static HTML mockup of a United Airlines airport-agent pre-chat landing page. It simulates a mobile browser experience (390px phone shell) where a traveler enters their details, picks a connection type (Talk / Video Chat / Text), and is handed off to an external Webex Contact Center customer app. Despite the United branding, this is a Webex sales demo (repo lives under the `wxsd-sales` org).

## Development & deployment

No build process, dependencies, or package manager — open `index.html` directly in a browser to preview.

Deployed via **GitHub Pages from the `gh-pages` branch** (`wxsd-sales/video-and-chat-custom-landing-page`); pushing to `gh-pages` publishes the site. Note `CLAUDE.md` is gitignored, so it never ships.

## Architecture

Everything lives in `index.html` — CSS, HTML, and JavaScript are all inline.

- **Phone shell** (`.phone-shell`): fixed 390px-wide container; header, scrollable `.form-area`, fixed `.submit-wrap`, and a hidden `.success-screen`.
- **Form fields**: first name, last name, help topic (`select`), airport location, plus a connection-type radio group (`.radio-option`). Note the field marked with the gold `.required-dot` is *airport*, but validation actually requires first name, last name, and airport.

### Submit flow — this is the important part

`handleSubmit()` does NOT just show a success screen. It:
1. Validates first name, last name, and airport are non-empty (browser `alert` lists what's missing).
2. Reads `token`, `email`, `destination` from the page's own URL query string (captured at load into module-scope consts) and forwards them, plus a `name` param, to the next app.
3. Branches on the selected radio label to pick an external Webex base URL:
   - **Video Chat** → `https://wxsd-sales.github.io/wxcc-chat-and-more-customer/`
   - **Talk** → `.../wxcc-chat-and-more-customer/index-audio.html`
   - **Text** → bare `return`, so it currently does nothing (no redirect).
4. Redirects via `window.location.href`. The subsequent `.success-screen` display toggle is effectively dead code because navigation has already begun.

So changing handoff targets means editing the `baseUrl` branches; wiring up the Text option means giving it a real URL instead of the `return`.

- **Radio selection** (`selectOption(el)`): clears `.selected` from all `.radio-option`s and adds it to the clicked one. All visual state is CSS-driven off `.selected`; the chosen option is read back by `.radio-label` text in `handleSubmit()`, so label text and the branch strings must stay in sync. Video Chat is selected by default.
- **Icons**: external SVGs from `https://travelhelp.united.com/assets/`, recolored via CSS `filter` (white when selected, muted navy otherwise).

## Design tokens

CSS custom properties on `:root` — navy (`--navy`, `--navy-mid`), gold (`--gold`, `--gold-light`), and neutral text/border/surface vars. Route palette changes through these variables.
