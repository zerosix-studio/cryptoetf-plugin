# CryptoETF Flows

Institutional demand for crypto, read from the money moving through US spot crypto ETFs. This plugin connects to the CryptoETF data service from [cryptoetf.today](https://cryptoetf.today) and adds skills that turn the raw numbers into short, accurate market briefs.

It covers sixteen assets: **BTC, ETH, SOL, XRP, HYPE, DOGE, LINK, AVAX, HBAR, LTC, BNB, DOT, SUI, NEAR, TRX and ZEC**. Figures are daily net flows in US dollars, taken from the issuers' own daily reports; for the smaller complexes, where no market-wide aggregator exists, they are computed from each issuer's fund data.

Published by [ZeroSix Studio](https://zerosix.studio), the team behind cryptoetf.today.

## What's inside

| Component | What it does |
|---|---|
| **CryptoETF connector** | Read-only data service at `mcp.cryptoetf.today`: latest flows, 30-day history per asset, weekly analytics, the CEFI sentiment index and spot prices |
| **etf-flows-daily** skill | A brief on the latest published day: total net flow, leaders, laggards |
| **etf-flows-weekly** skill | A seven-day recap with assets ranked by net flow and breadth |
| **etf-flow-history** skill | Trend, streaks and momentum for one asset, or a side-by-side comparison |
| **cefi-sentiment** skill | Reads the CEFI index (0–100, 50 = zero net flow) with its flow, trend and breadth components and checks it against recent flows |
| **/flows** and **/weekly** commands | One-step shortcuts for the daily brief and the weekly recap |

## Use it

Add the plugin, then connect the CryptoETF connector on the plugin's **Connectors** tab. No account and no API key are needed. Then ask in plain language:

- "What happened with crypto ETF flows yesterday?"
- "Which asset had the strongest week of inflows?"
- "Compare Bitcoin and Ethereum ETF flows over the past month."
- "Is ETF sentiment risk-on or risk-off right now?"

The skills keep the data honest: flows are reported one business day in arrears, so a brief names the actual trading day instead of calling it "today"; weekends are excluded from streaks and averages; and no per-fund figures are invented, because the connector reports asset-level totals only. Fund-level detail and longer history live on [cryptoetf.today](https://cryptoetf.today).

## Data and privacy

The plugin runs no code on your machine. The only network destination is the CryptoETF service at `https://mcp.cryptoetf.today/api/mcp`, operated by ZeroSix Studio. Each request carries only the tool name and, at most, an asset ticker; conversation content, files and credentials are never sent.

The service logs standard request metadata (IP address, time, endpoint) for 30 days to operate the service and enforce a limit of 60 requests per minute per IP. Nothing is sold or shared with third parties. Full policy: [cryptoetf.today/en/privacy](https://cryptoetf.today/en/privacy).

This is market data, not investment advice.

## Support

Open an issue at [github.com/zerosix-studio/cryptoetf-plugin](https://github.com/zerosix-studio/cryptoetf-plugin/issues) or use the contact form at [cryptoetf.today](https://cryptoetf.today/en/contact).

## License

MIT, see [LICENSE](LICENSE).
