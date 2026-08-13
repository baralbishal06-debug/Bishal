# Long Roads — a driving playlist

A single-page nostalgia music site built with Next.js (App Router), TypeScript,
and Tailwind CSS v4.

## Setup

```bash
npm install
npm run dev
```

## Assets you need to add

**Background scenes** — drop these in `public/bg/`:

- `scene-wide.png` — landscape, used by default and on desktop/landscape orientation
- `scene-tall.png` — portrait, swapped in via `@media (orientation: portrait)`. This
  should be a separately composed image, not a crop of the wide one.

**Audio** — the playlist lives in `lib/tracks.ts` (100 tracks: old and new Nepali,
old and new Hindi). Each track expects an MP3 at `public/audio/<slug>.mp3`, where
`<slug>` is a kebab-case version of `"title-artist"` — see the `src` field Player
reads from, or just log `tracks` to see every expected filename. Until a file
exists at that path, the player's transport still works but that track won't
produce sound (`onError` silently stops playback rather than throwing).

## Structure

```
app/
  layout.tsx      — fonts, metadata, viewport (viewportFit: "cover"), Analytics/Speed Insights
  globals.css      — Tailwind v4 import, @theme tokens, hero background, grain, vinyl spin, seek bar
  page.tsx         — server component assembling the page
components/
  Clock.tsx        — top-left, client (updates every 15s)
  ListenerCount.tsx — top-center, client (ambient simulated count)
  SocialLinks.tsx  — top-right, server
  GrainOverlay.tsx — fixed film-grain layer
  Player.tsx       — the centrepiece: desktop glass pill + mobile stacked card
lib/
  tracks.ts        — the 100-track playlist
```

## Notes

- The four fixed corners (clock, listener count, social links, player) all pad
  against `max(1rem, env(safe-area-inset-*))`, and `viewport.viewportFit` is set
  to `"cover"` in `app/layout.tsx` so this holds on notched/rounded-corner phones.
- The spec's `min-h-dh` was treated as `min-h-dvh` (Tailwind's dynamic-viewport-height
  utility) — `dh` isn't a real Tailwind unit, and `dvh` is what avoids the mobile
  browser-chrome jump the brief is going for.
- Album art isn't part of the asset list, so each vinyl label is a deterministic
  gradient derived from the track's id — stable per track, no image dependency.
  Swap `VinylLabel` in `components/Player.tsx` for real cover art whenever you have it.
