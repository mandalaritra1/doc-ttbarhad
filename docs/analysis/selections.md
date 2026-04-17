# Selections

## Objective

Present the object, event, and category selections used in the analysis flow.

## Selection Sequence

| Step | Purpose |
| --- | --- |
| Event quality and trigger | Keep certified collision events passing the hadronic trigger path |
| AK4 HT selection | Require high event activity |
| AK8 candidate selection | Build two boosted top candidates |
| Back-to-back topology | Suppress non-resonant and poorly reconstructed configurations |
| Subjet/top-candidate quality | Keep candidates compatible with boosted top reconstruction |
| Top-tag pass/fail split | Define signal-like and control-like categories |
| Rapidity category split | Separate central and forward event topologies |

## Category Names

| Category | Meaning |
| --- | --- |
| `2tcen` | Two top-tag pass, central rapidity category |
| `2tfwd` | Two top-tag pass, forward rapidity category |
| `atcen` | Antitag/fail control, central rapidity category |
| `atfwd` | Antitag/fail control, forward rapidity category |

## Presentation Checklist

- Show one cutflow table for the current production.
- Use one category naming convention everywhere.
- Label top-tag working point and tagger branch on plots.
- State whether the table is data, SM background, signal, or combined MC.

## Current Takeaway

The selection page should make the category logic readable without exposing
low-level processor details.

## Open Items

- Add the latest approved cutflow.
- Confirm the top-tag working point text for the next slide round.
