# Forecasting Hourly Electricity Demand: A Crash Course in Time Series Analysis
Graham Taylor

### Problem Statement and Goals
Forecasting electricity demand is a common and important task for all electric utility companies. These companies make decisions every day about how much electricity to produce, where and how to produce it, where to route or store it, and how much to buy from or sell to neighboring balancing authorities, as well as at what price. Those decisions depend on the demand they face from their customers.

This project has two goals. First, to better understand the advantages and disadvantages of various time series modeling, forecasting, and model evaluation methodologies. I have very little experience working with time series data, so this project is exploratory and educational in nature. Second, to produce forecasts as good as Portland General Electric's own day-ahead hourly demand forecasts. By my measure, after excluding hours missing either demand data or forecasts, PGE's forecasts have a mean absolute error (MAE) of 50.8 MWh.

### Data
PGE demand data and demand forecasts were collected from the U.S. Energy Information Administration (EIA) via their API (v1). The two series span 22 July 2015 to 10 October 2022 but are incomplete. Daily weather data was downloaded as a CSV from [weather.gov](https://www.weather.gov/wrh/Climate?wfo=pqr) under “Local Data/Records”, Portland (1940-2022).

### Packages and Functions
Project performed in Python 3.9.13 using the following packages:
- pandas
- numpy
- json
- requests
- sklearn
    - .metrics: mean_absolute_error
    - .preprocessing: MinMaxScaler
- datetime
- statistics
- sktime
    - .utils: plot_series
    - .forecasting.arima: ARIMA
- statsmodels
    - .tsa.stattools: adfuller
    - .tsa.seasonal: seasonal_decompose
    - .graphics.tsaplots: plot_acf
    - .graphics.tsaplots: plot_pacf
    - .tsa.api: ExponentialSmoothing
    - .tsa.statespace.sarimax: SARIMAX
- pmdarima
    - .arima: auto_arima

### Navigating the Project
The repository is divided into data, notebooks, and presentation slides. Within data and notebooks, there are folders named "incomplete". These contain work performed before I discovered that the original demand series was missing rows for 215 hours between the start and end of the series. There are also folders named "practice", which are not properly part of the project, but which I kept because of the project's exploratory nature.

### Analysis / Methodology
See the presentation slides, included as a PDF, for a journey through the analysis I performed and the methodologies I explored. The summary is that I used Holt-Winters and ARIMA models with an iterative modeling and forecasting method to predict hourly electricity demand. I also incorporated a dummy variable for "weekend" and daily high and low temperature data as exogenous features in several SARIMAX models.

### Conclusions
I achieved my first goal, to better understand the advantages and disadvantages of various time series modeling, forecasting, and model evaluation methodologies, and made significant progress toward my second, to produce forecasts as good as Portland General Electric's own day-ahead hourly demand forecasts. My best forecast MAE for forecasts across the whole series (excluding the first 12 weeks for model fitting) was 116.8 MWh.

### Extensions
- Correct imputation method in initial data cleaning to prevent target leakage
- Plot MAE vs. training period length to better understand optimal training period
- Evaluate models on RMSE
- Produce daily forecasts going forward to test my best models on new data
- Further explore usefulness of exogenous variables
- Learn and perform dynamic harmonic regression to model multiple seasonalities