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
│   │   ├── voice.md          # tone, words to use/avoid
│   │   ├── visual.md         # colors, type, motion, layout rules
│   │   ├── graphics-brief.md # how to generate images (see §6)
│   │   └── references/       # logos, approved screenshots, mood refs
│   ├── inventory/
│   │   └── current-site.md   # what exists today (URLs, stream, give, apps)
│   ├── specs/                # one feature = one spec
│   │   ├── listen-player.md
│   │   ├── prayer.md
│   │   ├── give.md
│   │   ├── schedule.md
│   │   └── apps-directories.md
│   └── prompts/              # copy-paste agent starter prompts
│       ├── 00-scaffold.md
│       ├── 01-listen-player.md
│       ├── 02-prayer-form.md
│       └── ...
├── apps/
│   └── web/                  # new site (empty until scaffold)
├── packages/                 # optional shared UI/tokens later
└── .cursor/
    └── rules/                # short always-on rules mirroring AGENTS.md
```

**Principle:** one job per MD. Agents implement specs; they do not invent product.

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

## 6. Graphics & visual context (fix the pain point)

Problem: agents generate random graphics because brand lives in chat, not files.

**Fix: brief files + references, not conversation memory.**

### `docs/brand/graphics-brief.md` should define

- Purpose of each image (hero atmosphere, prayer, radio, give — not decorative noise)  
- Style: photography vs illustration, lighting, color limits  
- Hard bans (generic stock church clichés, purple AI glow, emoji, etc. if you don’t want them)  
- Aspect ratios needed (hero 16:9, app icons, OG share image)  
- “Must include / must not include” checklist  

### `docs/brand/references/`

- Logo SVG/PNG  
- 3–6 approved stills or screenshots from the current site  
- Optional mood references  

### Agent pattern for images

1. Fresh chat  
2. Prompt: “Read `docs/brand/graphics-brief.md` and `docs/brand/visual.md`. Generate only the asset listed in section X. Save to `apps/web/public/...`.”  
3. Attach reference images from `docs/brand/references/` when the tool supports it  
4. Accept or reject; if reject, **edit the brief**, don’t argue in chat  

For layout/UI, prefer **code + CSS tokens** over generated marketing collage art. Use generated imagery only where the brief says a real visual anchor is required.

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
- **Graphics:** fix with `graphics-brief.md` + reference assets, not longer conversations  

When the private repo is ready, start with Phase 0 docs only — no UI until brand and inventory MDs exist.
