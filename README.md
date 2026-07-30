# salescloser-updates

Company meeting decks for SalesCloser. Written for the whole company (AEs, CSMs, product,
leadership, marketing), not for engineers: what changed in the product, and what a client
actually feels because of it.

**Live:** https://wishpond-dev.github.io/salescloser-updates/

## Layout

```
index.html            hub, lists every meeting by date
2026-07-30/           one folder per meeting, dated, so links keep working
  index.html
```

Self-contained HTML per deck. No build step, no dependencies, no external assets.

## Adding a meeting

1. `cp -r 2026-07-30 YYYY-MM-DD` and rewrite the slides.
2. Add a `.meeting` row to the top of the list in `index.html`.
3. Commit to `main`. GitHub Pages publishes it.

## Deck controls

Arrow keys / Space to navigate · `F` fullscreen · on-screen buttons · swipe on mobile.

## Where the content comes from

Each deck is built from what actually landed on `master` in the product repos for that
period, then translated out of engineering language. Every item carries its Shortcut story
and PR number so anyone can go read the original.
