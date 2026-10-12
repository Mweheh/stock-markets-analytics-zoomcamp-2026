# HW2 — Working with Financial Data in Pandas

- [Official assignment](https://github.com/DataTalksClub/stock-markets-analytics-zoomcamp/blob/main/cohorts/2026/homework2.md)
- [Submission form](https://courses.datatalks.club/sma-zoomcamp-2026/homework/hw02)
- [Archived observations and flags](answers.json)

## Earlier results: **audit pending**

| Question | Earlier observations | Audit status |
| --- | --- | --- |
| Q1 — Withdrawn IPO highest category value | About USD 499.99 million (~500) | Recompute from iposcoop source before claiming verified |
| Q2 — Median IPO Sharpe, 2026-09-11 | Conflicting earlier notes: 0.04 vs 0.0501 | **Conflict unresolved**; check method, actual median and form options |
| Q3 — Optimal IPO holding period | Earlier computation indicated 2 months (~0.9473 median growth) | **Does not match earlier reported options**; inspect IPO-date join and shift direction |
| Q4 — RSI <30 net income | Earlier output around USD 65,805.59 (~65.806 thousand) | Check exact dataset/rounding and form options |

Do not report HW2 as complete or validated yet. The official questions use iposcoop.com IPO data (not the course lecture's other IPO data source), date-specific market values, and a provided Parquet file for Q4. No authenticated executed HW2 notebook was accessible here, so no notebook is invented.

Q5: a proposed approach is to select IPOs using a time-ordered validation of financial quality, sector regime, valuation, liquidity and post-IPO volatility, after accounting for realistic fees and survivorship bias.
