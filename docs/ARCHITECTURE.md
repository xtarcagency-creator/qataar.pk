# Architecture

## Overview

```
index.html
├── <head>    Tailwind (Play CDN) + config, Google Fonts, custom CSS (ticket, animations)
├── <body>    Static shells: header, 3 views, bottom tabs, toast area, 3 sheets/modals
└── <script>  State → queue engine → render functions → event handlers
```

- **No build step, no framework, no backend.** Plain JavaScript re-renders each part of the page from one state object.
- **Persistence:** `localStorage` key `qataar_pk_refero_marketplace_v2`, holding the whole state as JSON.

## State

One global object, `appState`, saved by `saveState()` after every change.

```js
appState = {
  activeView: 'explore' | 'split' | 'counter',
  areaFilter, searchQuery, sortPreference, under800Only,   // explore filters
  activeShopId, selectedChairId /* 'any' | chairId */, selectedServiceIds,
  counterShopId,
  userToken: null | {
    tokenNumber, shopId, chairId, services, totalPkr, totalMins,
    status: 'waiting' | 'in_chair' | 'done'
  },
  shops: [Shop]
}

Shop  = { id, name, area, landmark, rating, photo, mapCoords:{x,y},
          nextToken, completedToday, services:[Service], chairs:[Chair] }
Service = { id, category, name, durationMins, pricePkr }
Chair = { id, number, ustadName, avatar,
          currentCustomer: null | { tokenNumber, name, service, source,
                                    durationMins, startedAt, pref, isUser },
          queue: [{ tokenNumber, name, service, durationMins, source, pref, isUser }] }
```

- `source` is `'APP'` or `'WALK-IN'`.
- `pref` is `'any'` or a `chairId`. It decides which chairs may serve this token.
- Token numbers come from `shop.nextToken` and are shared across the whole shop. They're displayed as `#KHI-<n>`.

## Queue engine

| Function | Responsibility |
|---|---|
| `claimQueueToken()` | Issues a token. Seats the customer straight away if the target chair is free, otherwise queues them. The target is the chosen Ustad, or the fastest chair for *Any* |
| `pullNext(shop, chair)` | **The core rule.** A freed chair takes the earliest token among (a) its own next in line and (b) *Any Ustad* tokens on other chairs. Tokens pinned to another Ustad are never taken |
| `advanceChairQueue()` | Finish cut → `pullNext` → update the user token's status → toast |
| `markNoShow()` | `pullNext`, then put the missed customer back as next on this chair. If nobody was waiting, release the token |
| `stepBackOneSpot()` | Swap queue positions with the person behind. Token numbers don't change |
| `addCounterWalkIn()` | Issue a `WALK-IN` token pinned to that chair |
| `getChairWaitMins()` / `getUserTokenProgress()` | Wait = time left on the current cut + durations of everyone queued ahead |

**Time:** a customer in the chair stores `startedAt` and `durationMins`.

- `minsLeft(c) = max(1, duration − elapsed)`
- `minsElapsed(c)` drives the counter timer.
- `withClock()` gives seeded or older saved data a start time when it loads.
- A 30-second `setInterval` re-renders, so countdowns keep moving.

## Rendering

- `renderAll()` toggles the views, then calls `renderFilterChipsAndSelects`, `renderActiveTokenBanner`, `renderExploreVenues`, `renderSplitView` and `renderCounterView`, plus `renderTokenModal` if the pass is open.
- The booking drawer (`renderShopDrawer`) only re-renders when it's opened, so timer and sync re-renders never wipe what's typed into the form.
- Buttons call global functions through inline `onclick`.

**Change-driven animation.** `passMemo` remembers the last shown token, Now Serving value and ahead-count. Animation classes are only added when a value changed, so the 30-second re-render doesn't replay them. `justClaimed` triggers the print-in once.

## Sync

```
Shop Counter tab ──saveState()──▶ localStorage ──'storage' event──▶ Customer tab
                                                   loadState() → renderAll()
                                                   notifyTokenChange(before, after) → toast + vibrate
```

Each tab keeps its own `activeView`. This only works **within one browser on one device**.

## Known limits

| Limit | Impact | Upgrade path |
|---|---|---|
| `localStorage` sync only | A customer's phone and the shop's tablet don't see each other | Realtime backend (Supabase / Firebase): store `shops`, `chairs`, `tokens` server-side and subscribe to changes |
| Whole state in one browser | Anyone can edit any shop; no login | Shop owner sign-in (phone OTP); customers stay login-free and are tied to their token + phone |
| Shop names and other text are put into the page unescaped | Harmless while data stays on one device; a real security hole once others can enter data | Escape text in the render templates before adding a backend |
| No background alerts | Alerts only show while the page is open | Web Push or SMS / WhatsApp at the *Leave home* step |
| Tailwind Play CDN | Builds the CSS in the browser on every load (slower on low-end phones) | Compile Tailwind once and put the CSS in the file |
| Seed data and Unsplash photos | Demo content only | Photos uploaded by shops |
