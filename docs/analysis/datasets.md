# Datasets

## Objective

Track the data and simulation inputs used for the current presentation cycle.

## Dataset Roles

| Sample group | Role in the analysis |
| --- | --- |
| Data | Primary input for validation and data-driven QCD background constraints |
| QCD multijet | Used for studies and closure checks, with final normalization/shape constrained from data |
| SM `ttbar` | Simulation-based background component |
| Signal models | Resonance benchmarks used to validate sensitivity and result interpretation |

## Run And Campaign Bookkeeping

The active workflow is Run-3 oriented, while older Run-2 analysis material
serves as methodological context. Dataset labels, campaign names, and tagger
branches should be kept explicit on plots and tables because the top-tagger
definition is campaign-dependent.

| Item | Meeting note |
| --- | --- |
| Data-taking year | State the exact year or IOV on every yield/plot table |
| Data stream | Use the analysis-approved hadronic trigger/data stream label |
| MC campaign | Include campaign and NanoAOD version when available |
| Signal mass points | List only the mass points shown in the current talk |

## Current Takeaway

Dataset documentation should make it obvious which year, campaign, and sample
version produced each figure.

## Open Items

- Fill the table with the exact current JSON/sample-map names.
- Add event counts after the baseline selection for the next review.
