# Datasets

Last updated: 2026-04-17

This analysis searches for heavy resonances decaying to top quark pairs in the all-hadronic final state. The dominant background is QCD multijet production, followed by SM $t\bar{t}$ production.

Signal samples currently use $Z' \to t\bar{t}$ benchmark mass points. The active sample inventory comes from the `data/nanoAOD/*.json` sample maps used by the analysis workflow.

!!! note "Inventory status"
    Cross sections, integrated luminosities, and selected yields still need to be confirmed.

## Data Samples

### 2023

Luminosity: TBD

| Era | Dataset  |
| --- | --- |
| B | `/JetMET*/Run2023B-19Dec2023-v1/NANOAOD` |
| C | `/JetMET*/Run2023C-19Dec2023-v1/NANOAOD` |
| D | `/JetMET*/Run2023D-19Dec2023-v1/NANOAOD` |

### 2024

Luminosity: TBD

| Era | Dataset  |
| --- | --- |
| C | `/JetMET*/Run2024C-MINIv6NANOv15-v1/NANOAOD` |
| D | `/JetMET*/Run2024D-MINIv6NANOv15-v1/NANOAOD` |
| E | `/JetMET*/Run2024E-MINIv6NANOv15-v1/NANOAOD` |
| F | `/JetMET*/Run2024F-MINIv6NANOv15-v2/NANOAOD` |
| G | `/JetMET*/Run2024G-MINIv6NANOv15-v2/NANOAOD` |
| H | `/JetMET*/Run2024H-MINIv6NANOv15-v2/NANOAOD` |
| I | `/JetMET*/Run2024I-MINIv6NANOv15-v*/NANOAOD` |
| I | `/JetMET*/Run2024I-MINIv6NANOv15_v2-v*/NANOAOD` |

## Background Samples

| Process | Year | Sample key | Dataset |
| --- | ---: | --- | --- |
| QCD multijet | 2024 | `inclusive` | `/QCD_Bin-PT-15to7000_Par-PT-Flat_TuneCH3_13p6TeV_herwig7/RunIII2024Summer24NanoAODv15-150X_mcRun3_2024_realistic_v2-v2/NANOAODSIM` |
| SM $t\bar{t}$ | 2023 | `inclusive` | `/TTto4Q_TuneCP5_13p6TeV_powheg-pythia8/Run3Summer23NanoAODv12-JMENano12p5_132X_mcRun3_2023_realistic_v5-v1/NANOAODSIM` |
| SM $t\bar{t}$ | 2024 | `inclusive` | `/TTto4Q_TuneCP5_13p6TeV_powheg-pythia8/RunIII2024Summer24NanoAODv15-150X_mcRun3_2024_realistic_v2-v2/NANOAODSIM` |

## Signal Samples

| Model | Year | Mass point | Dataset |
| --- | ---: | ---: | --- |
| $Z' \to t\bar{t}$ | 2023 | 1000 GeV | `/ZPrimeToTT_Par-M-1000-W-10_TuneCP5_13p6TeV_madgraph-pythia8/Run3Summer23NanoAODv12-130X_mcRun3_2023_realistic_v15-v3/NANOAODSIM` |
| $Z' \to t\bar{t}$ | 2023 | 4000 GeV | `/ZPrimeToTT_Par-M-4000-W-40_TuneCP5_13p6TeV_madgraph-pythia8/Run3Summer23BPixNanoAODv12-130X_mcRun3_2023_realistic_postBPix_v6-v2/NANOAODSIM` |
| $Z' \to t\bar{t}$ | 2023 | 5000 GeV | `/ZPrimeToTT_Par-M-5000-W-500_TuneCP5_13p6TeV_madgraph-pythia8/Run3Summer23NanoAODv12-130X_mcRun3_2023_realistic_v15-v3/NANOAODSIM` |
| $Z' \to t\bar{t}$ | 2024 | 4000 GeV | `/ZPrimeToTT_Par-M-4000-W-40_TuneCP5_13p6TeV_madgraph-pythia8/RunIII2024Summer24NanoAODv15-150X_mcRun3_2024_realistic_v2-v2/NANOAODSIM` |

## Excluded From Main Tables

| Source | Entries | Reason |
| --- | --- | --- |
| `ZPrime1.json` | 2023 mass points 2000, 2500, 3000, 3500, 4500, 6000, 7000 GeV | Empty placeholders with zero files |
| `local.json` | 2024 4000 GeV, 2 files | Local scratch/test files |
| `localData.json` | 2024C, 1 file | Local scratch/test file |

## Notes


- Dataset labels, NanoAOD version, and tagger branch should remain explicit because the Run-3 top tagger definition is campaign-dependent.
- Add event counts after baseline selection once the active processing campaign is frozen.

