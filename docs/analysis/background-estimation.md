# Background Estimation

## Objective

Summarize the current Run-3 2024 2DAlphabet background model, the completed
transfer-function scan, and the checks still required before the model is
frozen.

## Method Summary

The dominant background is QCD multijet production. QCD is derived from data
with a pass/fail construction, while SM `ttbar` is included from simulation.

| Component | Role |
| --- | --- |
| Fail region | QCD-enriched control sample |
| Pass region | Signal-like sample after the top-tag requirement |
| Top-candidate mass sidebands | Constrain the fail-to-pass behavior outside the signal window |
| Transfer function, \(R_{\mathrm{p/f}}(x,y)\) | Maps the fail-region QCD model into the pass region |
| SM `ttbar` | Simulated background included in the fit |
| Signal template | `signalRSGluon4000`, used to construct a standard signal-plus-background card |

The 2024 binning has 11 top-candidate-mass intervals and 15
\(m_{t\bar{t}}\) intervals. Central and forward rapidity categories are fitted
separately.

## Current 2024 Implementation

| Item | Current state |
| --- | --- |
| Categories | `cen2024`, `fwd2024` |
| Candidate forms | `0x0`, `0x1`, `0x2`, `1x0`, `1x1`, `1x2`, `2x1`, `2x2` |
| Fit areas | 16 total: eight forms in each category |
| Pairwise outputs | 25 stored comparisons per category; equal-parameter pairs are skipped |
| Postfit projections | Available for every tested candidate |
| Configured nominal defaults | Central `1x1`; forward `0x1` |
| Final model-selection status | Not yet frozen or fully documented |

The scan varies polynomial order within the existing 2D transfer-function
construction. It does **not** test unrelated functional families such as
Laurent, exponential, or Chebyshev functions.

## Reproducing The Scan

Run from the standalone `bgestimation/` area in the compatible
CMSSW/2DAlphabet environment:

```bash
cd /uscms_data/d3/amandal2/bg_ttbar/CMSSW_14_1_0_pre4/src/bgestimation

LOCAL_INPUT="/eos/user/a/amandal/ttbarhad_root_files/2dAlphabetInputs"

python -u fit_ftest.py \
  --preset 2024 \
  --input "$LOCAL_INPUT" \
  --signal RSGluon4000 \
  2>&1 | tee output_ftest_2024.log

python -u results_ftest.py \
  --preset 2024 \
  --signal RSGluon4000 \
  2>&1 | tee output_ftest_results_2024.log
```

The work areas are written under:

```text
ftest/2024/{cen,fwd}/ttbarfits_{cen,fwd}2024_ftest<TF>/
```

The numerical summaries and pairwise plots are written under:

```text
ftest_results/
```

Preserve each work area's `runConfig.json`, card, GOF ROOT file, and fit logs.
The current launcher updates the selected config's input path and signal list
before constructing the fit; `runConfig.json` is the fit-specific record.

## F-test Summary

The stored CSV tables contain the simple and complex candidate labels, parameter
counts, the bin count used by the script, observed F statistic, and p-value.
Rows with `p < 0.05` are highlighted in red in the presentation tables.

The raw output contains 11 central and 6 forward rows with `p < 0.05`. This
count includes all unequal-parameter pairings and is not itself a model-choice
criterion.

### Central tables

| | |
| --- | --- |
| ![Central F-test table, page 1](../assets/analysis/background-estimation/2024/ftest-tables/summary-2.png) | ![Central F-test table, page 2](../assets/analysis/background-estimation/2024/ftest-tables/summary-3.png) |
| ![Central F-test table, page 3](../assets/analysis/background-estimation/2024/ftest-tables/summary-4.png) | |

### Forward tables

| | |
| --- | --- |
| ![Forward F-test table, page 1](../assets/analysis/background-estimation/2024/ftest-tables/summary-5.png) | ![Forward F-test table, page 2](../assets/analysis/background-estimation/2024/ftest-tables/summary-6.png) |
| ![Forward F-test table, page 3](../assets/analysis/background-estimation/2024/ftest-tables/summary-7.png) | |

### Example pairwise plots

| Central: `0x0` versus `0x1` | Forward: `0x0` versus `0x1` |
| --- | --- |
| ![Central 0x0 versus 0x1 F-test](../assets/analysis/background-estimation/2024/pairwise/FTest_0x0_0x1_2024_cen.png) | ![Forward 0x0 versus 0x1 F-test](../assets/analysis/background-estimation/2024/pairwise/FTest_0x0_0x1_2024_fwd.png) |

## Statistical Interpretation

!!! warning "The scan is provisional"
    The standard F-test interpretation assumes nested models. The current
    implementation orders candidates by parameter count and compares every pair
    with different counts. Some stored pairs are not obviously nested. Use
    non-nested rows as diagnostics, not calibrated model-selection p-values.

Before freezing the model:

1. Define the allowed nested comparison graph.
2. Validate the effective number of fitted bins and degrees of freedom.
3. Confirm the treatment of blinded pass-region signal-window bins.
4. Check fit convergence, nuisance pulls, and correlations for retained models.
5. Use postfit residuals and closure checks alongside the F-test.

The current CSVs use `n_bins = 165`, corresponding to \(11\times15\). The
implementation should be reviewed to confirm whether the pass/fail region count
and blinded bins are represented consistently with the intended F-test
definition.

An F statistic of zero is written when the nominally more complex candidate
does not improve the saturated GOF value. Such a row appears as `p = 1`; this
does not by itself mean the fit failed.

## Postfit \(m_{t\bar{t}}\) Projections

Each source page contains six background-only projections: pass and fail
regions for the low, signal-window, and high top-candidate-mass intervals.

!!! caution "Source labels"
    These plots come from the 2024 F-test work areas, but their embedded labels
    currently read `138 fb^-1 (13 TeV)` and `CMS Preliminary`. This appears to
    be inherited Run-2 plotting text. Correct it at the plotting source before
    using the figures externally.

### Central candidates

| `0x0` | `0x1` |
| --- | --- |
| ![Central postfit projy 0x0](../assets/analysis/background-estimation/2024/postfit-projy/cen-0x0.png) | ![Central postfit projy 0x1](../assets/analysis/background-estimation/2024/postfit-projy/cen-0x1.png) |
| `0x2` | `1x0` |
| ![Central postfit projy 0x2](../assets/analysis/background-estimation/2024/postfit-projy/cen-0x2.png) | ![Central postfit projy 1x0](../assets/analysis/background-estimation/2024/postfit-projy/cen-1x0.png) |
| `1x1` | `1x2` |
| ![Central postfit projy 1x1](../assets/analysis/background-estimation/2024/postfit-projy/cen-1x1.png) | ![Central postfit projy 1x2](../assets/analysis/background-estimation/2024/postfit-projy/cen-1x2.png) |
| `2x1` | `2x2` |
| ![Central postfit projy 2x1](../assets/analysis/background-estimation/2024/postfit-projy/cen-2x1.png) | ![Central postfit projy 2x2](../assets/analysis/background-estimation/2024/postfit-projy/cen-2x2.png) |

### Forward candidates

| `0x0` | `0x1` |
| --- | --- |
| ![Forward postfit projy 0x0](../assets/analysis/background-estimation/2024/postfit-projy/fwd-0x0.png) | ![Forward postfit projy 0x1](../assets/analysis/background-estimation/2024/postfit-projy/fwd-0x1.png) |
| `0x2` | `1x0` |
| ![Forward postfit projy 0x2](../assets/analysis/background-estimation/2024/postfit-projy/fwd-0x2.png) | ![Forward postfit projy 1x0](../assets/analysis/background-estimation/2024/postfit-projy/fwd-1x0.png) |
| `1x1` | `1x2` |
| ![Forward postfit projy 1x1](../assets/analysis/background-estimation/2024/postfit-projy/fwd-1x1.png) | ![Forward postfit projy 1x2](../assets/analysis/background-estimation/2024/postfit-projy/fwd-1x2.png) |
| `2x1` | `2x2` |
| ![Forward postfit projy 2x1](../assets/analysis/background-estimation/2024/postfit-projy/fwd-2x1.png) | ![Forward postfit projy 2x2](../assets/analysis/background-estimation/2024/postfit-projy/fwd-2x2.png) |

## Presentation Downloads

- [All pairwise F-test plots](../assets/analysis/background-estimation/2024/pdfs/FTest_pairwise_results_2024.pdf)
- [F-test numerical tables](../assets/analysis/background-estimation/2024/pdfs/FTest_summary_tables_2024.pdf)
- [All postfit projection pages](../assets/analysis/background-estimation/2024/pdfs/FTest_postfit_projy_all_transfer_functions_2024.pdf)

## Current Takeaway

The transfer-function scan and postfit diagnostics are now available for both
2024 categories. The repository currently uses central `1x1` and forward
`0x1`, but the final choice should remain provisional until the nested-model
path, F-test degrees of freedom, plot labels, and fit-quality checks are
reviewed and documented.

## Open Items

- Restrict the formal F-test interpretation to verified nested candidates.
- Validate `n_bins` and blinded-bin accounting.
- Record the final central and forward transfer-function decision.
- Correct the inherited Run-2 labels in the postfit plotting code.
- Add closure, GOF, pull/correlation, and nuisance-impact summaries.
