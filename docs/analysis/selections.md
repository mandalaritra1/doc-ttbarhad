# Selections

The event selection of skimmer v1.2.1 (the inputs of the Run-3 2022–2026 fit). The same
cuts apply in every year; payloads differ per year where noted.

## Event selection

| Step | Requirement | Notes |
| --- | --- | --- |
| Lumi mask (data) | Golden JSON per year | 2022 `Cert_Collisions2022_355100_362760_Golden.json`, 2023 `..._366442_370790_...`, 2024 `..._378981_386951_...`, 2025 `..._391658_398903_...`, 2026 `Cert_Collisions2026_401624_403937_golden.json` |
| Trigger | `HLT_PFHT1050` | Data and MC, every year. No trigger SF (see below). A missing path stops the job |
| Event activity | $H_T > 1400$ GeV | Scalar sum of corrected AK4 PUPPI jets with $p_T > 30$ GeV, $|\eta| < 3.0$ |
| MET filters | 8 flags | goodVertices, globalSuperTightHalo2016Filter, EcalDeadCellTriggerPrimitiveFilter, BadPFMuonFilter, BadPFMuonDzFilter, hfNoisyHitsFilter, eeBadScFilter, ecalBadCalibFilter |
| Jet veto map | Reject the event if any eligible AK4 jet is in a vetoed $\eta$–$\phi$ cell | Eligible: AK4 PUPPI, $p_T > 15$ GeV, TightLepVeto ID, EM fraction < 0.9. JME payload per year |
| AK8 jets | ≥ 2 with $p_T > 400$ GeV, $|y| < 2.4$, PUPPI Tight ID | Rapidity, not $\eta$. Jet ID from the JME `jetid.json.gz` (NanoAODv15 has no `jetId` branch) |
| Candidate pair | The two leading-$p_T$ AK8 jets; $j_0$ = higher top score | |
| Back-to-back | $|\Delta\phi(j_0, j_1)| > 2.1$, both jets with two soft-drop subjets | |

Jets are corrected before any cut: the JEC is re-applied to data and MC with each year's
JME payload (data with the run-dependent residuals), MC jets are smeared for JER, and the
soft-drop mass is rebuilt from the re-corrected subjets. Details on
[Systematic uncertainties](uncertainties.md#jet-energy-and-jet-mass).

The veto map is the largest single loss after $H_T$: about 18% of high-$H_T$ events, in
data and signal alike (skimmer changelog item 2). The map covers about 2% of $\eta$–$\phi$,
but high-$H_T$ events have many eligible jets.

**Trigger.** No trigger scale factor is applied: $H_T > 1400$ GeV is taken to be on the
`HLT_PFHT1050` plateau, as argued in the Run-2 analysis. A Run-3 efficiency measurement
was deferred by decision on 2026-08-15 and stays an open item.

## Top tagging

GloParT-v3 TopvsQCD, mass-decorrelated:

$$D_{\mathrm{top}} = \frac{P_{\mathrm{TopbWqq}} + P_{\mathrm{TopbWq}}}{P_{\mathrm{TopbWqq}} + P_{\mathrm{TopbWq}} + P_{\mathrm{QCD}}}$$

| Name | Cut | Use |
| --- | --- | --- |
| tight | $D_{\mathrm{top}} > 0.9284$ (0.5% targeted QCD mis-tag) | $j_0$ always; $j_1$ in Pass |
| band | $0.8571 < D_{\mathrm{top}} < 0.9284$ (medium to tight) | $j_1$ in Fail |
| leptonic-top veto | $f_{lep} = L/(H+L) < 0.59$ | both jets, Pass and Fail |

$H$ = TopbWqq + TopbWq, $L$ = TopbWev + TopbWmv + TopbWtauhv. $D_{\mathrm{top}}$ ignores
the leptonic-top heads, so a jet that holds a leptonic top (lepton and b inside the cone)
can pass the tag; the $f_{lep}$ cut removes those. 0.59 is the tightest value that keeps
≥ 99% of fully merged hadronic tops in every $p_T$ × tight/band row of the simulation.

The thresholds were derived on 2024 QCD MC. 2022, 2023, 2025 and 2026 use the 2024 values.
Scale factors: [Top-tag scale factors](top-tag-scale-factors.md).

## Pass, Fail and rapidity categories

| Region | $j_0$ | $j_1$ |
| --- | --- | --- |
| **Pass** (`2t`) | tight, $f_{lep} < 0.59$ | tight, $f_{lep} < 0.59$, $105 < m_{SD} < 210$ GeV |
| **Fail** (`at`) | tight, $f_{lep} < 0.59$ | band, $f_{lep} < 0.59$, $105 < m_{SD} < 210$ GeV |

$j_0$ has no mass window: its $m_{SD}$ is the fit's jet-mass axis. Fail is a score band,
not everything below tight.

Each region is split by $\Delta y = y(j_0) - y(j_1)$: central $|\Delta y| < 1$, forward
$|\Delta y| \geq 1$. The four categories are `2tcen`, `2tfwd`, `atcen`, `atfwd`
(anacat 2, 3, 0, 1).

Code: `ttbarprocessor.py` (`_TAGGER_WPS`, `_FLEP_CUT`, tag and antitag masks),
`python/categories.py`, `python/jets.py` (AK8 and AK4 selection, $H_T$),
`python/corrections.py` (golden JSON, MET filters, veto map, jet ID).

## History

| Date | Change | Reference |
| --- | --- | --- |
| 2026-04-16 | Run-2 tagger thresholds replaced by the Run-3 GloParT-v3 formula and thresholds | research-notes bug `globalt3_wp_cutoff_values_wrong` |
| June 2026 | Pages documented $H_T > 1500$ GeV and the medium WP (0.8571) as the tag, with Fail = loose to medium. The June 2024 inputs in fact carried a stale loose placeholder WP | [June selections](../archive/2026-06/selections.md) |
| 2026-07-23 | Stale WP found; tight WP 0.9284 production (2024, 2025) | skimmer tight-WP production 2026-07-23/24 |
| 2026-09-24 | Skimmer v1.2: jet veto map, AK8 PUPPI Tight ID, JEC re-applied to data, $m_{SD}$ rebuilt from subjets, missing trigger path raises, Fail = band [0.8571, 0.9284) with its own SF | skimmer `9e405c1`, `27d8c36`, `77fa52e`, `e922747`, `8a078df`, `3d16500` |
| 2026-10-02 | $f_{lep} < 0.59$ on both jets, always on (v1.2.1) | skimmer `5b8d151`, `c10cb05` |
| 2026-10-03 | 2026 (eras B, D) added with the same selection | skimmer changelog item 38 |

$H_T > 1400$ GeV is the processor default (`htCut=1400`); the June pages' 1500 GeV did not
match it.

## Open items

- Trigger efficiency in Run-3 data (deferred).
- Per-year top-tag working points (all years use the 2024 thresholds).
- Control and validation regions beyond Pass/Fail × cen/fwd are not defined yet.
