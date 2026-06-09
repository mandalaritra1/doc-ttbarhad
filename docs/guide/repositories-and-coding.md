# Repositories and Coding Setup

This page tracks the code repositories used for the all-hadronic `ttbar`
analysis and the current working setup for 2DAlphabet studies.

## Project Repositories

| Repository | Role | Status |
| --- | --- | --- |
| [`TTbarHadronicSkimmer`](https://github.com/mandalaritra1/TTbarHadronicSkimmer) | Main analysis skimmer and project code repository | Primary project repository |

More repository-level conventions can be added here as the project workflow
settles.

## 2DAlphabet Environment

The current 2DAlphabet work area is on LPC:

```bash
/uscms_data/d1/amandal2/2dalphabet
```

The active 2DAlphabet checkout is:

```bash
/uscms_data/d1/amandal2/2dalphabet/CMSSW_11_3_4/src/2DAlphabet
```

### Daily Login

From a local machine, log in to LPC:

```bash
ssh amandal2@cmslpc-el9.fnal.gov
```
Source the CMS environment:
```bash
source /cvmfs/cms.cern.ch/cmsset_default.sh
```

Then bind the real `nobackup` target before entering the CMSSW container:
### Finding your own bind path from `~/nobackup`

On LPC, `~/nobackup` is often not a real directory. It is usually a symlink to a real `/uscms_data/...` location.

First, check it **outside** the Singularity container:

```bash
cd ~
ls -ld nobackup
readlink -f nobackup
```

Then activate the container using - 

```bash
cmssw-el7 --bind /uscms_data/d1/amandal2:/uscms_data/d1/amandal2
```





Inside the container, verify that the bound path is visible:

```bash
ls -ld /uscms_data/d1/amandal2
```

### CMSSW and Python Setup

Inside the container:

```bash
source /cvmfs/cms.cern.ch/cmsset_default.sh

cd /uscms_data/d1/amandal2/2dalphabet/CMSSW_11_3_4/src
cmsenv

source /uscms_data/d1/amandal2/2dalphabet/twoD-env/bin/activate

cd /uscms_data/d1/amandal2/2dalphabet/CMSSW_11_3_4/src/2DAlphabet
```

Optional prompt:

```bash
export PS1='\[\e[1;32m\](twoD-env)\[\e[0m\] \[\e[1;35m\]Singularity\[\e[0m\] \[\e[1;34m\]\W\[\e[0m\] \[\e[1;33m\]\$\[\e[0m\] '
```

### Sanity Checks

Confirm that the expected Python, Combine, and CMSSW environment are active:

```bash
which python3
which combine
echo $CMSSW_BASE
echo $SCRAM_ARCH
```

Check ROOT and Combine:

```bash
python3 - <<'PY'
import ROOT
print("ROOT:", ROOT.gROOT.GetVersion())
print("load combine:", ROOT.gSystem.Load("libHiggsAnalysisCombinedLimit"))
print("RooParametricHist2D:", ROOT.RooParametricHist2D)
PY
```

Check the 2DAlphabet import:

```bash
python3 - <<'PY'
from TwoDAlphabet.twoDalphabet import TwoDAlphabet
print("2DAlphabet import OK")
PY
```

## Historical Mechanical-Test Inputs

!!! note
    This section records the first April 2026 central-only mechanical test. It
    is retained for debugging history and is not the current 2024 F-test
    production path.

Input ROOT files currently live in:

```bash
/uscms_data/d1/amandal2/2dalphabet/CMSSW_11_3_4/src/2DAlphabet/rootfiles
```

The current files are:

```text
rootfiles/data_2024.root
rootfiles/TTbar_2024.root
```

Expected histogram names:

```text
MttvsMtCen2024Pass
MttvsMtCen2024Fail
MttvsMtFwd2024Pass
MttvsMtFwd2024Fail
```

The known central-region axes are:

| Axis | Observable | Binning |
| --- | --- | --- |
| X | top-candidate mass, `mt` | 100 bins, 0 to 500 |
| Y | `mtt` | 92 bins, 800 to 10000 |

## Historical Mechanical-Test Run Sequence

From the 2DAlphabet repository:

```bash
cd /uscms_data/d1/amandal2/2dalphabet/CMSSW_11_3_4/src/2DAlphabet
```

The main generated files are:

| File | Purpose |
| --- | --- |
| `ttbar_2024_cen_config.json` | Minimal central-only 2024 config |
| `run_ttbar_2024_cen.py` | Config and ROOT loading test |
| `run_ttbar_2024_cen_make.py` | Builds the QCD model with `qcd_fail = data_fail - ttbar_fail` and `qcd_pass = qcd_fail * Rpf` |
| `run_ttbar_2024_cen_card.py` | Creates `ttbar2024_cen_fit/flat_rpf_area/card.txt` |
| `run_ttbar_2024_cen_fit.py` | Runs FitDiagnostics through the 2DAlphabet wrapper |

Normal rerun:

```bash
rm -rf ttbar2024_cen_fit

python3 run_ttbar_2024_cen.py
python3 run_ttbar_2024_cen_make.py
python3 run_ttbar_2024_cen_card.py
python3 run_ttbar_2024_cen_fit.py
```

Expected model-building outputs include:

```text
base.root
binnings.p
ledger_df.csv
ledger_alphaObjs.csv
ledger_alphaParams.csv
organized_hists.root
runConfig.json
```

Expected datacard:

```text
ttbar2024_cen_fit/flat_rpf_area/card.txt
```

The current fit result is only a mechanical test because the temporary card
setup does not yet include a real signal process.

## Temporary Datacard Patch

For the current mechanical test only, the generated datacard may need process
IDs converted from floats to integers and then remapped so TTbar is process `0`
and QCD is process `1`.

Inspect the process lines:

```bash
nl -ba ttbar2024_cen_fit/flat_rpf_area/card.txt | sed -n '24,32p'
```

Apply the temporary patch only when needed:

```bash
python3 - <<'PY'
from pathlib import Path

card = Path("ttbar2024_cen_fit/flat_rpf_area/card.txt")
lines = card.read_text().splitlines()

new_lines = []
for line in lines:
    if line.strip().startswith("process"):
        parts = line.split()
        vals = parts[1:]
        try:
            nums = [float(x) for x in vals]
            if all(x.is_integer() for x in nums):
                line = "process " + " ".join(str(int(x)) for x in nums)
        except ValueError:
            pass
    new_lines.append(line)

card.write_text("\n".join(new_lines) + "\n")
print("patched float process IDs to integers")
PY
```

```bash
python3 - <<'PY'
from pathlib import Path

card = Path("ttbar2024_cen_fit/flat_rpf_area/card.txt")
txt = card.read_text()

txt = txt.replace(
    "process 1 1 1 1 1 1 2 2 2 2 2 2",
    "process 0 0 0 0 0 0 1 1 1 1 1 1"
)

card.write_text(txt)
print("patched TTbar as process 0, QCD as process 1")
PY
```

The expected temporary process ID line is:

```text
process 0 0 0 0 0 0 1 1 1 1 1 1
```

This patch should be removed once a real signal ROOT file is included.

## Historical Follow-up List

| Task | Target |
| --- | --- |
| Add signal input | Add `rootfiles/Zprime_2024.root` with the same pass/fail central/forward histogram names |
| Fix process IDs | Use `Zprime = 0`, `TTbar = 1`, and `QCD = 2` |
| Background-only fit | Include the signal process and freeze `r` with `--setParameters r=0 --freezeParameters r` |
| ABCDEF blinding | Include A, B, C, E, F and blind D |
| Region coverage | Add forward region, then combine central and forward |
| Model development | Replace the flat transfer function with `0x1` or `1x1` and add sensible analysis binning |
| Fit validation | Add systematics, goodness-of-fit, signal injection, and impacts |

## Current 2024 BGEstimation Workflow

The active standalone background-fit code reviewed on 2026-06-09 is:

```text
/Users/aritra/Projects/bg_ttbar/src/bgestimation
```

The LPC production commands documented in that repository use:

```text
/uscms_data/d3/amandal2/bg_ttbar/CMSSW_14_1_0_pre4/src/bgestimation
```

with input ROOT files under:

```text
/eos/user/a/amandal/ttbarhad_root_files/2dAlphabetInputs
```

The 2024 configs are:

```text
jsons/config/ttbar_cen2024.json
jsons/config/ttbar_fwd2024.json
```

They define 11 top-candidate-mass intervals and 15 `m_tt` intervals. The
currently configured nominal transfer functions are:

```text
cen2024 = 1x1
fwd2024 = 0x1
```

These are configured defaults; the final selection argument is still under
review.

### Transfer-function scan

```bash
cd /uscms_data/d3/amandal2/bg_ttbar/CMSSW_14_1_0_pre4/src/bgestimation

LOCAL_INPUT="/eos/user/a/amandal/ttbarhad_root_files/2dAlphabetInputs"

python -u fit_ftest.py \
  --preset 2024 \
  --input "$LOCAL_INPUT" \
  --signal RSGluon4000 \
  2>&1 | tee output_ftest_2024.log

python -u results_ftest.py \
  --preset 2024 \
  --signal RSGluon4000 \
  2>&1 | tee output_ftest_results_2024.log
```

The default candidate list is:

```text
0x0 0x1 0x2 1x0 1x1 1x2 2x1 2x2
```

for both `cen` and `fwd`.

### Output layout

```text
ftest/2024/cen/ttbarfits_cen2024_ftest<TF>/
ftest/2024/fwd/ttbarfits_fwd2024_ftest<TF>/
ftest_results/ftest_results_cen2024.csv
ftest_results/ftest_results_fwd2024.csv
ftest_results/FTest_*_2024_{cen,fwd}.png
```

Every tested candidate currently has a background-only
`plots_fit_b/postfit_projy.pdf`.

### Reproducibility notes

- Preserve the `runConfig.json` saved inside every fit area.
- The current `ttbar.py` updates `GLOBAL.path` and `GLOBAL.SIGNAME` in the
  selected config before constructing the fit.
- Save the datacard, saturated-GOF ROOT output, fit logs, and postfit
  diagnostics together.
- Use formal F-test p-values only for verified nested model pairs.
- Validate the script's current `n_bins = 165` treatment against pass/fail and
  blinded-bin counting.
- Correct the inherited `138 fb^-1 (13 TeV)` text in the 2024 postfit plots
  before external presentation.

See [Background Estimation](../analysis/background-estimation.md) for the
current tables, pairwise examples, full postfit projection gallery, and
downloadable presentation PDFs.
