# HW3 Q3 — Temporary audit (remove after resolution)

**Open issue:** Colab Q3 = **1243** on **33,163 TEST** records, but form choices are **271 / 571 / 1171 / 1571**. **Do not submit a guessed choice.**

## Verified from user-run Colab output

| Split | Rows | Only pred5 correct |
| --- | ---: | ---: |
| TRAIN | 136842 | 4104 |
| VALIDATION | 32287 | 0 |
| TEST | 33163 | 1243 |

- No missing target or pred5 values; DataFrame row index is unique.
- TEST: 15,831 correct tree predictions; 2,374 observations with all five manual rules wrong; intersection 1,243.
- **VALIDATION = 0 is explained, not suspicious.** On every VALIDATION row, `pred3_manual_dgs10_5=1` and `pred4_manual_dgs10_fedfunds=0`. Since the target is binary, one of these two is always correct, so all five manual rules can never be simultaneously wrong.
- The notebook's model candidates are `model`, `clf_20`, `clf_10`, `best_model`, `tree10`. The earlier check for `clf10` (without underscore) was an incorrect variable-name assumption.

## Next check: identify the model behind pred5

Paste **one diagnostic cell** in the existing executed notebook. No retraining, re-downloading, or new packages. It checks model depth/seed/training-row count and, **only if the existing** `X_all` **matches**, whether candidate models reproduce the stored `pred5_clf_10` and Q3 count.

```python
import numpy as np
from sklearn.tree import DecisionTreeClassifier

target = "is_positive_growth_30d_future"
manual = ["pred0_manual_cci", "pred1_manual_prev_g1",
          "pred2_manual_prev_g1_and_snp",
          "pred3_manual_dgs10_5",
          "pred4_manual_dgs10_fedfunds"]
test = new_df["split"].astype(str).str.strip().str.upper().eq("TEST").to_numpy()
y = new_df[target].to_numpy()
all_wrong = new_df[manual].ne(new_df[target], axis=0).all(axis=1).to_numpy()
stored = new_df["pred5_clf_10"].to_numpy()

print("Expected TRAIN+VALIDATION rows:", int((~test).sum()))
print("Stored Q3:", int(np.sum(test & all_wrong & (stored == y))))

X = globals().get("X_all")
if X is not None:
    print("X_all shape:", X.shape)
for name in ("clf_10", "tree10", "model", "best_model", "clf_20"):
    obj = globals().get(name)
    if not isinstance(obj, DecisionTreeClassifier) or not hasattr(obj, "tree_"):
        continue
    print("\\nModel:", name,
          "depth param:", obj.get_params()["max_depth"],
          "seed:", obj.get_params()["random_state"],
          "training samples:", obj.tree_.n_node_samples[0],
          "features:", obj.n_features_in_)
    if X is None or len(X) != len(new_df) or X.shape[1] != obj.n_features_in_:
        continue
    if hasattr(obj, "feature_names_in_") and hasattr(X, "columns"):
        if list(obj.feature_names_in_) != list(X.columns):
            print("SKIP: X_all feature columns/order differ")
            continue
    try:
        pred = obj.predict(X)
        print("Matches stored pred5:", float(np.mean(pred == stored)))
        print("Unique correct TEST count:", int(np.sum(test & all_wrong & (pred == y))))
    except (ValueError, TypeError) as exc:
        print("SKIP prediction:", str(exc)[:180])
```

If `X_all` is unavailable or candidate predictions do not match, examine the original cell that produces `pred5_clf_10`: exact `DecisionTreeClassifier(max_depth=10, random_state=42)`, TRAIN+VALIDATION fitting, categoricals including `month_wom`, and data/column order. A depth-10 tree should generally have a root training-sample count equal to the TRAIN+VALIDATION rows used for fitting, subject to preprocessing that drops rows.

*Audit-only file: delete once the correct answer has been reproduced and Q3 is validated.*
