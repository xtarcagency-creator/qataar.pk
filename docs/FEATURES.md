# Features

Built mobile-first (360–767px), and also works on tablet and desktop. Bottom tabs on mobile: **Explore**, **Map & Search**, **My Token**, **Shop Counter**.

## 1. Customer: find a shop

- **Explore feed:** venue cards showing a cover photo, live wait pill, rating, area and landmark, plus 3 services with duration and `Rs.` price. Tapping a service opens booking with it already selected.
- **Filters:** Karachi area (Nazimabad, Bahadurabad, North Nazimabad, Hyderi, Gulistan-e-Johar, DHA / Clifton), search, sort (shortest wait / open chairs / price / rating), and an "under Rs. 800" toggle.
- **Map view:** a Karachi neighbourhood map with `Rs. price • wait` pins. Split screen (cards + map) on desktop.

## 2. Customer: claim a token

- **Booking sheet:** a bottom sheet you can swipe down to close. Choose an Ustad or **Any Available Chair**, pick one or more services (the total `Rs.` updates as you go), and enter a name and `03XX-XXXXXXX` number. No password, no app download.
- **Seat assignment:**
  - *Any Ustad*: you're queued on the fastest chair, and **whichever chair frees up first takes you** (earliest token goes first).
  - *A specific Ustad*: you wait for that chair only.

## 3. Customer: live boarding pass

| Element | Behaviour |
|---|---|
| Your Token / Now Serving | Large tabular numbers (`#KHI-16`). Now Serving rolls when it changes |
| Chair · Ahead · Est. wait | Wait counts down in real time from when the current cut started |
| **Live Qataar lane** | `✂ ▢ ▢ YOU`: one block per person ahead. A block drops out as each cut finishes |
| Alert banner | Amber **Leave home now** when you're 1 away, green **Chair ready** when it's your turn |
| Stepper | Queued → Leave Home → In Chair → Done |
| **Step Back 1 Spot** | Stuck in traffic: swap places with the person behind you. Your token number stays the same |
| Cancel token | Removes you from the queue |

- **Print-in animation** when the token is first issued. All motion is switched off when the phone's *reduce motion* setting is on.
- **Alerts when the counter acts:** a toast, plus vibration on Android, for *Queue moved*, *Leave home now*, *Chair ready*, *You missed your call*, and *Haircut completed*.
- **Floating token pill:** while you browse other screens, your active token sits just above the bottom tabs.

## 4. Shop: Shop Counter (counter console)

For the counter phone or tablet.

- **Summary numbers:** customers active, cuts completed today, average chair wait, and chairs open.
- **One card per chair:**
  - *Now Serving*: token, name, source (`APP` / `WALK-IN`), and a **live timer** (`12m / 25m`) that turns red when the cut runs over time.
  - **Finish Cut & Call Next (+1)**: completes the cut and takes the next customer (see seat assignment above).
  - **Not Here? Mark No-Show** (app customers only): the customer moves back to be next after the following cut. If nobody else is waiting, their token is released and they're notified.
  - The waiting list for that chair.
  - **+ Walk-in Cut / + Walk-in Khat**: log customers who walked in without the app, so the numbers match what's happening in the shop.

## 5. Shop: List Your Shop (4 steps)

1. **Service categories:** Haircuts & Fades, Khat & Beard, Desi Champi & Massage, Facials & Cleansing, Groom Packages, Kids Cuts.
2. **Shop identity:** name, area, landmark.
3. **Chairs & Ustads:** 1–6 chairs, with an Ustad name for each.
4. **Rate card:** `Rs.` prices for cut, khat and champi.

Publishing adds the shop to the marketplace and map, and opens its Shop Counter.

## Design system

- **Fonts:** Plus Jakarta Sans for the UI, and JetBrains Mono with `tabular-nums` for tokens, timers and prices, so numbers don't jump around as they change.
- **Status colours:** green = open / under 15 min, amber = leave home / 1–2 away, red = busy / over 40 min.
- **Mobile:** 44–48px tap targets, 16px inputs (stops iOS from zooming in), and safe-area padding above the bottom tabs.
