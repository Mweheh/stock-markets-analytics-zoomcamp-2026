# HW3 Q3 — TEMPORARY discrepancy audit

**Delete this file after HW3 Q3 is resolved and validated.** It is not a homework deliverable.

## Facts established by actual user-run Colab

```text
split          rows   unique pred5 correct, all pred0..pred4 wrong
TRAIN        136842   4104
VALIDATION    32287   0
TEST          33163   1243
ALL          202292   5347
```

Official multiple-choice values: **271, 571, 1171, 1571**. None is supported by the current 1243 count.

**Confirmed:** TEST-only mask using `new_df['split']` is correct. Previous `TEST_MASK` assertion was an incomplete instruction, not a model bug. **Not confirmed:** the stored tree predictions were produced with the required training data, feature columns, target, indexing and seed, or why the VALIDATION count is exactly zero.

## Next diagnostic — paste ONE cell after original 2026 notebook execution

No package install, retraining, or data download. This reports whether VALIDATION labels, feature predictions, or row alignments are unusual.

```python
import pandas as pd
from sklearn.metrics import precision_score

d = new_df
target = 'is_positive_growth_30d_future'
hand_cols = ['pred0_manual_cci','pred1_manual_prev_g1',
             'pred2_manual_prev_g1_and_snp','pred3_manual_dgs10_5',
             'pred4_manual_dgs10_fedfunds']
p5 = 'pred5_clf_10'
group = d['split'].astype(str).str.strip().str.upper()
y = d[target]
manual_wrong = d[hand_cols].ne(y, axis=0).all(axis=1)
tree_correct = d[p5].eq(y)
print('Row labels:',d.index.is_unique)
print('Target NaNs per split:', y.isna().groupby(group).sum().to_dict())
print('Pred5 NaNs per split:',d[p5].isna().groupby(group).sum().to_dict())
print('Q3 diagnostics:')
print(pd.DataFrame({
    'rows':group.value_counts().sort_index(),
    'positive_target':y.groupby(group).sum(),
    'pred5_positive':d[p5].groupby(group).sum(),
    'pred5_correct':tree_correct.groupby(group).sum(),
    'all_five_manual_wrong':manual_wrong.groupby(group).sum(),
    'unique_correct':(tree_correct & manual_wrong).groupby(group).sum()
}).to_string())
print('Prediction agreement with label by split:')
print(pd.DataFrame({
    c: d[c].eq(y).groupby(group).mean()
    for c in hand_cols + [p5]
}).round(4).to_string())
print('Existing model object:', 'clf10' in globals())
if 'clf10' in globals():
    print('clf10 max_depth:',clf10.get_params().get('max_depth'))
    print('clf10 random_state:',clf10.get_params().get('random_state'))
    print('clf10 feature count:',getattr(clf10,'n_features_in_',None))
```

Then compare how `clf10` was actually fitted, the complete feature list (including 60 `month_wom` dummies), and the row ordering of its predictions. Do not infer the correct answer from the options.
