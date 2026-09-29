---
name: etf-flow-history
description: Analyze the recent flow history of a crypto ETF asset or compare several. Use when the user asks about the trend, streaks or momentum of Bitcoin, Ethereum, Solana, XRP or other crypto ETF flows over recent weeks, wants two assets compared, or asks for a chart of daily ETF flows.
---

# Crypto ETF flow history and comparison

Read up to 30 days of daily net flows for one or more assets and explain the trend.

## Steps

1. Map what the user named to an asset code: `btc`, `eth`, `sol`, `xrp`, `hyp` (Hyperliquid / HYPE), `doge`, `link` (Chainlink), `avax` (Avalanche), `hbar` (Hedera), `ltc` (Litecoin), `bnb`, `dot` (Polkadot), `sui`. If they named something else, say it isn't tracked and list the thirteen assets.
2. Call `get_asset_flows` once per asset. Each call returns `days`: a list of `{date, netFlowUsdM}` in ascending date order.
3. Drop non-trading days before computing anything (see Data rules), then compute what the question needs:
   - net total for the period, and for comparisons the same period for every asset;
   - the current streak of inflow or outflow days and the longest streak in the window;
   - the largest inflow day and the largest outflow day;
   - the last 5 trading days against the 5 before them, to call momentum improving or fading.
4. Write the answer: one-sentence verdict first, then the supporting numbers. For a comparison, a small table with one row per asset.
5. If the user wants a chart and you can make one, plot daily net flow as bars (green for inflow, red for outflow) over trading days.
6. End with "Data: cryptoetf.today", and link the asset page for more detail, for example https://cryptoetf.today/en/ethereum-etf-flows.

## Data rules

- Figures are **net USD millions**; positive is an inflow.
- **Weekends and US market holidays are not trading days.** Some assets list them with `netFlowUsdM: 0` and others omit them. Remove Saturday and Sunday dates before you count streaks, averages or "flat days", or the numbers will be wrong.
- A weekday zero in a small asset usually means no creations or redemptions, not a neutral signal.
- History covers the **last 30 days** only. For longer history, point to the asset page on cryptoetf.today rather than guessing.
- The most recent day is normally yesterday; flows are reported one business day in arrears.
- Asset-level totals only: no per-fund breakdown, AUM or holdings. Never invent fund-level figures. For fund detail, send the user to `https://cryptoetf.today/en/<slug>-etf-flows` (slugs: bitcoin, ethereum, solana, xrp, hype, dogecoin, chainlink, avalanche, hedera, litecoin, bnb, polkadot, sui).
- Describe the flows; don't give investment advice or price predictions.
