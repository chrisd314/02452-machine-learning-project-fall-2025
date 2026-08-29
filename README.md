# 02452 Machine Learning Project — Fall 2025

Exploratory data analysis, principal component analysis, regression, and multiclass classification of the Concrete Compressive Strength dataset for DTU course 02452 Machine Learning.

## Contents

- [Overview](#overview)
- [Repository Structure](#repository-structure)
- [Installation](#installation)
- [Configuration](#configuration)
- [Inputs and Output](#inputs-and-output)
- [Related Documentation](#related-documentation)

## Overview

This repository contains two Jupyter notebooks built around the Concrete Compressive Strength dataset described in `Concrete_Readme.txt`. The eight numeric predictors describe a concrete mixture and its age. The target is laboratory-measured compressive strength in MPa.

The notebooks cover two stages of the coursework:

1. Inspect the dataset, calculate descriptive statistics, visualize the predictors, standardize the feature matrix, and perform principal component analysis (PCA).
2. Treat compressive strength first as a continuous regression target and then as a derived three-class classification target. Compare baselines, linear models, and ANNs using cross-validation and statistical tests.

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

## Inputs and Output

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

## Related Documentation

- [Dataset description, attribution, and reuse notice](Concrete_Readme.txt)
- [Exploratory analysis and PCA notebook](01-ml-assignment-data-feature-extraction-and-visualization.ipynb)
- [Supervised learning notebook](02-ml-assignment-supervised-learning-classification-and-regression.ipynb)
