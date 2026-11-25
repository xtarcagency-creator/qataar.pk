# Qataar.pk

Live queue tokens for Karachi barbershops. Customers claim a token from home, watch the chair numbers move, and leave home when they're close to the front. Shops run their floor from one phone or tablet at the counter.

This is the **MVP**: a single `index.html` file with no build step and no backend.

## Run it

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

**GitHub Pages:** Settings → Pages → deploy from `main`, root folder. It will be served at `https://xtarcagency-creator.github.io/qataar.pk/`.

## Try the core loop (2 tabs)

1. **Tab 1 (customer):** Explore → open a shop → enter a name and an `03XX` number → **Claim Token**. Your boarding pass opens.
2. **Tab 2 (shop):** Shop Counter → pick the same shop → tap **Finish Cut & Call Next** on your chair.
3. Tab 1 updates on its own: the queue lane moves, then **Leave Home Now**, then **Chair Ready**.

Use **Reset Demo** in the footer to restore the 8 seeded shops.

## Docs

- [Features](docs/FEATURES.md): what the app does, screen by screen
- [Architecture](docs/ARCHITECTURE.md): state, queue engine, rendering, limits and next steps

## Status

| Works now | Not yet |
|---|---|
| Marketplace, map, booking, boarding pass, Shop Counter, List Your Shop | Sync between different devices (needs a backend) |
| Live sync between tabs on one device | SMS / push alerts when the app is closed |
| Time-based wait countdown, no-show handling | Accounts, payments, reviews |
