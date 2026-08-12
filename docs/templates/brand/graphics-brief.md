# Graphics brief (global — all generated images)

Use this for **every** image generation task. Per-image size/path live in `asset-slots.md`.

## Locked style

- **Medium:** TODO — pick one and keep it (example: restrained cinematic photography, natural light, slight film grain)
- **Mood:** honest, adult, quiet strength — not hype stage lighting
- **Palette:** must harmonize with `visual.md` tokens
- **Subjects that fit UCN:** voice/radio signal, night roads, sky, hands, quiet rooms, shortwave/antenna silhouettes, shared prayer — real-world anchors

## Hard bans

- Text, titles, slogans, or fake UI inside the image
- Logos or watermarks (logo is applied in code)
- Purple neon glow, generic “AI gradient religion” look
- Crowded megachurch concert stock vibes (unless a slot explicitly asks)
- Emoji, stickers, collage boards, floating badges
- Extra faces jammed into frame “for emotion”

## Technical

- Generate **text-free** imagery only
- Leave **negative space** where HTML headlines will sit (usually lower third or left third — note per slot)
- Prefer shallow depth of field or clear focal subject so CSS crop still works on mobile
- Output the exact aspect listed in the slot row

## References

Always attach files from `docs/brand/references/` when generating. If references are missing, **stop** and ask for them — do not invent a new style.

## Accept / reject

- Accept → mark slot `done` in `asset-slots.md`
- Reject → edit **this brief** or the slot row, then new fresh agent chat
- Do not iterate with vague “make it better” in a long thread
