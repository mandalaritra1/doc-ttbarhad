# Background estimation

## Method

QCD multijet production dominates every region and is taken from data with 2DAlphabet.
SM $t\bar t$ (TTto4Q) and the Z′ signal come from simulation with their templates and
nuisances ([Systematic uncertainties](uncertainties.md)).

For each year $yy$ and category (`Cen`, `Fwd`):

- **Fail**: the QCD template is one free parameter per bin (`QCD_<Cat><yy>Fail`), seeded
  with data minus $t\bar t$.
- **Pass**: QCD$_{\mathrm{Pass}}(x, y)$ = QCD$_{\mathrm{Fail}}(x, y) \times R_{P/F}(x, y)$, with
  $x = m_t$ and $y = m_{t\bar t}$ scaled to [0, 1].
- The transfer function is a product of polynomials,

  $$R_{P/F}(x, y) = 0.1\,\Big(p_0 + \sum_{i=1}^{n_x} p_i\, x^i\Big)\Big(1 + \sum_{j=1}^{n_y} p_{n_x + j}\, y^j\Big),$$

  order "$n_x$×$n_y$", parameters `QCD_<Cat><yy>rpf_par<i>` with $p_0 \in [0.001, 50]$ and
  $p_{i>0} \in [-50, 50]$.

The Pass top-mass window (105–210 GeV) is masked in every background-only fit, so the
Pass prediction there comes only from Fail and the Pass sidebands.

## Run-3 model (2022–2026)

| Item | Choice |
| --- | --- |
| Channels | 5 years × {Cen, Fwd} × {Pass, Fail} = 20 regions, one card |
| Sub-eras | 2022 preEE + postEE and 2023 preBPix + postBPix summed before the fit (one TF per calendar year) |
| Transfer functions | One per year and category (10). **Constant (0×0) in every year-region for the current limits** |
| $t\bar t$ rate | `ttbar_xsec<yy>`, lnN 30% per year |
| Nuisances | 39 (per year, correlated, 2026-only); list on [Systematic uncertainties](uncertainties.md) |
| Fit | masked background-only `MultiDimFit`, all nuisances floating, $r = 0$ frozen, strategy 2 with fallbacks 1 and 0 |
| Gates | status 0, covQual 3, EDM < 0.01, every card nuisance present and floating |
| Limits | `AsymptoticLimits --run blind` from the masked-fit snapshot |

Code: bgestimation branch `run3-v121`: `ttbar.py` (model), `run3/run3comb/make_run3_configs.py`
(configs), `run3/run3comb/fit_job.sh` (fit and limit recipe), `run3/run3comb/condor_campaign.py`.

### Transfer-function order: constant, provisional

The per-year F-test (2026-10-04) fits every order per year with the full model on the
masked data. Each order starts from the 0×0 solution so nested orders cannot land above
it; χ² is the saturated GoF at the fit minimum; `ftest_select.py` steps up one parameter
at a time while p < 0.05.

| Bins used | 2022 | 2023 | 2024 | 2025 | 2026 |
| --- | --- | --- | --- | --- | --- |
| n = 210 (all unmasked bins), cen | 0×1 | 0×1 | 2×2 | 0×1 | 0×0 |
| n = 210, fwd | 1×0 | 0×1 | 2×0 | 0×0 | 0×0 |
| n = 45 (Pass sidebands only) | 0×0 in 9 of 10 year-regions; fwd 2024 prefers 1×0 (p = 0.014) | | | | |

2024 reproduces the August choice (cen 2×2, fwd 2×0). In the combined five-year fit,
however, the F-test picks and a uniform 2×2/2×0 fail the convergence gates, so the
production limits use a constant TF everywhere. This is forced by convergence, not
selected by the F-test, and two reviews (2026-10-04 and 2026-10-08) asked for it to be
presented as provisional. The F-test p-values are analytic; a toy calibration is open.

### Blinded validation (2026-10-08)

| Check | Result |
| --- | --- |
| Masked saturated GoF, 200 toys | p = 0.27 |
| Closure (background-only Asimov) | $|\hat r| \leq 0.07\, r_{95}$ |
| Asimov signal injection | fitted/injected slopes 1.05–1.09 |
| Toy injection at $1 \times r_{95}$ | fine at 1% width, 2 TeV; under-recovers at 30% width, 4 TeV (median $\hat r/r$ = 0.68; Feldman–Cousins intervals needed) |
| Cost of systematics on the expected limit | ×1.15–2.27, largest at 1.4–1.6 TeV |
| 2026 contribution | about 4% on the expected limit |

### $t\bar t$ normalisation

The $t\bar t$ simulation lies above the data in the sidebands. The masked fit absorbs this
with the per-year $t\bar t$ rates (all pulled below 1) and pulls `toppt` by −1.55σ and
`ttag_band_hipt` by −1.7σ (tracker, 2026-10-22). Both reviews ask for this to be fixed
before the transfer function, ideally with a $t\bar t$-enriched control region (for example
with a b-tagged subjet). Options and the top-$p_T$ side are on
[Top-pT reweighting](top-pt-reweighting.md#open-decision). No decision as of 2026-10-22.

## Known limitations

- No template statistical uncertainty (`autoMCStats` does not work with the 2D pdfs).
- Lumi correlations, the JES source split, the tight/band SF covariance and the 2022/23
  SF transfer are placeholders.
- Summing the sub-eras needs a split-versus-merged closure check.
- TF positivity and the fitted TF surfaces are still to be shown.

## History

| Date | Change | Reference |
| --- | --- | --- |
| 2026-06-18 | 2024-only model, F-test picks 2×1 cen and fwd, masked GoF passing | [June background estimation](../archive/2026-06/background-estimation.md) |
| 2026-06-25 | 2025 added; floating `ttagSF` rateParam removed (degenerate with $r$); $t\bar t$ prior 20% → 30% | research-notes `ttbarhadronic_2025_combined_fit_wiring` |
| 2026-07-23 | June inputs found to carry a stale loose WP; all June fit choices superseded. Tight-WP inputs; central fit fails without a measured SF, flat placeholder $t\bar t$ scale 0.75 (cen) / 0.90 (fwd) | research-notes `ttbarhadronic_tightwp_2425_limit_campaign` |
| 2026-07-31 | Measured top-tag SF replaces the placeholder (skimmer v1) | skimmer tag `v1` |
| 2026-08-14 | 2024 + 2025, F-test ratified cen 2×2, fwd 2×0 (2×1's $y$ slope unsupported); 9 nuisances; fail-closed fit gates | bgestimation `1085d9a` |
| 2026-10-04 | Run-3 2022–2026 model: one TF per year-region, 39 nuisances, per-year F-test; constant TF for the limits | bgestimation `011ad5c` |
| 2026-10-08 | Blinded validation of the constant-TF five-year fit; Pass-sideband-only F-test | tracker `run3-validation`, bgestimation `4ccc225` |
| 2026-10-13 | Stacked and years-summed post-fit projections | bgestimation `2dd07f4` |
