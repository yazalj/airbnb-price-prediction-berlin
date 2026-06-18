# Airbnb Price Prediction in Berlin

## Project Overview

This repository contains our final project for the SoSe 2026 Machine Learning course.

The goal of the project is to build a machine learning model that predicts the nightly price of Airbnb listings. We use data from Inside Airbnb and focus on the city of Berlin.

The project includes data cleaning, exploratory data analysis, feature engineering, model training, model evaluation, and result interpretation.

## Research Question

Can we predict Airbnb nightly listing prices in Berlin using machine learning models based on listing characteristics, location information, and textual descriptions?

## Dataset

The data comes from [Inside Airbnb](https://insideairbnb.com/get-the-data/), an open-data platform that provides Airbnb datasets for different cities.

For this project, we use the Berlin dataset.

The required raw data files are:

* `listings.csv.gz`
* `calendar.csv.gz`
* `reviews.csv.gz`
* `neighbourhoods.csv`
* `neighbourhoods.geojson`

## Data Setup

The raw data files are **not included in this repository** because they are large and publicly available online.

Each group member should download the Berlin dataset manually from Inside Airbnb and place the files inside the following folder:

```text
data/raw/
```

After downloading the files, the folder should look like this:

```text
data/raw/
├── listings.csv.gz
├── calendar.csv.gz
├── reviews.csv.gz
├── neighbourhoods.csv
└── neighbourhoods.geojson
```

The code assumes that the data files are stored in this location.

## Modalities Used

The project uses multiple data modalities:

1. **Tabular data**

   * Listing characteristics such as room type, number of bedrooms, number of bathrooms, accommodates, minimum nights, availability, and review scores.

2. **Spatial data**

   * Location-related information such as latitude, longitude, neighbourhood, and distance to Berlin city center.

3. **Text data**

   * Textual information from listing names and descriptions, processed using simple text-based features such as TF-IDF.

Image data may be considered as a possible future extension, but it is not part of the first project version.

## Planned Models

We plan to compare several machine learning models:

* Baseline median price model
* Linear Regression / Ridge Regression
* Random Forest Regressor
* Gradient Boosting Regressor
* A combined model using tabular, spatial, and text features

## Evaluation Metrics

The models will be evaluated using:

* **MAE**: Mean Absolute Error
* **RMSE**: Root Mean Squared Error
* **R²**: Coefficient of determination

MAE will be especially useful because it can be interpreted as the average prediction error in the listing price.

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

### Member 1: Data Loading, Cleaning, and EDA

Responsibilities:

* Download and load the Berlin Airbnb data
* Clean the target price variable
* Handle missing values and outliers
* Perform exploratory data analysis
* Create initial visualizations

Main files:

* `src/data_loading.py`
* `src/cleaning.py`
* `notebooks/01_eda.ipynb`

### Member 2: Feature Engineering

Responsibilities:

* Create tabular features
* Encode categorical variables
* Create spatial features
* Create text features from names and descriptions
* Prepare the final feature matrix for modeling

Main files:

* `src/features_tabular.py`
* `src/features_spatial.py`
* `src/features_text.py`
* `notebooks/02_feature_engineering.ipynb`

### Member 3: Modeling and Evaluation

Responsibilities:

* Train baseline and machine learning models
* Compare model performance
* Tune selected models
* Create evaluation tables and plots
* Interpret model results

Main files:

* `src/models.py`
* `src/evaluate.py`
* `notebooks/03_modeling.ipynb`
* `notebooks/04_final_results.ipynb`

All group members will contribute to the intermediate presentation, final presentation, repository cleanup, and final discussion.

## Project Deliverables

The project deliverables are:

1. **Intermediate Presentation**

   * Preliminary ideas
   * Chosen city and dataset
   * Initial data exploration
   * Planned features and models
   * Problems encountered and next steps

2. **Final Presentation**

   * Project objective
   * Data preprocessing
   * Feature engineering
   * Model descriptions
   * Evaluation results
   * Discussion and conclusion

3. **Code Repository**

   * Organized code repository
   * Not everything in a single notebook
   * Code separated by task
   * Uploaded to StudIP by the deadline

## How to Run the Project

1. Clone the repository.

```bash
git clone <repository-url>
```

2. Install the required Python packages.

```bash
pip install -r requirements.txt
```

3. Download the Berlin data from Inside Airbnb.

4. Place the downloaded files in:

```text
data/raw/
```

5. Run the notebooks in order:

```text
01_eda.ipynb
02_feature_engineering.ipynb
03_modeling.ipynb
04_final_results.ipynb
```

## Notes

The raw data and processed data files are not tracked by Git. They should be downloaded and stored locally.

The repository is organized so that all group members can use the same folder structure and run the code on their own computers.

## Sources

- Inside Airbnb. *Get the Data*. Available at: https://insideairbnb.com/get-the-data/  
  Used as the source for the Berlin Airbnb datasets, including listings, calendar, reviews, and neighbourhood data.

- Inside Airbnb. *About Inside Airbnb*. Available at: https://insideairbnb.com/about/  
  Used for background information about the Inside Airbnb project and its data collection purpose.
