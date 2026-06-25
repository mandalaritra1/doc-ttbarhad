# Results

## Objective

Collect result-facing material only after selections, validation, background
estimation, and systematic assumptions have a stable reviewed configuration.

!!! info "Blinded expected result (2026-06-21, corrected)"
    Combined central + forward, transfer-function order `2x1`, **blinded**
    (expected-only). Dataset 109.95 fb⁻¹ (13.6 TeV), ZPrime → t̄t at 1% width.
    The expected limit is now built from a **data-driven sideband-fit Asimov**
    (see the note below); this supersedes the 2026-06-18 figure of ≈ 5.32 TeV.

## Current Status

| Input | Status |
| --- | --- |
| 2024 background fit | Central and forward, order `2x1`, passing goodness-of-fit |
| Transfer-function choice | **Selected: central `2x1`, forward `2x1`** (F-test) |
| Combined limit | **Blinded expected (snapshot Asimov), available below** |
| Impacts | Blinded Asimov, available below |
| Systematic inputs | Placeholder 2024 top-tag SF (0.90/tag); only ~2 of ~8 systematics in the limit card yet |
| Observed result | **Not unblinded** |

## Blinded Expected Limits

Combined central + forward 95% CL upper limits on the ZPrime (1% width)
production cross section, blinded (expected median and ±1σ/±2σ bands). The
theory crosses the median expected limit at **≈ 4.84 TeV**, i.e. the expected
exclusion reach is about 4.8 TeV. The bands are smooth and monotone across the
full 1–6 TeV grid.

![ZPrime 1% width blinded expected limits](../assets/analysis/results/2024/limits_zprime_1pct.png)

| Quantity | Value |
| --- | --- |
| Signal | ZPrime → t̄t, 1% width |
| Categories | central + forward, combined |
| Transfer function | `2x1` (both) |
| Mass range | 1–6 TeV (m_tt window ends at 6.5 TeV) |
| Expected exclusion | ≈ 4.84 TeV |
| Blinding | blinded (expected only) |

!!! warning "Why the number changed (5.32 → 4.84 TeV)"
    The earlier expected limit built its Asimov ("expected") dataset from the
    **prefit** transfer function — i.e. before the TF was fit to the data
    sidebands. For a data-driven QCD estimate that prefit TF is optimistically
    biased, which both **inflated the expected reach** and produced unstable
    median/−σ bands at 1.8–3.5 TeV (the reason the 1.8/2.0 TeV points were
    previously dropped). The corrected procedure fits the background-only model to
    data with the Pass signal window **masked** (so the analysis stays blinded),
    saves that postfit **snapshot**, and builds the Asimov from it. The result is
    the honest reach **≈ 4.84 TeV** with clean bands and **all mass points
    recovered**, consistent with the independent 1D cross-check (≈ 4.5 TeV ≈ Run-2).

!!! note "Mass coverage"
    All 13 mass points 1–6 TeV are now present (1.8/2.0 TeV recovered). Masses
    above ~6.5 TeV are outside the m_tt fit window.

## Wider widths (10% and 30%)

The same combined cen+fwd, `2x1`, blinded snapshot-Asimov chain, for the 10% and
30% width benchmarks. The expected reach grows with width (a wider resonance has a
larger production cross section), with clean bands across the grid.

| Width | Expected exclusion |
| --- | --- |
| 1% | ≈ 4.84 TeV |
| 10% | ≈ 5.91 TeV |
| 30% | > 6 TeV (limit stays below theory across 1–6 TeV; reach beyond the grid edge) |

![ZPrime 10% width blinded expected limits](../assets/analysis/results/2024/limits_zprime_10pct.png)

![ZPrime 30% width blinded expected limits](../assets/analysis/results/2024/limits_zprime_30pct.png)

!!! note "10/30 normalization"
    The 10/30 signal templates are scaled per mass by their theory σ so the limit
    fit stays numerically well-conditioned (signal strength r ~ O(1)); the plotted
    σ×B|95 is independent of that scale. A single fixed-fb scale was found to break
    the asymptotic band calculation at low mass and is not used.

## Impacts

Nuisance-parameter impacts on the signal strength (blinded Asimov, 3 TeV
benchmark). The fit recovers r̂ ≈ 1.00 ± 0.13; the t̄t cross section and pileup
lead, with the transfer-function parameters contributing moderately — the
result is statistics / background-estimation dominated.

![Blinded impact ranking at 3 TeV](../assets/analysis/results/2024/impacts_zprime3000.png)

## Readiness Conditions

Before adding result distributions or limits:

1. Freeze the central and forward transfer-function choices.
2. Validate the nested F-test comparisons and degrees of freedom.
3. Correct 2024 luminosity and collision-energy labels in fit plots.
4. Complete closure, GOF, fit-quality, and nuisance checks.
5. Confirm the systematic model and the intended blinding/unblinding state.
6. Record the exact input production and saved `runConfig.json` files.

## Result Presentation

Each future result entry should state:

- configuration and production date;
- dataset and luminosity;
- category or combination;
- transfer-function choice and justification;
- systematic model;
- blinding state;
- whether the figure is work in progress, under review, or approved.

## Current Takeaway

The 2024 analysis has a complete **blinded** result chain for all three widths:
selected `2x1` transfer functions, passing goodness-of-fit, combined expected
limits with clean ±1σ/±2σ bands across the full mass grid (**1% ≈ 4.84 TeV,
10% ≈ 5.91 TeV, 30% > 6 TeV**), and a well-behaved impact ranking. The expected
limits use a data-driven sideband-fit (snapshot) Asimov, which removed the earlier
band instability and an optimistic bias in the reach. Remaining before a final
result: propagating the full systematics into the limit card (only ~2 of ~8 enter
today), a measured 2024 top-tag scale factor, and unblinding approval.
