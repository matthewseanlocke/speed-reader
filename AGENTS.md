# AGENTS.md

This repository is a single-file, static HTML app in `index.html`.
No build system, package manager, or automated tests. Runs offline by opening the HTML file directly.

## Repository Overview
- Repository: https://github.com/matthewseanlocke/speed-reader
- Entry point: `index.html`
- Assets: PNG images (reference only)
- No bundler, no dependencies, no module system
- Version: v4.35 (pinned to bottom of screen on all views)

## Build / Lint / Test
No automated build, lint, or test commands.

Manual run: Open `index.html` in a browser

Manual smoke checks:
- Splash screen displays animated lightning bolt logo on load
- Begin screen shows first word with ORP highlighting (blinking)
- Start reading via click-to-begin overlay
- Pause/resume by clicking overlay (except controls) and Space
- Exit via Escape and Exit button
- Adjust settings and verify preview updates immediately
- Confirm saved settings restored on refresh
- Verify position/speed sliders update during reading
- Test "Load Random Text" loads different grab bag text
- Mobile: check container clears pinned version badge

Single test: Not available (no test runner)

## Code Style Guidelines

### Structure
- Keep all HTML, CSS, and JS inside `speed-reader.html`
- Maintain order: HTML, then `<style>`, then `<script>`
- Avoid splitting into multiple files unless requested

### Formatting
- Use 4 spaces for indentation
- Keep lines reasonably short for readability
- Use same-line brace style (`function foo() {`)

### Imports / Dependencies
- No external libraries or CDN dependencies
- Keep it self-contained and offline-friendly

### Types
- Plain JavaScript only (no TypeScript)
- Prefer clear runtime validation over type assumptions

### Naming Conventions
- JS functions/variables: descriptive camelCase
- CSS classes/ids: kebab-case
- DOM references: `const` by default; `let` only when reassigned
- Booleans: `is*` / `has*` / `should*` prefixes

### CSS
- Prefer class-based styling; inline styles only for runtime values (color, font-size)
- Avoid hardcoded colors that conflict with user settings
- Dark, minimal aesthetic; subtle transitions
- Use CSS custom properties (`--orp-anchor`) for dynamic values
- Neon glow effects: combine drop-shadow and box-shadow
- Start button: pulse animation with scale transform
- Splash logo: logoGlow and boltFlash animations

### JavaScript
- Small, focused functions; avoid `alert()`, use UI hints instead
- Use `localStorage` for preferences and text persistence
- Update preview and reading display immediately on setting changes
- Avoid new global variables unless necessary
- Clean up intervals and listeners on exit

### Event Handling
- Bind with `addEventListener`
- Space: pause/resume reading or trigger countdown
- Escape: exit reading mode
- Click on overlay: toggle pause/resume (except controls)
- Prevent default Space behavior during reading

### Error Handling
- Prefer graceful UI feedback over thrown errors
- Validate user input and clamp numeric values
- No exceptions for normal user flows

### Data Structures and State
- Words: global `words` array
- Settings keys: `textColor`, `redColor`, `fontFamily`, `textCase`, `fontSize`, `savedText`
  - `savedText` only saved when user edits text (not grab bag selection)
- State: `isReading`, `isPaused`, `currentIndex`, `currentSpeed`, `intervalId`, `controlsHidden`, `textIsDefault`

### ORP Algorithm
- Optimal Recognition Point (ORP) is the red highlighted letter
- Words <= 4 chars: ORP at index 0
- Words 5-7 chars: ORP at index 1
- Words > 7 chars: ORP at Math.floor(length * 0.35)
- Split into before-orp, orp-letter, after-orp segments

### Text / Content
- Default text randomly selected from `textGrabBag` on first load
- Includes: Pale Blue Dot, The Road Not Taken, I Have a Dream, Gettysburg Address, Hamlet, JFK Inaugural, FDR First Inaugural, Patrick Henry's Liberty Speech, Do Not Go Gentle Into That Good Night, Meditations, Walden, Civil Disobedience, Descartes' Meditations, Darwin's Origin of Species, Declaration of Independence, Psalm 23
- Keep default text ASCII-only
- "Load Random Text" uses grab bag; do not persist to `savedText`

### Z-Index Layering
- Base UI: default
- Reading overlay: 9999
- Reading controls: 10000
- Progress bar: 10001
- Countdown overlay: 10002
- Splash screen: 10003
- Version badge: 10004

### Accessibility / UX
- Click-to-begin overlay clickable anywhere
- Clicking anywhere on reading screen toggles pause/resume
- Controls hidden by default during reading
- Splash screen auto-fades after 2.5s
- Version badge pinned to bottom on all screens (pointer-events: none)
- Mobile: body aligned to top with bottom spacing to clear version badge

## UI/UX Constraints
- Dark, minimal settings screen
- ORP (red letter) stays anchored on screen
- Controls use backdrop-filter blur effect
- Buttons use neon glow styling:
  - Start: green (#4ecdc4), padding 25px 50px, pulse animation
  - Restart: blue (#667eea), padding 20px 30px
  - Exit: red (#ff6b6b), padding 20px 30px
- Text box: 120px min-height, 150px max-height
- Preview box: 250px min-height with center-aligned content
- Version badge: fixed at bottom 12px, pointer-events: none
- Desktop: body padding-bottom 50px, reading-controls bottom 60px
- Mobile: body padding-bottom 80px, reading-controls bottom 40px, container padding-bottom 60px, reading-overlay padding-bottom 80px

## Cursor / Copilot Rules
- No Cursor rules found (`.cursor/rules/` or `.cursorrules`)
- No Copilot instructions found (`.github/copilot-instructions.md`)

## When You Change Behavior
- Update preview logic and runtime display together
- Ensure new settings persist via `localStorage`
- Re-check read/pause/resume flows for regressions
- Bump version number on user-facing changes

## Notes for Agentic Tools
- Use `Read` for file inspection and `edit` for edits
- Avoid altering PNG files unless explicitly requested
- Keep ASCII characters unless file already uses non-ASCII
- When modifying ORP logic, update both preview and display functions
