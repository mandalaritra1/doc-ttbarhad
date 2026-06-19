# Introduction

## Goal

Search for heavy resonances decaying to a top-quark pair, $X \to t\bar{t}$, in the
**fully hadronic** final state, using **Run-3 2024** data (109.95 fb⁻¹, 13.6 TeV).
Benchmark signals are a spin-1 $Z'$ (1%, 10%, 30% width) and a Kaluza–Klein gluon
$g_{KK}$, with resonance masses from 1 to ~7 TeV.

At these masses both top quarks are highly boosted, so each top is reconstructed
as a single large-radius (AK8) jet whose substructure and soft-drop mass carry the
top signature. The resonance appears as a bump in the two-jet invariant mass
$m_{t\bar{t}}$.

## Strategy in one view

1. **Trigger + activity**: hadronic `HLT_PFHT1050`, AK4 $H_T > 1500$ GeV.
2. **Two top candidates**: the two leading AK8 jets ($p_T > 400$ GeV, $|y| < 2.4$),
   back-to-back ($|\Delta\phi| > 2.1$), each with a soft-drop mass near the top mass.
3. **Top tagging**: GloParT-v3 TopvsQCD discriminant separates real boosted tops
   from QCD jets.
4. **Regions**: events split into a double-tag **pass** region and an **antitag**
   **fail** region, and into **central / forward** categories by the rapidity
   separation $\Delta y$ of the two candidates.
5. **Background**: QCD multijet (dominant) is estimated **from data** with a
   pass/fail transfer function (2DAlphabet); SM $t\bar{t}$ is taken from simulation.
6. **Fit**: a 2D maximum-likelihood fit in $(m_t,\,m_{t\bar{t}})$ extracts limits on
   the resonance cross section.

## Key ingredients

| Ingredient | Choice / value (2024) |
| --- | --- |
| Trigger | `HLT_PFHT1050` |
| Event activity | AK4 $H_T > 1500$ GeV ($p_T>30$, $|\eta|<3.0$) |
| Top candidates | 2 leading AK8 jets, $p_T>400$ GeV, $|y|<2.4$ |
| Topology | $|\Delta\phi(j_0,j_1)| > 2.1$, valid subjets |
| Top mass window | $105 < m_{\mathrm{SD}} < 210$ GeV |
| Top tagger | GloParT-v3 TopvsQCD, medium WP $= 0.8571$ |
| Category split | central $|\Delta y| < 1.0$, forward otherwise |
| Resonance observable | $m_{t\bar{t}} = m(j_0 + j_1)$ |
| Background method | 2DAlphabet pass/fail (QCD from data, $t\bar{t}$ from MC) |

## Current status (2024)

The full 2024 chain runs end to end **blinded**: transfer-function order selected
by F-test (`2x1` central and forward), goodness-of-fit passing in both categories,
and a combined expected exclusion of **~5.3 TeV** for a 1%-width $Z'$. See
[Background Estimation](background-estimation.md) and [Results](results.md).

Remaining before a final result: a measured 2024 top-tag scale factor, full
systematics, and unblinding.
