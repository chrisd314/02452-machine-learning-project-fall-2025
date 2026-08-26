# 02452 Machine Learning Project — Fall 2025

Coursework notebooks for data exploration and supervised learning with the Concrete Compressive Strength dataset.

## Overview

The project covers two parts of the DTU 02452 Machine Learning coursework:

- data inspection, summary statistics, visualization, standardization, and principal component analysis (PCA)
- regression and classification with linear and regularized models, artificial neural networks, nested cross-validation, and statistical model comparison

Both notebooks read the tracked `Concrete_Data.xls` dataset. The classification work derives strength classes from the continuous compressive-strength target inside the second notebook.

## Notebook Sequence

The notebook numbers define the intended reading order:

1. `01-ml-assignment-data-feature-extraction-and-visualization.ipynb` inspects the data, calculates descriptive statistics, visualizes the attributes, standardizes the features, and performs PCA
2. `02-ml-assignment-supervised-learning-classification-and-regression.ipynb` performs the regression and classification analyses, including model selection, nested cross-validation, and statistical comparisons

Each notebook loads `Concrete_Data.xls` directly and can be evaluated independently. Within a notebook, cells must be evaluated from top to bottom because later cells use variables and fitted models created earlier.

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

The current notebooks display their tables, metrics, and plots as notebook output. They do not automatically write new workbooks, model files, or plot files.

## Limitations and Assumptions

- The dataset contains 1,030 observations, eight quantitative predictors, and one compressive-strength target measured in MPa
- `Concrete_Data.xls` must remain in the repository root because both notebooks use that relative path
- The PNG files under `Plots/` are tracked project artifacts rather than a guaranteed output of each notebook run; the current notebooks do not save or refresh them automatically
