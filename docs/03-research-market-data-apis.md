# Holdyfoldy — Market Data API Research

*Research completed Oct 8, 2026. Prices/limits change often — re-check provider pages before committing.*

## Providers compared

| Provider | Free tier | Paid from | Real-time? | WebSocket | Notes |
|---|---|---|---|---|---|
| **Finnhub** | 60 calls/min, free US real-time (IEX feed) | ~$49.99/mo | ✅ US free | ✅ | Best free real-time US; fundamentals, earnings, sentiment |
| **Twelve Data** | 800 calls/day | $29/mo | ✅ limited | ✅ limited | Multi-asset in one API; credit-based |
| **Massive (ex-Polygon.io)** | 5 calls/min, EOD only | $29/mo (15-min delayed); $199/mo real-time | ✅ paid | ✅ higher tiers | Highest-quality US real-time; free tier is a trial |
| **Alpha Vantage** | 25 calls/day | $49.99/mo (75/min, still delayed) | ❌ delayed | ❌ | Prototypes only |
| **IEX Cloud** | 50k credits/mo (legacy) | usage-based | ✅ | ⚠️ | Legacy credits may be retired — verify |
| **Alpaca** | free with brokerage account | free | ✅ US real-time | ✅ | Alpaca customers only; no intl |
| **Yahoo Finance (unofficial)** | free | n/a | ✅ near-real-time | ❌ | **No license — not legal for a commercial app.** Prototyping only. |

## ⚠️ Licensing — the part most builders skip

Displaying quotes in a commercial app needs a *display/redistribution* license; free tiers usually don't grant it:

- **Finnhub free:** personal use only — commercial display needs a paid agreement.
- **Alpha Vantage (free and premium):** personal, non-commercial; commercial = contact sales.
- **Twelve Data:** free = internal non-display; cheapest display/commercial tier ("Venture") is **$499/mo**.
- **Massive/Polygon:** free = individual/non-professional; "Stocks Business" display license starts at **$2,499/mo**.
- **Marketstack:** free = non-commercial; Basic **$9.99/mo** is the cheapest legit commercial step.

## Architecture pattern: server-side proxy

**Never call the provider from the app with an exposed key.** Run a backend proxy that:
- Hides the API key, batches symbols, caches aggressively (~30s),
- Degrades gracefully on rate limits,
- Lets you swap providers without shipping an app update.

## Recommendation

1. **Prototype** on Finnhub free (real-time US).
2. **Ship v1** on Twelve Data $29/mo or Finnhub paid, behind the proxy.
3. **Budget a proper display license** ($499–$2,499/mo tier) once the app monetizes.
4. **Free tier = delayed quotes (15-min); real-time = Pro feature.** This conveniently aligns cost with revenue.

## Sources
- API comparison (Sep 2026): https://dev.to/kevin_menesesgonzlez/how-to-choose-the-best-stock-api-in-2026-1674
- Licensing table (Oct 2026): https://www.moneyflock.com/contents/articles/free-stock-api-commercial-use-licensing
- Finnhub showdown: https://medium.com/@trading.dude/the-great-financial-data-api-showdown-fcdd2bad3fe7
