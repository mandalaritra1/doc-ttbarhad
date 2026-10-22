# Results

!!! info "Status"
    <!-- STATUS-LINE-PENDING: wording to be chosen by Aritra -->
    Blinded expected limits only. Provisional: the transfer function is constant in every
    year-region and the $t\bar t$ normalisation is still open.

## Run-3 blinded expected limits (2022–2026)

Expected 95% CL upper limits on $\sigma(pp \to Z') \times \mathcal{B}(Z' \to t\bar t)$ from
the five-year 2DAlphabet fit, built from the masked background-only fit snapshot
(`AsymptoticLimits --run blind`).

| Item | Value |
| --- | --- |
| Data | 2022–2026, 307.5 fb⁻¹, 13.6 TeV |
| Inputs | skimmer v1.2.1, EOS `2dAlphabetInputs/v1.2.1/run3_combined/` |
| Model | 20 regions, constant TF per year-region, 39 nuisances ([Background estimation](background-estimation.md)) |
| Signal | Z′ → $t\bar t$, $\Gamma/M$ = 1, 10, 30%; 1.2–6 TeV (campaign `lim_run3_min`) and 7–9 TeV (`lim_run3_min_hm`) |
| Fit gates | all points pass (36/36 at 1.2–6 TeV, 9/9 at 7–9 TeV) |

Above the end of the $m_{t\bar t}$ axis (6.5 TeV) only the in-range, mostly off-shell part
of the signal enters: 85–93% of the selected 1%-width signal at 7–9 TeV (2024), ≥ 98% for
10% and 30%.

### Expected mass reach

| Expected reach [TeV] | 1% | 10% | 30% |
| --- | --- | --- | --- |
| **Run 3 0ℓ, Run-2-equivalent theory** | **4.61** | **5.88** | **7.40** |
| Run 2 0ℓ expected (B2G-25-009) | 4.27 | 5.58 | 6.48 |
| Run 2 0ℓ + 1ℓ + 2ℓ expected (B2G-25-009) | 4.41 | 5.98 | 7.16 |

Provisional until the $t\bar t$ normalisation and the transfer function are validated.

### Theory line

The reach is quoted against a Run-2-equivalent Z′ cross section at 13.6 TeV: the
B2G-25-009 theory curve from HEPData (record ins3134005, Fig. 15a/b/c, 13 TeV) times
$\sigma(13.6)/\sigma(13)$ per mass and width from MadGraph LO topcolor scans at both
energies (same couplings and PDF, so the coupling normalisation cancels). The ratio is
1.09–1.30 for 1% width and at most 1.20 / 1.16 for 10% / 30%. An analytic Breit–Wigner ×
parton-luminosity check agrees to 1–3%.

File: bgestimation `run3/run3comb/theory/theory_run2equiv_136tev.json` (builder
`build_run2equiv_theory.py`), the default of `jsons/signal_xs.json` since 2026-10-13. The
older `signal_xs.json` line mixed conventions (1% = XSDB 13.6 TeV LO, 30% = unscaled Run-2
13 TeV) and gave 4.25 / 5.58 / 7.03 TeV with the same limits.

The Run-2 benchmark is about 5× our MadGraph topcolor cross section (5.1 / 5.5 / 5.9 for
1 / 10 / 30%, ±10% in mass); the cause is not understood, and the overlay-anchoring question
was closed on 2026-10-13 by using the Run-2 curve directly.

### Figures

Limit plots exist (`plot_limits_mpl.py --year Run3 --lumi 307.5 --theory-json
run3/run3comb/theory/theory_run2equiv_136tev.json`, research-notes
`ttbarhadronic_run3_reach_run2_equivalent_theory`) and will be added here once approved
for this site.

## What is still open

- $t\bar t$ normalisation and top-$p_T$ treatment ([Top-pT reweighting](top-pt-reweighting.md#open-decision)).
- Transfer function beyond a constant; toy calibration of the F-test; TF positivity.
- Coverage at 30% width, 4 TeV (Feldman–Cousins intervals).
- 2022/23 signal from an official NanoAODv15 sample; 2022/23 top-tag SF transfer.
- PDF Hessian prescription, JES sources, template statistics, luminosity correlations.

## History

| Date | Result | Reference |
| --- | --- | --- |
| 2026-06-18 → 06-25 | 2024 blinded expected limits, 1% reach 4.84 TeV (XSDB line), then 2025 and 2024 + 2025, on inputs with a stale loose WP, a 0.90/tag placeholder SF and 2 nuisances. Superseded 2026-07-23 | [June results](../archive/2026-06/results.md) |
| 2026-08-14 | 2024 + 2025 tight WP, fine grid 1.2–7 TeV × 3 widths, 9 nuisances, cen 2×2 / fwd 2×0; expected reach 4.66 (1%) / 6.16 TeV (10%) against `signal_xs.json` | research-notes `ttbarhadronic_tightwp_2425_finegrid_result`, bgestimation `1085d9a` |
| 2026-10-04 | Run-3 2022–2026, constant TF, 39 nuisances; 4.25 / 5.58 / 7.03 TeV against `signal_xs.json` | bgestimation `011ad5c` |
| 2026-10-08 | 7–9 TeV points added; blinded validation | tracker `run3-7to9tev`, `run3-validation` |
| 2026-10-13 | Run-2-equivalent theory line: 4.61 / 5.88 / 7.40 TeV | bgestimation `2dd07f4`, `9567d35` |
