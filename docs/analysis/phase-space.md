# Phase Space

## Objective

Define the analysis phase space in a compact, presentation-ready way.

## Baseline Region

The working phase space follows the boosted all-hadronic topology:

| Requirement | Presentation definition |
| --- | --- |
| Final state | Fully hadronic `ttbar` candidate events |
| AK8 candidates | At least two high-pt AK8 jets |
| AK8 acceptance | Central detector acceptance, using rapidity-based selection |
| Event activity | High AK4-based HT |
| Topology | Two selected AK8 jets separated back-to-back in azimuth |
| Candidate mass | Top-candidate soft-drop mass used for signal/control regions |

## Region Logic

| Region type | Purpose |
| --- | --- |
| Signal-like pass region | Events where both selected top candidates pass the top-tag selection |
| Fail/control region | Events used to constrain QCD multijet behavior |
| Sideband regions | Top-mass sidebands used to study transfer behavior |
| Central category | Small rapidity separation between the two top candidates |
| Forward category | Complement of the central rapidity category |

## Current Takeaway

The phase-space definition should stay stable across meetings so that yield,
validation, and background-estimation updates are comparable.

## Open Items

- Add the exact Run-3 values used in the current production pass.
- Add a compact region map once the final plotting convention is chosen.
