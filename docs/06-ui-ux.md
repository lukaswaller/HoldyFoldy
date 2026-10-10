# Holdyfoldy — UI and UX Plan

## Design language

- **Calm, trustworthy, fast.** Finance apps earn trust through restraint: deep neutrals, one accent (green/red used *only* for gains/losses), tabular numerals everywhere.
- **Privacy as brand.** The privacy toggle is a first-class citizen — prominent, satisfying, instant. This is the feature people tell friends about.
- Numbers: tabular figures, aligned decimals; red/green also paired with ▲/▼ (never color alone).

## Screens

1. **Home (portfolio)** — the money screen:
   - Header: total value, day change, total return — with the **privacy toggle** right beside them.
   - Lens switcher (chips): All / account / tag / watchlist — *this* is the "just a set of stocks at once" fix.
   - Holdings list: symbol, sparkline, price, day %, value — density toggle (comfortable/compact).
   - Pull-to-refresh; live pulse dot when streaming.
2. **View modes** — segmented control, always visible:
   - **Full** — everything, dollars and cents.
   - **Percent** — allocations and returns as % only (safe for screenshots).
   - **Indexed** — normalize to 100 at a chosen date (shows *performance*, never *size*).
   - **Privacy** — `$•••` everywhere; charts keep shapes, lose axis labels with values.
3. **Holding detail** — big chart (intraday → max range), stats grid (P/E, EPS, beta, dividend & yield), your position (lots, avg cost, realized/unrealized), dividend history, news.
4. **Watchlists** — multiple, custom groupings; any subset one tap away.
5. **Add transaction** — fast manual entry (symbol → type → qty/price/date) + CSV import with broker column mapping.
6. **Dividends** — the view My Stocks lacks: calendar, per-holding history, yield-on-cost, "all dividends earned" total.
7. **Alerts** — price / % move / ex-div / earnings / portfolio-level, managed in one list.
8. **Widgets** — home-screen widget with a privacy-safe variant (percent-only, no dollars).

## Key interactions

- **One-tap privacy:** toggle in the header; also auto-engages on screen recording/mirroring. Shake-to-hide as a hidden delight (settings option).
- **Lens chips** filter the entire home instantly — holdings, totals, charts all follow the lens.
- **Time-machine:** scrub a date on the portfolio chart to see value + holdings on any past day.
- **Benchmark overlay:** one tap overlays S&P 500 / NASDAQ on any performance chart.
- **Allocation treemap + donut** by sector/region/currency; rebalancing drift indicator (later version).

## Onboarding (first 60 seconds)

No account wall. "Add your first holding" → symbol search → quantity/price → instantly see it live in a beautiful portfolio. CSV import offered on step two. Value before signup.

## Accessibility & polish

- Dynamic type respected; compact density still meets touch targets.
- Red/green always paired with symbols/arrows.
- Reduce-motion: sparklines static, no animated counting.

## Later UX (v2+)

- Broker sync (SnapTrade/Plaid) — holdings appear automatically.
- Rebalancing planner with drift alerts.
- Tax center: yearly reports, realized gains, exports.
- Apple Watch complications (privacy-safe).
