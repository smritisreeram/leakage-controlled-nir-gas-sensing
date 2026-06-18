# Leakage-Controlled Machine Learning for Broadband NIR Gas Sensing

**Working repository** for the machine learning part of the paper.

This repository contains the Jupyter notebooks needed to reproduce the classification and regression experiments described in the paper. The central methodological focus is **leakage-controlled validation**: nested leave-one-concentration-out (LOCO) cross-validation, a concentration-level permutation null, bootstrap confidence intervals, and an explicit audit comparing nested CV against a deliberately leaked full-dataset selection baseline.

> **Note**: The full paper (including the sensor hardware section) is still in preparation. The final title and citation will be updated in a future release.

## Notebooks

- `classification.ipynb` — RBF-SVM gas identification (Ethanol / Acetone / Ammonia) on confound-corrected, nonzero-concentration spectra.
- `regression_acetone.ipynb` — PLS concentration regression for Acetone.
- `regression_ethanol.ipynb` — PLS concentration regression for Ethanol.
- `regression_ammonia.ipynb` — PLS concentration regression for Ammonia.

Each regression notebook also includes a flagged, exploratory binary high/low concentration classifier; these cells are marked `CLASSIFIER CELL — FLAGGED FOR REMOVAL` in-place and are not part of the paper's main regression result.

## Data

The notebooks expect raw spectra as `.txt` files under a `data/` folder, one subfolder per gas, matching the structure:

```
data/
├── Ethanol/
│   ├── ethanol_0ppm_1.txt
│   ├── ethanol_0ppm_2.txt
│   └── ...
├── Acetone/
│   └── ...
└── Ammonia/
    └── ...
```

Raw data is not included in this repository. Set `DATA_ROOT` near the top of each notebook to point at wherever you've placed the `data/` folder (default is `../data/`).

## How to Reproduce

1. Clone this repository:
   ```bash
   git clone https://github.com/<your-username>/<repo-name>.git
   cd <repo-name>
   ```
2. (Recommended) create a virtual environment, then install dependencies:
   ```bash
   python -m venv venv
   source venv/bin/activate   # on Windows: venv\Scripts\activate
   pip install -r requirements.txt
   ```
3. Place your raw spectra under `data/` as described above.
4. Launch Jupyter and run each notebook top to bottom:
   ```bash
   jupyter notebook
   ```
   Within each regression notebook, cells are numbered sequentially (`CELL 1`, `CELL 2`, ...) and are designed to be run in order. Cells flagged as classifier-related are independent of the regression cells and can be skipped or deleted without affecting the regression results.

## Validation Methodology

- **Nested LOCO-CV**: preprocessing and model hyperparameters (PLS components, SVM kernel/C) are selected using only the outer-training concentrations in each fold, never the held-out one.
- **Permutation null**: concentration labels are shuffled (preserving the 3-replicate block structure) to confirm real-label performance exceeds chance.
- **Bootstrap CIs**: concentration-level resampling of out-of-fold predictions gives 95% intervals on RMSEP/MAE/R².
- **Leakage audit**: every fold asserts zero concentration overlap between train and test.

## Citation

If you use this code, please cite it using the metadata in [`CITATION.cff`](./CITATION.cff), or via the DOI below once archived:

[![DOI](https://zenodo.org/badge/1273226526.svg)](https://doi.org/10.5281/zenodo.20745300)

## License

This project is licensed under the MIT License — see [`LICENSE`](./LICENSE) for details.
