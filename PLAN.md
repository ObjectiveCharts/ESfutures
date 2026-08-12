# ESfutures — Initial Plan Overview

**Repo:** [ObjectiveCharts/ESfutures](https://github.com/ObjectiveCharts/ESfutures)  
**Status:** Greenfield (empty repo)  
**Goal:** Define product direction and a phased build plan for an ObjectiveCharts ES futures charting product.

---

## Overview

Build **ObjectiveCharts ESfutures**: a focused charting and market-structure analysis tool for CME E-mini S&P 500 futures (`ES`). The product should emphasize clean, objective visuals and structural context—not signal spam—so traders can observe how price behaves around meaningful levels.

This plan assumes a web-first product with delayed market data for v1, then optional live data and deeper analytics later.

---

## Product thesis

| Principle | Meaning |
|---|---|
| One instrument, done well | Start with ES only; avoid multi-asset sprawl |
| Structure over signals | Levels, balance zones, and auction context—not auto buy/sell |
| Objective presentation | Clear price, volume, and structure; minimal chrome |
| Fast to read | First screen = chart + essentials; no dashboard clutter |

### Primary user

Intraday / short-horizon ES traders who want a dedicated structural chart view without fighting a general-purpose platform.

### v1 success look

A usable ES chart with:

1. Candles (and/or OHLC) on selectable timeframes  
2. Volume  
3. Contract/front-month handling (or continuous series)  
4. Basic overlays (VWAP, moving averages, horizontal levels)  
5. Clean responsive UI under the ObjectiveCharts brand  

---

## Technical approach

### Recommended stack (proposal)

| Layer | Choice | Why |
|---|---|---|
| App | Next.js (App Router) + TypeScript | Fast UI iteration, solid hosting story |
| Chart | Canvas/WebGL chart lib (e.g. Lightweight Charts or custom) | Performance for tick/bar series |
| API | Thin Node/Next route handlers or separate FastAPI service | Keep data ingestion isolated from UI |
| Data | Delayed ES OHLCV provider first | Unblocks development without live-feed contracts |
| Storage | Postgres (bars, symbols, user prefs) + object/cache for hot series | Simple, durable |
| Deploy | Vercel (web) + managed DB | Matches greenfield speed |

### Core domains

```
┌─────────────┐     ┌──────────────────┐     ┌────────────────┐
│  Market     │────▶│  Series store    │────▶│  Chart API     │
│  ingest     │     │  (bars / ticks)  │     │  + WebSocket   │
└─────────────┘     └──────────────────┘     └───────┬────────┘
                                                     │
                                                     ▼
                                            ┌────────────────┐
                                            │  Chart UI      │
                                            │  + overlays    │
                                            └────────────────┘
```

### Non-goals for v1

- Broker order routing / execution  
- Multi-asset universe  
- Full TradingView clone feature set  
- Mobile native apps  
- Guaranteed real-time CME licensed feed (unless data access is already arranged)

---

## Phases

### Phase 0 — Foundations

- [ ] Confirm product scope and brand (name, tone, visual direction)
- [ ] Choose market-data source and licensing path (delayed vs live)
- [ ] Scaffold monorepo or single app (`apps/web`, optional `services/ingest`)
- [ ] CI: lint, typecheck, unit tests, preview deploys
- [ ] README with local run instructions

### Phase 1 — Chart MVP

- [ ] Symbol model for ES (front month / continuous)
- [ ] Historical bar API (`interval`, `from`, `to`)
- [ ] Chart canvas: candles, crosshair, pan/zoom, timeframe switcher
- [ ] Volume pane
- [ ] Session markers (RTH vs ETH) if data supports it
- [ ] Empty/error/loading states

### Phase 2 — Structure overlays

- [ ] Horizontal levels (manual + saved)
- [ ] VWAP (session)
- [ ] Moving averages (configurable)
- [ ] Optional: balance-zone / inventory-shelf projection tool (user-defined base range → projected multiples)
- [ ] Overlay persistence per user or local profile

### Phase 3 — Live / near-live updates

- [ ] Streaming or polling updates for the active timeframe
- [ ] Contract roll rules documented and implemented
- [ ] Latency / freshness indicator in UI
- [ ] Rate limiting and reconnect behavior

### Phase 4 — Product polish

- [ ] Auth (if cloud sync of layouts is required)
- [ ] Layout save/load
- [ ] Keyboard shortcuts and accessibility pass
- [ ] Performance budget (interaction under load)
- [ ] Public landing + chart app shell with ObjectiveCharts branding

---

## Dependencies

| Dependency | Needed for | Notes |
|---|---|---|
| Market data agreement / API keys | Phase 1+ | Blocks real bars; synthetic data can stub UI |
| Brand assets / naming | Phase 0 / 4 | Logo, colors, domain |
| Hosting + DB | Phase 1 | Can start local-only |
| Auth provider (optional) | Phase 4 | Only if synced layouts matter |

---

## Risks

| Risk | Impact | Mitigation |
|---|---|---|
| Data licensing cost / delay | Can't ship real ES prices | Start with delayed/public demo series; abstract provider behind an interface |
| Chart scope creep | Slow MVP | Freeze v1 overlay list; park advanced tools |
| Continuous vs contract series confusion | Wrong history / gaps | Document roll method early; show active contract in UI |
| Overbuilding backend | Wasted work | Serve static/historical fixtures until ingest is proven |

---

## Open decisions (need owner input)

1. **Audience:** personal tool, public free chart, or paid product?  
2. **Data:** which provider, and delayed vs live for launch?  
3. **Contract model:** front-month only, or back-adjusted continuous?  
4. **Must-have overlays for v1** beyond VWAP / MAs / manual levels?  
5. **Stack preference:** OK with Next.js + TypeScript web app, or prefer Python-first / desktop?

---

## Suggested next step

Approve or revise this plan, then answer the open decisions above. After that, implement **Phase 0 + Phase 1** on a feature branch with a working chart against fixture or delayed data.
