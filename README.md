# 🔥 Roast My Listing

**Your listing sucks. Here's why. Here's the fix.**

Paste your marketplace listing → get a brutal AI critique with a score, point-by-point teardown, and a ready-to-use rewrite. Works with Etsy, eBay, Amazon, Avito, Airbnb, Craigslist — any marketplace.

---

## Features

- **Score out of 10** with a sarcastic verdict ("Your listing is a cry for help")
- **Point-by-point roast** — specific problems, not vague advice
- **Complete rewrite** — copy-paste and relist immediately
- **Before/After comparison** — see your original vs the AI rewrite side by side
- **Roast History** — all your past roasts saved locally, revisit anytime
- **Platform-specific analysis** — Etsy SEO, Amazon bullet points, Airbnb trust signals
- **Share card** — PNG score card for social media / messaging
- **BYOK** (Bring Your Own Key) — works with OpenAI, Google Gemini, or Anthropic
- **Single HTML file** — no backend, no account, no tracking
- **Works offline** — PWA with service worker

## How It Works

1. Paste your listing text
2. Pick your marketplace (Etsy, eBay, Amazon, Avito, Airbnb, Craigslist)
3. Get a score, a brutal roast, and a complete rewrite
4. Compare before/after, copy the rewrite → relist → sell

## Privacy & Cost

- **Your data never touches our servers** — listing text goes directly from your browser to the AI provider
- **API key stays in localStorage** — we never see it
- **No analytics, no cookies, no tracking**
- **Cost:** ~$0.01-0.03 per roast (OpenAI gpt-4o-mini) or **$0 with Google Gemini free tier**
- **We make $0 from this**

## Supported AI Providers

| Provider | Model | Cost per roast | Notes |
|----------|-------|---------------|-------|
| OpenAI | gpt-4o-mini | ~$0.01-0.03 | Best quality |
| Google Gemini | 2.0 Flash | Free (free tier) | Great for testing |
| Anthropic | Claude Sonnet | ~$0.01-0.03 | CORS limitations in browser |

## Tech Stack

- Single self-contained HTML file (~66KB)
- Vanilla JavaScript, zero frameworks
- Canvas API for share card generation
- PWA-ready (service worker + manifest)
- Web Share API with fallback

## Running Locally

```bash
# Just open the file
open index.html

# Or with a server (needed for PWA features)
python3 -m http.server 8000
```

## The Scoring Rubric

The AI evaluates your listing on:
- **Title optimization** — character usage, keywords, emotional hooks
- **Description quality** — structure, benefits vs features, call-to-action
- **Keyword density** — search terms buyers actually use
- **Pricing psychology** — how price is presented
- **Photo-description alignment** — does text match what photos show?

## License

MIT — free for personal and commercial use.

---

Made by [OpenClaw](https://github.com/openclaw). We make $0 from this.
