# BioTemporal-HAR

## Performance Diagnosis

The full WISDM notebook currently trains activity recognition with a strict
subject-held-out split. To reproduce the stronger feature baseline found during
diagnosis, run:

```powershell
py -3.14 src\improved_feature_baseline.py
```

This loads the cached fused windows, prints split diagnostics, trains enhanced
feature baselines, and writes `artifacts/improved_feature_baseline.csv`.

The notebook also evaluates a validation-selected activity ensemble on top of
that baseline: RF/ExtraTrees/BioTemporal probability blending plus temporal
smoothing. Identity recognition is reported with enrollment/probe cosine
matching; the strongest protocol uses predicted-activity-conditioned centroids,
which keeps the subject-held-out identity task realistic without using true
test activity labels.

For reporting, the original `RF+ExtraTrees vote + temporal smoothing` result is
kept as the fixed baseline reference. Newer Viterbi-smoothed feature models and
weighted/classwise blends are reported as stronger candidates rather than
silently moving the baseline.

For identity, the report separates the proposal-aligned BioTemporal-HAR encoder
embedding from the handcrafted Feature PCA reference. The notebook now evaluates
raw, activity-conditioned, PCA-whitened, and train-only LDA-projected
BioTemporal embeddings, then writes `artifacts/identity_method_comparison.csv`
and `artifacts/identity_method_comparison.png` for side-by-side comparison with
the Feature PCA result.

The current-result notebook is preserved at
`notebooks/BioTemporal_HAR_WISDM_full.ipynb`. Further activity-recognition
experiments centered on BioTemporal-HAR embeddings are in
`notebooks/BioTemporal_HAR_WISDM_full_biotemporal_activity_plus.ipynb`.
The change interpretation document is
`artifacts/result_interpretation_changes_report.md`.
