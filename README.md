# Global soil brGDGT–summer SPEI calibration

This repository contains notebooks for the manuscript analyses and figures, a trained XGBoost model, and an example workflow for reconstructing summer SPEI from brGDGT data.

## Files

| File | Purpose |
| --- | --- |
| `MS_analysis_and_figures.ipynb` | Manuscript analyses and figure generation. See the notebook for its required input data and paths. |
| `Reconstruct_summer_spei_Code.ipynb` | Apply the saved model to a new brGDGT dataset and export reconstructed summer SPEI. |
| `xgboost_model_spei.pkl` | The brGDGT–summer SPEI model used by the reconstruction notebook. |
| `README.md` | Instructions for using the repository. |

## Requirements

Use Python with `numpy`, `pandas`, `joblib`, `xgboost`, and `jupyter` installed. The manuscript analysis notebook may require additional packages; check its import cells before running it.

```bash
pip install numpy pandas joblib xgboost jupyter
```

## Reconstruct summer SPEI for a new dataset

1. Open `Reconstruct_summer_spei_Code.ipynb` in Jupyter Notebook or JupyterLab.
2. In its file-path section, set `input_file` to your brGDGT CSV, `model_file` to `xgboost_model_spei.pkl`, and `output_file` to the desired CSV path and filename.
3. Ensure the input CSV includes the following 15 variables, with the names used in the notebook:

   `Ia`, `Ib`, `Ic`, `IIa`, `IIa.`, `IIb`, `IIb.`, `IIc`, `IIc.`, `IIIa`, `IIIa.`, `IIIb`, `IIIb.`, `IIIc`, `IIIc.`

   The notebook accepts an apostrophe in place of a period in these column names (for example, `IIa'` in place of `IIa.`). Predictor values must be numeric, finite, non-negative, and present for every sample.
4. Run the notebook from top to bottom. The output CSV retains the original input columns and adds `Reconstructed summer SPEI`.

The reconstruction code applies `np.log1p()` to the predictors before prediction. Use this step only if it matches the preprocessing used to train the saved model; otherwise update the notebook to match the training workflow. If the saved model already contains preprocessing, do not apply it again.

## Reproduce manuscript analyses and figures

Open `MS_analysis_and_figures.ipynb`, review its input paths and dependencies, and run its cells in order. Data files required by that notebook are not listed among the four files shown here; provide them at the paths expected by the notebook.

## Notes

The `.pkl` file must be loaded in a compatible Python environment with the packages used to create it. Only load model files from sources you trust. Reconstructed values are model estimates and should be interpreted within the calibration's applicable domain.
