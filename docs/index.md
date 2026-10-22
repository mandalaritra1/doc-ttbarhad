# TTbarHadronic documentation

Search for heavy $t\bar t$ resonances (Z′) in the fully hadronic final state with the
Run-3 data of 2022–2026 (307.5 fb⁻¹, 13.6 TeV). Both boosted tops are reconstructed as AK8
jets and tagged with GloParT-v3; the QCD background is estimated from data with a
Pass/Fail transfer function (2DAlphabet), SM $t\bar t$ from simulation.

## Pages

- [Introduction](analysis/introduction.md)
- [Phase space](analysis/phase-space.md)
- [Datasets](analysis/datasets.md)
- [Selections](analysis/selections.md)
- [Data/MC validation](analysis/datamc-validation.md)
- Corrections and uncertainties: [Systematic uncertainties](analysis/uncertainties.md),
  [Top-pT reweighting](analysis/top-pt-reweighting.md),
  [Top-tag scale factors](analysis/top-tag-scale-factors.md)
- [Background estimation](analysis/background-estimation.md)
- [Results](analysis/results.md)
- [Meetings](meetings/recents.md)
- [Archive](archive/index.md): superseded pages, including the June 2024-only analysis

## Snapshot (2026-10-22)

| Area | State |
| --- | --- |
| Data | 2022–2026: 34.65 + 27.24 + 109.95 + 110.37 + 25.31 = 307.5 fb⁻¹ |
| Skimmer | v1.2.1 (2026-10-03) |
| Topology | $H_T > 1400$ GeV; 2 AK8 jets $p_T > 400$ GeV, $|y| < 2.4$, Tight ID, $|\Delta\phi| > 2.1$; jet veto map |
| Top tagger | GloParT-v3 TopvsQCD, tight 0.9284 (0.5% mis-tag); Fail band 0.8571–0.9284; $f_{lep} < 0.59$ |
| Categories | Pass / Fail × central ($|\Delta y| < 1$) / forward, per year |
| Background | QCD from data, constant TF per year-region (provisional); $t\bar t$ from MC with measured top-tag SFs |
| Uncertainties | 39 nuisances |
| Result | Blinded expected reach 4.61 / 5.88 / 7.40 TeV (1 / 10 / 30% width) against the Run-2-equivalent theory line, provisional |
| Open | $t\bar t$ normalisation and top-$p_T$; TF order; 2022/23 signal and SF transfer; PDF Hessian |

## Notes for presenters

- Keep these pages collaboration-facing; personal debugging notes belong elsewhere.
- Nothing observed goes on this site: blinded expected results only.
- When adding plots, use stable filenames and short captions that say what changed.

## History

| Date | Change |
| --- | --- |
| 2026-06-29 | Last update of the June 2024-only site ([archived front page](archive/2026-06/index.md)) |
| 2026-10-22 | Refreshed to the Run-3 2022–2026 state; June pages moved to the [archive](archive/index.md); corrections and uncertainties section added. Pre-refresh state: git tag `pre-refresh-2026-10-22` |
