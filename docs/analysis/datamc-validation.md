# Data/MC Validation

Data/MC comparisons check that the simulation describes the data in the
control-rich parts of the phase space before the background fit is trusted. The
analysis is **blinded**: the pass-region top-mass signal window is not compared
to data.

## Core plot set

| Observable | What it checks |
| --- | --- |
| Leading AK8 soft-drop mass | top-candidate mass scale/resolution and sideband shape |
| Subleading AK8 soft-drop mass | second candidate; stability of the pass/fail split |
| $m_{t\bar{t}}$ | the resonance spectrum (sidebands / fail region only while blind) |
| $H_T$ | event-activity modeling and trigger-plateau assumption |
| Jet rapidity / $\Delta y$ | central vs forward category behavior |
| Top-tagger score | tagging modeling and working-point placement |

These are shown in the **antitag (fail)** region and in the pass-region top-mass
**sidebands**, where signal contamination is negligible.

## Current status

The strongest validation available right now is the **background-only goodness
of fit** of the 2DAlphabet model, which probes whether data and the
(QCD + $t\bar{t}$) prediction agree across the blinded $(m_t, m_{t\bar{t}})$ plane:

- Central `2x1`: $p = 0.485$
- Forward `2x1` (with clamp-to-data): $p = 0.180$

Both pass, i.e. the model describes the sideband data well. The plots and method
are on the [Background Estimation](background-estimation.md) page.

| Region | Status |
| --- | --- |
| Fail/antitag, central | covered by the passing central GoF |
| Fail/antitag, forward | covered by the passing forward GoF |
| Pass sidebands, central/forward | included in the blinded fit; GoF passing |
| Pass signal window | blinded |

## Open items

- Add standalone Data/MC overlays (soft-drop mass, $H_T$, $m_{t\bar{t}}$, $\Delta y$,
  tagger score) for the antitag region and pass sidebands.
- Add a measured 2024 top-tag scale factor; the current 0.90/tag placeholder is
  the leading source of $t\bar{t}$ normalization uncertainty in these comparisons.
