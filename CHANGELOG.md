# Roast My Listing — Changelog

## v6.0 (2026-03-29) — Skeleton Loader, Toasts & Polish
### Added
- **Skeleton Loader** — 3 pulsing placeholder blocks (score circle, roast lines, rewrite block) shown during loading instead of empty space
- **Toast Notifications** — fixed-position toast system (bottom-right) with success/error/info variants, auto-dismiss after 3s
  - Used for: copy success, API errors, photo upload success/error
  - Replaces old centered photo-toast with unified toast component
- **Example Chips** — "Try an example:" section with 4 clickable chips (Etsy Ring, Airbnb Apt, eBay Laptop, Amazon Charger) that prefill textarea and platform
- **Result Cards Hover Glow** — subtle box-shadow glow + translateY lift on hover for score, roast, and rewrite cards

### Changed
- All `alert()` calls replaced with toast notifications
- Photo toast replaced with global toast system
- Copy feedback button uses toast notification
- SW cache bumped to `roast-v6`

## v5.0 (2026-03-29) — Photo Upload & Multimodal Roast
### Added
- **Photo Upload** — drag-and-drop zone + button to upload a product photo (JPG, PNG, WebP, max 5 MB)
- **Photo Preview** — 150x150 thumbnail with file name, size, and remove button
- **Multimodal AI Analysis** — photo sent alongside listing text to AI for combined roast
  - OpenAI: GPT-4o with `image_url` base64 content
  - Google Gemini: `inlineData` with mimeType + base64
  - Anthropic Claude: `image` source with base64 media
- **Photo Score** — new "photo" category in score breakdown (out of 10) evaluating quality, lighting, staging, background
- **Photo Roast Items** — AI roasts photo quality, angles, backgrounds, staging
- **Photo Tips** — dedicated "Photo Tips" section in rewrite card with re-shoot suggestions
- **Toast Notifications** — error toasts for file size/format validation
- **Drag-and-Drop** — visual feedback with border highlight on drag-over

### Changed
- OpenAI model upgraded to `gpt-4o` when photo is attached (vision required)
- API timeout increased to 45s when photo is attached (larger payload)
- Loading estimate dynamically updates based on photo presence
- SW cache bumped to `roast-v5`

## v4.0 (2026-03-29) — Launch Readiness
### Added
- **Animated progress bar** on loading screen with smooth fill from 0-100%
- **Estimated time indicator** ("~15 seconds") shown during loading
- **13 random loading phrases** shuffled each time (e.g. "Reading between the lines...", "Loading sarcasm module...", "Crafting the perfect burn...")
- **Copy Rewrite button** in rewrite card header for one-click copy of the improved listing
- **Animated score counter** — score counts up from 0 to final value on result display
- **"Roast Another" quick path** — clears textarea, returns to input screen, auto-focuses textarea
- **Accessibility: aria-live="polite"** on loading phrases for screen reader announcements
- **Accessibility: role="alert"** on error message cards
- **Accessibility: aria-label** on all interactive buttons (setup, roast, share, try again, settings, copy, history clear, onboarding)

### Changed
- Loading screen redesigned: static shimmer bar replaced with real animated progress bar
- "Try Another" button renamed to "Roast Another" for clarity
- SW cache bumped to `roast-v4`

## v3.0 (2026-03-29) — Premium Visual Redesign
### Added
- **Inter font** (Google Fonts) with weight 900 for hero/title elements
- **Multi-layered body gradient** — radial fire/ember/warm glow spots on dark base
- **Spring animation system** — `@keyframes cardIn` with cubic-bezier spring curve for cards, dropdowns, roast items
- **Shimmer sweep** — `::before` pseudo-element on all primary buttons (hover triggers sweep)
- **Shadow system v2** — `--shadow-lg` double-layer shadow, enhanced `--shadow-card`, fire glow on accent elements
- **Glassmorphism** — `backdrop-filter: blur()` on header, settings dropdown, inputs
- **Staggered roast items** — spring entrance with nth-child delays, colored `border-left` by severity (fire/ember/warm)
- **Focus rings** — 3px outer ring on inputs and selects (ember glow)
- **History hover effects** — translateY lift + subtle ember glow shadow
- **Comparison toggle** — smooth max-height/opacity transition (replaces display toggle)
- **Score circle** — gradient stroke (fire-ember-warm) + enhanced glow shadow
- **Active feedback** — `transform: scale(0.95)` on all interactive buttons

### Changed
- Buttons use `linear-gradient(135deg, fire, ember)` backgrounds instead of flat color
- Hero title gradient extended to 3-stop (fire-ember-warm) with drop-shadow
- Header title uses `text-shadow` fire glow
- All cards use `--shadow-lg` instead of `--shadow-card`
- `--font-main` now starts with "Inter"

## v2.0 (2026-03-29)
### Added
- **Roast History** — past roasts saved locally (max 20), last 5 displayed on input screen
- **Platform-Specific Prompts** — Etsy (SEO tags, title length), eBay (item specifics), Amazon (bullet points, backend keywords), Airbnb (emotional language, trust signals), Avito (Russian market), Craigslist (safety signals)
- **Before/After Comparison** — toggle to view original listing vs AI rewrite side by side
- **Amazon** added as platform option (7 total)
- Relative timestamps for history items ("5m ago", "2h ago", "3d ago")

### Fixed
- `--text-dim` contrast improved to #8a8279 (4.5:1+ on all backgrounds)
- `--score-high-text` variant for small text on dark green backgrounds
- History stores full listing text (not truncated) for accurate comparison view
- Share button disabled during canvas generation (prevents double-click)

### Changed
- SW cache bumped to `roast-v3`

## v1.1 (2026-03-29)
### Fixed
- Anthropic CORS error message now explains the issue specifically
- `!parsed.score` replaced with explicit null/undefined check (score:0 is valid)
- Loading phrase contrast improved (--text-dim to --text-muted)
- Settings gear button added to result screen
- Focus management on state transitions
- Roast items capped at 7

## v1.0 (2026-03-29)
- Initial release
- BYOK: OpenAI gpt-4o-mini, Google Gemini 2.0 Flash, Anthropic Claude
- Score + roast + rewrite pipeline
- Canvas share card (1200x800)
- Onboarding overlay
- PWA (service worker + manifest)
