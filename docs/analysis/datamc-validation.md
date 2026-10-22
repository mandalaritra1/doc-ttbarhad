# Data/MC validation

The analysis is blinded: the Pass top-mass window (105–210 GeV) is never compared with data.
Validation uses regions without signal sensitivity: the Fail region, the Pass $m_t$
sidebands through the masked fit, and the semileptonic $t\bar t$ tag-and-probe sample.

## What is validated and how

| Check | Region | Status |
| --- | --- | --- |
| Background model | Fail + Pass sidebands, all years | Masked saturated GoF p = 0.27 (200 toys); closure and injection, see [Background estimation](background-estimation.md#blinded-validation-2026-10-08) |
| Post-fit $m_{t\bar t}$ projections | Pass sidebands and Fail, per year and summed | Produced 2026-10-13 (bgestimation `run3/run3comb/postfit/`) |
| Top-tag efficiency | Semileptonic $t\bar t$ T&P (2024, 2025, 2026 data) | Measured SFs, 2025/2024 consistent with 1, 2026 drift applied ([Top-tag scale factors](top-tag-scale-factors.md)) |
| $f_{lep}$ cut | T&P (fully merged tops) | SF_flep 0.99–1.00, consistent with 1 |
| $f_{lep}$ cut | Fail data, 2024 | The cut removes 22.7 ± 1.1% (cen) and 21.6 ± 1.3% (fwd) of Fail data; the MC prediction is 26.4% / 21.1% (skimmer changelog, decision log 2026-10-02) |
| Top-$p_T$ spectrum | T&P probe $p_T$ (control region) | Data/MC slope after the TOP-23-008 recipe compatible with the data/NNLO residual ([Top-pT reweighting](top-pt-reweighting.md#open-decision)) |
| Jet mass scale | T&P top peak | Data/MC peak positions within 2 GeV; mass scale 0.998 profiled in the SF fit |
| Data tag rate over time | Hadronic data, per era and per run | Leading-jet tag rate drops in 2026 D; the per-run QCD tag rate shows no step in the CMSSW_16_0_6 runs (skimmer changelog item 40) |

## Diagnostic histograms in the skimmer output

`jet0_tdisc` / `jet1_tdisc` (GloParT-v3 score per category), `flep` (both jets before the
$f_{lep}$ cut; data fills Fail only), `jet_eta_phi` (jets on the veto-map cells, from
skimmer `90ce290`, not yet in the v1.2.1 files), and the kinematic set (`ttbarmass`,
`jetmsd`, `jetmsd1`, `ht`, `jetdy`, `chi`, jet $p_T$, $\eta$, $\phi$, $y$).

## Open items

- Standalone data/MC overlays on the v1.2.1 outputs (soft-drop mass, $H_T$, $m_{t\bar t}$,
  $\Delta y$, tagger score) in Fail and in the Pass sidebands, per year.
- Defined control and validation regions; the current categories are only Pass/Fail ×
  cen/fwd.
- A $t\bar t$-enriched control region for the $t\bar t$ normalisation.
- Trigger efficiency in Run-3 data.

## History

| Date | Change | Reference |
| --- | --- | --- |
| June 2026 | 2024 masked GoF (2×1, cen p = 0.485, fwd p = 0.180) on inputs later found to carry a stale WP | [June validation](../archive/2026-06/datamc-validation.md) |
| 2026-07-30 | Data/MC control plots in the T&P; WP mis-tag rates in 2024 data are 1.4–1.9× the MC target | research-notes `glopartv3_toptag_wp_data_mistag_2024` |
| 2026-08-14 | 2024 + 2025 eight-stage blinded validation (GoF, closure, injection, impacts) | research-notes `ttbarhadronic_tightwp_2425_finegrid_result` |
| 2026-09-24 | Score histograms `jet0/1_tdisc` added for CR/VR data/MC | skimmer `28c02a9` |
| 2026-10-02 | $f_{lep}$ cut validated in Fail data and in the T&P | skimmer changelog items 33, 36 |
| 2026-10-03 | `jet_eta_phi` added; `jetdy` filled with $|\Delta y|$ | skimmer changelog item 41 |
| 2026-10-08 | Run-3 five-year blinded validation | tracker `run3-validation` |
