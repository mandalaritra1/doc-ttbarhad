# TTbarHadronic documentation

Documentation for the search for heavy $t\bar{t}$ resonances ($Z'$, KK-gluon) in
the **fully hadronic** final state with Run-3 **2024** data (109.95 fb⁻¹,
13.6 TeV). Both boosted tops are reconstructed as AK8 jets; the QCD background is
estimated from data with a pass/fail transfer function (2DAlphabet).

## Current Flow

- [Introduction](analysis/introduction.md)
- [Phase Space](analysis/phase-space.md)
- [Datasets](analysis/datasets.md)
- [Selections](analysis/selections.md)
- [Data/MC Validation](analysis/datamc-validation.md)
- [Background Estimation](analysis/background-estimation.md)
- [Results](analysis/results.md)

## Recents

- [Meetings and progress tracker](meetings/recents.md)

## Analysis Snapshot (2024)

| Area | State |
| --- | --- |
| Dataset | 2024, 109.95 fb⁻¹ (13.6 TeV) |
| Topology | $H_T > 1500$ GeV; 2 AK8 jets $p_T>400$ GeV, $\|\Delta\phi\|>2.1$ |
| Top tagger | GloParT-v3 TopvsQCD, medium WP 0.8571 |
| Categories | double-tag pass / antitag fail × central ($\|\Delta y\|<1$) / forward |
| Background | QCD from data (2DAlphabet, TF order `2x1`); $t\bar{t}$ from MC |
| Goodness of fit | passing — central $p=0.485$, forward $p=0.180$ (blinded) |
| Result | blinded expected exclusion **~5.3 TeV** ($Z'$, 1% width) |
| Pending | measured 2024 top-tag SF, full systematics, unblinding |

## Notes For Presenters

- Keep these pages focused on collaboration, review, and meeting updates.
- Move personal debugging notes and low-level implementation details elsewhere.
- When adding plots, prefer stable filenames and short captions that say what changed.
