# Airbnb Price Prediction in Berlin

## Project Objective
This project aims to predict nightly Airbnb listing prices in Berlin using machine learning models.

## Dataset
The data comes from Inside Airbnb. We use the Berlin dataset, including listing, calendar, review, and neighbourhood information.

## Modalities
We plan to use:
- Tabular features from listing data
- Spatial features such as latitude, longitude, neighbourhood, and distance to city center
- Text features from listing names and descriptions

## Models
We plan to compare:
- Baseline median prediction
- Linear/Ridge Regression
- Random Forest Regressor
- Gradient Boosting Regressor
- A combined model using tabular, spatial, and text features

## Evaluation
The models will be evaluated using:
- MAE
- RMSE
- R²

## Repository Structure
- `data/`: raw and processed data, not uploaded to GitHub
- `notebooks/`: exploratory notebooks
- `src/`: reusable Python scripts
- `reports/`: figures and presentation files
