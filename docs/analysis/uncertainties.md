# Systematic uncertainties

Every nuisance parameter in the Run-3 (2022–2026) 2DAlphabet fit, what it varies,
where the template comes from, and how it is correlated across years. The model is
the one in bgestimation branch `run3-v121` (commit `9567d35`, 2026-10-13) on the
skimmer v1.2.1 inputs.

Sources: `run3/run3comb/README.md` and `run3/run3comb/make_run3_configs.py` in
bgestimation; `python/weights.py`, `python/corrections.py`, `python/jets.py`,
`ttbarprocessor.py` and `plots/make2Drootfiles.py` in the skimmer (line numbers at
`81fbb7b`).

## How a template is built

1. The skimmer fills the `mtt_vs_mt` histogram (jet0 $m_{SD}$ × $m_{t\bar t}$) once per
   entry on its `systematic` axis. Weight variations (`pileup`, `pdf`, `q2`, `isr`,
   `fsr`, `toppt`, every `ttag_*`) are coffea `Weights` up/down slots
   (`python/weights.py:51-105`). Jet variations (`jes`, `jer`, `jms`, `jmr`) are
   separate passes of the processor with a modified jet collection
   (`python/jets.py:72-149`).
2. `pdf`, `q2`, `isr` and `fsr` are normalised to the generator-level nominal sum of
   weights before any selection (`ttbarprocessor.py:1335-1360`, commit `5bcdf08`),
   so they carry acceptance and shape only. All other variations keep their rate
   effect.
3. `plots/make2Drootfiles.py` writes one TH2 per region and variation,
   `MttvsMt{Cen,Fwd}<yy>{Pass,Fail}<SYST>{up,down}` (`:367`). `JMRdown` is built
   there as $2\,\mathrm{nominal} - \mathrm{up}$ (`:306-320`). Negative bins of MC
   templates are clipped to zero (`:323-339`).
4. `run3/run3comb/stage_run3_inputs.py` sums 2022 preEE + postEE and 2023 preBPix +
   postBPix into one channel per calendar year, so every shape systematic is fully
   correlated between the two halves of a year.
5. In the configs every shape nuisance is `CODE 2` (template morphing, `SIGMA 1`);
   luminosity, $t\bar t$ rate and the SF year transfer are `CODE 0` (lnN).

The 2025 MC is the 2024 MC scaled by luminosity, so 2025 shares the 2024 templates.
The 2026 MC is the Summer24 sample run with the 2026 JEC, pileup and top-tag drift.

## Glossary

"Both" = $t\bar t$ (TTto4Q) and the Z′ signal. Correlation is across years unless
stated.

### Normalisation

| Nuisance | Type | Processes | Correlation | Value and origin |
| --- | --- | --- | --- | --- |
| `lumi22` | lnN | both | 2022 only | 1.4% (LUM-22-001) |
| `lumi23` | lnN | both | 2023 only | 1.3% (LUM-23-001) |
| `lumi24`, `lumi25`, `lumi26` | lnN | both | one per year | 2% each, placeholder until the luminosity papers |
| `ttbar_xsec<yy>` | lnN | $t\bar t$ | one per year | 30%. An empirical per-year $t\bar t$ rate, not a cross-section uncertainty |
| `ttag_yr22`, `ttag_yr23` | lnN | signal | one per year | 25% (about 12% per tagged jet). The 2024 top-tag SFs used on Summer22/23 MC, with no year-transfer measurement. Provisional |

Luminosities in the skimmer (`python/lumi.py`): 2022 7.98 + 26.67, 2023 17.794 +
9.451, 2024 109.95, 2025 110.37, 2026 25.31 fb⁻¹. The $t\bar t$ cross-section is
419.9 pb (924 pb × 0.4544 all-hadronic BR, `data/nanoAOD/xsec_pb.json`,
`python/rescale_xsec.py`).

### Jet energy and jet mass

| Nuisance | Processes | Correlation | Variation | Code |
| --- | --- | --- | --- | --- |
| `jes22`, `jes23`, `jes2425`, `jes26` | both | per MC campaign | JES `Total` source of the JME payload, ±1σ, on AK8 jets, AK4 jets ($H_T$) and the soft-drop subjets. $m_{SD}$ is rebuilt from the varied subjets | `corrections.py:238-300`, `jets.py:121-125` |
| `jer22`, `jer23`, `jer2425`, `jer26` | both | per MC campaign | JER scale factor up/down in the coffea smearing, same three collections | `jets.py:126-130` |
| `jms` | both | all years | $m_{SD} \times (1 \pm 0.01)$ on AK8 jets. Nominal JMS = 1 | `jets.py:10, 131-135` |
| `jmr` | both | all years | Up: Gaussian smearing $m_{SD} \times (1 + 0.02\,\mathcal{N}(0,1))$, seeded from the event numbers. Down: mirrored, $2\,\mathrm{nom} - \mathrm{up}$. Nominal JMR = 1 | `jets.py:11, 136-147`, `make2Drootfiles.py:306` |

JME payloads per IOV are in `corrections.py:30-44`: Summer22, Summer22EE, Summer23,
Summer23BPix, Summer24Prompt24 (V5 JEC, JRV2 JER) for 2024 and 2025 MC, and
Summer24Prompt26 (V1 JEC, JRV1 JER) for 2026. The groups follow `JEC_GROUP` in
`make_run3_configs.py:43`: the 2025 templates are the 2024 ones, so 2024 and 2025
share one JES and one JER nuisance.

### Event weights

| Nuisance | Processes | Correlation | Variation | Code |
| --- | --- | --- | --- | --- |
| `pileup` | both | all years | Up/down of the pileup weight (minimum-bias cross-section 69.2 mb → 72.4 / 66.0 mb). LUM payloads for 2022–2025; derived data/Summer24 weights for 2026 and for the 2022/23 Z′ (Summer24 stand-in) | `corrections.py:331-388` |
| `pdf` | both | all years | $1 \pm \mathrm{std}/\mathrm{mean}$ over the `LHEPdfWeight` members (replica-style), normalised | `corrections.py:303-328` |
| `q2` | both | all years | Envelope (max/min) of the six $\mu_R,\mu_F$ variations, the two anti-correlated ones dropped, normalised | `corrections.py:529-559` |
| `isr` | both | all years | `PSWeight` ISR shower scale ×2 (up) / ×0.5 (down), normalised | `corrections.py:575-590` |
| `fsr` | both | all years | Same for FSR | `corrections.py:575-590` |
| `toppt` | $t\bar t$ | all years | Up = no top-pT reweighting, down = $2w - 1$. Not normalised. See [Top-pT reweighting](top-pt-reweighting.md) | `weights.py:57-65` |

### Top tagging

All top-tag nuisances act only on jets gen-matched as fully merged tops, except
`ttag_nonmerged`. Every year uses the SFs of the one 2024 tag-and-probe, so these
are correlated across years. Details on [Top-tag scale factors](top-tag-scale-factors.md).

| Nuisance | Processes | Correlation | What it varies | Code |
| --- | --- | --- | --- | --- |
| `ttag_pt1`, `ttag_pt2`, `ttag_pt3` | both | all years | Total SF uncertainty in pT bin 400–480 / 480–600 / ≥ 600 GeV. Tight SF (jet0, tagged jet1) and band SF (antitag jet1, below 600 GeV) of the same bin move together. Doubled above 1.2 TeV | `weights.py:239-291` |
| `ttag_band_hipt` | both | all years | Interim band SF for antitag jets ≥ 600 GeV: 1.50 ± 0.30 (600–800), 1.00 ± 0.50 (≥ 800). Fail only | `weights.py:274-302` |
| `ttag_nonmerged` | both | all years | ±25% per tagged or antitag jet that is not a fully merged top (those keep SF 1) | `weights.py:213-237` |
| `ttag_flep_tag` | both | all years | SF of the $f_{lep} < 0.59$ cut for tight jets, per pT bin, correlated across pT and between jets | `weights.py:107-155` |
| `ttag_flep_band` | both | all years | Same for the antitag jet (one inclusive value) | `weights.py:107-155` |
| `ttag_drift_pt1`, `_pt2`, `_pt3` | both | 2026 only | 2026/2024 tight-tag efficiency ratio, one per pT bin. Doubled above 1.2 TeV | `weights.py:157-211` |
| `ttag_drift_band` | both | 2026 only | 2026/2024 band ratio, one value for all pT | `weights.py:157-211` |

### Background transfer function

The QCD multijet background has no template. In each year and category
(`Cen<yy>`, `Fwd<yy>`) the Fail-region QCD is one free parameter per bin
(`BinnedDistribution` `QCD_<region>Fail`, seeded with data minus $t\bar t$, optionally
floored at zero with `--floor-qcd`), and the Pass-region QCD is Fail × $R_{P/F}(x, y)$
(`ttbar.py:541-602` at `run3-v121`). The
transfer function is

$$R_{P/F}(x, y) = 0.1\,\Big(p_0 + \sum_{i=1}^{n_x} p_i\, x^i\Big)\Big(1 + \sum_{j=1}^{n_y} p_{n_x + j}\, y^j\Big),$$

with $x$ ($m_{SD}$) and $y$ ($m_{t\bar t}$) scaled to [0, 1]. Parameters are
`QCD_<region>rpf_par<i>`, bounded to $p_0 \in [0.001, 50]$ and $p_{i>0} \in [-50, 50]$
(`ttbar.py:348-458`). Orders in `jsons/TransferFunctions.json`: cen 2×2 (5
parameters), fwd 2×0 (3 parameters) in every year. A per-year dict is supported and
`ftest_select.py` chooses orders from the F-test.

Fit binning (`jsons/config/ttbar_cen2425.json`): $m_{SD}$ edges 25, 60, 105, 130, 150,
170, 175, 180, 185, 190, 210, 475 GeV, signal window 105–210 GeV masked in Pass in the
background-only fit; $m_{t\bar t}$ edges 800, 1100, then 200 GeV steps to 3700, last
bin 3700–6500 GeV.

## Count

39 nuisances in the five-year card: 20 per-year (5 lumi, 5 $t\bar t$ rate, 4 JES,
4 JER, 2 SF year transfer), 15 correlated (14 in `CORRELATED` plus `toppt`) and 4
drift. The TF and Fail-bin parameters come on top. The 2024 + 2025 fit before the
Run-3 model had 9 nuisances: `lumi24`, `lumi25`, `ttbar_xsec`, `jes`, `jer`,
`pileup`, `pdf`, `q2`, `ttag_pt1`.

## Open issues

- **PDF prescription.** `std/mean` over the members is the replica formula; the set
  needs the Hessian sum, which is about 10× larger (skimmer changelog item 8, parked
  to the end).
- **JES.** One `Total` source per campaign. The source split and its year correlations
  are not done.
- **Luminosity.** 2024–2026 at 2%, uncorrelated, as placeholders.
- **Template statistics.** No MC statistical uncertainty: `autoMCStats` does not work
  with the 2D pdfs.
- **Top-tag correlations.** The fit covariance between the tight and band SF is not
  used; the 2022/23 transfer is a flat 25% on the signal only.
- **JMR down.** The mirrored template goes negative in sparse bins and is clipped
  (about 700 bins per signal region in v1.2).
- **$t\bar t$ normalisation.** The $t\bar t$ MC is above the data in the sidebands, so
  `ttbar_xsec<yy>` absorbs a mis-modelling. A $t\bar t$ control-region constraint is
  the proper fix. See [Top-pT reweighting](top-pt-reweighting.md#open-decision).
- **Sub-era sums.** Summing 2022 and 2023 halves needs a split-versus-merged closure
  check.
- **Residual leptonic $t\bar t$.** After the $f_{lep}$ cut, TTtoLNu2Q and TTto2L2Nu
  are not in the $t\bar t$ template; their share (about 4% of TTto4Q in Fail, 2% in
  Pass) is to be covered by a TTto4Q normalisation uncertainty, not yet added.

## Detailed studies

- Skimmer `CHANGELOG_v1.1_to_v1.2.md`: items 1, 8–10, 17, 18, 31–40 and the decision log.
- bgestimation `run3/run3comb/README.md` (pipeline, correlation model, known limitations)
  and `run3/run3comb/REVIEW_BRIEF_run3comb.md`.
- ai-wiki `wiki/repos/ttbarhadronic_skimmer.md`, `wiki/repos/ttbarhadronic_bgestimation.md`.
- Methods deck "New methods in skimmer v1.2.1", 2026-10-04 (claude.ai artifact
  `WQYNFWgY6oRbpUYX42K3L6`).

## History

| Date | Change | Commit |
| --- | --- | --- |
| 2026-07-31 | First measured 2024 top-tag SFs replace the flat placeholder | skimmer `32cdded` |
| 2026-09-24 | `ttag_pt2`, `ttag_pt3` reach the templates (were lost in the axis overflow, only `TTAG_PT1` existed) | `d664e04` |
| 2026-09-24 | `jms` 1%, `jmr` 2%; JES/JER propagated to $m_{SD}$ | `d3fbc3e` |
| 2026-09-24 | $m_{SD}$ rebuilt from re-corrected subjets | `e922747` |
| 2026-09-24 | `q2`, `pdf` normalised to the generator-level sum of weights | `5bcdf08` |
| 2026-09-24 | `isr`, `fsr` added | `13dc1e3` |
| 2026-09-24 | Top-tag SF uncertainty doubled above 1.2 TeV | `571526c` |
| 2026-10-01 | Top-tag SFs on fully merged tops only; `ttag_nonmerged` | `e975cdc`, `f3c1965` |
| 2026-10-02 | `ttag_band_hipt`, `ttag_flep_tag/band`, `toppt` | `6ed19bc`, `c34dbd1`, `4b2a2b6` |
| 2026-10-02/03 | Derived pileup weights for the 2022/23 Z′ and for 2026 | `c7c1365`, `d939c67` |
| 2026-10-03 | `ttag_drift_*` for 2026 | `5de1df9` |
| 2026-10-04 | Run-3 correlation model (per-year rate, JES/JER per campaign, `ttag_yr22/23`) | bgestimation `011ad5c` |
