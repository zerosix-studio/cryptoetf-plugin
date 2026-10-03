---
name: cefi-sentiment
description: Read the CEFI crypto ETF sentiment index. Use when the user asks about crypto ETF sentiment, whether institutional demand is bullish or bearish, risk-on or risk-off mood, fear or greed in crypto ETFs, or mentions the CEFI index.
---

# CEFI sentiment index

CEFI (Crypto ETF Flow Index) is a 0–100 index computed by cryptoetf.today from US spot crypto ETF flows across all sixteen tracked assets. It is one number for the whole market, not one per asset.

Net flows of all funds are added up in dollars, and three components are scored against the past year:

- **flow** — today's net flow, smoothed (40% of the index)
- **trend** — net flow over the last 20 trading days (30%)
- **breadth** — share of funds with inflows (30%)

Each component is on the same 0–100 scale as the index. 50 means zero net flow; a typical inflow day scores about 69, a typical outflow day about 31.

## Steps

1. Call `get_cefi_index` from the CryptoETF connector. It returns `current` (0–100), the `date` it was computed for and `components` (`flow`, `trend`, `breadth`, each 0–100; `null` if not available).
2. Place the value in its zone:
   - 0–20 extreme fear
   - 20–40 fear
   - 40–60 neutral
   - 60–80 greed
   - 80–100 extreme greed

   50 means zero net flow: above it net inflows dominate (risk-on), below it net outflows do (risk-off).
3. Use `components` to say what drives the reading — for example a high trend with a lower flow means the last month was strong but today is quieter; low breadth means a few large funds carry the total.
4. Add the evidence behind the reading: call `get_weekly_analytics` and say whether the last week's flows agree with the index. If they seem to disagree, say so plainly: the trend component looks back 20 trading days, so the index moves more slowly than a single week of flows.
5. Answer in two or three sentences: the value and zone, what it means, and the supporting flow picture. End with "Data: cryptoetf.today" and link https://cryptoetf.today/en/crypto-etf-sentiment-index for the chart and history.

## Rules

- The connector returns only the current value and its components. Don't invent past values or per-asset CEFI scores. For history, point to the index page.
- Don't describe weights or a formula for the index beyond what is written here.
- Treat CEFI as a description of ETF demand, not a trading signal. Don't give investment advice or price predictions.
