# ValueInvestor
## Introduction
### Problem Statement
The goal is to develop an intelligent system that aids in our value investing efforts using historical stock market data. Specifically, we aim to predict stock price valuations on a daily, weekly, and monthly basis to inform our BUY, HOLD, and SELL decisions. The challenge is to maximize capital returns, minimize losses, and reduce the HOLD period while using intrinsic value principles.

### Data
The dataset comprises trading data of 8 portfolio companies from emerging markets from 8 countries, including stock prices for 2020 Q1, Q2, Q3, Q4, and 2021 Q1. Each company’s stock data is provided in different sheets, reflecting varying operating days based on the country and market in which the stocks are traded. For this project, we will use only the 2020 data to predict with the 2021 Q1 data. The data is available [here](https://docs.google.com/spreadsheets/d/1MiunF_O8eNWIcfaOA4PVm668RN7FgLNA0a6U4LWf5Bk/).

![image](https://github.com/user-attachments/assets/51a93386-aad9-4de9-a903-27fb2345ea72)

### Goal
The primary goal is to predict stock price valuations on a daily basis. Based on these predictions, we will recommend BUY, HOLD, or SELL decisions to maximize capital returns and minimize losses. Ideally, the goal is to avoid any losses and minimize the HOLD period for stocks.

## Approach
### ARIMA models
First, I began with the ARIMA model for forecasting, aiming to predict the next day's closing stock price to inform our trade decisions. The model order was identified using differencing and statistical tests for stationarity, including the augmented Dickey-Fuller (ADF) test, KPSS test, and examining the autocorrelation (ACF) and partial autocorrelation (PACF) plots. To check for seasonality, a Fourier transform was applied to transform the data into the frequency domain, but no significant peaks were identified, indicating no seasonal effects. The optimum model order for the ARIMA model was also determined using the auto.arima function, which selects the model with the lowest AIC. Additionally, we used the ARIMAX model, an extension of ARIMA that includes exogenous variables. We incorporated available exogenous variables such as the previous day's opening price, high, low, percentage change, and stock volume. Considering the potential influence of the USD exchange rate on stock markets in these countries, exchange rate data was obtained using the ExchangeRate API and included as additional exogenous variables in the ARIMAX model. This approach yielded the best-performing model, with performance metrics shown below:

<table>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/d717d531-2edf-4dd4-947d-61b2e70a4f9f" alt="image1"></td>
    <td><img src="https://github.com/user-attachments/assets/884579ed-9e87-454e-aa4e-785e05989f61" alt="image2"></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/69ab73a8-996e-40a6-a105-98925163424e" alt="image3"></td>
    <td><img src="https://github.com/user-attachments/assets/76c50f28-ce9c-4c3e-a299-24f8f379bac8" alt="image4"></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/267a5891-ba3f-4a87-ae4a-8288a0948534" alt="image5"></td>
    <td><img src="https://github.com/user-attachments/assets/fea4d93c-6ada-4b7a-925e-8a4e815d66fa" alt="image6"></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/c3d9a2d9-c536-4c3c-9acb-921b38021670" alt="image7"></td>
    <td><img src="https://github.com/user-attachments/assets/2a74e259-176b-4a3f-a46d-547946ac2fbb" alt="image8"></td>
  </tr>
</table>


| Company                                   | MAE        | RMSE       | MAPE      | SMAPE     | R2 Score |
|-------------------------------------------|------------|------------|-----------|-----------|----------|
| Russia - Sberbank Rossii PAO (S           | 0.924462   | 1.188260   | 0.340182  | 0.340169  | 0.966553 |
| Turkey - Koc Holding AS (KCHOL)           | 0.033983   | 0.040156   | 0.176834  | 0.176852  | 0.997404 |
| Egypt - Medinet Nasr Housing (M           | 0.013541   | 0.017783   | 0.358459  | 0.357542  | 0.931498 |
| Brazil - Minerva SABrazil (BEEF           | 0.083234   | 0.108192   | 0.816683  | 0.817466  | 0.799682 |
| Argentina - Pampa Energia SA (P           | 0.664229   | 0.835070   | 0.835859  | 0.835907  | 0.908441 |
| Colombia - Cementos Argos SA (C           | 41.366800  | 51.413962  | 0.736000  | 0.737044  | 0.975332 |
| South Africa - Impala Platinum            | 196.604548 | 246.191198 | 1.050214  | 1.051551  | 0.944802 |
| South Korea - Dongkuk Steel Mil           | 70.502292  | 86.732662  | 0.897802  | 0.897001  | 0.821215 |

### Using DARTS toolbox
To implement deep learning models for forecasting, I used the DARTS toolbox, which provides a user-friendly API for state-of-the-art forecasting models, including (i) VARIMA (Vector ARIMA), (ii) N-BEATS, (iii) Block LSTM, (iv) Transformer, and (v) XGBoost. VARIMA is a multivariate extension of the standard ARIMA, modeling all the companies' time series simultaneously in a large vector autoregressive model. Except for VARIMA, the remaining models can utilize past values of exogenous variables to enhance forecasting accuracy. The neural network models (N-BEATS, Block LSTM, and Transformer) can build global models, trained on multiple time series to improve performance by leveraging more data. These global models are then fine-tuned on specific time series. Among these, the Block LSTM model, as implemented in DARTS with default parameters, achieved the best performance.

![image](https://github.com/user-attachments/assets/26699f39-a5f7-4dad-aaae-3db846c1d5ff)

| Company                                   | MAE        | RMSE       | MAPE      | SMAPE     | MASE      | R2 Score |
|-------------------------------------------|------------|------------|-----------|-----------|-----------|----------|
| Russia - Sberbank Rossii PAO (S           | 7.4415     | 7.96238    | 2.735204  | 2.771913  | 2.263574  | -0.436962|
| Turkey - Koc Holding AS (KCHOL)           | 0.449825   | 0.506451   | 2.297605  | 2.330193  | 1.510463  | 0.508852 |
| Egypt - Medinet Nasr Housing (M           | 0.044849   | 0.06163    | 1.191111  | 1.188003  | 0.769116  | 0.193878 |
| Brazil - Minerva SABrazil (BEEF           | 0.335303   | 0.366915   | 3.336538  | 3.272202  | 1.154371  | -1.211641|
| Argentina - Pampa Energia SA (P           | 1.459672   | 2.034917   | 1.865078  | 1.851077  | 1.019877  | 0.459727 |
| Colombia - Cementos Argos SA (C           | 147.995483 | 183.538271 | 2.59625   | 2.639568  | 1.573149  | 0.653457 |
| South Africa - Impala Platinum            | 815.987008 | 944.777482 | 4.281864  | 4.406949  | 1.689134  | 0.163631 |
| South Korea - Dongkuk Steel Mil           | 358.544965 | 403.093674 | 4.549231  | 4.679083  | 2.665469  | -3.052066|

### Trading Strategy
The trading strategy used employs Bollinger Bands and custom buy-hold-sell logic to optimize trading decisions. Bollinger Bands are calculated using a moving average and standard deviations to create upper and lower bands, helping identify potential buy and sell signals based on the stock's volatility. The strategy buys stocks when the price crosses the lower Bollinger Band and meets a specified threshold. It sells them if the price drops below a set stop-loss level exceeds the upper Bollinger Band with a sufficient margin, or if the holding period exceeds a maximum limit. The forecasting model predicts future stock prices, leveraging historical data, with optimal parameters for Bollinger Bands and trading strategy determined through iterative testing. This approach aims to maximize returns, minimize losses, and optimize the holding period, providing a robust and adaptive trading solution based on intrinsic value principles.
By implementing this strategy with two of our best forecasting models, (i) ARIMAX and (ii) Block LSTM, we achieved average returns of USD 1024 and USD 1051, respectively, from an initial investment of USD 1000 in each company.
For the BLOCK-LSTM model:

![image](https://github.com/user-attachments/assets/079e5be5-30ba-4921-a7b1-e10618071611)
![image](https://github.com/user-attachments/assets/8eb99279-8a50-494e-ac25-7b2a1b0c06d7)
![image](https://github.com/user-attachments/assets/bd7a1d56-12e7-428e-b737-3e49a4536907)
![image](https://github.com/user-attachments/assets/2d1b6e07-cd37-4142-b16a-37cc5e11f744)
![image](https://github.com/user-attachments/assets/717a9365-74a2-4c79-9e9f-9fa2b0056e16)
![image](https://github.com/user-attachments/assets/38d7e8c1-c17b-4242-a78f-2d5b5c7470e5)
![image](https://github.com/user-attachments/assets/5991d871-a469-482f-a268-4acbc6cc4dd6)
![image](https://github.com/user-attachments/assets/3ea62f68-d37f-41ce-b4d3-f89f17183625)

## Summary
By using the implemented forecasting models and the trading strategy, we achieved an annualized ROI of up to 22% when utilizing next-day forecasts obtained from the Block LSTM models.
