# Molecular Property Prediction with Graph Neural Networks — BBBP

ENG 2440 Medical Imaging & AI in Healthcare, Assignment 3.

Blood–brain barrier penetration on the MoleculeNet **BBBP** dataset: a dataset audit, a
Bemis–Murcko scaffold split against a stratified random split, an ECFP4 + logistic-regression
baseline, a three-layer edge-aware graph isomorphism network (`GINEConv`), and an error analysis
of the network's most confident mistakes against nearest-neighbour similarity.

The write-up is **[`report/main.pdf`](report/main.pdf)**; the notebook holds the code and its
outputs. Its sections follow Parts A–G of the assignment brief, one section per part.

## Contents

| Path | What it is |
| --- | --- |
| `main.ipynb` | The full pipeline, executed top to bottom with all outputs visible. |
| `requirements.txt` | Pinned versions of everything the notebook imports. |
| `report/main.tex` | The report source: one self-contained LaTeX file. |
| `report/Images/` | The seven figures, exported from the notebook's plots. |
| `report/main.pdf` | The compiled report: 6 body pages, references, 1 appendix page. |

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

Notebook parts, report sections and the brief's required tasks all use the same letters.

| Part | Contents |
| --- | --- |
| A | Load BBBP; audit size, labels, parse failures and duplicates under canonical SMILES; six example structures; atom/bond count distributions; one graph's shapes. |
| B | Graph representation: feature columns and their meanings, embedding encoders, the depiction-versus-graph comparison, batching and integrity checks. |
| C | Stratified random split and Bemis–Murcko scaffold split, with coverage and overlap checks. |
| D | ECFP4 fingerprints and the logistic-regression baseline. |
| E | The `GINEConv` network: architecture, training, early stopping, three seeds per split. |
| F | Test evaluation: metrics, ROC and PR curves, confusion matrices. |
| G | Error and similarity analysis on the scaffold-split test set. |

## Rebuilding the report

```bash
cd report && tectonic main.tex
```

`report/` is a single `main.tex` plus `Images/`, with no external bibliography pass, so it also
compiles unchanged on Overleaf — upload the folder and press recompile.
