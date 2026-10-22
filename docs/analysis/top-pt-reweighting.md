# Top-pT reweighting

## What it is and why

POWHEG + Pythia8 $t\bar t$ predicts a harder top-quark $p_T$ spectrum than the data and
than NNLO QCD + NLO EW. The TOP PAG corrects the simulation with a per-top scale factor
and treats the correction itself as the uncertainty. Our $t\bar t$ background sits at
$p_T$ 400–2000+ GeV, where the correction reaches 25%, so it moves both the $t\bar t$
rate and the $m_{t\bar t}$ shape in Pass and Fail.

It applies to the $t\bar t$ MC (TTto4Q) only. The Z′ signal is not reweighted, and the
TWiki excludes single top and $t\bar t X$.

## Current recipe (since 2026-10-02, `4b2a2b6`)

Per top, on the parton-level top after radiation and before decay (`GenPart` with the
`isLastCopy` flag and `abs(pdgId) == 6`):

$$\mathrm{SF}(p_T) = \big[\,0.103\, e^{-0.0118\, p_T} - 0.000134\, p_T + 0.973\,\big] \times \big[\,0.991 + 0.000075\, p_T\,\big].$$

- First factor: NNLO QCD + NLO EW / POWHEG + Pythia8 (CP5) at 13 TeV, fitted up to
  2 TeV and held at its 2 TeV value above.
- Second factor: 13 → 13.6 TeV extrapolation from the ratio of the POWHEG (hvq) top
  $p_T$ at 13.6 and 13 TeV, fitted up to 1 TeV and held at its 1 TeV value above.
- Event weight $w = \sqrt{\mathrm{SF}(t)\,\mathrm{SF}(\bar t)}$; events without gen
  tops get 1.
- The weight is not renormalised. It lowers the $t\bar t$ yield as well as softening
  the spectrum.

Uncertainty, nuisance `toppt` ($t\bar t$ only, one nuisance for all years): up = no
reweighting ($w = 1$), down = $2w - 1$, i.e. the full correction, symmetrised
(TOP-23-008). With $w \geq 0.752$ the down weight stays positive.

Code: `python/corrections.py:489-527` (`top_pt_sf`, `GetTopPtWeight`) and
`python/weights.py:57-65`. The weight is added before the `--noSyst` early return, so
noSyst outputs carry it too. Tests: `tests/test_toppt.py`.

## Old recipe (v1 to v1.2)

`git show 4b2a2b6^:python/corrections.py`, function `pTReweighting` (line 678),
called in `weights.py` as `pTReweighting(jet0.pt, jet1.pt)`:

$$w_{\mathrm{old}} = \sqrt{e^{0.0615 - 0.0005\, p_{T,j_0}}\; e^{0.0615 - 0.0005\, p_{T,j_1}}}$$

This is the Run-2 data/POWHEG fit, evaluated on the two reconstructed AK8 jets,
with no cap and no uncertainty.

## Weight per top

Computed with the skimmer's `top_pt_sf` (new) and the old formula. "Event, both tops at
this $p_T$" is the event weight when $t$ and $\bar t$ have the same $p_T$. The old
formula took the reco AK8 jet $p_T$, the new one takes the gen top $p_T$, so the columns
compare the functions, not the same events.

| $p_T$ [GeV] | NNLO/POWHEG factor | 13 → 13.6 factor | new SF | `toppt` down ($2w-1$, both tops at this $p_T$) | old $e^{0.0615 - 0.0005\,p_T}$ |
| --- | --- | --- | --- | --- | --- |
| 400 | 0.920 | 1.021 | 0.940 | 0.879 | 0.871 |
| 500 | 0.906 | 1.029 | 0.932 | 0.864 | 0.828 |
| 600 | 0.893 | 1.036 | 0.925 | 0.850 | 0.788 |
| 800 | 0.866 | 1.051 | 0.910 | 0.820 | 0.713 |
| 1000 | 0.839 | 1.066 | 0.894 | 0.789 | 0.645 |
| 1200 | 0.812 | 1.066 | 0.866 | 0.732 | 0.584 |
| 1500 | 0.772 | 1.066 | 0.823 | 0.646 | 0.502 |
| 2000 | 0.705 | 1.066 | 0.752 | 0.503 | 0.391 |

Above 2 TeV the new SF stays at 0.752; the old one keeps falling (0.305 at 2.5 TeV).

Effect on the templates: mean $t\bar t$ weight 0.75 → 0.92, so TTto4Q +23% in Pass and
Fail; a one-chunk TTto4Q check gave `toppt` ±9% (skimmer changelog item 35 and the
validation log, 2026-10-02).

A figure of the two SFs is made by `plots/plot_methods_v121.py` (`6d01296`).

## Why it changed

- The TOP PAG TWiki TopPtReweighting defines the correction on the parton-level
  `isLastCopy` top $p_T$, not on reconstructed jets, and holds the Run-2 data/POWHEG
  function at 500 GeV. Our use, uncapped on reco AK8 $p_T$ up to 2+ TeV, was outside
  its validity.
- It had no uncertainty.
- The TOP PAG answer on CMS Talk (October 2025) says to use the TOP-23-008 treatment in
  Run 3. TOP-26-002 uses it as well.

The decision was taken on 2026-10-02 (skimmer changelog decision log, item 35), with a
to-do to check data/MC in a $t\bar t$-rich control region, which the TWiki asks for
when $t\bar t$ is only a background.

References (from the `top_pt_sf` docstring):

- TOP PAG TWiki TopPtReweighting: <https://twiki.cern.ch/twiki/bin/viewauth/CMS/TopPtReweighting>
- CMS AN-2024/019 v9, sec. 5.7, eqs. (3)–(4) and Fig. 25 (TOP-23-008, tW at 13.6 TeV):
  <https://cms-alcm.web.cern.ch/notes/CMS-AN-2024-019/AN2024_019_v9.pdf>
- TOP-23-008 paper, sec. 7: arXiv:2409.06444
- TOP PAG recommendation for Run 3 (CMS Talk, Oct 2025):
  <https://cms-talk.web.cern.ch/t/top-pt-reweighting-recommendations-for-run-3-analyses/133877>
- 13 → 13.6 TeV extrapolation from the TOP conveners (CMS PubTalk, TOP-23-008, Apr 2024):
  <https://cms-pub-talk.web.cern.ch/t/new-version-of-the-paper-v6-an-v8-and-supplementary-material/23359>

Run-2 precedent: B2G-25-009 (Run-2 $t\bar t$ resonances) applied no reweighting in the
all-hadronic and 1ℓ channels and covered it with uncertainties.

## Open decision

With the new recipe the $t\bar t$ MC is above the data, and the masked
background-only Run-3 fit pulls `toppt` by −1.55σ, towards its down variation (a stronger
correction), together with `ttag_band_hipt` (−1.7σ) and the per-year $t\bar t$ rates
(tracker item `ttbar-norm-run3`, 2026-10-22). The Run-3 combination keeps the TOP recipe
with a $t\bar t$ rate per year as its working default (decision 2026-10-04). Two
reviews of 2026-10-02 (Fable 5.1 and GPT-6-Astra, skimmer `plots/out/toppt_norm/`)
looked at the nominal choice. The options:

| Option | What | Reviews |
| --- | --- | --- |
| (a) | Keep the TOP recipe, let `toppt` and the 30% per-year rate absorb the rest | Astra: keep provisionally, validate with (d) before final results. Fable: no, the fit starts with $t\bar t$ above data |
| (b) | Add the TWiki data/NNLO residual $e^{0.0416 - 0.0003\,p_T}$ per top (0.77 at 1 TeV), held at 1 TeV, with a second nuisance of 100% of the residual | Fable: interim choice. Astra: reject without control-region evidence |
| (c) | No reweighting (Run-2 B2G practice) with a larger uncertainty | Astra: defensible alternative. Fable: no, the Fail region is $t\bar t$-dominated so the fit cannot separate $t\bar t$ from QCD |
| (d) | Own data/MC correction from the semileptonic tag-and-probe probe $p_T$ spectrum (tag-independent, pass + fail probes), with an extrapolation uncertainty above the last populated bin | Both: the eventual answer |

What (d) has so far (toptag-sf-derivation `PLAN.md`, 2026-10-02, a control region
orthogonal to the search): with the probe $p_T$ standing in for both top $p_T$s, the
data/MC slope of the probe yield is $b = 4.4 \pm 0.7 \times 10^{-4}$/GeV without the
recipe and $3.7 \pm 0.7 \times 10^{-4}$/GeV with it, compatible with the TWiki
residual ($3.0 \times 10^{-4}$). Above 560 GeV the slope is $1.0 \pm 1.3 \times 10^{-4}$.
The normalisation is not a cross-section statement (no lepton, trigger or b-tag SFs),
and below 560 GeV the slope is confounded by the merged fraction in the $m_{SD}$
window. The exact version with gen top $p_T$ needs a TTbar-only T&P re-production
(`gen_top_pt` added in T&P `8e2d8e7`); it has not run yet.

Points raised in the reviews to carry into the decision:

- The 13 → 13.6 TeV factor raises the weight by 3–7% at high $p_T$, against the trend
  of the data.
- The data JEC change (−4.6% on data AK8 $p_T$ above 400 GeV, skimmer item 16) moves
  the data $m_{t\bar t}$ tail by more than the MC JES nuisance and is partly degenerate
  with a top-pT slope. It can be checked on a QCD-dominated shape (Fail, $m_{SD} < 105$).
- `toppt` covers 100% of the applied correction, not every possible mis-modelling of
  the boosted spectrum. A persistent control-region difference should become a
  residual correction or nuisance, not a wider `toppt`.
- The proper fix for the rate is a $t\bar t$ control-region constraint (bgestimation
  `run3/run3comb/README.md`, known limitations), first priority in the methods deck of
  2026-10-04.

## Detailed studies

- Skimmer `CHANGELOG_v1.1_to_v1.2.md`, item 35 and the 2026-10-02 decision-log entry.
- Skimmer `plots/out/toppt_norm/` (review brief and the two reviews; these contain
  unblinded Pass ratios and are for Aritra only).
- toptag-sf-derivation `PLAN.md`, entry "2026-10-02 — Top-pT data/MC in the T&P",
  `results/toppt_datamc.py`, `outputs/toppt_datamc_2024_proxy.json`.
- ai-wiki `wiki/repos/ttbarhadronic_skimmer.md`.

## History

| Date | Change | Reference |
| --- | --- | --- |
| v1 to v1.2 (to 2026-10-01) | Run-2 data/POWHEG formula on the two reco AK8 jets, no cap, no uncertainty | `git show 4b2a2b6^:python/corrections.py` |
| 2026-10-02 | TOP-23-008 recipe on gen last-copy tops with the `toppt` nuisance (mean $t\bar t$ weight 0.75 → 0.92) | skimmer `4b2a2b6`, changelog item 35 |
| 2026-10-02 | Two reviews of the $t\bar t$ normalisation and top-$p_T$ options | skimmer `plots/out/toppt_norm/` |
| 2026-10-04 | Run-3 combination: TOP recipe with a per-year $t\bar t$ rate as the working default | tracker decision log |
| 2026-10-22 | Still open: fit pulls `toppt` −1.55σ; to be settled with the $t\bar t$ normalisation | tracker `toppt-rw`, `ttbar-norm-run3` |
