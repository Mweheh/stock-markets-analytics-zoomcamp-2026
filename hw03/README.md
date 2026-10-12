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
| Q3 | **1243** unique correct records on **33,163 TEST rows** | **INVALID AS SUBMISSION: form options are 271/571/1171/1571. Audit required.** |
| Q4 | Best `max_depth=4`, test precision **0.62917** | Matches option (not independently recomputed) |
| Q5 | Exploratory | Draft below |

Important: numerical data above comes from a pasted Colab output, not direct access to the executed .ipynb. The runnable notebook itself has not been uploaded to this repository. Follow-up audit has **confirmed the TEST-only filter**: 136,842 TRAIN, 32,287 VALIDATION and 33,163 TEST rows; unique correct by split is 4,104 TRAIN, 0 VALIDATION and 1,243 TEST. The discrepancy therefore **cannot be fixed by changing the TEST mask alone**. Recheck the fitted tree, preprocessing, feature set and row alignment (see temporary audit file). Latest diagnostics: index is unique, no target/pred5 NaNs, 15,831 pred5 correct on TEST, 2,374 TEST rows where all five manual rules are wrong, and 1,243 intersections. VALIDATION has zero all-hand-wrong rows **because pred3 is always 1 and pred4 is always 0 there**. With a binary target, one of those two rules must be correct. The actual model object candidates include `clf_10` and `tree10`; checking `clf10` without the underscore did not establish a missing model. The remaining audit focuses on model parameters, training samples, and feature/prediction alignment.

## Q5 — Exploratory response (proposed)

I would add VIX and changes in trading volume as new features. A higher VIX can mean more market uncertainty, which may reduce the chance of positive returns. A big change in trading volume can show stronger buying or selling activity. These features may help the model predict if a stock will have a positive return in the next 30 days.

The homework asks to choose the best depth using TEST precision. That follows the assignment, but is not a clean out-of-sample methodology for real-world model selection; tune on validation data and reserve TEST for one final check in future projects.
