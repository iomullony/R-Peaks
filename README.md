# R-Peaks

Detect R-peaks in ECG signals using wavelet denoising and K-Means clustering.

## Overview

- Implements a reproducible pipeline for R-peak detection and evaluation:
  - Wavelet denoising (sym8)
  - Absolute slope feature
  - Segmented K-Means to identify QRS regions
  - Region-based R-peak selection + refractory filtering
- Includes notebooks for development, evaluation on MIT-BIH and ECGRDVQ, and a LaTeX report.

## Contents

- `R-peaks.ipynb` — Primary development notebook (MIT-BIH)
- `ECGRDVQ R-peaks.ipynb` — Per-record development on ECGRDVQ
- `ECGRDVQ results.ipynb` — Batch evaluation and comparison versus Pan–Tompkins
- `report/` — LaTeX report sources and generated PDFs
- `databases/` — Expected location for local MIT-BIH files (see below)

## Quick Start (Python environment)

1. Create and activate a virtual environment (recommended):

```bash
python3 -m venv .venv
source .venv/bin/activate
```

2. Install core Python packages (adjust to your environment):

```bash
pip install -r requirements.txt
```

3. Launch Jupyter and open the notebooks:

```bash
jupyter lab
# or
jupyter notebook
```

## Datasets

- MIT-BIH Arrhythmia Database:
  - Download the dataset and place the files under `databases/mit-bih-arrhythmia-database-1.0.0/` (the repository expects the same folder layout currently in this workspace).
- ECGRDVQ (PhysioNet):
  - Notebooks stream records from PhysioNet using `wfdb`. The clinical CSV used is:
    `https://physionet.org/files/ecgrdvq/1.0.0/SCR-002.Clinical.Data.csv`
  - No manual download is required for ECGRDVQ when online access is available.

## Running the notebooks

- Open `R-peaks.ipynb` to run experiments with MIT-BIH records.
- Open `ECGRDVQ R-peaks.ipynb` for single-record exploration on ECGRDVQ.
- Use `ECGRDVQ results.ipynb` to run batch tests and comparisons (it builds `convenience_df` and uses it consistently for evaluations).

Notes:
- After editing notebook code (e.g. changing how subsets are taken), restart the kernel and run cells from the top to ensure state consistency.

## Building the LaTeX report

From the `report/` directory you can build the PDF with `latexmk` (or `pdflatex` twice):

```bash
cd report
latexmk -pdf main.tex
```

If the build fails with missing packages (e.g. `siunitx.sty`), install `texlive-latex-extra` or `texlive-full` on Ubuntu:

```bash
sudo apt update
sudo apt install texlive-latex-extra
# or for a full TeX Live:
sudo apt install texlive-full
```

## Common troubleshooting

- Notebook shows incorrect subset/order of records: ensure you run the cells that create `convenience_df` before downstream cells. Prefer using `convenience_df[...]` when deriving subsets.
- Plot indexing errors when filtering records: use `reset_index(drop=True)` or enumerate `records.iterrows()` and create subplots sized to `len(records)` (the notebooks already include this fix).
- Missing LaTeX package errors: install TeX Live packages as above.