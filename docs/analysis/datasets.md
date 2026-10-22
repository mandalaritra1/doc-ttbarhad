# Datasets

All inputs are NanoAODv15, which carries the GloParT-v3 (`globalParT3_*`) branches. The
sample maps are `data/nanoAOD/*.json` in the skimmer; luminosities are in `python/lumi.py`.

## Data and luminosity

JetMET primary datasets (`JetMET` in 2022, `JetMET0` and `JetMET1` from 2023), certified
with the golden JSON of each year ([Selections](selections.md)).

| Year | IOV | Eras | Processing | Luminosity [fb⁻¹] |
| --- | --- | --- | --- | --- |
| 2022 | preEE | C, D | `NanoAODv15` re-NANO | 7.98 |
| 2022 | postEE | E, F, G | `NanoAODv15` re-NANO | 26.67 |
| 2023 | preBPix | B, C | `NanoAODv15` re-NANO (`_v2`–`_v4` for some blocks) | 17.794 |
| 2023 | postBPix | D | `NanoAODv15` re-NANO | 9.451 |
| 2024 | | C–I | `MINIv6NANOv15` | 109.95 |
| 2025 | | C–G | `PromptReco` (v1, v2) | 110.37 |
| 2026 | | B, D | `PromptReco-v1` | 25.31 |
| **Total** | | | | **307.5** |

Luminosity sources:

- 2024: golden-JSON certified value. 2022 and 2023: the values used since the start,
  marked preliminary in `python/lumi.py`; kept unchanged on 2026-09-24 because the PPD
  tables had not moved.
- 2025: PPD Run3-2025 table on the PdmV Run-3 TWiki (version of 20 Jan 2026), C 21.56 +
  D 25.82 + E 14.05 + F 26.69 + G 22.25. Reproduced per era with
  `brilcalc --normtag normtag_BRIL` on `Cert_Collisions2025_391658_398903_Golden.json`.
  Era B (0.25 fb⁻¹) is not used because the PPD table starts at C.
- 2026: PdmV 2026 table (preliminary), B 15.28 + D 10.03. Era A and the low-pileup era C
  are not used.

Luminosity uncertainties in the fit: 2022 1.4%, 2023 1.3%, 2024–2026 2% (placeholders),
uncorrelated between years.

## Simulation

| Process | 2022 | 2023 | 2024 | 2025 | 2026 |
| --- | --- | --- | --- | --- | --- |
| $t\bar t$ (TTto4Q) | Summer22 / Summer22EE NanoAODv15 | Summer23 / Summer23BPix NanoAODv15 | Summer24 NanoAODv15 | 2024 output × lumi ratio (1.0038) | Summer24, run with the 2026 settings |
| $Z' \to t\bar t$ | Summer24 stand-in | Summer24 stand-in | Summer24 | 2024 output × lumi ratio | Summer24, run with the 2026 settings |
| QCD multijet | not used in the fit (data-driven) | | | | |

- **$t\bar t$**: `TTto4Q_TuneCP5_13p6TeV_powheg-pythia8`, σ = 419.9 pb (924 pb NNLO
  inclusive × 0.4544 all-hadronic BR, `data/nanoAOD/xsec_pb.json`).
- **Z′**: `ZPrimeToTT_Par-M-<M>-W-<Γ>_TuneCP5_13p6TeV_madgraph-pythia8`,
  `RunIII2024Summer24NanoAODv15`, widths 1/10/30%, 22 masses from 0.4 to 9 TeV. Every mass
  is normalised to σ = 1 pb; the limit is quoted against a theory line on
  [Results](results.md).
- **2025** has no own $t\bar t$ or Z′ MC (no Winter25/Summer25 samples in DAS on
  2026-09-24). Following the PPD recommendation, the 2024 Summer24 output stands in,
  scaled by $L_{2025}/L_{2024}$ (decision 2026-09-24).
- **2026** MC is the Summer24 sample run with `--iov 2026`: 2026 JEC, derived 2026 pileup
  weights and the 2026 top-tag drift (2026-10-03).
- **2022/2023 Z′**: there is no NanoAODv15 Z′ for 2022/23, so the Summer24 sample stands in
  with derived data22/23 ÷ Summer24 pileup weights (2026-10-02). A private re-NANO study
  (W1%, 2/4/6 TeV) found the shapes year-independent and the 2022/23 efficiency 1.16–1.56×
  the 2024 value, so the stand-in normalisation is conservative there (decision 2026-10-01).
  An official NanoAODv15 request goes through the B2G MC contact.
- QCD MC (`QCD_PT-*` and `QCD-4Jets_HT-*`) is kept for validation only; the fit takes QCD
  from data.

Signal-region data stay blinded in the Pass top-mass window.

## History

| Date | Change | Reference |
| --- | --- | --- |
| June 2026 | 2024 data (and a 2023 NanoAODv12 inventory), $t\bar t$ at 350.6 pb | [June datasets](../archive/2026-06/datasets.md) |
| June 2026 | 2025 data wired with lumi-scaled 2024 MC (110.59 fb⁻¹ then) | research-notes `ttbarhadronic_2025_combined_fit_wiring` |
| 2026-06-21 | 2022/2023 NanoAODv15 filesets added (era-split keys) | skimmer `data/nanoAOD/*.json` |
| 2026-07-23 | $t\bar t$ σ corrected 350.6 → 419.9 pb | skimmer `b511d65` |
| 2026-09-24 | 2025 lumi 110.59 → 110.37 fb⁻¹ (PPD table, newest golden JSON); 2022–2024 golden JSONs refreshed after the 2026 tracker-ML review; one lumi table `python/lumi.py` | skimmer `d4b719c`, `f6ff4c1`, `7219cba` |
| 2026-09-24 | 2025 and 2026 MC = 2024 Summer24 × lumi | decision log |
| 2026-10-01/02 | 2022/2023 Z′ = Summer24 stand-in, with derived pileup weights | skimmer `c7c1365` |
| 2026-10-03 | 2026 eras B + D added (25.31 fb⁻¹); 2026 MC run with `--iov 2026` | skimmer changelog item 38, `5de1df9` |

## Open items

- Official NanoAODv15 Z′ for 2022/2023.
- Final luminosities and their correlations for 2024–2026.
- 2026 on the full certified dataset.
