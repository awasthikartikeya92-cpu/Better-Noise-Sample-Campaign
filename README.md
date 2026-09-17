# Fan intent capture tool — "Rate your obsession"

A five-question fan intent quiz built for a rock label release campaign. Scores each visitor into Casual, True Fan, or Superfan and gates early access, presale, and merch drop behind an email capture. Same mechanic as a lead-scoring tool, built for fandom instead of pipeline.

It is a single self-contained `index.html`, no build step, no dependencies, no external services. Both images are embedded as base64 so the file works entirely on its own.

## Run it locally

Just open `index.html` in a browser, or serve it:

```bash
npx serve .
```

## Deploy on Vercel

**Option A — GitHub import (recommended)**
1. Push this folder to a new GitHub repo.
2. In Vercel, click "Add New Project" and import the repo.
3. Framework preset: "Other" (Vercel will detect the static `index.html` automatically).
4. Deploy. No environment variables or build command needed.

**Option B — Vercel CLI**
```bash
npm i -g vercel
vercel --prod
```

## Customizing for a real artist

Everything content-related lives in the `<script>` block at the bottom of `index.html`:

- `QUESTIONS` — the five quiz questions and their point values
- `TIERS` — the three result tiers, their copy, and CTA labels
- Band name and single title: search for `IRONVEIL` and `Aftershock` and replace
- `HERO_URI` / laminate photo: search for `background: var(--bg-panel) url(` in the `<style>` block (hero) and `LAMINATE_URI` in the `<script>` block (result photo), swap the base64 strings for your own images, or point them at hosted image URLs if you're not embedding

## Known limitation — no real backend

The email capture form on the result screen validates input and shows a confirmation, but does not send data anywhere. For a live campaign, wire `submitCapture()` in the script to your ESP or CRM (Klaviyo, HubSpot, Mailchimp, etc.) via a serverless function:

1. Add an API route, e.g. `api/subscribe.js` (Vercel supports this natively, drop it in an `/api` folder and it becomes a serverless function automatically).
2. In `submitCapture()`, replace the direct call to `state.screen = "confirm"` with a `fetch('/api/subscribe', { method: 'POST', body: JSON.stringify({ name, email, tier: tier.name }) })`, then transition to the confirm screen on success.
3. Store your ESP API key as a Vercel environment variable, never in the client-side code.

## Design notes

Branding is black, white, and red, matching Better Noise Music's dark, roster-driven visual identity. Typography is Anton (display) and Inter (body), both loaded from Google Fonts. The dark theme is fixed by design and does not shift with a viewer's system light/dark setting, since the label's identity isn't meant to go pale.
