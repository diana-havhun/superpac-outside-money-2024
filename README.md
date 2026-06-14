# Does Outside Money Win Elections?

**Super PAC spending and electoral outcomes in 2024 U.S. House races**

An analysis of whether independent expenditures by Super PACs are associated with winning U.S. House races in 2024 — combining FEC campaign-finance filings with certified election results, using SQL, exploratory analysis, and two regression models.

## Bottom line

In 2024, Super PAC money flowed overwhelmingly toward close races and candidates who were already strong — not toward long shots. Candidates with Super PAC backing won **55%** of competitive races versus **41%** without it. But once incumbency and party are taken into account, outside money becomes a *secondary* predictor of victory, behind both. The clearest reading of the data: **Super PAC spending marks winners more than it makes them.**

## The four questions

1. **Does Super PAC spending track electoral outcomes** — vote share and win/loss?
2. **Is it more important than a candidate's own fundraising?**
3. **Where does the money flow** — competitive or safe districts?
4. **Do Democrats and Republicans deploy it differently?**

## What's in this repo

- [superpac_outside_money_2024.ipynb](superpac_outside_money_2024.ipynb) — full analysis with code (SQL, EDA, models)
- [superpac_outside_money_2024_report.pdf](superpac_outside_money_2024_report.pdf) — clean report, no code

## Key findings

- **Money chases close races.** Toss-up districts averaged ~7× the Super PAC spending of safe districts. Among above-average-spending districts, 56% were competitive, versus just 12% of low-spending ones.
- **Structure beats money.** In both a linear model (margin) and a logistic model (win/loss), incumbency and party out-predicted Super PAC support. Outside money has a real but secondary effect.
- **It complements, not substitutes.** Candidates with high Super PAC support also spent far more of their own money — outside money piles onto strong campaigns rather than replacing fundraising.
- **A note on causation.** Every relationship here is associative, not causal: Super PACs back candidates who are already viable, so money and candidate strength are entangled by design. This is the endogeneity problem at the heart of campaign-finance research.

## Methods

- **District competitiveness** classified by two-party margin: Toss-up (0–5 pts), Competitive (5–15), Likely Safe (15–30), Safe (30+).
- **Super PAC spending** = independent expenditures (FEC types 24E *for* / 24A *against*) by Independent Expenditure-Only committees (committee type "O"), aggregated per candidate.
- Data merged into a candidate-level table, loaded into **SQLite** for SQL analysis; two **scikit-learn** models (linear + logistic regression) fit on competitive districts.

## Data

This analysis uses four public datasets (not included here due to size — download and place in a `data/` folder, or alongside the notebook, to reproduce). FEC bulk files are pipe-delimited with no headers; the notebook assigns column names per the FEC file layouts.

- **MIT Election Lab — U.S. House returns, 1976–2024** (`1976-2024-house.tab`)
  https://electionlab.mit.edu/data
- **FEC — Contributions from committees to candidates & independent expenditures** (`itpas2.txt`, from `pas224.zip`) — the 24E/24A records used as Super PAC spending
  https://www.fec.gov/files/bulk-downloads/2024/pas224.zip
- **FEC — All Candidates summary** (`weball24.txt`, from `weball24.zip`) — total disbursements and incumbency
  https://www.fec.gov/files/bulk-downloads/2024/weball24.zip
- **FEC — Committee Master** (`cm.txt`, from `cm24.zip`) — used to identify Super PACs (committee type "O")
  https://www.fec.gov/files/bulk-downloads/2024/cm24.zip

FEC file layouts/column definitions: https://www.fec.gov/campaign-finance-data/

## Tools

Python (pandas, scikit-learn, matplotlib, seaborn), SQL (SQLite), Jupyter.
