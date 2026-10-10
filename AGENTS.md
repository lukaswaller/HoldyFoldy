# AGENTS.md — Holdyfoldy

## What this is
Holdyfoldy is a live stock portfolio tracker for iOS + Android: real-time quotes via a backend proxy, FIFO/average-cost lot accounting, dividend tracking, filtered lenses (accounts/tags/watchlists), and privacy viewing modes that hide absolute dollar amounts.

## Stack (exact — do not substitute)
- Flutter (Dart), single codebase for iOS + Android
- Charts: fl_chart · State: Riverpod
- Backend proxy for ALL market data (Dart Frog or Node) + PostgreSQL via Supabase
- Data providers: Finnhub (prototype), Twelve Data (v1 ship)
- Push: Firebase Cloud Messaging · Local cache: drift (SQLite)

## Docs (read before structural work)
- docs/01-vision-and-features.md — product spec / PRD
- docs/05-architecture.md — THE contract: ledger/lots engine, data models, flows, non-goals
- docs/06-ui-ux.md — screens, lenses, viewing modes
- docs/02-04 — research (incl. data licensing — read before touching quotes)

## Commands
- flutter pub get
- flutter run
- flutter test
- flutter analyze

## Do not
- Never call a market-data provider directly from the app — all quotes go through the backend proxy (API key + licensing).
- Never log or transmit absolute dollar values in analytics (privacy brand promise).
- Money is integer minor units or Decimal — never float for money.
- Always store fxRateAtDate on transactions; never recompute history with today's FX rate.
- No new dependencies without asking.

## How to work
- Plan before coding: explain the approach, wait for approval on anything structural.
- One micro-task at a time; the portfolio engine is pure Dart with tests against hand-computed ledgers.
- Update docs/05-architecture.md when a decision changes.
