# Roast My Listing -- QA Audit

**Auditor:** Nash (QA, OpenClaw)
**Date:** 2026-03-29
**Version:** index.html v1 (49.8 KB)
**Files audited:** index.html, sw.js, manifest.json

---

## SCORE: 8.5 / 10

Excellent first AI product. The developer clearly learned from past audits -- localStorage has try/catch, escapeHtml covers all 5 entities including `"` and `'`, prefers-reduced-motion is present, ARIA is thorough, focus-visible is on every interactive element. The CORS/Anthropic issue is proactively warned. Very few critical or major bugs.

---

## P1 -- Critical (0)

None found.

---

## P2 -- Major (3)

### M1. Google API key exposed in URL query parameter
**Location:** line ~696
```javascript
fetch('https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=' + state.apiKey,
```
**Impact:** The API key is in the URL, which means:
- It will appear in browser history
- It will appear in the Service Worker's fetch event (even though API calls are excluded from caching, the URL is still visible in `event.request.url`)
- If any browser extension or proxy logs URLs, the key leaks
**Spec says:** "API key does not leave localStorage" (AC-10). Technically it leaves localStorage and enters a URL string.
**Verdict:** This is how Google's API actually works (key-in-URL is their design). Not a code bug per se, but worth a privacy note to users choosing Google. **No code fix needed** -- just awareness. Downgrading to P3.

### M2. Anthropic CORS -- functional block, not just warning
**Location:** lines 715-747
**Impact:** Anthropic requests WILL fail from browser due to CORS. The warning on setup screen (line 321-322) is good, but the user can still save the key and attempt a roast, which will fail with a generic network error, not the specific "CORS blocked" message.
**Fix:** When Anthropic is selected and the roast fails with a TypeError (network error from CORS), detect it and show a specific message: "Anthropic API blocked the request (CORS). Use OpenAI or Google instead."
**Repro:** Select Anthropic, enter valid key, submit listing. Error will be generic "Something went wrong" instead of explaining CORS.

### M3. `score` validation accepts 0 as falsy
**Location:** line 759
```javascript
if (!parsed.roast || !parsed.score || !parsed.rewrite || !parsed.summary)
```
**Impact:** If AI returns `"score": 0`, the check `!parsed.score` evaluates to `true` (since `!0 === true`), and the response is treated as "off-script" even though `0` is a valid (if harsh) score.
**Fix:** Change to explicit check: `!parsed.roast || parsed.score === undefined || parsed.score === null || !parsed.rewrite || !parsed.summary`

---

## P3 -- Minor (8)

### m1. No `response_format` equivalent for Anthropic
**Location:** lines 718-729
**Impact:** OpenAI gets `response_format: { type: 'json_object' }`, Google gets `responseMimeType: 'application/json'`, but Anthropic has no JSON mode enforcement. This increases the chance of invalid JSON from Claude.
**Fix:** Not critical -- the JSON parse fallback handles it correctly with "AI went off-script". But could add a note in the prompt like "Return ONLY valid JSON, no markdown fences."

### m2. Share card -- `ctx.letterSpacing` is not a Canvas API property
**Location:** line 988
```javascript
ctx.letterSpacing = '4px';
```
**Impact:** `letterSpacing` is a relatively new Canvas property (part of Canvas Text Metrics spec). It works in Chrome 99+ but NOT in Safari < 17.5 or Firefox < 115. On older browsers, it silently fails (no error, just no letter spacing). Not a breaking bug -- purely cosmetic.
**Fix:** None needed for v1.

### m3. Loading phrases use `--text-dim` (contrast 3.4:1)
**Location:** line 205, CSS class `.loading-phrase`
```css
.loading-phrase { ... color: var(--text-dim); ... }
```
**Impact:** The rotating loading phrases are colored `#78716c` on `#1c1917` = 3.4:1 ratio, which fails WCAG AA for normal text. Design.md says `--text-dim` is "decorative only", but these phrases ARE readable content.
**Fix:** Change to `var(--text-muted)` (#a8a29e, 5.1:1 on bg).

### m4. `document.execCommand('copy')` fallback is deprecated
**Location:** lines 933-946
**Impact:** The clipboard fallback for older browsers uses `document.execCommand('copy')` which is deprecated. It still works in all current browsers, but may be removed eventually.
**Fix:** Acceptable for v1. The primary path (navigator.clipboard) works in all modern browsers.

### m5. No max-length enforcement on API response parsing
**Location:** lines 749-758
**Impact:** If the AI returns a very large JSON response (e.g., 100KB), the entire thing gets parsed and rendered into DOM with innerHTML. Could cause performance issues or layout breaking with extremely long roast text.
**Fix:** Low priority. Add `data.roast.slice(0, 7)` to cap at 7 items (spec says max 7). Already unlikely to be an issue with current models.

### m6. Settings gear on result screen goes directly to changeKey() instead of toggleSettings()
**Location:** lines 601-603
```javascript
gearBtnResult.addEventListener('click', function() {
    changeKey();
});
```
**Impact:** On the result screen, clicking the gear icon immediately wipes the key and goes to setup, instead of showing the settings dropdown first (like on the input screen). Inconsistent behavior.
**Fix:** Show a settings dropdown on the result screen too, with "Change" and "Close" options.

### m7. Service Worker caches index.html but never revalidates
**Location:** sw.js, lines 31-47
**Impact:** The fetch strategy is cache-first with no revalidation. Once cached, users will never get updates unless:
- They manually clear the cache, OR
- The SW version is bumped (CACHE_NAME changed)
**Fix:** Consider stale-while-revalidate strategy, or at minimum document that CACHE_NAME must be bumped on each deploy.

### m8. Focus not managed on state transitions
**Location:** multiple (showSection function, line 442-455)
**Impact:** When transitioning from loading to result, focus stays on whatever was last focused (the roast button in input section, now hidden). Screen reader users may be disoriented. The `announce()` call helps, but focus should also move to the result section.
**Fix:** Add `$('#sec-result').focus()` after `showSection('result')`, or focus the score card. Add `tabindex="-1"` to the section for programmatic focus.

---

## Checklist Results

| # | Check | Status | Notes |
|---|-------|--------|-------|
| 1 | localStorage try/catch | PASS | `lsGet`, `lsSet`, `lsRemove` all wrapped |
| 2 | WCAG AA contrast 4.5:1 | PARTIAL | `.loading-phrase` uses `--text-dim` (3.4:1) -- see m3 |
| 3 | Focus trap in modals/overlay | PASS | Setup card has focus trap (lines 539-551) |
| 4 | prefers-reduced-motion | PASS | Lines 37-43, kills all animations |
| 5 | Keyboard navigation | PASS | Arrow keys on provider radiogroup, focus-visible on all buttons, tab order logical |
| 6 | Offline | PASS | SW caches index.html and manifest (cache-first) |
| 7 | Mobile viewport | PASS | `<meta viewport>` present, responsive breakpoints at 400px/640px/1024px |
| 8 | escapeHtml for attributes (incl. " and ') | PASS | escapeHtml handles &, <, >, ", ' -- all 5 entities |
| 9 | XSS via API responses | PASS | All AI response data goes through escapeHtml before innerHTML |
| 10 | API key security | PASS | Key in localStorage only, privacy note shown, no external analytics |

---

## AI-Product Specific Checks

| # | Check | Status | Notes |
|---|-------|--------|-------|
| 1 | API key handling | PASS | localStorage + try/catch + masked display |
| 2 | System prompt -- JSON format | PASS | Explicit JSON example in prompt, response_format for OpenAI/Google |
| 3 | Response parsing -- invalid JSON | PASS | try/catch on JSON.parse, "off-script" fallback with raw display |
| 4 | Response parsing -- partial JSON | PASS | JSON.parse will throw on partial, caught correctly |
| 5 | Response parsing -- empty response | PASS | `if (!content) throw { type: 'empty' }` |
| 6 | XSS via AI response | PASS | escapeHtml on all fields: summary, issue, roast, fix, title, description |
| 7 | Error states -- timeout | PASS | AbortController + 30s timeout |
| 8 | Error states -- 401 | PASS | Specific "Invalid API key" message + "Change API Key" button |
| 9 | Error states -- 429 | PASS | "Too many roasts, wait a minute" |
| 10 | Error states -- network | PASS | Generic catch handler |
| 11 | CORS -- OpenAI | PASS | OpenAI supports browser CORS |
| 12 | CORS -- Google | PASS | Google Gemini supports browser CORS |
| 13 | CORS -- Anthropic | WARN | Will fail (CORS), warning shown but error message is generic (M2) |
| 14 | Share card -- Canvas | PASS | Gradient, arc, text all correct |
| 15 | Share card -- Score circle | PASS | Proper arc calculation with start/end angles |
| 16 | Share card -- Web Share API | PASS | Feature detection + download fallback |
| 17 | Score 0 handling | FAIL | `!parsed.score` is truthy for 0 (M3) |

---

## Acceptance Criteria Verification

| AC | Status | Notes |
|----|--------|-------|
| AC-1: Input | PASS | Textarea 10-10000 chars, dropdown with 6 platforms (spec says 5 -- Craigslist added, fine), button disabled < 10, loading animation works |
| AC-2: API Key | PASS | 3 providers, localStorage, error on invalid, gear icon, "How to get a key" with instructions |
| AC-3: Roast result | PASS | Score X/10 + progress bar + color, summary, roast items with issue/roast/fix, breakdown collapsible |
| AC-4: Rewrite | PASS | Title + description, Copy button, "Copied!" for 2s |
| AC-5: Share | PASS | Canvas PNG, Web Share API with fallback to download |
| AC-6: Responsive | PASS | 360px+ tested via CSS, breakpoints at 400/640/1024 |
| AC-7: Error handling | PASS | Timeout, invalid JSON, rate limit, validation (M3 minor edge case with score=0) |
| AC-8: Localization | PASS | Prompt instructs AI to match language |
| AC-9: Performance | PASS | Single file, 49.8KB (under 50KB), no external deps |
| AC-10: Privacy | PASS | No analytics, no trackers, key stays in browser |

---

## Spec Deviations (non-bugs)

1. **Spec says max 10,000 chars** but also "from 10 to 10,000" -- the `maxlength="10000"` attribute enforces this. Good.
2. **Spec says 5 platforms** (Avito/Etsy/eBay/Airbnb/Other) -- code adds Craigslist. Acceptable addition.
3. **Anthropic CORS warning** -- not in spec, proactively added. Good call.
4. **`anthropic-dangerous-direct-browser-access` header** -- added for completeness, though CORS will still block. Shows awareness.

---

## Summary

| Severity | Count |
|----------|-------|
| P1 Critical | 0 |
| P2 Major | 2 (M2 Anthropic CORS error msg, M3 score=0 validation) |
| P3 Minor | 8 |

**Verdict:** Ship-ready after fixing M3 (one-line fix). M2 is nice-to-have for UX polish. The codebase demonstrates strong security practices (escapeHtml, try/catch, abort controller), good accessibility (ARIA, focus-visible, screen reader announcements, reduced-motion), and solid error handling. This is the cleanest first-pass AI product I've audited.

**Score: 8.5/10**
