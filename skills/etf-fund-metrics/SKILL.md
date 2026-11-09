---
name: etf-fund-metrics
description: What sits inside US spot crypto ETFs beyond daily flows. Use when the user asks how many coins the ETFs hold or what share of supply they own, whether ETF holders are in profit or underwater and at what average entry price, whether bitcoin ETFs buy more than miners produce, which funds stake and at what yield, or how much issuers earn from fees.
---

# Inside the funds: holdings, entry price, staking, fees

Five connector tools describe the stock of the funds rather than one day's flow. Pick the one that matches the question; don't call all five.

| Question | Tool | Page for charts and history |
|---|---|---|
| How many coins do the ETFs hold? What share of supply? | `get_etf_holdings` | https://cryptoetf.today/en/crypto-etf-holdings (bitcoin: /en/bitcoin-etf-holdings) |
| Are ETF holders in profit? What is their average entry? | `get_cost_basis` | https://cryptoetf.today/en/crypto-etf-cost-basis |
| Do bitcoin ETFs absorb more than miners produce? | `get_btc_etf_vs_miners` | https://cryptoetf.today/en/bitcoin-etf-buying-vs-mined |
| Which funds stake, how much, at what yield? | `get_staking` | https://cryptoetf.today/en/crypto-etf-staking (also /en/ethereum-etf-staking, /en/solana-etf-staking) |
| How much do issuers earn from fees? | `get_issuer_revenue` | https://cryptoetf.today/en/crypto-etf-issuer-revenue |

All tools except `get_btc_etf_vs_miners` take an optional `asset` (btc, eth, sol, xrp, hyp, doge, link, avax, hbar, ltc, bnb, dot, sui, near, trx, zec). Pass it when the user names one asset.

## Steps

1. Call the one tool that answers the question, filtered by asset when one is named.
2. Lead with the answer in one sentence, with the `asOf` date written out ("as of 6 November").
3. Add two or three supporting facts from the same response: the change over 7 or 30 days, the largest funds, the streak.
4. Say which figures are estimates where the response marks them (see below).
5. End with "Data: cryptoetf.today" and the page from the table above for charts and longer history.

## Reading each response

**`get_etf_holdings`**
- `coins` is the total held by all ETFs of the asset; `supplyPct` is already a percent of circulating supply (4.74 means 4.74%, not 474%).
- `coinsReported` comes from issuers; `coinsEstimated` is net assets divided by the same-day coin price for issuers that don't publish coins. Per fund, `coinsBasis` says which. When estimated coins are a large part of the total, say the total is partly estimated.
- `change7dCoins` and `change30dCoins` are changes in coins, not dollars. `change7dPctPoints` is a change in percentage points of supply.
- `history` gives the last 30 trading days of `coins` and `supplyPct` per asset.
- Converted trusts (GBTC, ETHE) are included in holdings.

**`get_cost_basis`**
- `avgEntryUsd` is the average price per coin paid through net creations and redemptions; `price` is the latest coin price; `pnlPct` is a fraction (−0.072 means −7.2%).
- `state` is `above` or `below` entry; `streakDays` and `streakSince` say for how long.
- Converted trusts (GBTC, ETHE) are left out of this calculation, so `qtyCoin` is smaller than the holdings total. Don't compare the two as if they should match.
- This is an estimate built from daily flows, not a figure any issuer reports. Call it "estimated average entry".

**`get_btc_etf_vs_miners`**
- Windows: `week` (last full Monday–Sunday week), `30d`, `ytd`, `all` (since 11 January 2024). Each has `etfNetBtc`, `minedBtc` and `ratio`.
- `ratio` is ETF net BTC divided by mined BTC. It is `null` when ETFs had net outflows: say the ETFs were net sellers in that window, don't report a ratio.
- `streakWeeks.length` is the run of full weeks with ETF demand above new supply; 0 means the latest full week was not one.
- `weeks` lists the last 12 weeks; a week with `partial: true` is still in progress and must not be read as a full week.
- Bitcoin only. For other assets there is no mined-supply comparison.

**`get_staking`**
- `status` per fund: `active`, `allowed_not_started`, `pending_amendment`, `no`, `not_stakeable` (proof-of-work assets and XRP) or `unknown`.
- `disclosedYieldNetPct` and `disclosedYieldGrossPct` are what the issuer published, dated by `disclosedYieldAsOf`. Give the date with the yield.
- `coinsPerShare.annualPct` is a separate estimate: the annualised growth in coins behind each share from daily snapshots. It is `null` where the series isn't clean. Don't present it as the issuer's yield.
- `rewardMode`: `accrue_to_nav` means rewards stay in the fund; `cash_distribution` means they are paid out, with `lastDistribution` and `distributionYieldPct`.
- At asset level, `stakedCoins` and `stakedShareOfEtfCoinsPct` are estimates (coins held times the disclosed or target staked share).
- Each fund carries the SEC filing the facts were taken from in `filing`; cite it when the user asks where a figure comes from.

**`get_issuer_revenue`**
- Revenue is net assets times the prospectus sponsor fee, with temporary waivers applied (`waiverActive`, `waiverNote`). It is arithmetic on the published fee, not revenue an issuer reported, and it excludes the sponsor's share of staking rewards. Say so in one short clause.
- `revenueUsdAnnualised` is at current net assets; `revenueUsd30d` sums the last 30 calendar days.
- `totals` is the whole market, `issuers` ranks issuers, `funds` lists funds (filtered by `asset` when given). `avgFeePct` and `feePct` are percent per year.

## Rules

- Never invent a figure that isn't in a response, and never fill a null with a guess or a calculation from the price.
- Don't mix dates: holdings, revenue and miners use the last trading day, cost basis can be a day later. Name the date that belongs to each figure.
- Round for reading: coins to the nearest thousand for large totals, dollars to millions or billions, percentages to one decimal.
- Plain description of data, not advice. Don't tell the user to buy, sell or hold.
