# Uncommon Christian Network — Initial Plan Overview

**Product:** [Uncommon Christian Network (UCN)](https://uncommonchristiannetwork.com/)  
**Repo today:** [ObjectiveCharts/ESfutures](https://github.com/ObjectiveCharts/ESfutures) (greenfield; name is legacy)  
**Status:** Planning — ES futures is out of scope  

---

## Overview

Build and maintain the digital home for **Uncommon Christian Network**: a worldwide prayer community and 24/7 English-speaking Christian radio station. Listeners hear talk and music live, leave prayer requests by voice or text for a real prayer team, and find shortwave / podcast distribution — **always free, never gated**.

This repo should become the engineering source of truth for the UCN web experience (and related services), replacing ad-hoc site work with a maintainable product codebase.

---

## Product thesis

| Principle | Meaning |
|---|---|
| Access first | No paywall, no login wall, no member-only prayer |
| Prayer + radio | Two pillars: live signal and human prayer response |
| Reach beyond the net | Streaming, directories, apps, shortwave — one network |
| Adult faith | Honest, grown-up conversation — not children’s programming |
| Support without gating | Giving funds the mission; listening stays free |

### Primary audiences

1. **Listeners** — adults seeking live Christian radio / on-demand teaching  
2. **Prayer requesters** — people who need a real team to pray with them  
3. **Supporters** — donors funding airtime, streaming, and programming  
4. **Operators** — prayer team and producers managing requests and schedule  

### v1 success look

A production-ready UCN site where someone can, in one visit:

1. Hear **live radio** with clear now-playing / schedule context  
2. Submit a **prayer request** (text and/or voicemail)  
3. Browse **podcasts / on-demand** teaching  
4. **Give** (one-time, monthly, shortwave) without friction  
5. Discover **where else to listen** (directories, apps, smart devices)  

---

## Current surface (from live site)

Already public at `uncommonchristiannetwork.com`:

- Hero + brand (“Where faith finds a voice”)  
- Live player (“Uncommon Radio” 24/7)  
- Schedule / library placeholders  
- Prayer network messaging  
- App / directory distribution (30+ outlets)  
- Support / give flows (monthly, one-time, shortwave)  

**Implication:** This plan is not inventing a new ministry — it is hardening, rebuilding, or extending an existing product into this repository with clear architecture and ownership.

---

## Technical approach (proposal)

| Layer | Choice | Why |
|---|---|---|
| Web | Next.js (App Router) + TypeScript | Fast, SEO-friendly ministry site + app shell |
| Styling | CSS variables + purposeful type | Brand-led, non-generic UI |
| Audio | HLS/Icecast (or current stream URL) + custom player UI | Live radio is the core loop |
| Prayer intake | Form + optional Twilio/voice mail webhook | Text + voicemail → operator queue |
| CMS | Headless (Sanity / Notion / MDX) for shows, episodes, pages | Non-devs can update schedule/content |
| Giving | Existing processor (Stripe / donor platform) via deep links or embed | Don’t reinvent payments in v1 |
| Hosting | Vercel (web) + small API/worker for prayer webhooks | Simple ops |

### Core domains

```
┌──────────────┐   ┌─────────────────┐   ┌──────────────────┐
│ Stream /     │   │ Prayer intake   │   │ Content CMS      │
│ schedule API │   │ (text + voice)  │   │ (shows/episodes) │
└──────┬───────┘   └────────┬────────┘   └────────┬─────────┘
       │                    │                     │
       └────────────────────┼─────────────────────┘
                            ▼
                 ┌──────────────────────┐
                 │ UCN Web (Next.js)    │
                 │ listen · pray · give │
                 └──────────────────────┘
```

### Non-goals for v1

- Building a full custom radio automation suite (use existing stream)  
- Social network / member forums  
- Paywalled content or accounts required to listen/pray  
- Native iOS/Android apps (directory distribution first; native later)  
- Replacing shortwave operations systems  

---

## Phases

### Phase 0 — Foundations

- [ ] Confirm this repo is the UCN codebase (rename GitHub repo/org if desired)  
- [ ] Capture brand tokens (colors, type, voice, logo usage) from current site  
- [ ] Inventory current stack: stream URL, prayer pipeline, giving links, CMS/host  
- [ ] Scaffold Next.js app + CI (lint, typecheck, preview deploys)  
- [ ] README: local run, env vars, content workflow  

### Phase 1 — Listen (MVP)

- [ ] Full-bleed brand hero with one clear CTA (Listen / Pray)  
- [ ] Reliable live player (play/pause, status, now playing when available)  
- [ ] Schedule section fed by real data (not “Loading the schedule…”)  
- [ ] On-demand / podcast library entry points  
- [ ] Mobile-first player that works in background where the browser allows  

### Phase 2 — Pray

- [ ] Prayer request form (name optional, request text, consent/privacy copy)  
- [ ] Voicemail or “call/text the prayer team” path wired to real operators  
- [ ] Confirmation UX + spam protection  
- [ ] Simple operator inbox (email, Slack, or lightweight admin)  

### Phase 3 — Support & distribute

- [ ] Give flows: monthly, one-time, shortwave — clear copy, working checkout  
- [ ] “Listen on apps / directories” page with maintained link list  
- [ ] Shareable show/episode URLs for podcasts and teaching  

### Phase 4 — Operate & grow

- [ ] CMS for schedule, shows, testimonies, pages  
- [ ] Analytics that respect privacy (listen starts, prayer submits, give clicks)  
- [ ] Performance / accessibility pass  
- [ ] Optional: PWA “install” experience before native apps  

---

## Dependencies

| Dependency | Needed for | Notes |
|---|---|---|
| Live stream endpoint + CORS/HTTPS | Phase 1 | Must be stable and documented |
| Prayer delivery channel | Phase 2 | Email, SMS, Twilio, or existing team process |
| Donor platform credentials/links | Phase 3 | Prefer keep existing processor |
| Brand assets | Phase 0–1 | Logo, wordmark, imagery rights |
| Repo/org naming | Phase 0 | `ESfutures` / `ObjectiveCharts` are mismatched |

---

## Risks

| Risk | Impact | Mitigation |
|---|---|---|
| Rebuilding without inventory | Break live stream or give links | Document current URLs/providers before cutover |
| Player flakiness on mobile | Core listen loop fails | Test iOS Safari / Android Chrome early |
| Prayer spam / abuse | Operator overload | Rate limits, honeypot, moderation queue |
| Scope creep into “church OS” | Slow launch | Freeze v1 to listen · pray · give · find-us |
| Brand mismatch during rebuild | Feels generic / off-mission | Match live site voice; brand-first hero |

---

## Open decisions

1. **Is this a rebuild of `uncommonchristiannetwork.com` or a new companion app?**  
2. **Who operates prayer requests today, and what tool should v1 deliver into?**  
3. **Preferred giving platform** (keep current vs Stripe/Donorbox/etc.)?  
4. **Should the GitHub repo/org be renamed** to match UCN?  
5. **Content ownership** — who updates schedule and library week to week?

---

## Suggested next step

Confirm the open decisions (especially rebuild vs companion, and prayer pipeline). Then implement **Phase 0 + Phase 1**: scaffold the app, wire the live player to the real stream, and replace schedule placeholders with real data.
