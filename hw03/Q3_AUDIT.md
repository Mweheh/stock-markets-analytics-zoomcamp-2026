# HW3 Q3 — Discrepancy audit

**Reported Colab count: 1243. Official options: 271, 571, 1171, 1571. Do not choose an option yet.**

The official specification requires:
1. Exactly the **TEST** records (not TRAIN/VALIDATION and not all rows).
2. A decision tree of depth 10 and `random_state=42`, trained on the combined TRAIN+VALIDATION rows.
3. `pred5_clf_10` predicted on the entire set in the same row order.
4. `pred5_clf_10 == is_positive_growth_30d_future`.
5. Every **pred0 through pred4** must differ from the target on that same row.

Paste the following **diagnostic cell** after the HW3 cells that produce `new_df`. It deliberately stops if the TEST selection is ambiguous, rather than silently counting the wrong rows.

```python
import pandas as pd

audit_df = new_df
TARGET = "is_positive_growth_30d_future"
assert TARGET in audit_df.columns, "Target missing from new_df"

# Discover the exact predictor column names from your existing notebook.
pred_cols = {}
for i in range(6):
    choices = [c for c in audit_df.columns
               if c == f"pred{i}" or c.startswith(f"pred{i}_")]
    print(f"pred{i}: {choices}")
    if len(choices) != 1:
        raise ValueError(f"Expected one pred{i} column, got {choices}")
    pred_cols[i] = choices[0]

print("Rows in ALL data:", len(audit_df))
print("Possible split columns:",
      [c for c in audit_df.columns
       if any(k in c.lower() for k in ("split", "subset", "sample", "dataset", "partition", "set"))])

# Set TEST_MASK using the actual TEST indicator/split from the notebook.
# Example ONLY, if the notebook has a 'split' column:
# TEST_MASK = audit_df["split"].astype(str).str.upper().eq("TEST")
# Do not replace this with a guessed row fraction or reset index.

assert "TEST_MASK" in globals(), (
    "Define TEST_MASK from the notebook's actual TEST rows before counting"
)
assert len(TEST_MASK) == len(audit_df)
assert pd.Series(TEST_MASK, index=audit_df.index).notna().all()
TEST_MASK = pd.Series(TEST_MASK, index=audit_df.index).astype(bool)

target = audit_df[TARGET].astype(int)
p5_correct = audit_df[pred_cols[5]].astype(int).eq(target)
hand_wrong = pd.concat(
    [audit_df[pred_cols[i]].astype(int).ne(target) for i in range(5)],
    axis=1,
).all(axis=1)

print("TEST rows:", TEST_MASK.sum())
print("Tree correct on TEST:", (TEST_MASK & p5_correct).sum())
print("All five hand rules wrong on TEST:", (TEST_MASK & hand_wrong).sum())
print("Q3 unique correct on TEST:", (TEST_MASK & p5_correct & hand_wrong).sum())
print("Unique correct on ALL rows:", (p5_correct & hand_wrong).sum())
```

**If TEST_MASK is unclear**, print the split-related column names and `new_df.head()` and inspect how `X_test` was built in the original notebook. Do not select 1171 simply because it is numerically closer to 1243. A mismatch can arise from filtering, row/index alignment, a missing hand rule, incorrect tree training data, or a TEST-mask error; none is proven from the pasted aggregate outputs alone.

To preserve the actual implementation on GitHub: in the *executed* Colab choose File → Download → Download .ipynb and add that file under `hw03/`. No downloaded notebook has been synthesized here.
