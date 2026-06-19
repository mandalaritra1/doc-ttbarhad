# Phase Space

## Boosted all-hadronic topology

The analysis selects events with two highly boosted, hadronically decaying top
candidates, each reconstructed as one AK8 jet. The phase space is defined by the
trigger, event activity, and the two leading AK8 jets.

| Requirement | 2024 value |
| --- | --- |
| Trigger | `HLT_PFHT1050` |
| Event activity | AK4 $H_T > 1500$ GeV (AK4 jets $p_T > 30$ GeV, $|\eta| < 3.0$) |
| AK8 multiplicity | $\geq 2$ AK8 jets passing kinematics |
| AK8 kinematics | $p_T > 400$ GeV, $|y| < 2.4$ |
| Back-to-back | $|\Delta\phi(j_0, j_1)| > 2.1$ |
| Subjet quality | both candidates have two valid soft-drop subjets |
| Top mass window | $105 < m_{\mathrm{SD}} < 210$ GeV |

The two top candidates $j_0, j_1$ are the two highest-$p_T$ AK8 jets; of those two,
$j_0$ is defined as the one with the **higher** top-tagger score.

## Observables

| Observable | Definition | Range / binning |
| --- | --- | --- |
| $m_t$ | soft-drop mass of the AK8 top candidate | fit axis; signal window $105$–$210$ GeV |
| $m_{t\bar{t}}$ | invariant mass $m(j_0 + j_1)$ | resonance observable, up to 6.5 TeV |
| $\Delta y$ | $y(j_0) - y(j_1)$ | category split |
| $\chi$ | $\exp(|\Delta y|)$ | angular discriminant |

## Region map

Events are classified in two independent dimensions: the **top-tag** outcome and
the **rapidity** category.

| | Central ($|\Delta y| < 1.0$) | Forward ($|\Delta y| \geq 1.0$) |
| --- | --- | --- |
| **Pass** (both jets top-tagged) | `2tcen` | `2tfwd` |
| **Fail** (antitag) | `atcen` | `atfwd` |

Within each region the $m_t$ axis is further split into a low sideband, the
top-mass **signal window** ($105$–$210$ GeV, blinded in the pass region), and a
high sideband. The pass/fail and sideband structure is what the 2DAlphabet
transfer function exploits to predict the QCD background.

The central category targets the more centrally produced, low-$\Delta y$ signal;
the forward category captures the complementary, more QCD-like topology.
