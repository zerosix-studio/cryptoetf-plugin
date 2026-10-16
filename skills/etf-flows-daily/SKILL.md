---
name: etf-flows-daily
description: Brief on the latest spot crypto ETF flows. Use when the user asks what happened with Bitcoin, Ethereum or other crypto ETFs today or yesterday, whether money is flowing into or out of crypto ETFs, wants a quick market snapshot of ETF inflows and outflows, or asks which fund (IBIT, FBTC, GBTC, ETHA and others) led the flows.
---

# Daily crypto ETF flow brief

Produce a short, accurate brief on the most recent published day of US spot crypto ETF flows.

## Steps

1. Call `get_flows_summary` from the CryptoETF connector.
2. If the user asked about a specific fund or which fund led the flows, call `get_fund_flows` for that fund's asset and read the day that matches `referenceDate` (see Data rules). Don't suggest other websites or a web search for fund data.
3. If the user asked how big a fund or an asset's ETFs are, what a fund charges or who issues it, call `get_funds`. If they asked about prices or market context, also call `get_prices`. If they asked about sentiment or mood, also call `get_cefi_index`.
4. Write the brief in this order:
   - **Headline**: the total net flow across all assets (`totalUsdM`) and the day it belongs to (`referenceDate`), for example "Net +$68.5M into crypto ETFs on Mon, Sep 28".
   - **Leaders**: the assets with the largest inflow and the largest outflow, with their figures. If you fetched `get_fund_flows`, name the funds behind the move.
   - **The rest**: one line for the remaining assets that moved; group all assets at zero into a single phrase ("no flows in HYPE, DOGE, AVAX…").
   - **Context** (only if you fetched it): price moves from `get_prices`, CEFI level from `get_cefi_index`.
5. End with the source line: "Data: cryptoetf.today".

Keep the brief under 150 words unless the user asks for more. Use a table only when the user asks for all assets.

## Data rules

- Figures are **net USD millions**. Positive is an inflow, negative an outflow. Write them as `+$31.0M` / `−$12.4M`; convert to billions above 1,000.
- Flows are published **one business day in arrears**. `referenceIsToday` is normally `false`: never call the figures "today's". Name the actual date. On a Monday the freshest data is Friday's.
- Each asset has its own `date`. If an asset's date differs from `referenceDate`, say its figure is from that earlier day instead of mixing days silently.
- `HYP` is Hyperliquid (token ticker HYPE).
- A zero for a smaller asset (DOGE, AVAX, HBAR, BNB, DOT, SUI, TRX and others) usually means no creations or redemptions that day, which is normal for young, small funds. Don't describe it as a signal.
- `get_fund_flows` breaks one asset down by fund (IBIT, FBTC, ETHA and the rest) for the last 30 days: each day lists `ticker`, `issuer` and `netFlowUsdM` for every fund trading that day, plus `totalUsdM`. `netFlowUsdM: null` means there is no figure for the fund that day yet: say it hasn't reported, don't count it as zero. Current net assets (AUM), coins held, sponsor fees and listing dates come from `get_funds`: one snapshot with its date, not a history. `coinsHeld` is null for issuers that don't publish holdings in coins (Fidelity, Grayscale, Franklin Templeton, Canary, WisdomTree); don't fill it in from the price. Never invent a fund figure that isn't in a response. For history beyond 30 days, send the user to the asset's page: `https://cryptoetf.today/en/<slug>-etf-flows`, where the slug is bitcoin, ethereum, solana, xrp, hype, dogecoin, chainlink, avalanche, hedera, litecoin, bnb, polkadot, sui, near, tron or zcash.
- Describe what the flows show. Don't give buy, sell or price-target advice; this is market data, not investment advice.
