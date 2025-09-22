
# Automotive Sales Forecasting: End-to-End Prediction for the Indonesian Market

This repository contains a single, comprehensive Jupyter Notebook that details an end-to-end pipeline for forecasting monthly automotive sales in Indonesia. The project's objective is to predict the sales figures for August 2025 for five major car brands: **Toyota, Daihatsu, Honda, Mitsubishi, and Suzuki**.

The approach is rooted in rigorous time-series analysis, featuring robust data processing, extensive feature engineering, and a competitive model evaluation framework designed to prevent data leakage and ensure stable, reliable predictions.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1aWtR0jZtnFqbw5RaekPeQMfcJ8vkfIQE)

## Table of Contents
- [Key Methodologies & Features](#key-methodologies--features)
- [Data Sources](#data-sources)
- [How to Run](#how-to-run)
- [Final Model Selection & Results](#final-model-selection--results)
- [License](#license)

## Key Methodologies & Features

This project showcases a wide range of data science and machine learning techniques for time-series forecasting:

-   **Robust Data Preprocessing**: Handles raw data ingestion, cleaning, reshaping from wide to long format, and implements a conservative statistical imputation strategy for missing values.
-   **Advanced Feature Engineering**:
    -   **Lag Features**: Incorporates sales data from previous months and years (`sales_l1`, `sales_l12`).
    -   **Macroeconomic Indicators**: Integrates external data like BI interest rates and CPI, using lagged values to prevent data leakage.
    -   **Calendar & Event Features**: Quantifies the impact of key events like Ramadan, Eid al-Fitr, school holidays, and the GIIAS auto show by calculating their proportion of days in a month.
    -   **Google Trends Data**: Leverages public search interest data as a proxy for market sentiment.
-   **Rigorous Cross-Validation**: Employs a **rolling-origin cross-validation** strategy with a one-month forecast horizon (H=1) and a sliding training window (36-48 months) to simulate real-world forecasting and prevent overfitting.
-   **Competitive Model Tournament**: Systematically evaluates a wide array of models to identify the best performer for each brand:
    -   **Statistical Baselines**: `Naive`, `SNAIVE`, `ETS`, `Theta`.
    -   **Exogenous Models**: `SARIMAX` and `Ridge` regression on residuals.
    -   **Machine Learning**: `LightGBM` with conservative, anti-overfit parameters.
    -   **Custom Heuristics**: Develops specialized, rule-based models like **ASI (August Seasonal Index)** and **AUGR (August/July Ratio)**, which proved highly effective for the specific target month.
-   **Anti-Leakage Safeguards**: Strict measures are implemented throughout the pipeline to ensure future information is never used to train the models.



## Data Sources

The model is built using three primary data sources, which are downloaded automatically by the notebook:

1.  **Historical Sales Data**: Monthly sales figures for the five target brands.
2.  **External Macroeconomic Data**: Key economic indicators, including the BI 7-Day Reverse Repo Rate and various Consumer Price Index (CPI) metrics.
3.  **Google Trends Data**: Public search interest scores for each brand in Indonesia under the "Autos & Vehicles" category.

> **Note**: The raw data is not stored in this repository. The notebook downloads it directly from public Google Drive links. To run the notebook successfully, please ensure these links are accessible.

## How to Run

This project is best run in a Google Colab environment.

1.  **Open the Notebook**: Click the "Open In Colab" badge at the top of this README.
2.  **Ensure Data Accessibility**: The notebook downloads data from Google Drive links defined in the first code cell. Verify that these links are public and accessible to "Anyone with the link".
3.  **Execute the Pipeline**: Select **"Run all"** from the "Runtime" menu in Colab. The notebook will execute all steps sequentially, from data download to final prediction.
4.  **Find the Output**: The final prediction file, `submission_team_rendang.csv`, will be saved in the `/content/outputs/` directory in the Colab environment.

**Key Dependencies** (automatically available in Colab):
-   `pandas`
-   `numpy`
-   `statsmodels`
-   `scikit-learn`
-   `lightgbm`
-   `requests`

## Final Model Selection & Results

After an exhaustive cross-validation tournament focused specifically on historical August performance, a hybrid strategy was chosen. Standard time-series models were found to be most stable, with a custom heuristic model providing the best results for Mitsubishi's unique sales pattern.

#### Cross-Validation Performance (August-Only Folds)

| Brand | Model Selected | sMAPE (CV) |
| :--- | :--- | :---: |
| TOYOTA | `THETA_LOG_36` | 10.18% |
| DAIHATSU | `THETA_LOG_36` | 4.34% |
| HONDA | `THETA_LOG_36` | 6.28% |
| MITSUBISHI | `ASI_REFINE` | **9.70%** |
| SUZUKI | `THETA_LOG_36` | 4.72% |
| **Overall Mean** | | **7.04%** |

#### Final Forecast: August 2025 Sales

The final models were trained on all available data up to July 2025 to produce the following predictions:

| Brand | Forecast (Units) |
| :--- | :---: |
| **TOYOTA** | **8,773** |
| **DAIHATSU** | **6,209** |
| **HONDA** | **2,899** |
| **MITSUBISHI** | **7,879** |
| **SUZUKI** | **2,696** |



---

## License

This project is licensed under the **MIT License**.
