# Top-tag scale factors

## What they are and why

Pass and Fail are defined by the GloParT-v3 top tagger, and the QCD background is
taken from data through the Pass/Fail transfer function, so the QCD mis-tag rate needs
no correction. The $t\bar t$ background and the Z′ signal come from simulation, and
their real-top tag efficiency differs from the data. The scale factors
$\mathrm{SF} = \varepsilon_{\mathrm{data}} / \varepsilon_{\mathrm{MC}}$ correct that, per
jet, for genuine fully merged hadronic tops.

## Tagger, working point and categories

$$D_{\mathrm{top}} = \frac{P_{\mathrm{TopbWqq}} + P_{\mathrm{TopbWq}}}{P_{\mathrm{TopbWqq}} + P_{\mathrm{TopbWq}} + P_{\mathrm{QCD}}}$$

| Name | Cut | Meaning |
| --- | --- | --- |
| tight | $D_{\mathrm{top}} > 0.9284$ | 0.5% targeted QCD mis-tag (2024 GloParT-v3) |
| band | $0.8571 < D_{\mathrm{top}} < 0.9284$ | between medium (1% mis-tag) and tight |
| leptonic-top veto | $f_{lep} = L/(H+L) < 0.59$ on both jets | $H$ = TopbWqq + TopbWq, $L$ = TopbWev + TopbWmv + TopbWtauhv |

The two leading-$p_T$ AK8 jets form the candidate; jet0 is the one with the higher
score. Pass: jet0 tight, jet1 tight with $105 < m_{SD} < 210$ GeV. Fail: jet0 tight,
jet1 in the band with the same $m_{SD}$ window. Both jets need $f_{lep} < 0.59$.
Central/forward split at $|y_{j_0} - y_{j_1}| < 1$ (`ttbarprocessor.py:998`).

Code: `ttbarprocessor.py:84` (`_FLEP_CUT`), `:93-125` (`_TAGGER_WPS`), `:315-329`
(`_tscore`), `:888-917` (jet0/jet1, tag and antitag masks), `python/categories.py:5-35`.
All Run-3 IOVs use the 2024 thresholds. The SF JSON writes the band as
[0.8571, 0.9284); the code uses strict inequalities on both sides.

The $f_{lep}$ cut is there because $D_{\mathrm{top}}$ ignores the leptonic-top heads, so
a jet holding a leptonic top (lepton and b inside the cone) can pass the tag. 0.59 is
the tightest value that keeps ≥ 99% of fully merged hadronic tops in every pT × tight/band
row of the MC (skimmer item 33, adopted 2026-10-02).

## Measurement: semileptonic $t\bar t$ tag-and-probe

Repository `toptag-sf-derivation` (README, PLAN.md). Method of CMS DP-2025/010.

- Tag side: one muon (HLT_Mu50 or HLT_IsoMu24), a b-tagged AK4 jet near it, JME
  jet-veto map. Full 2024 data, 109.95 fb⁻¹.
- Probe: the highest-$p_T$ AK8 jet in the hemisphere opposite the muon, PUPPI Tight ID,
  $|y| < 2.4$, $p_T > 400$ GeV, $105 < m_{SD} < 210$ GeV. No tag cut defines the probe.
- The $t\bar t$ MC probes are split by gen containment within ΔR 0.8: fully merged (b
  and both W quarks), partially merged, rest.
- Fit: the probe $m_{SD}$ spectrum in three score categories (pass ≥ 0.9284, band
  [0.8571, 0.9284), fail) at once, in Combine, per probe-pT bin. Category
  normalisations float, so the composition comes from data; rest tag rates are fixed;
  the jet mass scale starts at 0.998 and is profiled as a shape nuisance. The POIs are
  the fully merged tight SF and band SF.
- Objects are the v1.2 ones: V5/JRV2 JEC, $m_{SD}$ rebuilt from re-corrected subjets,
  Tight ID, $|y| < 2.4$, veto map. 20,938 data probes.
- Uncertainty per bin: stat ⊕ exact JES/JER (variation ntuples) ⊕ JMR 2% ⊕ pileup ⊕
  model. The breakdown per bin is in the SF JSON.

T&P commit `e997fed` (branch `aritra`), result
`results/outputs/ntuples_2024_v12_fits/band_final_s998.json` (sha256 `edc0fdeb…`).

## Values (2024, `data/toptag/ttag_sf_v1.2.json`)

pT bin edges 400 / 480 / 600 GeV, last bin open. List index 0 (< 400 GeV) is a filler
and never used.

| $p_T$ [GeV] | tight SF (jet0, tagged jet1) | band SF (antitag jet1) |
| --- | --- | --- |
| 400–480 | 0.816 +0.032 / −0.032 | 1.286 +0.100 / −0.097 |
| 480–600 | 0.822 +0.052 / −0.050 | 1.254 +0.130 / −0.126 |
| 600–800 | 0.821 +0.075 / −0.072 | 1.50 ± 0.30 (interim, `antitag_hipt`) |
| ≥ 800 | 0.821 +0.075 / −0.072 | 1.00 ± 0.50 (interim, `antitag_hipt`) |
| > 1200 | 0.821, uncertainty doubled | 1.00 ± 0.50 (no doubling) |

An 800+ GeV check bin gives a tight SF of 0.909 +0.28 / −0.26, consistent with the
600+ value, and is not applied.

### High-pT band SF (interim since 2026-10-02, `6ed19bc`)

The 600+ band fit gives 1.578, a measurement dominated by 600–800 GeV probes. The
four-bin fit gives 1.67 (600–800) and 0.50 (800+, Combine 68% interval [0.18, 0.88]).
Two reviews (Fable 5.1, GPT-6-Astra) found the pattern unstable (50 GeV sub-bins 1.81 /
0.95 at 600–700 GeV) and the T&P band sample too soft (17% of probes above 800 GeV
against 37% in the analysis). So band jets ≥ 600 GeV take 1.50 ± 0.30 (600–800) and
1.00 ± 0.50 (≥ 800) with one nuisance, `ttag_band_hipt`, and no doubling, until the
T&P is re-run.

### $f_{lep}$ cut SF (since 2026-10-02, `c34dbd1`)

Factorised: $\mathrm{SF}_{\mathrm{total}} = \mathrm{SF}_{\mathrm{TopvsQCD}} \times \mathrm{SF}_{flep}$,
with $\mathrm{SF}_{flep} = \varepsilon_{\mathrm{data}}/\varepsilon_{\mathrm{MC}}$ and
$\varepsilon = P(f_{lep} < 0.59 \mid \text{score category})$ for fully merged tops.

| Block | $p_T$ [GeV] | nominal | up | down | $\varepsilon_{\mathrm{MC}}$ |
| --- | --- | --- | --- | --- | --- |
| tag | 400–480 | 0.99899 | 1.00135 | 0.99624 | 0.99508 |
| tag | 480–600 | 0.99456 | 0.99825 | 0.99003 | 0.99690 |
| tag | ≥ 600 | 0.99985 | 1.00384 | 0.99140 | 0.99617 |
| band | ≥ 400 (inclusive) | 0.99197 | 1.00061 | 0.98182 | 0.99145 |

Deviations doubled above 1200 GeV (tag) and 600 GeV (band); up capped at
$1/\varepsilon_{\mathrm{MC}}$. T&P `d98a7f9`, `results/outputs/flep_sf_2024_consumer.json`.

### 2026 drift (since 2026-10-03, `5de1df9`)

2026 uses the 2024 SFs, but a joint 2024 + 2026 T&P fit with the shared Summer24 MC
finds a lower real-top efficiency in 2026 data. The 2024 SFs of fully merged jets are
multiplied by the 2026/2024 ratio (T&P `f0cc036`, `outputs/drift_joint_2026_summary.json`):

| $p_T$ [GeV] | tight ratio | 2026 tight SF | band ratio |
| --- | --- | --- | --- |
| 400–480 | 0.943 ± 0.055 | 0.769 | 0.946 ± 0.103 (all pT) |
| 480–600 | 0.827 ± 0.059 | 0.679 | |
| ≥ 600 | 0.816 ± 0.098 | 0.670 | |

The bin-to-bin and tight-band correlations of the ratios are small ($|\rho| \leq 0.28$,
matrix in the JSON). Most of the deficit sits in PromptReco CMSSW_16_0_6 (runs
403441–403684, 4.9 fb⁻¹, ratio 0.73 ± 0.06). Without those runs the T&P gives 0.996 /
0.853 / 0.824 and band × 0.876. The runs stay in: the per-run QCD tag rate in the
hadronic data shows no step there, which rules out a shift of the tagger score
(skimmer item 40). 2025 shows no drift (2025/2024 inclusive 0.996 ± 0.027).

## How the SFs are applied

`python/weights.py`, table loaded from `data/toptag/ttag_sf_v1.2.json` (`:13`).

- **Scope.** Only jets gen-matched as fully merged tops (b and both W quarks within
  ΔR 0.8, the T&P definition) get an SF (`applies_to: fully_merged`); all other jets
  keep SF 1 (`:89-95`, `e975cdc`).
- **Which SF.** jet0 and a tagged jet1 take the tight SF; an antitag jet1 takes the
  band SF below 600 GeV and the `antitag_hipt` SF above (`:239-302`).
- **Doubling.** Beyond `pt_measured_max` = 1200 GeV a jet keeps the top-bin SF with its
  deviation doubled, same nuisance (`:257-272`, `571526c`). The 600+ bin has median
  probe pT 680 GeV, 90% below 870 GeV, while multi-TeV Z′ jets reach 1.5–2 TeV.
- **Years.** 2022preEE/postEE, 2023preBPix/postBPix, 2025 and 2026 alias the 2024
  entry (`aliases` in the JSON). 2026 also gets the drift factors.

## Nuisances

| Nuisance | Varies | Correlation | Code |
| --- | --- | --- | --- |
| `ttag_pt1`, `ttag_pt2`, `ttag_pt3` | Tight and band SF of one pT bin together (Run-2 scheme, one total per bin) | between jets, $t\bar t$ and Z′, all years | `weights.py:284-291` |
| `ttag_band_hipt` | Band SF ≥ 600 GeV (both sub-bins together) | all years | `weights.py:293-302` |
| `ttag_nonmerged` | ±25% per tagged or antitag jet that is not fully merged, $(1 \pm 0.25)^{n}$ | all years | `weights.py:213-237` |
| `ttag_flep_tag` | SF_flep of jet0 and a tagged jet1, all pT bins together | all years | `weights.py:150-154` |
| `ttag_flep_band` | SF_flep of an antitag jet1 | all years | `weights.py:152-155` |
| `ttag_drift_pt1/2/3` | 2026 tight ratio per pT bin, doubled above 1.2 TeV | 2026 only | `weights.py:157-211` |
| `ttag_drift_band` | 2026 band ratio, all pT, no doubling | 2026 only | `weights.py:157-211` |
| `ttag_yr22`, `ttag_yr23` | lnN 1.25 on the signal: SF transfer to Summer22/23 MC | per year | bgestimation `make_run3_configs.py:44-47` |

The ±25% for non-merged jets is a flat choice (decision of 2026-10-01). Before the
$f_{lep}$ cut, 93–95% of the non-merged tagged or antitag Z′ jets at 2 TeV held the W
lepton.

## Open issues

- **Band SF above 600 GeV** is interim: the T&P re-run with a Poisson likelihood, 2025
  data, $f_{lep}$ on the probes and a constrained partially merged component is to
  replace it. The Run-3 masked fit pulls `ttag_band_hipt` by −1.7σ (tracker
  item `ttbar-norm-run3`, 2026-10-22).
- **Tight/band covariance.** The T&P fit correlation is not used; tight and band SF of a
  bin move together in `ttag_pt<i>`.
- **2022/2023.** No T&P on Summer22/23 MC. The flat 25% `ttag_yr22/23` on the signal is
  provisional, and $t\bar t$ has no equivalent term (its per-year rate floats).
- **2026.** Re-measure the drift on the full certified 2026 data; decide on the
  CMSSW_16_0_6 runs.
- **Per-year working points.** 2022–2026 use the 2024 thresholds.
- **Non-merged jets.** ±25% is a flat assignment, not a measurement.
- **Electron channel.** The T&P uses muons only.

## Detailed studies

- `toptag-sf-derivation`: README (method, workflow, invariants), PLAN.md progress log
  (2026-09-24 band fit, 2026-10-02 SF_flep, 2026-10-03 drift and release periods).
- Skimmer `CHANGELOG_v1.1_to_v1.2.md`: items 1, 7, 18, 31–36, 40 and the decision log.
- Skimmer `plots/out/ttag2026/` (drift reviews), `plots/out/flepsf/` (SF_flep reviews).
- research-notes `topics/glopartv3_toptag_sf_p0_2024.md`,
  `topics/glopartv3_flep_sf_2024.md`, `topics/ttbarhadronic_leptonic_tops_in_toptag.md`.
- ai-wiki `wiki/topics/toptag_sf_measurement.md`.

## History

| Date | State | Commit |
| --- | --- | --- |
| before 2026-07-31 | No measured 2024 SF; a flat placeholder (0.75 cen / 0.90 fwd) in the fit | toptag-sf-derivation `PLAN.md` §1 |
| 2026-07-31 | First T&P SFs (pass/fail fit, pre-v1.2 objects, per-pT thresholds): tight 0.945 / 0.818 / 0.880. The antitag jet got the loose SF 1.046 / 0.926 / 0.947. Only `ttag_pt1` reached the templates | skimmer `32cdded`, `data/toptag/ttag_sf_v1.1.json` |
| 2026-09-24 | `ttag_pt2/3` filled; doubling above 1.2 TeV | `d664e04`, `571526c` |
| 2026-09-24 | Re-measured on the v1.2 objects at the analysis cuts; antitag jet gets its own band SF | `3d16500` |
| 2026-10-01 | SFs only on fully merged tops; `ttag_nonmerged` | `e975cdc`, `f3c1965` |
| 2026-10-02 | $f_{lep} < 0.59$ in the selection; interim high-pT band SF; SF_flep | `c10cb05`, `6ed19bc`, `c34dbd1` |
| 2026-10-03 | 2026 drift | `b867fd4`, `5de1df9` |

The 400–480 GeV drop from 0.945 to 0.816 comes from the JES change: a one-at-a-time
refit at 440–480 GeV goes 0.874 (old) → 0.865 (new cuts) → 0.851 (rebuilt $m_{SD}$) →
0.762 (V5 JES). The old data AK8 $p_T$ read high just above the merging threshold
(skimmer item 7).

The band SF is measured directly. Building it from the medium and tight SFs,
$(\mathrm{SF}_M\varepsilon_M - \mathrm{SF}_T\varepsilon_T)/(\varepsilon_M - \varepsilon_T)$,
subtracts two nearly equal numbers (about 10% of tops sit in the band) and gives
±0.8–1.2.
