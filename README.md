# Balancing Energy Price Prediction

This notebook explores different machine learning models to predict balancing energy prices in Estonia. Balancing energy is crucial for stabilizing the electricity grid by compensating for real-time supply and demand fluctuations. The price of balancing energy reflects the cost of these adjustments.

## Problem Description

The goal is to build a model that can predict balancing energy prices 24 hours ahead, trained on historical data up to 2024-09-01 and tested on data from 2024-09-01 to 2024-09-30. The models are trained once, and then daily predictions are made for the next 24 hours using previously predicted values. At the start of each new day in the test set, the predicted values from the previous day are replaced with the actual ground truth values to simulate real-time data availability.

## Data

The dataset used for this project contains hourly balancing energy prices for Estonia, Latvia, and Lithuania, along with 'Upward' and 'Downward' price components. For this analysis, only the 'Upward' price for Estonia is used.

The data includes the following columns:

- `datetime_from`: Start time of the hour
- `datetime_to`: End time of the hour
- `area`: Country (Estonia, Latvia, Lithuania)
- `Upward`: Upward balancing energy price
- `Downward`: Downward balancing energy price

The data undergoes preprocessing to combine 'Upward' and 'Downward' prices (using 'Upward' as the target variable 'price'), filter for 'Estonia' only, and handle datetime formatting and missing values.

## Exploratory Data Analysis

Initial data exploration reveals that:

- Most prices fall between approximately -100 and 500.
- Trends appear to be more date-dependent than time-dependent, with spikes observed on special dates like New Year.
- Negative prices indicate that balancing energy providers may be willing to pay.
- Feature engineering on the datetime column (e.g., hour, day, month, year) is necessary to help models identify patterns associated with specific times or dates.

## Models

Three different models are implemented and evaluated:

1.  **Linear Regression**: A simple linear model to establish a baseline performance.
2.  **XGBoost**: A gradient boosting model expected to handle sharp jumps and complex relationships in the data well. Feature engineering is particularly beneficial for this model.
3.  **LSTM (Long Short-Term Memory)**: A recurrent neural network suitable for time series forecasting, included for comparison with traditional machine learning models.

## Feature Engineering

Several features are engineered from the raw data to improve model performance, including:

- Lagged prices (up to 24 hours)
- Rolling means (24 and 72 hours)
- Price changes (1, 6, and 24 hours)
- Rolling standard deviations (6 and 24 hours)
- Rolling price ranges (6 hours)
- Time-based features (hour, day, month, year)

## Model Evaluation

The models are evaluated based on Root Mean Squared Error (RMSE) and Mean Absolute Error (MAE) on the test set. The predictions are visualized against the actual prices for different timeframes (full test set, one week, one day).

## Confidence Intervals

The notebook discusses methods for quantifying the uncertainty of the predictions by calculating confidence intervals.

- For Linear Regression, analytical confidence intervals can be derived using libraries like `statsmodels`.
- For XGBoost and LSTM, empirical methods like bootstrapping or quantile regression can be used. For LSTM, a Monte Carlo dropout approach is also discussed.

## External Data

The potential benefit of incorporating external data such as weather, energy demand, or market prices to improve model accuracy is discussed.

## Production Usage

A potential workflow for deploying and using the trained models in a production environment is outlined, covering:

- Automated data updating and model retraining.
- Daily prediction generation.
- Integration with applications via APIs.
- Monitoring of model performance and data quality.

## Conclusion

The models implemented show similar performance on this dataset, with XGBoost performing slightly better. The high volatility of the data makes accurate prediction challenging. Incorporating external data is suggested as a potential way to improve model performance. Quantifying uncertainty through confidence intervals is also a valuable addition.
