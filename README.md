# remasker-reproduced

Reproduction of the main experiment of "ReMasker: Imputing Tabular Data with Masked Autoencoding" (ICLR 2024): imputation on UCI datasets with 30% of values missing (MAR), measured by RMSE, Wasserstein distance and AUROC.

This repository uses the authors' official code (https://github.com/alps-lab/remasker).

## Files
- `reproduced_ReMasker_results.ipynb`: runs ReMasker on 8 datasets
- `reproduced_baseline_results.ipynb`: runs HyperImpute and 6 baselines on the same datasets
- `baseline/`: result files (JSON), one per dataset; comparision with baseline models
- `repro/`: result files (JSON), one per dataset; results of ReMasker

## How to run
Use Google Colab with a T4 GPU, and run the notebook cells in order:

1. Download the authors' code and install the libraries:
```
   !git clone https://github.com/alps-lab/remasker.git
   %cd remasker
   !pip install timm hyperimpute
```
2. Apply two small fixes (both are in the notebook):
   - `np.float` in `utils.py` is replaced with `float` (newer NumPy removed it).
   - A block of leftover analysis code is removed from `plugin_mae.py`. It opens a file that exists only on the authors' computer, so every run crashes without this fix. Run this cell once only.
3. Run one dataset at a time (about 2 minutes for a small dataset):
```
   !python run_exp.py --dataset yacht --exp_name repro --note run1
```
   Datasets: yacht, diabetes, climate, credit, raisin, compression, wine, obesity.
4. For the baselines, use the second notebook. It runs:
```
   !python run_baseline.py --dataset yacht --exp_name baselines --note run1
```
## Environment

- Python: 3.13.16
- NumPy: 2.1.3
- HyperImpute: 0.1.17
- Platform: Google Colab, NVIDIA T4 GPU

## Settings
- Missing values: MAR, 30%
- Repetitions: 3 per dataset
- Epochs: 600, batch size: 64, masking ratio: 0.5
- Optimizer: Adam, learning rate 1e-3, weight decay 0.05
- Model size: set per dataset by the authors' `configs.py`
- Baselines: HyperImpute, ICE, MICE, MissForest, mean, median, most frequent
