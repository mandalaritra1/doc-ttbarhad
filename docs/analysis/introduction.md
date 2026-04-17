# Introduction

## Objective

Summarize the all-hadronic `ttbar` resonance analysis in a form that can be
used directly for group meetings and presentation preparation.

## Analysis Story

The analysis targets heavy resonances decaying to a top-quark pair where both
tops decay hadronically. In the boosted regime, each top candidate is captured
primarily by a large-radius AK8 jet. The core presentation narrative is:

- Select a high-activity all-hadronic event topology.
- Build two boosted top candidates from AK8 jets.
- Split events into top-tag pass and fail regions.
- Validate modeling with Data/MC comparisons.
- Estimate the QCD multijet background from data control information.
- Present results with explicit caveats and readiness status.

## Key Ingredients

| Ingredient | Role |
| --- | --- |
| AK8 jets | Reconstruct boosted hadronic top candidates |
| AK4 jets | Define event activity and HT |
| Top tagger | Separate signal-like boosted tops from QCD-like jets |
| Central/forward split | Track rapidity-dependent behavior |
| Pass/fail regions | Support validation and background prediction |
| `m_tt` | Main resonance-sensitive observable |

## Current Takeaway

The documentation is organized as a meeting track. Each section should answer
one question: what is stable enough to show, what changed since the last
meeting, and what still needs a decision.

## Open Items

- Add the one-slide analysis overview used in the latest presentation.
- Add the current milestone table once the next meeting schedule is fixed.
