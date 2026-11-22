# Molecular Property Prediction with Graph Neural Networks — BBBP

Blood–brain barrier penetration on the MoleculeNet **BBBP** dataset: a dataset audit, a
Bemis–Murcko scaffold split against a stratified random split, an ECFP4 + logistic-regression
baseline, a three-layer edge-aware graph isomorphism network (`GINEConv`), and an error analysis
of the network's most confident mistakes against nearest-neighbour similarity.

The write-up is **[`report/main.pdf`](report/main.pdf)**; the notebook holds the code and its
outputs.

## Contents

| Path | What it is |
| --- | --- |
| `main.ipynb` | The full pipeline, executed top to bottom with all outputs visible. |
| `requirements.txt` | Pinned versions of everything the notebook imports. |
| `report/` | LaTeX sources (`main.tex`, `Text/`, `Images/`, `Bibliography/`) and the compiled `main.pdf`. |
| `report/Images/` | The figures, exported from the notebook's plots. |

There are no supporting `.py` files: the notebook is self-contained.

## Environment

Python 3.11. From the repository root:

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
```

Conda works equally well (`conda create -n bbbp python=3.11`, then the same `pip install`).
Everything runs on CPU; no GPU is required.

The notebook's first cell also runs `pip install torch-geometric rdkit scikit-learn`, so it works
in a fresh Colab runtime without the file above.

## Running it

Open `main.ipynb` **from the repository root** and run all cells in order, top to bottom. There is
no other entry point and no step that must be run separately.

```bash
jupyter lab main.ipynb
```

Paths are relative to the working directory, so the root matters: the first run downloads BBBP to
`./data/MoleculeNet` (about 8 MB, one network call) and PyTorch Geometric caches the processed
graphs there. `data/` is git-ignored; delete it to force a clean re-download.

A full run takes about **2.5 minutes** on a laptop CPU. Training is deterministic at the seeds in
the notebook — split seed 42, network seeds 0, 1 and 2 — so a re-run reproduces the numbers in the
report exactly.

## Notebook layout

| Part | Contents |
| --- | --- |
| A | Load BBBP, audit records, labels, parse failures and duplicates under canonical SMILES. |
| B | Structures, atom/bond count distributions, graph-versus-depiction comparison. |
| C | Attribute vocabularies, embedding encoders, batching and integrity checks. |
| D | Stratified random split and Bemis–Murcko scaffold split, with overlap checks. |
| E | ECFP4 fingerprints and the logistic-regression baseline. |
| F | The `GINEConv` network: training, early stopping, three seeds per split. |
| G | Test evaluation and the error/similarity analysis. |

## Rebuilding the report

```bash
cd report && tectonic -Z shell-escape main.tex
```

It also compiles unchanged on Overleaf (upload the `report/` folder; the bibliography backend is
`bibtex`).
