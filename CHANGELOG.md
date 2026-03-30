# Roast My Listing — Changelog

## v22.0 (2026-03-29) — Data Portability

- **Export roast history as JSON backup**: "Export" button in Recent Roasts section downloads full roast history (scores, listings, results, timestamps) as a portable JSON file
- SW cache bumped to `roast-v22`

## v21.0 (2026-03-29) — Title Character Limit Warning

- **Title character limit warning**: Extracts first line of listing as "title" and shows per-platform character limit (Etsy=140, Amazon=200, eBay=80, Airbnb=50, Avito=50, Craigslist=70); color-coded: muted (safe), yellow (close), red (over limit); updates live on input and platform change
- SW cache bumped to `roast-v21`

## v20.0 (2026-03-29) — Print Styles

- **Print stylesheet**: `@media print` rules — hides nav, buttons, overlays, footer; shows only score + roast items + rewrite; white background, black text for ink saving; page breaks on roast items
- SW cache bumped to `roast-v20`

## v19.0 (2026-03-29) — Error Handling Hardening

- **JSON.parse guard**: User-friendly error message when AI response is garbled instead of raw dump
- SW cache bumped to `roast-v19`

## v18.0 (2026-03-29) — SEO / Meta Pass

- **robots meta**: Added `<meta name="robots" content="index, follow">`
- **apple-touch-icon**: Inline SVG data URL with product emoji (🔥)
- SW cache bumped to `roast-v18`

## v17.0 (2026-03-29) — Accessibility Pass

- **Skip link**: "Skip to main content" link for keyboard users
- **Platform select**: Added `aria-label="Select marketplace platform"`
- **Score display**: Added `role="status"` + `aria-live="polite"` for screen reader announcements
- SW cache bumped to `roast-v17`

## v16.0 (2026-03-29) — Loading Tips

- **Loading Tips**: Rotating useful tips shown during AI analysis (e.g., "Did you know? Roast works with product photos too!")
- SW cache bumped to `roast-v16`
## v15.0 (2026-03-29) — Word Count Target

### Added
- **Word count target by platform** -- shows recommended word count range per platform (Etsy: 300-500, Amazon: 200-400, eBay: 150-300, Airbnb: 250-500, Avito: 100-300, Craigslist: 80-200) with a progress bar toward the target
- Color-coded: orange (below target), green (in range), yellow (over target)
- Updates live as user types; responds to platform dropdown changes
- SW cache bumped to `roast-v15`

---

## v14.0 (2026-03-29) — Auto-detect Platform

### Added
- **Auto-detect platform** -- analyzes listing text for platform-specific keywords (Etsy: handmade, vintage, tags; Airbnb: check-in, amenities, host; eBay: auction, bid, no returns; Amazon: prime, asin, bullet point) and auto-selects the matching platform dropdown; triggers on 20+ chars with toast notification
- SW cache bumped to `roast-v14`

---

## v13.0 (2026-03-29) — SEO Keyword Hints

### Added
- **SEO keyword hints** -- after roast, a new card appears below platform tips showing 3-5 suggested keywords extracted from the listing text via simple word frequency analysis (most frequent nouns/adjectives, stopwords excluded); no AI required

### Technical
- SW cache bumped to `roast-v13`

---

## v12.0 (2026-03-29) — Compare & Intensity

### Added
- **Multiple listings comparison** -- "Compare 2 Listings" toggle enables a second textarea; AI compares both listings side-by-side with scores, strengths, weaknesses, and a winner verdict
- **Roast intensity slider** -- 5-level slider from "Gentle" to "Savage" that adjusts AI prompt tone; default is "Medium" (unchanged behavior)

### Technical
- SW cache bumped to `roast-v12`

---

## v11.0 (2026-03-29) — Micro-improvements

### Changed
- **Onboarding "How it works" polish** — steps now stagger-animate in with `howStepIn` keyframes; icons scale up with glow on hover; subtle horizontal connector line between steps
- **Skeleton shimmer more visible** — shimmer gradient widened to 300% with added white highlight peaks for a more pronounced loading effect; easing changed to ease-in-out at 1.8s cycle
- SW cache bumped to `roast-v11`

### Preserved
- All v10.x features intact (How it works guide, Gemini suggestion in errors)

---

## v9.0 (2026-03-29) — Pre-Launch Final Polish

### Added
- **Listing Length Analyzer** — real-time stats bar beneath textarea showing word count, character count, reading time (~200 wpm), and sentence count; updates on every keystroke, fades in when text is present
- **Improvement Score Tracker** — all roast scores stored in localStorage (`rml_all_scores`) over time; history section shows an SVG sparkline chart (last 20 scores) and "Your average score: X.X/10" badge
- **Share on Twitter/X** — "Share on X" button in result actions; opens Twitter intent URL in new tab with pre-filled tweet: "My listing just got roasted. Score: X/10. Try it: [URL]"

### Changed
- SW cache bumped to `roast-v9`

---

## v8.0 (2026-03-29) — Roast Comparison, Platform Tips & Summary Export

### Added
- **Roast Comparison** — when re-roasting the same listing (matched by text hash), a banner shows "Improved! Score went from X to Y" or "Score dropped..." with visual before/after scores; previous scores stored in localStorage (`rml_score_history`)
- **Platform-Specific Tips** — after each roast, a hardcoded tips card appears with 2-3 actionable tips specific to the selected platform (Etsy SEO tags, Amazon A+ content, eBay item specifics, Airbnb photo strategy, Avito trust signals, Craigslist safety)
- **Copy Roast Summary** — "Copy Roast Summary" button in result actions; copies a text-format summary: "Score: 7/10. Verdict: ... Issues: - ... Rewrite Title: ... Rewrite: ..."

### Changed
- SW cache bumped to `roast-v8`

---

## v7.0 (2026-03-29) — PWA Install, Keyboard Shortcut & Auto-Save

### Added
- **PWA Install Prompt** — fixed bottom banner shown after 2+ visits via `beforeinstallprompt` event; "Install" triggers native prompt, "Not now" dismisses and persists choice in localStorage (`rml_pwa_dismissed`)
- **Keyboard Shortcut** — `Ctrl/Cmd+Enter` submits roast when textarea is focused and button is enabled
- **Auto-Save Draft** — textarea content saved to localStorage (`rml_draft`) on input with 1s debounce; restored on page load if textarea is empty; cleared automatically after successful roast

### Changed
- SW cache bumped to `roast-v7`

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
