---
name: cefi-sentiment
description: Read the CEFI crypto ETF sentiment index. Use when the user asks about crypto ETF sentiment, whether institutional demand is bullish or bearish, risk-on or risk-off mood, fear or greed in crypto ETFs, or mentions the CEFI index.
---

# CEFI sentiment index

CEFI (Crypto ETF Flow Index) is a composite sentiment score computed by cryptoetf.today from spot crypto ETF flows across all thirteen tracked assets. It is one number for the whole market, not one per asset.

## Steps

1. Call `get_cefi_index` from the CryptoETF connector. It returns `current` (0–100) and the `date` it was computed for.
2. Place the value in its zone:
   - 0–20 extreme fear
   - 20–40 fear
   - 40–60 neutral
   - 60–80 greed
   - 80–100 extreme greed

   50 is neutral: above it net inflows dominate (risk-on), below it net outflows do (risk-off).
3. Add the evidence behind the reading: call `get_weekly_analytics` and say whether the last week's flows agree with the index. If they seem to disagree (for example steady inflows while the index sits below 50), say so plainly: the index is a smoothed measure and moves more slowly than a single week of flows.
4. Answer in two or three sentences: the value and zone, what it means, and the supporting flow picture. End with "Data: cryptoetf.today" and link https://cryptoetf.today/en/crypto-etf-sentiment-index for the chart and history.

## Rules

- The connector returns only the current value. Don't invent past values, a trend, or per-asset CEFI scores. For history, point to the index page.
- Don't describe components, weights or a formula for the index beyond what is written here.
- Treat CEFI as a description of ETF demand, not a trading signal. Don't give investment advice or price predictions.
