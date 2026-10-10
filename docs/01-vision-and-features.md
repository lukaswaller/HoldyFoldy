# Holdyfoldy — Vision and Feature List

**Working title:** Holdyfoldy
**One-liner:** Your portfolio, live — with viewing modes that respect your privacy.

## The idea

Inspired by the "My Stocks Portfolio & Market" app, which gets the basics right but is limited and buggy: viewing just a subset of stocks is awkward, tracking purchases/lots is convoluted, and the UX fights you. This app rebuilds that concept properly:

- **Live market data** via a real data subscription — prices update in real time, not on a delay or a manual refresh.
- **Portfolio tracking done right** — buys/sells, cost basis, dividends, clean P&L.
- **Viewing modes** — switch between full-detail, percentage-only, and **privacy mode** that hides absolute dollar amounts (so you can check your portfolio on the bus without exposing your net worth).
- Everything the major stock apps have, with the rough edges sanded off.

## Who it's for

- Retail investors who actively track a portfolio and want it live on their phone.
- Anyone who checks stocks in public and doesn't want dollar amounts on display.
- Ex–"My Stocks" users annoyed by its limitations.

## Core features (MVP)

1. **Live quotes** — real-time (or near-real-time) prices via a market data API, with streaming updates on the portfolio screen.
2. **Watchlists** — multiple watchlists, custom groupings/tags; view *any subset* of stocks instantly (the thing My Stocks makes painful).
3. **Portfolio & lots** — record buys/sells (with fees), FIFO/average cost basis, realized vs unrealized P&L, dividend tracking.
4. **Viewing modes** — the signature feature:
   - *Full* — everything, dollars and cents.
   - *Percent* — gains/losses as % only.
   - *Privacy* — hides all absolute money values (great for public/shoulder-surfers); optionally disguise the app (generic widget name).
   - *Focus* — one watchlist/tag at a time, full-screen.
5. **Charts** — intraday + historical, per-stock and whole-portfolio performance vs benchmarks.
6. **Alerts** — price, % move, and portfolio-level alerts with push notifications.
7. **Multi-currency** — hold stocks in different currencies with FX conversion.

## Later phases

- Transaction import (CSV/broker import).
- News + fundamentals + earnings calendar per stock.
- Widgets (including a privacy-safe widget), Apple Watch complications.
- Paper trading / what-if scenarios.
- Dividend calendar and income projections.

## What makes it stand out

- **Viewing modes, especially privacy mode** — no major app does discreet display well; it's a genuine differentiator.
- Subset viewing and purchase tracking that *isn't* buggy — directly attacks My Stocks' weaknesses.
- Fast, clean, modern — the "just works" portfolio app.

## Monetization (draft)

- Free: delayed quotes (15 min), 1 portfolio, basic watchlists.
- **Pro subscription:** real-time data, unlimited portfolios/watchlists, alerts, advanced charts. (Real-time exchange data costs money — the sub funds the data bill. See `02 - Research / Market Data APIs`.)

## Open questions

- Exact name + brand.
- Which data provider? (Biggest decision — see Research.)
- Real-time for which exchanges in v1? (US first is cheapest/easiest.)
- How is privacy mode triggered? (Toggle, Face ID, shake, location?)

## Status

📝 Planning — vision drafted, research in progress.
