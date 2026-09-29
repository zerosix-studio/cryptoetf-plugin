---
name: etf-flows-weekly
description: Weekly recap of spot crypto ETF flows. Use when the user asks how crypto ETFs did this week or over the last seven days, which asset had the strongest or weakest week of inflows, or wants a weekly market wrap-up for a newsletter, post or report.
---

# Weekly crypto ETF flow recap

Summarize the last seven days of US spot crypto ETF flows across all tracked assets.

## Steps

1. Call `get_weekly_analytics` from the CryptoETF connector. It returns, per asset, the net flow for the window (`netFlowUsdM`), `avgDailyUsdM`, and the number of positive and negative days, plus the window (`from`, `to`).
2. If the user wants the day-by-day shape for a major asset (for example "was it one big day or steady buying?"), call `get_asset_flows` for that asset and read the days inside the window.
3. Write the recap:
   - **Headline**: the combined net flow of all assets for the window, with the dates.
   - **Ranking**: assets ordered by net flow, largest inflow first. Show the top movers with their positive/negative day counts ("BTC +$703.1M, 4 of 4 trading days positive").
   - **Breadth**: how many assets saw net inflows versus net outflows.
   - **Notable**: one or two observations, such as a streak, a reversal from outflows, or a single day that made most of the week.
4. End with "Data: cryptoetf.today".

Offer a table when more than five assets moved or when the user wants something to paste into a report.

## Data rules

- Figures are **net USD millions**; positive is an inflow.
- The window covers the last seven calendar days. Use `tradingDays` for the number of trading days in it, and `avgDailyUsdM` as the average per trading day. If a response has no `tradingDays` field, don't quote `avgDailyUsdM`: in that older format it isn't comparable across assets. Report the net flow and day counts instead.
- Positive plus negative days is the number of days with non-zero flow. Trading days beyond that had no creations or redemptions, which is common for small funds.
- The latest published day is normally yesterday: flows are reported one business day in arrears.
- `HYP` is Hyperliquid (token ticker HYPE).
- Asset-level totals only: no per-fund breakdown, AUM or holdings. Don't invent fund figures. For fund detail, send the user to the asset's page, `https://cryptoetf.today/en/<slug>-etf-flows` (slugs: bitcoin, ethereum, solana, xrp, hype, dogecoin, chainlink, avalanche, hedera, litecoin, bnb, polkadot, sui).
- Describe the flows; don't give investment advice or price predictions.
