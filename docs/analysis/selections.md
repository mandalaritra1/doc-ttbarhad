# Selections

## Event selection

| Step | Requirement (2024) |
| --- | --- |
| Lumi mask | golden JSON (data only) |
| Trigger | `HLT_PFHT1050` |
| MET filters | standard Run-3 set |
| Event activity | AK4 $H_T > 1500$ GeV (AK4 jets $p_T > 30$ GeV, $|\eta| < 3.0$) |
| AK8 kinematics | $\geq 2$ AK8 jets with $p_T > 400$ GeV, $|y| < 2.4$ |
| Top-pair candidate | $|\Delta\phi(j_0,j_1)| > 2.1$ and both jets have two valid subjets |

The two leading-$p_T$ AK8 jets form the $t\bar{t}$ candidate. They are then
ordered by top-tagger score: $j_0$ is the higher-score jet, $j_1$ the other.

## Top tagging

The tagger is the **GloParT-v3 TopvsQCD** discriminant,

$$D_{\mathrm{top}} = \frac{P_{\mathrm{TopbWqq}} + P_{\mathrm{TopbWq}}}{P_{\mathrm{TopbWqq}} + P_{\mathrm{TopbWq}} + P_{\mathrm{QCD}}}.$$

2024 working points (campaign-dependent — keep explicit on plots):

| WP | $D_{\mathrm{top}}$ threshold |
| --- | --- |
| loose | 0.6488 |
| **medium (used for the tag)** | **0.8571** |
| tight | 0.9284 |

A jet is "top-tagged" if $D_{\mathrm{top}} >$ medium **and** $105 < m_{\mathrm{SD}} < 210$ GeV.

## Pass / fail regions

| Region | Definition |
| --- | --- |
| **Pass** (`2t`) | both $j_0$ and $j_1$ top-tagged (double tag) — signal-like |
| **Fail** (`at`, antitag) | $j_0$ tagged; $j_1$ in the **antitag** window loose $< D_{\mathrm{top}} <$ medium, with the mass window — QCD-enriched |

The fail (antitag) region anchors the data-driven QCD template; the transfer
function maps it into the pass region.

## Rapidity categories

The two-candidate rapidity separation $\Delta y = y(j_0) - y(j_1)$ splits events:

| Category | Requirement |
| --- | --- |
| central | $|\Delta y| < 1.0$ |
| forward | $|\Delta y| \geq 1.0$ |

## Category names

Crossing the two splits gives the four fit categories:

| Category | Top-tag | Rapidity |
| --- | --- | --- |
| `2tcen` | pass (double tag) | central |
| `2tfwd` | pass (double tag) | forward |
| `atcen` | fail (antitag) | central |
| `atfwd` | fail (antitag) | forward |

Central and forward are fitted as separate 2DAlphabet workspaces and combined for
the final limit.

## Notes

- Label the tagger branch (`globalParT3`) and working point on every figure: the
  Run-3 top-tag definition is campaign-dependent.
- AK4 JetID is currently not applied (Run-3 NanoAOD working point not yet
  centrally validated).
- A measured 2024 top-tag scale factor is still pending; the fits currently use a
  placeholder of 0.90 per tag.
