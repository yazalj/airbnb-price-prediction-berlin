# Airbnb Price Prediction in Berlin

## Project Overview

This repository contains our final project for the B.Inf.1236 Machine Learning course, SoSe 2026.

The goal of the project is to build and evaluate machine learning models that predict the nightly price of Airbnb listings in Berlin. We use data from Inside Airbnb and combine different types of information, including listing characteristics, location data, and textual descriptions.

The project includes:

- data loading and cleaning
- exploratory data analysis
- feature engineering
- model training
- model evaluation
- result interpretation
- intermediate and final presentations

## Group Members

Add the group member names here:

| Name | GitHub Username ||
|---|---|---|
| Yazan Aljerro | yazalj ||
| Zeynep Sude Kırlı | zeyneps13 ||
| Natasha Machate |  ||

## Research Question

Can we predict Airbnb nightly listing prices in Berlin using machine learning models based on listing characteristics, location information, and textual descriptions?

## Dataset

The data comes from [Inside Airbnb](https://insideairbnb.com/get-the-data/), an open-data platform that provides Airbnb datasets for different cities.

For this project, we use the Berlin dataset.

The required raw data files are:

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

The notebook is written to support both `.csv` and `.csv.gz` versions of the main files.

## Data Setup

The raw data files are not included in this repository because they are large and publicly available online.

Each group member should download the Berlin dataset manually from Inside Airbnb and place the files inside:

```text
data/raw/
```

On our local setup, the folder should look like this:

```text
data/raw/
├── listings.csv
├── calendar.csv
├── reviews.csv
├── neighbourhoods.csv
└── neighbourhoods.geojson
```

Compressed versions are also acceptable:

```text
data/raw/
├── listings.csv.gz
├── calendar.csv.gz
├── reviews.csv.gz
├── neighbourhoods.csv
└── neighbourhoods.geojson
```

The first notebook creates a cleaned dataset locally:

```text
data/processed/listings_cleaned.csv
```

This processed file is not tracked by Git and should not be uploaded to GitHub. It can be reproduced by running the notebook.

## Modalities Used

The project uses multiple data modalities.

### 1. Tabular data

Listing characteristics such as:

- room type
- accommodates
- bedrooms
- bathrooms
- beds
- minimum nights
- availability
- number of reviews
- review scores

### 2. Spatial data

Location-related information such as:

- latitude
- longitude
- neighbourhood
- distance to Berlin city center

### 3. Text data

Textual information from listing names and descriptions, processed using simple text-based features such as TF-IDF.

Image data may be considered as a possible future extension, but it is not part of the first project version.

## Planned Models

We plan to compare several machine learning models:

- baseline median price model
- Linear Regression / Ridge Regression
- Random Forest Regressor
- Gradient Boosting Regressor
- combined model using tabular, spatial, and text features

## Evaluation Metrics

The models will be evaluated using:

- **MAE**: Mean Absolute Error
- **RMSE**: Root Mean Squared Error
- **R²**: Coefficient of determination

MAE is especially useful because it can be interpreted as the average prediction error in the nightly listing price.

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
├── src/
│   ├── data_loading.py
│   ├── cleaning.py
│   ├── features_tabular.py
│   ├── features_text.py
│   ├── features_spatial.py
│   ├── models.py
│   └── evaluate.py
│
├── reports/
│   ├── figures/
│   ├── intermediate_presentation/
│   └── final_presentation/
│
├── README.md
├── requirements.txt
└── .gitignore
```

## Group Responsibilities

The work is divided into three main parts.

### Responsibility Assignment

Add the responsible group member for each part here:

| Part | Main Responsibility | Responsible Group Member |
|---|---|---|
| Part 1 | Data loading, cleaning, and EDA | Zeynep Sude Kırlı |
| Part 2 | Feature engineering |  |
| Part 3 | Modeling and evaluation |  |

### Part 1: Data Loading, Cleaning, and EDA

Responsibilities:

- download and load the Berlin Airbnb data
- clean the target price variable
- handle missing values and outliers
- perform exploratory data analysis
- create initial visualizations
- save the cleaned dataset locally

Main files:

- `notebooks/01_eda.ipynb`
- `src/data_loading.py`
- `src/cleaning.py`

### Part 2: Feature Engineering

Responsibilities:

- create tabular features
- encode categorical variables
- create spatial features
- create text features from names and descriptions
- prepare the final feature matrix for modeling

Main files:

- `notebooks/02_feature_engineering.ipynb`
- `src/features_tabular.py`
- `src/features_spatial.py`
- `src/features_text.py`

### Part 3: Modeling and Evaluation

Responsibilities:

- build a baseline model
- train machine learning models
- compare model performance
- create evaluation tables and plots
- interpret the model results

Main files:

- `notebooks/03_modeling.ipynb`
- `notebooks/04_final_results.ipynb`
- `src/models.py`
- `src/evaluate.py`

All group members will contribute to the intermediate presentation, final presentation, repository cleanup, and final discussion.

## Project Deliverables

The project deliverables are:

### 1. Intermediate Presentation

The intermediate presentation should include:

- chosen city and dataset
- selected data modalities
- initial dataset overview
- first cleaning and EDA results
- planned feature engineering
- planned models
- problems encountered and next steps

### 2. Final Presentation

The final presentation should include:

- project objective
- data preprocessing
- feature engineering
- model descriptions
- evaluation results
- discussion and conclusion

### 3. Code Repository

The repository should be organized and reproducible. The code should be separated by task and should not be placed entirely in one notebook.

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

Open the full project folder:

```text
airbnb-price-prediction-berlin/
```

Do not open only the `notebooks/` folder, because the notebooks use paths relative to the project root.

### 6. Select the correct notebook kernel

In VS Code, open:

```text
notebooks/01_eda.ipynb
```

Then click **Select Kernel** and choose either:

```text
Python (Airbnb ML Project)
```

or the local virtual environment:

```text
.venv
```

If the kernel does not appear immediately, click the refresh icon in the kernel selection menu.

### 7. Add the raw data

Download the Berlin dataset from Inside Airbnb and place the files in:

```text
data/raw/
```

The expected local structure is:

```text
data/raw/
├── listings.csv
├── calendar.csv
├── reviews.csv
├── neighbourhoods.csv
└── neighbourhoods.geojson
```

or, if using compressed files:

```text
data/raw/
├── listings.csv.gz
├── calendar.csv.gz
├── reviews.csv.gz
├── neighbourhoods.csv
└── neighbourhoods.geojson
```

### 8. Run the notebooks in order

```text
notebooks/01_eda.ipynb
notebooks/02_feature_engineering.ipynb
notebooks/03_modeling.ipynb
notebooks/04_final_results.ipynb
```

The first notebook creates:

```text
data/processed/listings_cleaned.csv
```

This file is generated locally and should not be uploaded to GitHub.

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
```

The compressed raw files should also stay local if they are used:

```text
data/raw/listings.csv.gz
data/raw/calendar.csv.gz
data/raw/reviews.csv.gz
```

Only code, notebooks, documentation, figures, and presentation files should be committed to GitHub.

The repository is organized so that all group members can use the same folder structure and reproduce the data processing steps on their own computers.

## Troubleshooting

### The notebook cannot find the raw data files

Make sure the project is opened from the root folder, not from the `notebooks/` folder.

Correct:

```text
airbnb-price-prediction-berlin/
```

Incorrect:

```text
airbnb-price-prediction-berlin/notebooks/
```

Also make sure the files are placed in:

```text
data/raw/
```

### The Jupyter kernel does not appear in VS Code

First make sure the virtual environment is activated and the kernel was registered:

```bash
python -m ipykernel install --user --name airbnb-ml-project --display-name "Python (Airbnb ML Project)"
```

Then in VS Code, click **Select Kernel** and choose the `.venv` environment. If it does not appear, click the refresh icon in the kernel selection window.

### The notebook starts inside the `notebooks/` directory

The notebook includes a small path fix so that it changes the working directory back to the project root when needed. This helps the notebook find `data/raw/` and save files to `data/processed/`.

## Sources

- Inside Airbnb. *Get the Data*.  
  Available at: https://insideairbnb.com/get-the-data/  
  Used as the source for the Berlin Airbnb datasets, including listings, calendar, reviews, and neighbourhood data.

- Inside Airbnb. *About Inside Airbnb*.  
  Available at: https://insideairbnb.com/about/  
  Used for background information about the Inside Airbnb project and its data collection purpose.

- Pandas Documentation. *User Guide*.  
  Available at: https://pandas.pydata.org/docs/user_guide/  
  Used for data loading, cleaning, and manipulation.

- Matplotlib Documentation.  
  Available at: https://matplotlib.org/stable/users/index.html  
  Used for creating visualizations.

- Seaborn Documentation.  
  Available at: https://seaborn.pydata.org/  
  Used for exploratory data visualization.

- GeoPandas Documentation.  
  Available at: https://geopandas.org/en/stable/  
  Used for loading and working with neighbourhood GeoJSON data.

- Scikit-learn Documentation. *User Guide*.  
  Available at: https://scikit-learn.org/stable/user_guide.html  
  Used for feature engineering, machine learning models, and evaluation metrics.
