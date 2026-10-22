# TTbarHadronic documentation

!!! warning "Archived: superseded"
    June front page (2024-only snapshot), kept unchanged for the record (site tag `pre-refresh-2026-10-22`).
    Superseded: 2026-07-23, when the June 2024 inputs were found to carry a stale loose placeholder working point; replaced by the Run-3 2022–2026 analysis (2026-10-04).
    Current version: [the current overview](../../index.md).

Documentation for the search for heavy $t\bar{t}$ resonances ($Z'$, KK-gluon) in
the **fully hadronic** final state with Run-3 **2024** data (109.95 fb⁻¹,
13.6 TeV). Both boosted tops are reconstructed as AK8 jets; the QCD background is
estimated from data with a pass/fail transfer function (2DAlphabet).

## Current Flow

- [Introduction](introduction.md)
- [Phase Space](phase-space.md)
- [Datasets](datasets.md)
- [Selections](selections.md)
- [Data/MC Validation](datamc-validation.md)
- [Background Estimation](background-estimation.md)
- [Results](results.md)

## Recents

- [Meetings and progress tracker](../../meetings/recents.md)

## Analysis Snapshot (2024)

| Area | State |
| --- | --- |
| Dataset | 2024, 109.95 fb⁻¹ (13.6 TeV) |
| Topology | $H_T > 1500$ GeV; 2 AK8 jets $p_T>400$ GeV, $\|\Delta\phi\|>2.1$ |
| Top tagger | GloParT-v3 TopvsQCD, medium WP 0.8571 |
| Categories | double-tag pass / antitag fail × central ($\|\Delta y\|<1$) / forward |
| Background | QCD from data (2DAlphabet, TF order `2x1`); $t\bar{t}$ from MC |
| Goodness of fit | passing — central $p=0.485$, forward $p=0.180$ (blinded) |
| Result | blinded expected exclusion **~4.84 TeV** ($Z'$, 1% width) |
| Pending | measured 2024 top-tag SF, full systematics, unblinding |

## Notes For Presenters

- Keep these pages focused on collaboration, review, and meeting updates.
- Move personal debugging notes and low-level implementation details elsewhere.
- When adding plots, prefer stable filenames and short captions that say what changed.
