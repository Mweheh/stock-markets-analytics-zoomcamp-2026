# HW3 — Categorical Features and Decision Trees

- [Official assignment](https://github.com/DataTalksClub/stock-markets-analytics-zoomcamp/blob/main/cohorts/2026/homework3.md)
- [Submission form](https://courses.datatalks.club/sma-zoomcamp-2026/homework/hw03)
- [User-run Google Colab](https://colab.research.google.com/drive/1i0mKulJ-JJ4ApGKL1rwjaqHejR3wRDKd?usp=sharing)
- [Reported outputs, answers and caveats](answers.json)
- [Q3 discrepancy audit instructions](Q3_AUDIT.md)

## Results reported from executed Google Colab

| Question | Colab output | Form status |
| --- | --- | --- |
| Q1 | `month_wom_October_w4`, abs corr **0.025**; 60 month-week dummies, 115 total | Matches option (not independently recomputed) |
| Q2 | `pred3_manual_dgs10_5`: **0.588**; `pred4`: 0.503 | Matches option (not independently recomputed) |
| Q3 | **1243** unique correct records | **INVALID AS SUBMISSION: form options are 271/571/1171/1571. Audit required.** |
| Q4 | Best `max_depth=4`, test precision **0.62917** | Matches option (not independently recomputed) |
| Q5 | Exploratory | Draft below |

Important: numerical data above comes from a pasted Colab output, not direct access to the executed .ipynb. The runnable notebook itself has not been uploaded to this repository. Q3 must be checked against the **TEST-only** subset and the five hand rules, with `DecisionTreeClassifier(max_depth=10, random_state=42)`.

## Q5 — Exploratory response (proposed)

I would add **market regime and risk indicators** including VIX (market-wide uncertainty), rolling realized volatility and ATR (stock-specific risk), relative strength versus a country/sector index (market context), and exchange-rate returns for non-US stocks. VIX and equity/index series can be retrieved from Yahoo Finance; macroeconomic risk-free rates and credit spreads from FRED. Features must be aligned to information actually available at prediction time, with publication lags respected. I would compare their incremental value using time-ordered validation by ticker/region, not randomized splitting.

The homework asks to choose the best depth using TEST precision. That follows the assignment, but is not a clean out-of-sample methodology for real-world model selection; tune on validation data and reserve TEST for one final check in future projects.
