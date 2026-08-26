# 02452 Machine Learning Project — Fall 2025

Coursework notebooks for data exploration and supervised learning with the Concrete Compressive Strength dataset.

## Overview

The project covers two parts of the DTU 02452 Machine Learning coursework:

- data inspection, summary statistics, visualization, standardization, and principal component analysis (PCA)
- regression and classification with linear and regularized models, artificial neural networks, nested cross-validation, and statistical model comparison

Both notebooks read the tracked `Concrete_Data.xls` dataset. The classification work derives strength classes from the continuous compressive-strength target inside the second notebook.

## Requirements

The project uses Python and Jupyter notebooks. No exact Python version is established in the repository. The main analysis dependencies include NumPy, pandas, Matplotlib, seaborn, scikit-learn, SciPy, statsmodels, and `xlrd` for reading the legacy `.xls` dataset.

Create a project-local virtual environment and install the recorded dependencies with `uv`:

```powershell
uv venv
uv pip install -r requirements.txt
```

## Usage

Open either notebook in the VS Code notebook editor or another Jupyter-capable editor configured to use the project kernel, then run its cells from top to bottom:

- `01-ml-assignment-data-feature-extraction-and-visualization.ipynb`
- `02-ml-assignment-supervised-learning-classification-and-regression.ipynb`

Keep `Concrete_Data.xls` in the repository root because both notebooks load it using that relative path. The notebooks are independent and may be run separately.

## Project Structure

```text
02452-machine-learning-project-fall-2025/
├── 01-ml-assignment-data-feature-extraction-and-visualization.ipynb
├── 02-ml-assignment-supervised-learning-classification-and-regression.ipynb
├── Concrete_Data.xls
├── Concrete_Readme.txt
├── Plots/
├── requirements.txt
└── README.md
```

[Concrete_Readme.txt](Concrete_Readme.txt) contains the dataset description, variable units, attribution, and reuse notice. `Plots/` contains the exported figures retained with the coursework.

## Output

The current notebooks display their tables, metrics, and plots as notebook output. They do not automatically write new workbooks, model files, or plot files. The PNG files under `Plots/` are tracked project artifacts rather than a guaranteed output of each notebook run.

## Notes

- The dataset contains 1,030 observations, eight quantitative predictors, and one compressive-strength target measured in MPa
- Run cells in order because later analysis cells depend on variables and fitted models created earlier in the notebook
- This is an offline coursework repository; it has no API credentials or external service configuration
