# Holdyfoldy — Architecture Document

*The contract Cursor builds from. Keep it current: when a decision changes, update this doc first.*

## 1. Tech stack

| Layer | Choice | Why |
|---|---|---|
| App framework | **Flutter (Dart)** | One codebase iOS + Android; hot reload; strong finance-charting story |
| Charts | **fl_chart** (MIT) | Most-installed Flutter chart lib; line/bar/pie/scatter/radar/candlestick, declarative |
| State management | **Riverpod** | Streams-friendly for live data |
| Market data (prototype) | **Finnhub** free (real-time US) | Best free real-time; proves the streaming pipeline |
| Market data (v1 ship) | **Twelve Data** $29/mo or Finnhub paid | Behind the proxy; upgrade to display license ($499+/mo) when monetized |
| Backend proxy | **Dart Frog** or Node/Express | Hides API key, batches symbols, caches ~30s, degrades gracefully; lets us swap providers without an app update |
| Database | **PostgreSQL** (via Supabase) | Portfolio math (lots, TWR/MWR, historical FX) needs relational queries |
| Auth | Supabase Auth (OAuth) | — |
| Push | Firebase Cloud Messaging | Price/dividend/earnings alerts |
| Local cache | drift (SQLite) | Offline-first: instant opens, cached quotes + portfolio snapshot |
| IAP | in_app_purchase | Free/Pro entitlements |

## 2. Module map

```
lib/
├── market/                  # Pure Dart where possible (testable)
│   ├── quotes.dart          # Quote stream: websocket → throttled (1-2/sec) → UI
│   ├── symbols.dart         # Symbol search, ticker normalization
│   └── history.dart         # OHLC history, corporate actions
├── portfolio/               # The accounting engine — the heart of the app
│   ├── ledger.dart          # Transactions: buy/sell/dividend/split/fee/transfer
│   ├── lots.dart            # Cost-basis: FIFO + average-cost; derives lots from ordered txns
│   ├── performance.dart     # TWR, MWR/IRR, realized vs unrealized P&L (total return, not price return)
│   ├── dividends.dart       # Dividend ledger, yield-on-cost, calendar
│   └── fx.dart              # Multi-currency: historical FX at txn date, live FX for value
├── lenses/                  # Viewing modes
│   ├── privacy.dart         # Zen mode: masks $ app-wide, keeps % and charts
│   ├── filters.dart         # Per-account / tag / asset-class / watchlist lenses
│   └── views.dart           # Full $ / percent-only / indexed-100 relative modes
├── ui/                      # Home, watchlists, holding detail, charts, alerts, settings
└── services/                # Proxy client, persistence, notifications, IAP, import
```

**Rules:**
- `portfolio/` is pure Dart with unit tests against hand-computed ledgers (FIFO edge cases, splits, DRIPs).
- The app **never** calls the market-data provider directly — all quotes go through the backend proxy.
- Never log or transmit absolute dollar values in analytics (privacy brand promise, enforced in code).

## 3. Data models

```dart
Transaction { id, accountId, symbol, type (buy|sell|dividend|split|fee|transfer|cash),
              date, quantity, price, currency, fxRateAtDate, fees, notes }
Lot { id, symbol, openDate, quantity, costBasis, method (fifo|average) }
Holding { symbol, quantity, avgCost, marketValue, dayChange, totalReturn, twr }
Dividend { symbol, exDate, payDate, amountPerShare, totalReceived, yieldOnCost }
Quote { symbol, price, change, changePct, dayHigh, dayLow, volume, timestamp, delayed: bool }
Lens { id, name, kind (account|tag|assetClass|watchlist), filter }
ViewMode { full | percentOnly | indexed100 | privacy }
Alert { id, symbol?, kind (price|pctMove|dividend|earnings|portfolio), condition, triggered }
Snapshot { date, totalValue, perHoldingValue }   // daily portfolio snapshots → time-machine view
```

## 4. Key flows

**Live portfolio home**
1. App opens → local snapshot renders instantly (offline-first).
2. Subscribes to proxy websocket for visible symbols → throttled quote updates (1–2/sec) → P&L recomputed.
3. Pauses subscription when backgrounded; resumes with a snapshot refresh.

**Recording a purchase**
1. Manual entry or CSV import (per-broker column mapping, idempotent re-import).
2. `ledger` appends → `lots` re-derives → `performance` recomputes → UI updates.
3. Buy commissions go *into* cost basis; standalone fees are separate line items.

**Privacy mode**
1. One tap (or auto on screen-record/mirror) → `ViewMode.privacy` masks every `$` app-wide (`$•••`).
2. Percentages, charts, and allocation visuals keep working — nothing breaks, nothing leaks.
3. Widgets and notifications respect the mode (no dollar amounts in a privacy-safe widget).

**Alerts**
1. User sets alert → stored server-side → proxy evaluates on quote ticks → FCM push.

## 5. API contracts (backend proxy)

```
GET  /quotes?symbols=AAPL,TSLA        → { quotes: Quote[] }            (cached ~30s)
WS   /stream                          → subscribe {symbols} / quote ticks
GET  /history/:symbol?range=1y        → OHLC[]
GET  /search?q=...                   → symbol matches
POST /alerts  /  DELETE /alerts/:id
```
Portfolio math lives client-side in v1 (Supabase sync later); proxy is quotes-only at first.

## 6. Coding conventions

- All money as integer minor units or `Decimal` — never float for money.
- Historical FX: always store `fxRateAtDate` on the transaction; never recompute history with today's rate.
- Time-machine view needs daily `Snapshot`s — write one per day per portfolio from day one (it's the #1 engineering hard part; don't bolt it on later).
- Every user-visible string localized from day one.

## 7. Non-goals (v1)

- No broker sync (SnapTrade/Plaid — Phase 2).
- No trade execution.
- No crypto/NFT (add when the stock core is solid).
- No social features.

## 8. Build order

1. Ledger + lots + performance engine (pure Dart, tested against hand-computed cases).
2. Manual entry + CSV import + multi-currency.
3. Proxy + Finnhub → live quotes on home/watchlists (fl_chart).
4. Lenses (filters) + view modes incl. privacy mode.
5. Alerts (FCM) + dividend calendar.
6. Pro IAP (real-time, unlimited, tax export) + snapshots/time-machine.

## Open technical questions

- Proxy hosting: Supabase Edge Functions vs. small VPS (cost at scale).
- Snapshot cadence and backfill strategy for time-machine.
- Exact Pro gating: which views are Pro-only vs. free.
