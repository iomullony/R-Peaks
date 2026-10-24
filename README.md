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

## Quick Start

1. Create and activate a virtual environment (recommended):

```bash
python3 -m venv .venv
source .venv/bin/activate
```

2. Install core Python packages (adjust to your environment):

```bash
pip install -r requirements.txt
```

## Datasets

- MIT-BIH Arrhythmia Database:
  - Download the dataset from `https://physionet.org/content/mitdb/1.0.0/` and place the files under `databases/mitdb/` (the repository expects the same folder layout currently in this workspace).
    ```bash
    wget -r -N -c -np -nH --cut-dirs=3 -P mitdb https://physionet.org/files/mitdb/1.0.0/
    ```
- ECGRDVQ (PhysioNet):
  - Notebooks stream records from PhysioNet using `wfdb`. The clinical CSV used is:
    `https://physionet.org/files/ecgrdvq/1.0.0/SCR-002.Clinical.Data.csv`
  - No manual download is required for ECGRDVQ when online access is available (but a local version would make it faster).
    ```bash
    wget -r -N -c -np -nH --cut-dirs=3 -P ecgrdvq https://physionet.org/files/ecgrdvq/1.0.0/
    ```

## Running the notebooks

- Open `R-peaks.ipynb` to run experiments with MIT-BIH records.
- Open `ECGRDVQ R-peaks.ipynb` for single-record exploration on ECGRDVQ.
- Use `ECGRDVQ results.ipynb` to run batch tests and comparisons (it builds `convenience_df` and uses it consistently for evaluations).

Notes:
- After editing notebook code (e.g. changing how subsets are taken), restart the kernel and run cells from the top to ensure state consistency.


## Common troubleshooting

- Notebook shows incorrect subset/order of records: ensure you run the cells that create `convenience_df` before downstream cells. Prefer using `convenience_df[...]` when deriving subsets.
- Plot indexing errors when filtering records: use `reset_index(drop=True)` or enumerate `records.iterrows()` and create subplots sized to `len(records)` (the notebooks already include this fix).
- Missing LaTeX package errors: install TeX Live packages as above.