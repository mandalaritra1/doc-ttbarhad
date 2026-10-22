# Introduction

## Goal

Search for heavy resonances decaying to a top-quark pair, $X \to t\bar t$, in the fully
hadronic final state with the Run-3 data of 2022–2026 (307.5 fb⁻¹ at 13.6 TeV). The
benchmark is a spin-1 $Z'$ with widths $\Gamma/M$ = 1%, 10% and 30%, from 1.2 to 9 TeV.

At these masses both tops are highly boosted, so each is one large-radius (AK8) jet whose
substructure and soft-drop mass carry the top signature. The resonance shows up as a bump
in the dijet mass $m_{t\bar t}$.

## Strategy

1. **Trigger and activity**: `HLT_PFHT1050`, AK4 $H_T > 1400$ GeV.
2. **Two top candidates**: the two leading AK8 jets ($p_T > 400$ GeV, $|y| < 2.4$, Tight
   ID), back to back.
3. **Top tagging**: GloParT-v3 TopvsQCD at the tight working point (0.5% QCD mis-tag),
   with a leptonic-top veto $f_{lep} < 0.59$.
4. **Regions**: Pass (both jets tight) and Fail (second jet in the score band just below
   tight), each split into central and forward by $|\Delta y|$.
5. **Background**: QCD multijet from data with a Pass/Fail transfer function (2DAlphabet);
   SM $t\bar t$ from simulation with measured top-tag scale factors.
6. **Fit**: a simultaneous 2D binned likelihood fit in ($m_t$, $m_{t\bar t}$) over all five
   years, with one transfer function per year and category.

## Key ingredients

| Ingredient | Choice |
| --- | --- |
| Data | 2022 34.65, 2023 27.24, 2024 109.95, 2025 110.37, 2026 25.31 fb⁻¹ |
| Simulation | Summer22/23 (2022/23 $t\bar t$), Summer24 (2024; also stands in for 2025, 2026 and the 2022/23 Z′) |
| Trigger | `HLT_PFHT1050`, no trigger SF |
| Top tagger | GloParT-v3 TopvsQCD, tight 0.9284; Fail band 0.8571–0.9284 |
| Top-tag SFs | semileptonic $t\bar t$ tag-and-probe on 2024 data; fully merged tops only |
| $t\bar t$ | TTto4Q (POWHEG + Pythia8), 419.9 pb, TOP-23-008 top-$p_T$ reweighting, 30% rate per year |
| Background method | 2DAlphabet (QCD from data) |
| Uncertainties | 39 nuisances, see [Systematic uncertainties](uncertainties.md) |

## Current status (2026-10-22)

- Skimmer v1.2.1 inputs for all five years (2026-10-03), staged on EOS.
- Run-3 2022–2026 combination (2026-10-04) with 39 nuisances; blinded expected limits at
  1.2–9 TeV for the three widths, all fits passing the production gates.
- Expected reach against the Run-2-equivalent theory line: 4.61 / 5.88 / 7.40 TeV for
  1 / 10 / 30% width (provisional; [Results](results.md)).
- Open: the $t\bar t$ normalisation and top-$p_T$ treatment, the transfer-function order
  (constant TF for now), and the items on each page.

## History

| Date | Change | Reference |
| --- | --- | --- |
| June 2026 | 2024-only analysis, medium WP as documented, blinded 1% reach quoted from inputs later found stale | [June introduction](../archive/2026-06/introduction.md) |
| 2026-07-23 | Stale loose-WP inputs found; tight-WP production for 2024 and 2025 | research-notes `ttbarhadronic_tightwp_2425_limit_campaign` |
| 2026-07-31 | Skimmer v1: corrected 2024/2025 pileup weights and the first measured top-tag SF | skimmer tag `v1` |
| 2026-08-01 | Skimmer v1.1: signal normalisation fix below 1 TeV | skimmer tag `v1.1` (`fab5038`) |
| 2026-08-14 | 2024 + 2025 blinded expected limits, 39 points, 9 nuisances, TF cen 2×2 / fwd 2×0 | bgestimation `1085d9a` |
| 2026-09-24 | Skimmer v1.2: veto map, jet ID, JEC on data, rebuilt $m_{SD}$, band SF | skimmer tag `v1.2` |
| 2026-10-02/03 | Skimmer v1.2.1: $f_{lep}$ cut, SF scope, top-$p_T$ recipe, 2022/23/26 added | skimmer changelog items 31–40 |
| 2026-10-04 | Run-3 2022–2026 combination | bgestimation `011ad5c` |
| 2026-10-13 | Run-2-equivalent theory line for the reach | bgestimation `9567d35` |
