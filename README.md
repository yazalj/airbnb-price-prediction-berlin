# Airbnb Price Prediction in Berlin

## Project Overview

This repository contains our final project for the B.Inf.1236 Machine Learning course, SoSe 2026.

The goal is to build and evaluate machine-learning models that predict the advertised nightly price of Airbnb listings in Berlin. The project uses data from Inside Airbnb and evaluates listing-level tabular features, spatial information, and simple features derived from listing text.

The implemented workflow includes:

- data loading, cleaning, and exploratory data analysis
- feature engineering for tabular, spatial, and text-derived information
- train/test splitting, training-only imputation, and training-only cross-validation
- baseline, linear, tree-ensemble, and PyTorch neural-network models
- model comparison, ablation studies, random-split and unseen-host stress tests, error analysis, and uncertainty estimates
- a concise final-results notebook for presentation and reporting

## Group Members

| Name | GitHub Username | Notes |
|---|---|---|
| Yazan Aljerro | yazalj |  |
| Zeynep Sude Kırlı | zeyneps13 |  |
| Natasha Machate | KitCatasha |  |

## Research Question

How accurately can advertised Airbnb nightly prices in Berlin be predicted from listing characteristics and location information, and do the available simple text-derived features provide additional predictive value?

## Dataset

The data comes from [Inside Airbnb](https://insideairbnb.com/get-the-data/), an open-data platform that provides Airbnb datasets for different cities.

For this project, we use the Berlin dataset. The processed data and the reported notebook results are based on the Berlin snapshot scraped on 23–24 September 2025. Running the workflow with a newer snapshot may produce different row counts, feature distributions, and model results.

The raw files loaded by `notebooks/01_eda.ipynb` are:

```text
listings.csv
calendar.csv
reviews.csv
neighbourhoods.csv
neighbourhoods.geojson
```

Depending on how the files are downloaded, the compressed versions may also appear as:

```text
listings.csv.gz
calendar.csv.gz
reviews.csv.gz
```

The first notebook supports both `.csv` and `.csv.gz` versions of the listings, calendar, and reviews files. The current predictive table is built from the detailed listings data. The neighbourhood GeoJSON is used for spatial visualizations; the separate calendar and reviews tables are loaded for dataset inspection but are not merged into the final modeling table.

Image data is not used in this project.

## Data Setup

The raw and processed data files are not included in the repository because they are large and publicly available.

Each group member should download the Berlin files from Inside Airbnb and place them inside:

```text
data/raw/
```

Expected local structure:

```text
data/raw/
├── listings.csv
├── calendar.csv
├── reviews.csv
├── neighbourhoods.csv
└── neighbourhoods.geojson
```

Compressed versions of the three main CSV files are also accepted:

```text
data/raw/
├── listings.csv.gz
├── calendar.csv.gz
├── reviews.csv.gz
├── neighbourhoods.csv
└── neighbourhoods.geojson
```

The notebooks create three local generated files:

```text
data/processed/listings_cleaned.csv
data/processed/modeling_dataset.csv
data/processed/final_results.json
```

- `01_eda.ipynb` creates `listings_cleaned.csv`.
- `02_feature_engineering.ipynb` creates `modeling_dataset.csv`.
- `03_modeling.ipynb` creates `final_results.json`, which keeps Notebook 04 synchronized with the executed modeling results.

All three files are ignored by Git and can be reproduced by running the notebooks in order.

## Modalities Used

The project evaluates three data modalities.

### 1. Tabular data

Examples include:

- room type and accommodation capacity
- bedrooms, beds, and parsed bathroom count
- minimum and maximum nights
- availability windows
- review counts and review scores
- host and booking indicators

### 2. Spatial data

The spatial representation includes:

- latitude and longitude
- Berlin borough one-hot indicators
- Haversine distance to the Berlin city centre

### 3. Text-derived data

The current text representation uses simple numerical summaries from the listing name, description, and neighbourhood overview:

- character length
- word count

TF-IDF, pretrained language embeddings, and image features were not implemented. The text ablation showed that these six simple text summaries did not improve the selected Random Forest, so the final selected model uses tabular and spatial features without them.

## Models Evaluated

The implemented comparison includes:

- median-price baseline
- mean-price baseline
- Linear Regression
- Ridge Regression with cross-validated regularization
- Random Forest Regressor with a compact hyperparameter search
- Gradient Boosting Regressor with structure and loss-function comparisons
- a PyTorch multilayer perceptron with train-only imputation and standardization, a log-transformed target, AdamW, dropout, Huber loss, and early stopping

The final candidate is selected by the lowest training cross-validated MAE rather than by the test-set score.

## Evaluation Metrics

The primary metric is:

- **MAE**: Mean Absolute Error in euros per night

Supporting metrics are:

- **RMSE**: Root Mean Squared Error
- **R²**: Coefficient of determination
- median absolute error
- proportions of predictions within €10, €25, and €50 of the advertised price

MAE is the primary metric because it measures the average absolute euro error directly. RMSE is also reported because it penalizes large mistakes more strongly.

### Validation strategy

- fixed 80/20 listing-level train/test split
- target-decile stratification to preserve the skewed price distribution
- reconstruction of originally missing numeric values followed by median imputation fitted inside each training split or fold
- three-fold cross-validation on the training set for model and hyperparameter selection
- five predetermined repeated listing-level holdout splits as a stability analysis
- five group-based stress-test splits with zero host overlap
- listing-level and host-cluster bootstrap confidence intervals for MAE and RMSE on the fixed test predictions

### Main result

In the latest fully executed `03_modeling.ipynb`, the training-selected final model is a Random Forest trained on `log1p(price)` using tabular and full spatial features while excluding the six simple text summaries.

Its fixed holdout performance is:

- **MAE:** €29.50 per night
- **RMSE:** €48.99 per night
- **R²:** 0.648
- **Within €25:** 63.4%
- **Within €50:** 84.4%

Across five repeated random listing splits, its mean MAE was €29.33 ± €0.34. Across five group-based splits with no host overlap, mean MAE increased to €34.40 ± €1.47, showing that prediction for listings from previously unseen hosts is more difficult.

The raw-target Random Forest has slightly better RMSE and R², while the log-target model has better MAE. The final choice therefore reflects the declared priority given to typical absolute euro error rather than universal superiority on every metric.

### Important limitations

- The 99th-percentile price restriction and one-hot category vocabulary were defined in Parts 1 and 2 before splitting. Notebook 3 corrects the main fill-value leakage by reconstructing the relevant missing numeric values and fitting median imputation inside every training split or fold.
- The model's declared target population excludes listings with missing prices and the removed extreme upper tail.
- The compact hyperparameter searches and candidate comparison reuse the same training cross-validation folds, so their selection scores can be mildly optimistic. The fixed test set provides the main independent evaluation.
- The unseen-host analysis is a secondary stress test of the already selected configuration, not a separate independent model-selection benchmark.
- The current text modality measures text quantity rather than semantic content.
- The data comes from one Berlin snapshot and cannot establish performance for future market conditions, other cities, or changing platform behavior.
- The analysis is predictive and does not support causal claims about why a feature is associated with price.

## Repository Structure

```text
airbnb-price-prediction-berlin/
│
├── data/
│   ├── raw/
│   │   └── .gitkeep
│   └── processed/
│       └── .gitkeep
│
├── notebooks/
│   ├── 01_eda.ipynb
│   ├── 02_feature_engineering.ipynb
│   ├── 03_modeling.ipynb
│   └── 04_final_results.ipynb
│
├── README.md
├── requirements.txt
└── .gitignore
```

The workflow is separated into four task-based notebooks. Generated datasets remain local under `data/processed/`, while the executed modeling and final-results notebooks retain the principal tables and figures needed to inspect the results.

## Group Responsibilities

The work is divided into three main parts.

### Responsibility Assignment

| Part | Main Responsibility | Responsible Group Member |
|---|---|---|
| Part 1 | Data loading, cleaning, and EDA | Zeynep Sude Kırlı |
| Part 2 | Feature engineering | Natasha Machate |
| Part 3 | Modeling, evaluation, and final-results summary | Yazan Aljerro |
| Shared | Presentations, repository cleanup, and final discussion | All group members |

### Part 1: Data Loading, Cleaning, and EDA

Responsibilities:

- load the Berlin Airbnb files
- clean and validate the target price
- handle selected missing values and create missingness indicators
- remove non-positive and upper-tail prices according to the documented rule
- perform exploratory data analysis and spatial visualizations
- save the cleaned listing-level dataset locally

Main file:

- `notebooks/01_eda.ipynb`

### Part 2: Feature Engineering

Responsibilities:

- select listing-level predictors
- create the distance-to-centre spatial feature
- derive simple text-length and word-count features
- encode categorical and binary variables
- prepare and save the numeric modeling table

Main file:

- `notebooks/02_feature_engineering.ipynb`

### Part 3: Modeling and Evaluation

Responsibilities:

- audit the modeling table and prevent identifier leakage
- reconstruct originally missing numeric values and fit imputation inside training splits
- create train/test and cross-validation splits
- train baselines, linear models, tree ensembles, and a PyTorch MLP
- tune selected hyperparameters on training data
- compare raw-price and log-price targets
- perform spatial and text ablations
- assess random-split stability, unseen-host generalization, subgroup errors, feature importance, and bootstrap uncertainty
- summarize the final results and limitations

Main files:

- `notebooks/03_modeling.ipynb`
- `notebooks/04_final_results.ipynb`

All group members contribute to the presentations, repository cleanup, and final discussion.

## Project Deliverables

### 1. Intermediate Presentation

The intermediate presentation covers:

- chosen city and dataset
- selected data modalities
- initial dataset overview
- cleaning and EDA results
- planned feature engineering and models
- encountered problems and next steps

### 2. Final Presentation

The final presentation should briefly communicate:

- project objective and Berlin dataset
- preprocessing and feature engineering
- model descriptions and validation strategy
- principal evaluation results
- modality and model trade-offs
- limitations and conclusions

`notebooks/04_final_results.ipynb` is the concise presentation-oriented summary; the complete experiments and diagnostics remain in `notebooks/03_modeling.ipynb`.

### 3. Code Repository

The repository separates data preparation, feature engineering, modeling, and final reporting into different notebooks. Raw and generated data are excluded from version control and can be reproduced locally.

## How to Run the Project Locally

### 1. Clone the repository

```bash
git clone https://github.com/yazalj/airbnb-price-prediction-berlin.git
cd airbnb-price-prediction-berlin
```

### 2. Create a virtual environment

On Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

On macOS/Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install the required packages

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Register the virtual environment as a Jupyter kernel

```bash
python -m ipykernel install --user --name airbnb-ml-project --display-name "Python (Airbnb ML Project)"
```

### 5. Open the project in VS Code

Open the full repository folder:

```text
airbnb-price-prediction-berlin/
```

Do not open only the `notebooks/` folder, because the workflow depends on the shared repository structure.

### 6. Select the correct notebook kernel

Open a notebook, click **Select Kernel**, and choose either:

```text
Python (Airbnb ML Project)
```

or the local `.venv` environment. If it does not appear immediately, refresh the kernel list.

### 7. Add the raw data

Download the Berlin files from Inside Airbnb and place them in `data/raw/` using one of the structures shown in the Dataset section.

### 8. Run the notebooks in order

```text
notebooks/01_eda.ipynb
notebooks/02_feature_engineering.ipynb
notebooks/03_modeling.ipynb
notebooks/04_final_results.ipynb
```

The generated files are:

```text
01_eda.ipynb                 -> data/processed/listings_cleaned.csv
02_feature_engineering.ipynb -> data/processed/modeling_dataset.csv
03_modeling.ipynb             -> data/processed/final_results.json
```

`03_modeling.ipynb` performs the full model fitting and evaluation. It includes split-aware imputation, a CPU-compatible PyTorch MLP, repeated random splits, unseen-host stress tests, and bootstrap analyses, so it takes considerably longer than the first two notebooks. A GPU is not required by the current implementation.

`04_final_results.ipynb` is a concise reporting notebook that reads the synchronized result bundle generated by Notebook 03, so its numerical tables do not need to be copied or updated manually.

## Notes

Raw and processed data files are ignored by Git.

The following files should stay local:

```text
data/raw/listings.csv
data/raw/calendar.csv
data/raw/reviews.csv
data/raw/neighbourhoods.csv
data/raw/neighbourhoods.geojson
data/processed/listings_cleaned.csv
data/processed/modeling_dataset.csv
data/processed/final_results.json
```

The compressed raw files should also stay local if used:

```text
data/raw/listings.csv.gz
data/raw/calendar.csv.gz
data/raw/reviews.csv.gz
```

Only code, notebooks, documentation, and presentation material should be committed to GitHub.

Because the dependency versions in `requirements.txt` are not pinned, very small numerical differences may occur across Python, NumPy, scikit-learn, or PyTorch versions. The qualitative conclusions should be checked again after any environment or data change.

## Troubleshooting

### A notebook cannot find the data files

Open the complete repository folder and confirm that the files are placed under `data/raw/` or `data/processed/` as expected. Run the notebooks in numerical order so that each generated input exists before the next notebook starts.

### The Jupyter kernel does not appear in VS Code

Activate the virtual environment and register it again:

```bash
python -m ipykernel install --user --name airbnb-ml-project --display-name "Python (Airbnb ML Project)"
```

Then refresh the kernel selection menu and choose the `.venv` environment.

### PyTorch is not installed

With the project environment activated, run:

```bash
pip install torch
```

Then restart the notebook kernel.

### Results differ slightly from the committed notebook outputs

Confirm that the same Berlin data snapshot is being used. Minor differences can also arise from library versions and floating-point behavior. Larger differences usually indicate a different dataset snapshot, changed preprocessing, or notebooks run out of order.

## Sources

- Inside Airbnb. *Get the Data*.  
  Available at: https://insideairbnb.com/get-the-data/  
  Source of the Berlin listings, calendar, reviews, and neighbourhood files.

- Inside Airbnb. *About Inside Airbnb*.  
  Available at: https://insideairbnb.com/about/  
  Background information about the project and data collection.

- Pandas Documentation. *User Guide*.  
  Available at: https://pandas.pydata.org/docs/user_guide/  
  Used for data loading, cleaning, feature preparation, and result tables.

- NumPy Documentation.  
  Available at: https://numpy.org/doc/stable/  
  Used for numerical operations and reproducible array processing.

- Matplotlib Documentation.  
  Available at: https://matplotlib.org/stable/users/index.html  
  Used for visualizations.

- Seaborn Documentation.  
  Available at: https://seaborn.pydata.org/  
  Used for exploratory data visualization.

- GeoPandas Documentation.  
  Available at: https://geopandas.org/en/stable/  
  Used for neighbourhood GeoJSON data and spatial plots.

- Scikit-learn Documentation. *User Guide*.  
  Available at: https://scikit-learn.org/stable/user_guide.html  
  Used for preprocessing pipelines, regression models, cross-validation, metrics, ablations, and permutation importance.

- PyTorch Documentation.  
  Available at: https://pytorch.org/docs/stable/  
  Used for the multilayer perceptron benchmark.
