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

## New diagnostic findings (user-run Colab)

- Index unique: True
- NaN in target/pred5: zero in all splits
- TEST: 33,163 rows, 18,906 positive targets, 8,628 pred5 positives, 15,831 pred5 correct, 2,374 with all hand rules wrong, **1,243 uniquely correct**
- TRAIN: 136,842 rows, 4,629 all hand rules wrong, 4,104 uniquely correct
- VALIDATION: 32,287 rows, **zero all-hand-wrong**, 25,030 pred5 correct, zero uniquely correct
- A variable named `clf10` is not present in the current runtime. This **does not prove the tree is missing or incorrect**. It may have another name.

**Interpretation:** TEST filtering and absent values do not explain the mismatch. Zero all-hand-wrong cases in VALIDATION suggests checking the rule prediction construction and possible complementarity. This is not enough evidence to infer the correct multiple-choice option.

### Next minimal diagnostic

Run one cell only; no training/download needed:

```python
import pandas as pd
d=new_df
spl=d["split"].astype(str).str.strip().str.upper()
manual=["pred0_manual_cci","pred1_manual_prev_g1",
        "pred2_manual_prev_g1_and_snp","pred3_manual_dgs10_5",
        "pred4_manual_dgs10_fedfunds"]
print("Manual prediction column values:")
for c in manual:
    print(c, d.groupby(spl)[c].agg(['min','max','nunique','mean']).to_string())
print("\\nPairwise manual pattern counts by split:")
for part in ["TRAIN","VALIDATION","TEST"]:
    piece=d.loc[spl.eq(part),manual]
    print(part, piece.astype(str).agg(''.join,axis=1).value_counts().head(8).to_dict())
print("\\nModel variable candidates:",
      [k for k,v in globals().items()
       if not k.startswith('_') and hasattr(v,'get_params') and
       hasattr(v,'predict')][:25])
```

**Next step after this output:** compare rule expressions and model `fit`/predict feature list in the user's actual Colab to the official 2026 notebook. Do not select 1171 or 1571 based only on closeness.

## Previous diagnostic — already run

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
