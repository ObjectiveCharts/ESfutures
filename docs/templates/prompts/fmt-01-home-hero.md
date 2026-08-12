# Task: Format home hero (no image generation)

## Read first

- `docs/brand/visual.md`
- `docs/brand/formatting.md`
- `docs/brand/asset-slots.md` (use existing `hero-home` path only)

## Goal

Implement the home first viewport using tokens and formatting rules. Use the already-generated hero asset if present; otherwise a solid token background placeholder.

## Do

1. Brand wordmark / name at hero level  
2. One headline + one support sentence + CTA group  
3. Full-bleed background using `hero-home` (or placeholder)  
4. HTML text over the image — never bake text into a new graphic  
5. Mobile + desktop pass  

## Done when

- [ ] Hero matches formatting.md budget  
- [ ] Tokens only — no one-off colors  
- [ ] No cards, badges, or stat strips in the hero  
- [ ] Looks correct at ~390px and ~1280px widths  

## Out of scope

- Do not call image generation tools  
- Do not build other sections  
- Do not change the live production site  
