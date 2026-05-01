# 2DAlphabet LPC Daily Run Instructions

This note is the **short operational version**: what to do each time you log in, and how to rerun the files/scripts we already generated.

Working base:

```bash
/uscms_data/d1/amandal2/2dalphabet
```

2DAlphabet repo:

```bash
/uscms_data/d1/amandal2/2dalphabet/CMSSW_11_3_4/src/2DAlphabet
```

---

## 1. Log in to LPC

From your local machine:

```bash
ssh amandal2@cmslpc-el9.fnal.gov
```

or whichever LPC node/login alias you normally use.

---

## 2. Enter `cmssw-el7` with the nobackup area bound

Your `~/nobackup` is a symlink to:

```bash
/uscms_data/d1/amandal2
```

Bind the real path before entering the container:

```bash
export SINGULARITY_BINDPATH=/uscms_data/d1/amandal2:/uscms_data/d1/amandal2
cmssw-el7
```

If that does not work, try the Apptainer name instead:

```bash
export APPTAINER_BINDPATH=/uscms_data/d1/amandal2:/uscms_data/d1/amandal2
cmssw-el7
```

Inside the container, check:

```bash
ls -ld /uscms_data/d1/amandal2
```

---

## 3. Set up CMSSW and Python

Inside the container:

```bash
source /cvmfs/cms.cern.ch/cmsset_default.sh

cd /uscms_data/d1/amandal2/2dalphabet/CMSSW_11_3_4/src
cmsenv

source /uscms_data/d1/amandal2/2dalphabet/twoD-env/bin/activate

cd /uscms_data/d1/amandal2/2dalphabet/CMSSW_11_3_4/src/2DAlphabet
```

Optional readable prompt:

```bash
export PS1='\[\e[1;32m\](twoD-env)\[\e[0m\] \[\e[1;35m\]Singularity\[\e[0m\] \[\e[1;34m\]\W\[\e[0m\] \[\e[1;33m\]\$\[\e[0m\] '
```

Example:

```bash
(twoD-env) Singularity 2DAlphabet $
```

---

## 4. Quick sanity checks

```bash
which python3
which combine
echo $CMSSW_BASE
echo $SCRAM_ARCH
```

Check ROOT/Combine:

```bash
python3 - <<'PY'
import ROOT
print("ROOT:", ROOT.gROOT.GetVersion())
print("load combine:", ROOT.gSystem.Load("libHiggsAnalysisCombinedLimit"))
print("RooParametricHist2D:", ROOT.RooParametricHist2D)
PY
```

Check 2DAlphabet:

```bash
python3 - <<'PY'
from TwoDAlphabet.twoDalphabet import TwoDAlphabet
print("2DAlphabet import OK")
PY
```

---

## 5. Input ROOT files

Current input files live here:

```bash
/uscms_data/d1/amandal2/2dalphabet/CMSSW_11_3_4/src/2DAlphabet/rootfiles
```

Current files:

```bash
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

Check them:

```bash
python3 - <<'PY'
import ROOT

for fname in ["rootfiles/data_2024.root", "rootfiles/TTbar_2024.root"]:
    print("\n###", fname)
    f = ROOT.TFile.Open(fname)
    if not f or f.IsZombie():
        raise RuntimeError(f"Could not open {fname}")

    for hname in [
        "MttvsMtCen2024Pass",
        "MttvsMtCen2024Fail",
        "MttvsMtFwd2024Pass",
        "MttvsMtFwd2024Fail",
    ]:
        h = f.Get(hname)
        print(hname, "OK" if h else "MISSING")
        if h:
            print("  class:", h.ClassName())
            print("  integral:", h.Integral())
            print("  x:", h.GetXaxis().GetNbins(), h.GetXaxis().GetXmin(), h.GetXaxis().GetXmax())
            print("  y:", h.GetYaxis().GetNbins(), h.GetYaxis().GetXmin(), h.GetYaxis().GetXmax())
PY
```

Known central axes:

```text
X axis = mt / top-candidate mass, 100 bins, 0 to 500
Y axis = mtt, 92 bins, 800 to 10000
```

---

## 6. Generated scripts

Main generated files:

```text
ttbar_2024_cen_config.json
run_ttbar_2024_cen.py
run_ttbar_2024_cen_make.py
run_ttbar_2024_cen_card.py
run_ttbar_2024_cen_fit.py
```

Purpose:

```text
ttbar_2024_cen_config.json
  Minimal central-only 2024 config.

run_ttbar_2024_cen.py
  Minimal config/ROOT loading test.

run_ttbar_2024_cen_make.py
  Builds the QCD model:
    qcd_fail = data_fail - ttbar_fail
    qcd_pass = qcd_fail * Rpf

run_ttbar_2024_cen_card.py
  Creates:
    ttbar2024_cen_fit/flat_rpf_area/card.txt

run_ttbar_2024_cen_fit.py
  Runs FitDiagnostics through the 2DAlphabet wrapper.
```

---

## 7. Normal rerun sequence

From the repo:

```bash
cd /uscms_data/d1/amandal2/2dalphabet/CMSSW_11_3_4/src/2DAlphabet
```

Clean old generated fit area:

```bash
rm -rf ttbar2024_cen_fit
```

Test config loading:

```bash
python3 run_ttbar_2024_cen.py
```

Expected:

```text
TwoDAlphabet object created successfully
```

Build model:

```bash
python3 run_ttbar_2024_cen_make.py
```

Check output:

```bash
ls -lh ttbar2024_cen_fit
```

Expected important files:

```text
base.root
binnings.p
ledger_df.csv
ledger_alphaObjs.csv
ledger_alphaParams.csv
organized_hists.root
runConfig.json
```

Make datacard:

```bash
python3 run_ttbar_2024_cen_card.py
```

Expected:

```bash
ttbar2024_cen_fit/flat_rpf_area/card.txt
```

Check:

```bash
ls -lh ttbar2024_cen_fit/flat_rpf_area
```

---

## 8. Current temporary datacard patch

This is only for the current mechanical test without a real signal.

Inspect card process lines:

```bash
nl -ba ttbar2024_cen_fit/flat_rpf_area/card.txt | sed -n '24,32p'
```

If the process IDs are floats, convert them to integers:

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

Then, for the temporary mechanical test only, patch TTbar as process `0` and QCD as process `1`:

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

Check:

```bash
nl -ba ttbar2024_cen_fit/flat_rpf_area/card.txt | sed -n '24,32p'
```

Expected temporary line:

```text
process 0 0 0 0 0 0 1 1 1 1 1 1
```

Important: this is a temporary hack. The proper fix is to add a real signal ROOT file.

---

## 9. Run the fit

Through wrapper:

```bash
python3 run_ttbar_2024_cen_fit.py
```

Or manually:

```bash
cd ttbar2024_cen_fit/flat_rpf_area

combine -M FitDiagnostics card.txt \
  --text2workspace "--channel-masks" \
  --setParameters r=1 \
  --saveWorkspace \
  --cminDefaultMinimizerStrategy 0 \
  --rMin -10 --rMax 10 -v 1
```

Return:

```bash
cd /uscms_data/d1/amandal2/2dalphabet/CMSSW_11_3_4/src/2DAlphabet
```

The current `r` result is not meaningful because the current card uses a temporary parser workaround.

---

## 10. Next real work

### Add signal file

Put a signal file here:

```bash
rootfiles/Zprime_2024.root
```

It should contain:

```text
MttvsMtCen2024Pass
MttvsMtCen2024Fail
MttvsMtFwd2024Pass
MttvsMtFwd2024Fail
```

Then update config/process IDs:

```text
Zprime = signal, combine_idx 0
TTbar  = background, combine_idx 1
QCD    = background, combine_idx 2
```

For background-only fit, include the signal process but freeze:

```bash
--setParameters r=0 --freezeParameters r
```

Possible wrapper call:

```python
twoD.MLfit(
    "flat_rpf_area",
    rMin=-1,
    rMax=1,
    setParams={"r": 0},
    extra="--freezeParameters r",
    verbosity=1
)
```

### Implement ABCDEF blinding

Final desired fit:

```text
include:
  A = Fail_LOW
  B = Pass_LOW
  C = Fail_SIG
  E = Fail_HIGH
  F = Pass_HIGH

exclude/blind:
  D = Pass_SIG
```

Important:

```text
C remains included.
D is blinded.
```

### Later

- Add forward region.
- Combine central and forward.
- Replace flat transfer function with `0x1` or `1x1`.
- Rebin histograms to sensible analysis bins.
- Add systematics.
- Run GOF, signal injection, impacts.
