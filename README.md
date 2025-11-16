# Bike Sharing Demand Prediction 

## Problem Overview

BoomBikes, a US bike-sharing provider, experienced a major decline in revenue during the COVID-19 pandemic. As the market is expected to recover, the company wants to understand:

1. Which factors impact bike rental demand?
2. How strongly each variable affects customer usage?
3 .How to predict future demand using historical trends?

This project builds a Multiple Linear Regression (MLR) model to analyze and predict daily bike rental demand (cnt) using temporal and meteorological variables.

## Business Objective

The main objectives are:

1. Identify significant variables influencing bike demand
2. Understand how features affect customer behavior
3. Build a regression model to forecast daily demand
4. Provide business insights for operational planning

## Dataset Description

The dataset includes:
1. Temporal Features: season, month, weekday, workingday, year
2. Weather Features: temperature, humidity, windspeed, weathersit
3. Target Variable: cnt (total rentals per day)
4. Columns casual and registered were removed because cnt = casual + registered Keeping them would leak the target variable into the model.

## Approach 

1. Import the library. 
2. Reading the Data from day.csv file and understandin the data with a check if any null value is present or not Loaded data, checked structure, data types, and performed initial understanding.
3. Data Cleaning and Feature Transformation
   * Checked for missing values
   * Dropped casual and registered
   * Converted yr into proper year format
   * Ensured all datatypes are appropriate

Converted categorical codes (e.g., season=1,2,3,4) into meaningful labels before encoding.
This improves interpretability and avoids wrong ordinal assumptions.

4. Exploratory Data Analysis

    Performed:
    * Correlation matrix
    * Histogram plot with Kernel Density Estimate
    * scatter plot: temp vs demand count
  
    Key Findings:
    temp strongly correlates with cnt
    humidity and windspeed negatively impact demand
    bad weather drastically reduces usage
    Dropped atemp to remove multicollinearity

5. Feature Engineering with One-Hot Encode Categorical Features

    * Created dummy variables for:
    * season
    * weathersit
    * month
    Used: drop_first=True to avoid dummy variable trap and multicollinearity.Converted boolean columns (True/False) into integers (0/1) for VIF calculations.

6. Multicollinearity Check (VIF)

    * Calculated VIF for all independent variables
    * Found extremely high VIF for atemp due to correlation with temp
    * Rechecked VIF until all values were acceptable

7. Train-Test Split

    * Used 80% of data for training
    * Used 20% for testing
    * Ensures generalization and prevents overfitting

8. Feature Scaling

    Scaled only continuous variables using StandardScaler:
    * temp
    * hum
    * windspeed
    Dummy variables were not scaled.

9. Model Building (Sklearn Linear Regression)

    Trained the LR model on scaled training data.
   * Model Performance:
   * Train R² : 0.84
   * Test R²  : 0.86
   * Test RMSE: ~684

    Excellent generalization — model performs consistently on unseen data.

10. OLS Model (Statsmodels) + INTERPRETATION

    Used OLS to obtain:
   * p-values
   * coefficients
   * t-stats
   * confidence intervals
   * Significant Predictors:
   * temp
   * year
   * hum
   * windspeed
   * weathersit (mist & snow/rain)
   * spring and winter
   * holiday
   * weekday
   * month_september
   * Insignificant Predictors (can be removed):
   * workingday
   * season_summer
   * most month dummies

11. Residual Analysis

    Validated Linear Regression assumptions.
   * Residual Histogram + KDE:
   * Residuals centered around zero → normal distribution.
   * Q–Q Plot:Residuals align closely with the diagonal line → normality assumption holds.
   * Residuals vs Predicted Plot: Random scatter, no funnel → homoscedasticity satisfied.

Conclusion:
Model satisfies all OLS assumptions.

12. Final Test R² Score
    Final Test R² Score = 0.8632409951977176