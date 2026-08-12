# Formatting rules (layout — not images)

Agents building UI must follow this file. Broken formatting is fixed here and in CSS — **not** by generating new graphics.

## Hard rules

1. **Hero budget:** brand + one headline + one short support sentence + one CTA group + one full-bleed visual. Nothing else in the first viewport.
2. **Brand first:** product name is hero-level, not a small nav-only label.
3. **No cards in the hero.** Cards only when they wrap a real user interaction.
4. **One job per section:** one headline, one short support line, then content/action.
5. **Text is always HTML/CSS** — never rendered inside a generated PNG/JPG (logo excepted).
6. **Hero media is full-bleed** (edge-to-edge). No inset rounded hero thumbnails.
7. **No overlays on media:** no floating badges, stickers, chips, or detached labels on the hero image.
8. **Measure:** prose ~36–42rem max width; media may full-bleed.
9. **Mobile first:** stack sections; keep tap targets ≥44px.
10. **No clutter strips:** no pill clusters, stat rows, icon grids, or promo cards competing with the main job.

## Section recipe

```text
[eyebrow optional — rare]
Headline
One support sentence.
[Primary CTA] [Secondary CTA optional]
[Content / form / player]
```

## Player / pray / give chrome

- Treat interactive chrome as UI (tokens + components), not as artwork.
- Keep controls visually quieter than the brand wordmark and headline.

## Review checklist

- [ ] Removing the nav, would you still know this is UCN?
- [ ] Can you delete a box/shadow/radius without hurting understanding? If yes, delete it.
- [ ] Is there only one dominant visual on the first screen?
- [ ] Does mobile stack without horizontal scroll or tiny type?
