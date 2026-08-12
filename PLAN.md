# Uncommon Christian Network — Rewrite Plan

**Product:** [Uncommon Christian Network (UCN)](https://uncommonchristiannetwork.com/)  
**Approach:** Complete rewrite in a **new codebase** — fresh folders and files  
**Hard rule:** Do **not** change the current live site or its deploy  
**Build method:** Cursor + agents + markdown files as the source of truth  

---

## 1. Goal

Rebuild UCN (site + apps) the right way in a clean repo, while the existing site keeps running unchanged. Ship the rewrite only when it is ready to cut over (or keep them side-by-side as long as needed).

| Keep running | Build separately |
|---|---|
| Current `uncommonchristiannetwork.com` | New private UCN repo (create later) |
| Current stream, prayer, give links | Fresh Next.js (or chosen) app from empty folders |
| Current hosting / DNS | Preview deploys only until cutover |

---

## 2. Why markdown-first (and why dropping chat memory is fine)

Long agent chats rot: graphics briefs get lost, brand rules drift, and later agents invent context. **Markdown files should be the memory** — not the conversation.

**Recommended workflow**

1. Write / update an MD file (spec, prompt, or checklist)  
2. Start a **fresh** Cursor agent with little or no prior chat  
3. Point it at that file: “Implement `docs/prompts/01-listen-player.md` exactly”  
4. Agent commits; you review; update the MD if the truth changed  
5. Next task → new chat → next MD  

Chat is disposable. **Repo docs are durable.**

What to stop doing: pasting huge history into new chats and hoping the model “remembers” brand, layout, or image style.

---

## 3. Repo layout (greenfield)

Create this structure in the **new private repo** (not in public `ESfutures`):

```text
ucn/                          # private repo root
├── README.md                 # how to run + how agents should work
├── AGENTS.md                 # standing rules for every Cursor agent
├── PLAN.md                   # this plan (living)
├── docs/
│   ├── product/
│   │   ├── vision.md         # mission, audience, non-goals
│   │   ├── information-architecture.md
│   │   └── cutover.md        # when/how to replace the live site
│   ├── brand/
│   │   ├── voice.md
│   │   ├── visual.md           # tokens: color, type, space, radius
│   │   ├── formatting.md       # layout rules — see §6
│   │   ├── graphics-brief.md   # image generation rules — see §6
│   │   ├── asset-slots.md      # every image slot: size, crop, path
│   │   └── references/         # logo + 3–6 approved refs only
│   ├── inventory/
│   │   └── current-site.md
│   ├── specs/
│   ├── prompts/
│   │   ├── 00-scaffold.md
│   │   ├── gfx-01-hero.md      # one prompt = one graphic
│   │   └── ...
│   └── templates/              # starter MD copies (this PR includes samples)
├── apps/
│   └── web/
│       ├── public/assets/      # final raster/SVG only — named by slot
│       └── src/styles/tokens.css
└── .cursor/rules/
```

**Principle:** one job per MD. Agents implement specs; they do not invent product or invent image styles.

---

## 4. AGENTS.md (standing rules)

Every agent run should obey something like this (full text lives in-repo):

- Do not touch production / current live site deploy  
- Only change files under this rewrite repo  
- Read `docs/brand/*` before any UI or image work  
- Implement only the linked spec/prompt; do not expand scope  
- Prefer small PRs: one phase or one feature  
- After UI work, update the spec if behavior changed  
- Never commit secrets (stream keys, Twilio, donor tokens)  

---

## 5. Build phases (rewrite only)

### Phase 0 — Docs & inventory (no app yet)

- [ ] Create private GitHub repo for UCN  
- [ ] Add `AGENTS.md`, `PLAN.md`, `docs/**` skeleton  
- [ ] Fill `docs/inventory/current-site.md` (stream URL, prayer path, give links, directories)  
- [ ] Fill brand docs from the live site (observe only — do not edit live)  
- [ ] Write `docs/prompts/00-scaffold.md`  

### Phase 1 — Scaffold fresh app

- [ ] New agent + `00-scaffold.md` → empty `apps/web` Next.js + TypeScript  
- [ ] Design tokens from `docs/brand/visual.md`  
- [ ] Preview deploy (Vercel preview) — **not** production DNS  
- [ ] README: local run + “how to run an agent task”  

### Phase 2 — Listen

- [ ] Spec + prompt for live player, schedule, on-demand entry  
- [ ] Wire to **existing** stream endpoint (read-only use of current media)  
- [ ] Mobile-first player UX  

### Phase 3 — Pray

- [ ] Spec for text + voicemail paths  
- [ ] Deliver into current operator channel (email/SMS/Twilio) without changing live site forms  

### Phase 4 — Give & distribute

- [ ] Deep-link or embed **existing** giving processors  
- [ ] Apps / directories page from maintained MD list  

### Phase 5 — Content system

- [ ] CMS or MDX for schedule, shows, testimonies  
- [ ] Operators can update without redeploying code when possible  

### Phase 6 — Cutover (only when ready)

- [ ] Follow `docs/product/cutover.md`  
- [ ] Point DNS / hosting to the rewrite  
- [ ] Keep a rollback path to the old site  

Until Phase 6, the **current site stays exactly as it is**.

---

## 6. Graphics & formatting (main pain point)

Two different problems get mixed together. Split them on purpose.

| Problem | Fix in | Not fixed by |
|---|---|---|
| **Formatting** — type, spacing, alignment, responsive layout | CSS tokens + `formatting.md` + layout code | Generating more images |
| **Graphics** — hero photos, atmosphere, icons-as-art | `graphics-brief.md` + `asset-slots.md` + one-slot prompts | Longer chat context |

**Rule:** most of the site should be **type + layout + one real image**. If formatting feels broken, do not ask for new graphics — fix tokens and structure.

---

### 6A. Formatting system (do this before generating art)

Put all layout truth in code-facing docs so every agent uses the same grid.

**`docs/brand/visual.md` → becomes `tokens.css`**

- Color: background, surface, text, muted, accent (2–3 accents max)  
- Type: one display font + one body font (no Inter/Roboto/Arial defaults)  
- Scale: steps for H1 / H2 / body / small — with mobile sizes  
- Space: 4/8/16/24/40/64 scale only  
- Radius / borders: pick one language and stick to it  

**`docs/brand/formatting.md` rules (non-negotiable for agents)**

1. **One composition per first viewport** — brand, one headline, one support line, one CTA group, one full-bleed visual. Nothing else.  
2. **No cards in the hero.** Cards only when they wrap a real interaction.  
3. **One job per section** — one headline + one short support sentence.  
4. **Text is HTML/CSS, never baked into generated images** (except logo). AI text-in-image is the #1 formatting failure.  
5. **Image is background or full-bleed plane** — not a rounded inset thumbnail in the hero.  
6. **Fixed content widths** — e.g. measure ~36–42rem for prose; full-bleed only for media.  
7. **Mobile first** — stack; do not shrink desktop chrome.  
8. **No competing chrome** — no pill clusters, stat strips, floating badges on media.  

**Agent pattern for formatting (fresh chat)**

> Read `docs/brand/visual.md` and `docs/brand/formatting.md`.  
> Implement layout for `<section>` only.  
> Do not generate images.  
> Use tokens from `tokens.css`. Commit.

---

### 6B. Graphics system (slot-based, not “make it pretty”)

Never ask an agent: “make graphics for the site.”  
Always ask: “generate **slot `hero-home`** per `asset-slots.md`.”

**`docs/brand/asset-slots.md` — one row per image**

| Slot ID | Page | Role | Aspect | Size | Path | Status |
|---|---|---|---|---|---|---|
| `hero-home` | Home | Full-bleed atmosphere | 16:9 | 2400×1350 | `public/assets/hero-home.webp` | todo |
| `og-default` | Share | Open Graph | 1.91:1 | 1200×630 | `public/assets/og-default.jpg` | todo |
| `pray-atmosphere` | Pray | Section visual | 4:3 | 1600×1200 | `public/assets/pray-atmosphere.webp` | todo |

Only slots that exist in this table may be generated. No orphan images.

**`docs/brand/graphics-brief.md` — global image rules**

- Medium: e.g. cinematic photography / restrained editorial (pick one; lock it)  
- Palette must match `visual.md`  
- Subject: radio / voice / night sky / road / hands in prayer — concrete, not abstract purple fog  
- **No text, no logos, no watermarks, no UI mockups inside the image**  
- **No** generic glowing crosses, stock megachurch stages, or random AI “worship concert” looks unless you explicitly want them  
- Lighting / grain / depth of field notes  
- Safe crop zones (faces/subjects not in the vertical center band if text will overlay)  

**`docs/brand/references/`**

- Official logo only  
- 3–6 stills you personally approve (screenshots of current site or photos you own)  
- Agents must attach these as references when generating  

**One graphic = one prompt file** (`docs/prompts/gfx-01-hero-home.md`)

```markdown
# Task: Generate slot hero-home

## Read first
- docs/brand/graphics-brief.md
- docs/brand/asset-slots.md (row: hero-home)
- docs/brand/visual.md

## Attach
- docs/brand/references/* (as image refs)

## Do
1. Generate exactly one image for slot hero-home
2. Match aspect and intent in asset-slots.md
3. No text in the image
4. Save to the path in asset-slots.md
5. Update Status column to done

## Out of scope
- Do not redesign the page
- Do not generate other slots
```

**Accept / reject loop**

- Reject → edit `graphics-brief.md` or the slot row → new fresh chat  
- Do not pile “make it more X” into a long thread; the brief should change  

---

### 6C. What usually breaks (and the fix)

| Symptom | Cause | Fix |
|---|---|---|
| Text looks wrong on the image | Text was generated inside the PNG | Overlay HTML text on a text-free photo |
| Every page looks different | No tokens / each agent invents CSS | Lock `tokens.css`; ban new colors in prompts |
| Hero feels cluttered | Too many elements + inset image | Enforce formatting.md hero budget |
| Images don’t crop well on mobile | Wrong aspect / subject centered | Define safe crop in slot row; generate 16:9 and crop with CSS `object-position` |
| “AI slop” look | Vague prompts, no references | Hard bans + 3–6 reference images |
| Inconsistent icons | Mixing generated icons + emoji + lucide randomly | Pick one icon set in code; don’t generate icons as art unless listed as a slot |

---

### 6D. Phase 0 addition for this pain point

Before any UI scaffold graphics:

- [ ] Fill `visual.md` + `formatting.md` (even draft tokens)  
- [ ] Fill `graphics-brief.md` with hard bans  
- [ ] Create `asset-slots.md` with **only** the images v1 actually needs (aim for ≤6)  
- [ ] Drop logo + references into `references/`  
- [ ] Write one `gfx-*.md` prompt per slot  

**v1 graphic budget (suggestion):** home hero, pray section, give/support atmosphere, default OG image, optional app icon. Everything else = CSS and type.

Starter templates for these files live under `docs/templates/` in this PR — copy them into the private UCN repo and fill in.

---

## 7. How to run Cursor day-to-day

| Step | You do | Agent does |
|---|---|---|
| 1 | Pick next unchecked item in `PLAN.md` / a prompt file | — |
| 2 | Open **new** agent chat (clear context) | — |
| 3 | Paste only: “Follow `docs/prompts/0N-….md`. Read linked brand/spec files. Commit on a branch.” | Implements that slice |
| 4 | Review PR / diff | — |
| 5 | Update MD if truth changed | Optional follow-up: “Update the spec to match what shipped” |

### Prompt file template (`docs/prompts/NN-name.md`)

```markdown
# Task: <short name>

## Read first
- AGENTS.md
- docs/brand/visual.md
- docs/brand/voice.md
- docs/specs/<feature>.md

## Goal
<one paragraph>

## Done when
- [ ] ...
- [ ] ...

## Out of scope
- Do not modify production / live site
- Do not ...

## Notes
<stream URL env var name, paths, constraints>
```

---

## 8. Site + apps scope

**Web (first):** full rewrite of the listener experience — listen, pray, give, find-us.  

**Apps (later, same monorepo):**

- PWA / installable web app before native  
- Then iOS/Android only if directories + PWA are not enough  
- Shared brand tokens and copy from `docs/brand/*` so agents don’t restyle each surface  

Native apps get their own `docs/specs/ios.md` / `android.md` and prompts when you start them — still markdown-first, still fresh chats.

---

## 9. What stays untouched

- Live site files, CMS, and hosting for current production  
- DNS until cutover  
- Existing stream ingestion / radio automation (consume the stream; don’t rebuild the station in v1)  
- Existing donor accounts (link out or embed; don’t migrate money systems early)  

---

## 10. Open decisions

1. **Private repo name/owner** (when you create it)  
2. **Rebuild stack confirmation** — Next.js + TypeScript OK?  
3. **Prayer delivery** — where should the new form send requests?  
4. **Giving** — keep current checkout URLs?  
5. **Cutover style** — big-bang DNS flip vs soft launch on a subdomain (`next.uncommonchristiannetwork.com`)?  

---

## 11. Suggested sequence (after private repo exists)

1. Copy this `PLAN.md` into the private repo  
2. Agent task: create docs skeleton + `AGENTS.md` only  
3. You fill inventory + brand briefs (human; highest leverage)  
4. Agent task: scaffold `apps/web` from `00-scaffold.md`  
5. One feature per fresh agent run via `docs/prompts/*`  

---

## Bottom line

- **Rewrite:** yes, fresh folders/files  
- **Touch current site:** no  
- **Organize with MD + Cursor agents:** yes — treat MD as prompts and memory  
- **Erase chat context:** yes, intentionally — start clean and point at files  
- **Formatting:** CSS tokens + `formatting.md` — not more images  
- **Graphics:** slot table + brief + one prompt per image — never “make the site look better”  

When the private repo is ready: Phase 0 docs first, especially brand/formatting/graphics templates, then scaffold UI.
