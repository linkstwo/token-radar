# Token Radar

[![token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/c/token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/linkstwo/token-radar/rank.svg)](https://make.fun/c/token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/linkstwo/token-radar/reward.svg)](https://make.fun/c/token-radar-01)

A token launch radar: it reads a sealed snapshot of the protocol universe
(DefiLlama, 758 protocols), finds the protocols that record no token, ranks
them by implied value, and shows a reader how to farm each one — as a static
web dashboard that renders with no network.

## What it does

- **Board:** 198 pre-token protocols ranked by implied value. The snapshot's
  40-protocol watchlist (TVL at or above $50M, no token on record) is the
  floor, never the ceiling: the board adds 158 pre-token protocols below the
  floor, so it finds launches early.
- **Value model (radar-2):** one function of the snapshot's own figures, the
  same for every row —
  `implied = round(TVL x categoryMultiple x growthMult x maturityMult x breadthMult)`.
  The category multiple (0.18x–0.50x) is a business-model prior: fee-taking
  vaults and curators earn the most per locked dollar, stablecoin issuers the
  least. Growth is the 7-day TVL change clamped to [-50%, +150%]; maturity
  grows with the days since the protocol was first listed with no token;
  breadth grows with the number of chains. No protocol name is special-cased.
- **Separation:** wrapped, staked, bridged and exchange products are excluded
  from the ranking entirely, and the 24 live-token protocols are set aside
  with their market caps shown — the market's own value, never a model's.
- **Farm plans:** every row carries a 7-step plan derived from its category,
  chains, TVL and age, each step with a cost and a time (capital tier,
  chain-specific gas, a second qualifying action, a weekly cadence, a hold
  period, a governance or quest step, and safe-claim rules).
- **Dashboard:** search, category / chain / minimum-TVL filters, a watchlist-only
  toggle, sortable columns, a Top 5 strip, a per-protocol detail drawer with
  the model's full breakdown, and a Live token tab. The page recomputes each
  row's implied value and checks it against the number board.json carries.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <snapshot.json>
```

Node 22, standard library only. The program runs twice and writes the same
bytes both times, with no network, in well under 120 seconds. It writes
`board.json`, `data.js`, `dashboard/`, `screenshots/` and `dossier.json`
into the output directory.

## Files

- `main.ts` — entrypoint: reads the snapshot, writes the board and the dossier.
- `lib/model.ts` — the radar-2 model, the farm plans and the snapshot parser.
- `vendor/makefun-board.ts` — the make.fun dossier library, unchanged.
- `dashboard/` — the static page (`index.html`, `app.css`, `app.js`).
- `screenshots/` — the board, a protocol detail view and a filtered view.

Implied value is a model, not a promise: it says what a token would be worth
if the protocol's record stayed as it is, and it does not say whether or when
a token is issued. A protocol on the board records no token — that is what
DefiLlama records, not proof it has none.
