# Configuration Reference

This project has no external configuration system. The dataset location and experiment parameters are defined directly in the two notebooks. This reference identifies every setting required to reproduce the current workflow without searching through notebook JSON.

The primary documented environment is Windows with Visual Studio Code, the Microsoft Python extension, and the Microsoft Jupyter extension. The dependency snapshot is Windows-oriented because it includes `pywin32`; other operating systems have not been validated.

For installation and the end-to-end workflow, see [README.md](README.md).

## Contents

- [Configuration Overview](#configuration-overview)
- [Configuration Sources and Precedence](#configuration-sources-and-precedence)
- [Environment Variables](#environment-variables)
- [Credentials and Authentication](#credentials-and-authentication)
- [API and Network Configuration](#api-and-network-configuration)
- [Dataset and Path Configuration](#dataset-and-path-configuration)
- [Configuration Files](#configuration-files)
- [User-Managed Configuration vs Runtime State](#user-managed-configuration-vs-runtime-state)
- [Command-Line Configuration](#command-line-configuration)
- [Notebook 01 Constants](#notebook-01-constants)
- [Notebook 02 Regression Constants](#notebook-02-regression-constants)
- [Notebook 02 Classification Constants](#notebook-02-classification-constants)
- [Warning and Display Configuration](#warning-and-display-configuration)
- [Logging Configuration](#logging-configuration)
- [Platform-Specific Configuration](#platform-specific-configuration)
- [Production vs Development Configuration](#production-vs-development-configuration)
- [Configuration Example](#configuration-example)
- [Configuration Validation](#configuration-validation)
- [Configuration Troubleshooting](#configuration-troubleshooting)
- [Configuration Checklist](#configuration-checklist)

## Configuration Overview

| Area | Required before first execution | Current source | Default/current value |
| --- | --- | --- | --- |
| Python environment | Yes | Selected notebook kernel plus `requirements.txt` | Saved kernel metadata reports Python 3.11.9 and kernel name `ml_project`. |
| Dataset | Yes | Hard-coded notebook path | `Concrete_Data.xls`, resolved from the process working directory. |
| Target column | Yes | Hard-coded in both notebooks | Exact source header `Concrete compressive strength(MPa, megapascals) `, including its trailing space. |
| Analysis parameters | No change required | Python constants in notebook cells | Values documented below. |
| Credentials/API access | No | Not implemented | None. |
| Output location | No | Not implemented | Results render inside the notebook; no active file export exists. |

For an unchanged run, install the recorded dependencies, keep the dataset in the repository root, select the correct kernel, and execute cells from top to bottom. There are no optional feature flags or hidden first-run setup files.

## Configuration Sources and Precedence

The project uses the following configuration sources:

1. **Live notebook cell values.** Executing an edited cell changes the current kernel state and therefore takes precedence for all later cells.
2. **Committed notebook source.** Restarting the kernel and running all cells restores the committed experiment definitions.
3. **Process working directory.** The relative dataset path is resolved here; it is not resolved from the notebook file object explicitly.
4. **Selected Python environment.** Installed package versions determine available APIs and numerical behavior.

The project does **not** read environment variables, `.env`, JSON, YAML, TOML, INI, command-line arguments, a user-home configuration file, or GUI settings. There is no configuration precedence involving those mechanisms.

## Environment Variables

No environment variables are consumed by either notebook. None are required, and defining variables such as a dataset path or random seed has no effect unless notebook code is changed to read them.

## Credentials and Authentication

No credentials or authentication are used. The notebooks make no API or portal requests and do not create tokens, cookies, sessions, or credential caches.

## API and Network Configuration

There are no external API clients, endpoints, timeouts, retries, proxies, or rate-limit settings in the implementation. Runtime analysis is local. Package installation and repository cloning may require network access, but they are not controlled by project configuration.

## Dataset and Path Configuration

Both notebooks execute the equivalent of:

```python
df = pd.read_excel("Concrete_Data.xls")
```

| Path/artifact | Fixed or configurable | Resolution | Created automatically | Notes |
| --- | --- | --- | --- | --- |
| `Concrete_Data.xls` | Fixed in both notebooks | Relative to the process working directory | No | Required primary input. Keep the original filename/case or edit both notebooks. |
| `Concrete_Readme.txt` | Fixed repository documentation; not read by code | Repository root | No | Dataset schema, provenance, and reuse notice. |
| `Plots/` | Fixed tracked artifact directory; not read by code | Repository root | No | Current notebook cells do not create or update it. |
| Notebook output | Managed by the notebook frontend | Embedded inside each `.ipynb` on save | Yes, when cells run and the notebook is saved | May contain tables, images, warnings, and environment-specific details. |

Recommended launch location:

```text
02452-machine-learning-project-fall-2025/  <- process working directory
├── Concrete_Data.xls
├── 01-ml-assignment-data-feature-extraction-and-visualization.ipynb
└── 02-ml-assignment-supervised-learning-classification-and-regression.ipynb
```

The dataset contains 1,030 observations, eight quantitative predictors, and one quantitative target. The implementation expects the original workbook schema. In particular, it drops/selects the target using the exact header with a trailing space. Feature labels are cleaned for display in notebook 01 only; the workbook itself is not changed.

## Configuration Files

### `requirements.txt`

- **Location:** repository root
- **Format:** pip requirements file
- **Purpose:** records the environment used for notebook execution
- **Versioning:** most entries are pinned; `statsmodels` has no version constraint
- **Platform note:** includes the Windows-specific package `pywin32==311`
- **Notebook frontend note:** includes kernel/client infrastructure but does not declare `jupyterlab` or `notebook`

After creating `.venv` and selecting it with **Python: Select Interpreter** in VS Code, open a new integrated terminal and install the snapshot with:

```console
python -m pip install -r requirements.txt
```

### Notebook metadata

Each `.ipynb` includes a Jupyter kernelspec named `ml_project` and language metadata reporting Python 3.11.9. The kernel name is descriptive, not an automatically created environment. A newly cloned repository still requires the user to create/select a compatible interpreter.

### Absent configuration files

There is no `.env.example`, `pyproject.toml`, `setup.cfg`, `package.json`, Docker configuration, experiment YAML/JSON, or model manifest.

## User-Managed Configuration vs Runtime State

### User-managed

- Python environment and selected notebook kernel
- Working directory
- Presence and schema of `Concrete_Data.xls`
- Any intentional edits to notebook constants

### Application-managed runtime state

- Python variables, transformed arrays, fitted estimators, predictions, and result tables in kernel memory
- Cell execution counts and outputs embedded when a notebook is saved

No cache directory, model file, database, log, report, temporary-file convention, or serialized session is created by project code.

## Command-Line Configuration

The notebooks implement no CLI arguments. They are not Python entry-point scripts and do not use `argparse`, `click`, or equivalent parsing. Start them through a notebook-capable editor/frontend and configure the experiment by editing cells only when necessary.

## Notebook 01 Constants

`01-ml-assignment-data-feature-extraction-and-visualization.ipynb` defines these material behaviors:

| Setting | Current value | Effect of changing it |
| --- | --- | --- |
| Dataset path | `Concrete_Data.xls` | Selects the workbook loaded by pandas. The same change must be made independently in notebook 02. |
| Target header | Exact original compressive-strength header | Determines which column is removed from `X` and assigned to `y`. |
| Histogram bins | `20` | Changes histogram granularity for all eight predictors. |
| PCA preprocessing | `StandardScaler()` | Centers and scales predictors before PCA. Removing it changes component orientation and explained variance substantially. |
| PCA component count | `X.shape[1]` (eight) | Fits one component per predictor. |
| PCA `random_state` | `0` | Passed to `PCA`; it affects only PCA solvers that use randomness, and the solver is left on scikit-learn's `auto` setting. |
| Projection dimensions | `Z[:, 0]` and `Z[:, 1]` | Selects PC1 and PC2 for the scatter plot. |
| Loading-vector scale | `2.5` | Changes arrow length in the loading-vector visualization only. |

Several distribution/scatter cells and a target-interval visualization are fully commented out. They are examples in source, not active configuration and do not run unless deliberately uncommented.

## Notebook 02 Regression Constants

`02-ml-assignment-supervised-learning-classification-and-regression.ipynb` uses:

| Setting | Current value | Effect |
| --- | --- | --- |
| Initial train/test split | `test_size=0.2`, `random_state=42` | Creates the regression training and held-out subsets used in parts A and B. No stratification is used for the continuous target. |
| Standardization | `StandardScaler` | Applied before linear/ridge/MLP models; pipelines place scaling inside CV where implemented. |
| Part A ridge candidates | `np.logspace(-4, 4, 100)` | Tests 100 `Ridge(alpha=...)` values from `0.0001` through `10000`. |
| Part A CV | 10-fold shuffled `KFold`, seed `42` | Selects and evaluates ridge regularization on the initial training subset. |
| Nested regression CV | Outer 10-fold plus inner 10-fold shuffled `KFold`, seed `42` | Produces outer-fold model errors and inner-fold hyperparameter selection. |
| Nested ridge candidates | Same 100-value logspace | Controls ridge search in each inner loop. |
| Regression ANN architectures | `(1,)`, `(2,)`, `(5,)`, `(10,)`, `(20,)`, plus `(1,1)`, `(2,2)`, `(5,5)`, `(10,10)`, `(20,20)` | Tests one or two equal-width hidden layers. |
| Regression ANN training | `max_iter=2000`, `early_stopping=True`, `random_state=42` | Controls `MLPRegressor` fitting. Other estimator defaults come from the installed scikit-learn version. |
| Parallel jobs | `n_jobs=-1` | Lets grid searches use all available processors. |
| Regression baseline | Mean of the outer training-fold target | Predicts one constant value for every outer test observation. |
| Statistical alpha | `0.05` | Builds 95% confidence intervals and marks paired t-test p-values below 0.05 significant. |

## Notebook 02 Classification Constants

### Derived class definition

The classification target is created from the continuous MPa target as follows:

| Class | Code condition | Meaning used in the notebook |
| --- | --- | --- |
| `0` | strength `< 25.01` | Low |
| `1` | strength `>= 25.01` and `< 55.02` | Medium |
| `2` | strength `>= 55.02` | High |

These conditions are the executable source of truth. Boundary comments in an inactive notebook 01 cell use slightly different inclusive ranges and do not control classification.

### Model-selection settings

| Setting | Current value | Effect |
| --- | --- | --- |
| Nested classification CV | Outer 10-fold plus inner 10-fold `StratifiedKFold`, shuffle enabled, seed `42` | Preserves class proportions while selecting/evaluating models. |
| Baseline | `DummyClassifier(strategy="most_frequent")` | Always predicts the most frequent class in each outer training fold. |
| Logistic regression | Multinomial, `lbfgs`, `max_iter=2000`, seed `42` | Fits the scaled linear classifier. |
| Logistic `C` candidates | `0.001`, `0.01`, `0.1`, `1`, `10`, `100` | Controls inverse regularization strength. |
| MLP layer widths | `8`, `16`, `25`, `32` | Builds 4 one-layer, 6 strictly descending two-layer, and 4 strictly descending three-layer shapes (14 architectures). |
| MLP activations | `relu`, `tanh` | Expands the classifier grid. |
| MLP alpha candidates | `0.0001`, `0.001`, `0.01` | Controls L2 regularization. |
| Initial learning rates | `0.001`, `0.01` | Passed as `learning_rate_init`. |
| Learning-rate schedules | `constant`, `adaptive` | Passed as `learning_rate`. |
| MLP training | `max_iter=2000`, `early_stopping=True`, seed `42` | Controls fitting of every candidate. |
| Parallel jobs | `n_jobs=-1` | Uses all available processors for both classifier grids. |
| Final logistic `C` | Mode of the ten outer-fold selected `C` values | Trains the final pipeline on all `X`/`y_classes`. |
| Final displayed split | 20% stratified, seed `42` | Used after the final pipeline has already been fit on all observations; it is not an independent holdout. |

The MLP grid contains 336 hyperparameter combinations (`14 × 2 × 3 × 2 × 2`) before inner folds are considered. Changing this grid or either fold count can greatly change execution time.

## Warning and Display Configuration

Notebook 01 sets the Seaborn style/theme globally. Notebook 02 suppresses these warning categories for the remainder of the live kernel:

- `sklearn.exceptions.ConvergenceWarning`
- `UserWarning`
- `FutureWarning`

There is no debug switch to re-enable them. For diagnostic work, remove/comment those filters or restart the kernel and run a modified import cell. This changes only warning visibility, not the saved estimator settings.

## Logging Configuration

No Python logging handlers are configured. Text progress, metrics, and errors appear in notebook cell output. There is no log level, log directory, file rotation, or console/file toggle.

## Platform-Specific Configuration

**Classification: Windows-first recorded workflow; not proven Windows-only.**

- Use VS Code on Windows with the Python and Jupyter extensions for the documented setup.
- Notebook metadata records Python 3.11.9, but the repository does not declare a supported version range.
- The complete dependency snapshot is Windows-oriented because it pins `pywin32`.
- The active notebook code uses relative paths and no explicit Windows-only API. That absence alone does not establish support; no other operating system is documented as validated.
- `n_jobs=-1` can result in different process/thread behavior across operating systems and numerical-library builds, so results/resource use outside the recorded environment require independent validation.

No frozen executable, subprocess integration, Docker image, or OS scheduler is involved.

## Production vs Development Configuration

The repository has no production/development profiles. Every run is an interactive analysis run using the same notebook constants. If experimenting with reduced grids or alternative thresholds, preserve the original values in version control and clearly label resulting outputs; there is no configuration layer that separates such variants automatically.

## Configuration Example

A clean Windows/VS Code setup requires no secret file. First create the environment in a VS Code integrated terminal:

```console
git clone https://github.com/chrisd314/02452-machine-learning-project-fall-2025.git
cd 02452-machine-learning-project-fall-2025
py -m venv .venv
```

Run **Python: Select Interpreter**, choose `.venv\Scripts\python.exe`, and open a new integrated terminal. Then run:

```console
python -m pip install -r requirements.txt
python -c "from pathlib import Path; print(Path('Concrete_Data.xls').is_file())"
```

The final command should print `True`. Open each notebook, use **Select Kernel** → **Python Environments** to choose the same `.venv`, and run from the first cell.

To use another dataset intentionally, it must provide the expected numeric schema or the data-selection cells must be updated. Changing only the filename is insufficient if the target header or columns differ.

## Configuration Validation

Current validation is limited:

- Notebook 01 prints per-column missing-value counts and the `(N, M)` feature shape after loading.
- Both notebooks fail immediately if the workbook cannot be read or the exact target header is absent.
- Scikit-learn validates numeric inputs, fold viability, estimator parameters, and class availability during fitting.
- There is no explicit schema assertion, range check, duplicate-row check, class-boundary validation, dependency-version check, disk-space check, or run-time estimate.
- Notebook 02 suppresses several warning categories that might otherwise reveal convergence or deprecation concerns.

Recommended preflight checks:

1. Confirm the working directory contains `Concrete_Data.xls`.
2. Confirm the selected kernel reports the intended Python environment.
3. Run the data-loading cell and verify 1,030 instances, eight predictors, and zero missing values for the tracked dataset.
4. Confirm adequate CPU time is available before starting notebook 02.
5. Restart and run all cells so stale kernel variables do not affect results.

## Configuration Troubleshooting

| Symptom | Configuration to check | Resolution |
| --- | --- | --- |
| Dataset cannot be found | Process working directory and dataset filename | Launch from the repository root; do not rely only on the notebook file’s visual location in the editor. |
| Target-column `KeyError` | Exact workbook header | Restore the tracked workbook or update both target lookups consistently. |
| `.xls` engine/import failure | Active kernel and `xlrd` installation | Install `requirements.txt` into the selected kernel environment. |
| Kernel named `ml_project` is unavailable | Local kernelspec/interpreter selection | Select the newly created `.venv`; the metadata name does not provision it automatically. |
| Full requirements fail outside Windows | `pywin32==311` | Use the documented Windows/VS Code environment or establish a separately tested platform-specific dependency set. |
| Machine becomes heavily loaded | `n_jobs=-1`, fold counts, and candidate grids | Run when resources are available or intentionally reduce these constants for exploratory work. |
| Classification counts or metrics changed unexpectedly | Thresholds, random seeds, grid definitions, and cell order | Restore documented values, restart the kernel, and run all cells. |
| Convergence concerns are not visible | Warning filters in notebook 02 | Temporarily remove the filters and rerun from the import cell. |
| Static PNGs differ from output | No configured export process | Update `Plots/` manually only if deliberate; reruns do not synchronize it. |

## Configuration Checklist

- [ ] VS Code has the Microsoft Python and Jupyter extensions installed.
- [ ] Repository is opened with its root as the process working directory.
- [ ] `Concrete_Data.xls` exists with its original filename and schema.
- [ ] `Concrete_Readme.txt` remains available with the dataset attribution.
- [ ] `.venv\Scripts\python.exe` is selected as both the VS Code interpreter and notebook kernel.
- [ ] Dependencies from `requirements.txt` are installed.
- [ ] Any non-Windows dependency adjustments have been independently validated.
- [ ] Notebook 01 is run from the first cell after a kernel restart.
- [ ] Sufficient CPU time is available before running notebook 02.
- [ ] Notebook 02 is run from the first cell after a kernel restart.
- [ ] Warning suppression is acceptable for the intended run, or diagnostic filters have been adjusted deliberately.
- [ ] Embedded outputs are reviewed for stale results and local path disclosure before committing.
- [ ] `Plots/` updates, if any, were made deliberately because export is not automatic.
