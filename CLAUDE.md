# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a single-file static HTML mockup of a United Airlines airport agent pre-chat landing page. It simulates a mobile browser experience (390px phone shell) where travelers can request to speak with an airport agent via Talk, Video Chat, or Text.

## Development

No build process, dependencies, or package manager. Open `index.html` directly in a browser to preview.

## Architecture

Everything lives in `index.html` — CSS, HTML, and JavaScript are all inline.

- **Phone shell** (`.phone-shell`): Fixed 390px-wide container simulating a mobile device with a fake status bar and browser bar showing `airporthelp.united.com/pre-chat`
- **Form flow**: Fields for first name, last name, help topic (select), and airport location, followed by a connection-type radio group (Talk / Video Chat / Text)
- **Submit logic** (`handleSubmit()`): Validates that first name, last name, and airport are filled, then swaps the form/submit areas for a success screen (`.success-screen`) using `display` toggling
- **Radio selection** (`selectOption(el)`): Removes `.selected` from all `.radio-option` elements and applies it to the clicked one; CSS handles all visual state based on this class
- **Icons**: Loaded from `https://travelhelp.united.com/assets/` (external SVGs)

## Design tokens

Defined as CSS custom properties on `:root` — navy (`--navy`, `--navy-mid`), gold (`--gold`, `--gold-light`), and neutral text/border/surface variables. Changes to the color palette should go through these variables.
