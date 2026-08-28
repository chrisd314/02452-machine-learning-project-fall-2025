# 02452 Machine Learning Project — Fall 2025

Exploratory data analysis, principal component analysis, regression, and multiclass classification of the Concrete Compressive Strength dataset for DTU course 02452 Machine Learning.

## Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Repository Structure](#repository-structure)
- [Installation](#installation)
- [Configuration](#configuration)
- [Inputs and Outputs](#inputs-and-outputs)
- [Troubleshooting](#troubleshooting)
- [Related Documentation](#related-documentation)

## Overview

This repository contains two executed Jupyter notebooks built around the Concrete Compressive Strength dataset described in `Concrete_Readme.txt`. The eight numeric predictors describe a concrete mixture and its age. The target is laboratory-measured compressive strength in MPa.

The notebooks cover two stages of the coursework:

1. Inspect the dataset, calculate descriptive statistics, visualize the predictors, standardize the feature matrix, and perform principal component analysis (PCA).
2. Treat compressive strength first as a continuous regression target and then as a derived three-class classification target. Compare baselines, linear models, and ANNs using cross-validation and statistical tests.

## Architecture

| Component | Responsibility |
| --- | --- |
| `01-ml-assignment-data-feature-extraction-and-visualization.ipynb` | Exploratory analysis and PCA |
| `02-ml-assignment-supervised-learning-classification-and-regression.ipynb` | Regression and classification experiments |
| `Concrete_Data.xls` | Primary input |
| `Concrete_Readme.txt` | Dataset reference |
| `Plots/` | Tracked reference figures |

There is no shared Python package between the notebooks and no persistent model state. The second notebook rebuilds every model in memory during execution.

## Repository Structure

```text
02452-machine-learning-project-fall-2025/
├── 01-ml-assignment-data-feature-extraction-and-visualization.ipynb
├── 02-ml-assignment-supervised-learning-classification-and-regression.ipynb
├── Concrete_Data.xls
├── Concrete_Readme.txt
├── Plots/
│   ├── ANN Training Loss per Outer Fold.png
│   ├── Boxplots.png
│   ├── Correlation Matrix.png
│   ├── Generalization Error vs λ.png
│   ├── Held-Out Test Set RMSE Comparison.png
│   ├── Histograms.png
│   ├── PCA Cumulative Variance.png
│   ├── PCA Projection.png
│   ├── PCA Scree Plot.png
│   └── ...
├── .gitignore
├── configuration.md
├── requirements.txt
└── README.md
```

The abbreviated `Plots/` listing shows representative artifacts.


## Installation


1. Clone the repository and create its environment:

```console
git clone https://github.com/chrisd314/02452-machine-learning-project-fall-2025.git
cd 02452-machine-learning-project-fall-2025
py -m venv .venv
```

2. Open this repository folder and activate the virtual environment.
3. Install the requirements:

```console
python -m pip install -r requirements.txt
```

4. Open the notebook in a notebook-capable editor or server and select the project’s .venv Python interpreter as the notebook kernel. The committed ml_project kernel name is metadata only. It does not create or install the environment automatically.

## Configuration

There are no environment variables, `.env` files, command-line options, or external credentials. Configuration consists of:

- keeping `Concrete_Data.xls` at the relative path hard-coded in both notebooks
- selecting an environment containing the declared packages
- changing notebook constants directly when intentionally running a different experiment.

See [configuration.md](configuration.md) for the exact dataset schema, hyperparameter grids, class boundaries, working-directory behavior, and preflight checklist.

## Inputs and Outputs

### Dataset schema

| Column role | Field | Unit |
| --- | --- | --- |
| Predictor | Cement | kg per m³ mixture |
| Predictor | Blast Furnace Slag | kg per m³ mixture |
| Predictor | Fly Ash | kg per m³ mixture |
| Predictor | Water | kg per m³ mixture |
| Predictor | Superplasticizer | kg per m³ mixture |
| Predictor | Coarse Aggregate | kg per m³ mixture |
| Predictor | Fine Aggregate | kg per m³ mixture |
| Predictor | Age | days |
| Target | Concrete compressive strength | MPa |

The implementation addresses the target by its exact workbook header, including a trailing space. Renaming or trimming that column in the workbook will break both notebooks unless their lookup expressions are updated too.

## Troubleshooting

| Problem | Likely cause | Resolution |
| --- | --- | --- |
| `FileNotFoundError` for `Concrete_Data.xls` | The notebook server’s working directory is not the repository root, or the file was moved. | Start the frontend from the repository root and verify the filename and capitalization. |
| `KeyError` for the compressive-strength column | The workbook header was edited; the code expects its exact original text, including trailing whitespace. | Restore the tracked dataset or update the lookup consistently in both notebooks for an intentional schema change. |
| Pandas reports that it cannot determine or use an Excel engine | `xlrd` is missing or the selected environment differs from `requirements.txt`. | Select `.venv\Scripts\python.exe` in VS Code, open a new integrated terminal, and install the recorded requirements. |
| Import errors when opening a notebook | The selected kernel is not the environment where requirements were installed. | Select the `.venv` interpreter/kernel and restart the notebook kernel. |
| Installation on another operating system fails at `pywin32` | The committed snapshot is Windows-oriented, and that platform is outside the validated workflow. | Use the documented Windows/VS Code setup or prepare and independently validate a platform-specific dependency set; none is supplied here. |
| Notebook 02 runs for a long time or saturates the machine | Nested searches fit many candidates and use all processors through `n_jobs=-1`. | Allow the cell to finish, or deliberately reduce the documented grids/fold counts in a separate experiment. |
| A later cell reports an undefined variable | Cells were run out of order or the kernel was restarted. | Restart the kernel and run the notebook from the first cell. |
| Results differ after changing constants | Hyperparameter grids, folds, thresholds, or random seeds were changed. | Compare against [configuration.md](configuration.md) and restart/run all cells to remove stale state. |
| A plot in `Plots/` does not match fresh notebook output | PNG export is not automated. | Treat the notebook as current runtime output and update the static artifact deliberately if required. |

## Related Documentation

- [Configuration reference](configuration.md)
- [Dataset description, attribution, and reuse notice](Concrete_Readme.txt)
- [Exploratory analysis and PCA notebook](01-ml-assignment-data-feature-extraction-and-visualization.ipynb)
- [Supervised learning notebook](02-ml-assignment-supervised-learning-classification-and-regression.ipynb)
