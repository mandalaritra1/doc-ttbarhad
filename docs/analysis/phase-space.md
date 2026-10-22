# Phase space

## Boosted all-hadronic topology

Two highly boosted, hadronically decaying top quarks, each reconstructed as one AK8 jet.
The full cut list is on [Selections](selections.md); the defining requirements are:

| Requirement | Value (2022–2026) |
| --- | --- |
| Trigger | `HLT_PFHT1050` |
| Event activity | AK4 $H_T > 1400$ GeV (AK4 PUPPI, $p_T > 30$ GeV, $|\eta| < 3.0$) |
| AK8 jets | ≥ 2 with $p_T > 400$ GeV, $|y| < 2.4$, PUPPI Tight ID; jet veto map on the event |
| Back-to-back | $|\Delta\phi(j_0, j_1)| > 2.1$, both jets with two soft-drop subjets |
| Top tag | $j_0$ tight ($D_{\mathrm{top}} > 0.9284$); $j_1$ tight (Pass) or in the band 0.8571–0.9284 (Fail); $f_{lep} < 0.59$ on both |
| $j_1$ mass window | $105 < m_{SD} < 210$ GeV |

$j_0$ and $j_1$ are the two highest-$p_T$ AK8 jets; $j_0$ is the one with the higher
top-tagger score.

## Observables

| Observable | Definition | Use |
| --- | --- | --- |
| $m_t$ | soft-drop mass of $j_0$, rebuilt from re-corrected subjets | fit $x$ axis, 25–475 GeV; signal window 105–210 GeV |
| $m_{t\bar t}$ | $m(j_0 + j_1)$ | fit $y$ axis, 0.8–6.5 TeV |
| $\Delta y$ | $y(j_0) - y(j_1)$ | central / forward split |
| $\chi$ | $\exp|\Delta y|$ | angular variable (histogram only) |

Fit binning: $m_t$ edges 25, 60, 105, 130, 150, 170, 175, 180, 185, 190, 210, 475 GeV;
$m_{t\bar t}$ edges 800, 1100, then 200 GeV steps to 3700, last bin 3700–6500 GeV.

## Region map

| | Central ($|\Delta y| < 1$) | Forward ($|\Delta y| \geq 1$) |
| --- | --- | --- |
| **Pass** (both jets tight) | `2tcen` | `2tfwd` |
| **Fail** ($j_1$ in the band) | `atcen` | `atfwd` |

Along $m_t$ each region has a low sideband (25–105 GeV), the top-mass window (105–210 GeV,
masked in Pass while blind) and a high sideband (210–475 GeV). The 2DAlphabet transfer
function uses the Fail region and the Pass sidebands to predict the QCD background in the
Pass window ([Background estimation](background-estimation.md)).

In the Run-3 fit each year (2022, 2023, 2024, 2025, 2026) has its own four categories,
so the combined fit has 20 Pass/Fail × cen/fwd regions.

## History

| Date | Change | Reference |
| --- | --- | --- |
| June 2026 | 2024 only; $H_T > 1500$ GeV and the medium WP as documented | [June phase space](../archive/2026-06/phase-space.md) |
| 2026-07-23 | Tight WP 0.9284 for the tag | tight-WP production |
| 2026-09-24 | Jet veto map, AK8 Tight ID; Fail = band below tight | skimmer v1.2 |
| 2026-10-02 | $f_{lep} < 0.59$ on both jets | skimmer v1.2.1 |
| 2026-10-04 | Five years, 20 regions in one fit | bgestimation `011ad5c` |
