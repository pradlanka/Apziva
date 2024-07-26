# Apziva AI Residency project repository

This repository contains various data science and machine learning projects done for Apziva AI Residency. Each project includes detailed descriptions, data analysis, methodology, and results. Below are the summaries and key information for each project.

---
## [Project 1: Happy Customers [fw9kjShd4x6PASAT]](https://github.com/pradlanka/Apziva/tree/main/fw9kjShd4x6PASAT)

### Introduction/Background
This project focuses on predicting customer happiness based on survey responses. The goal is to measure and improve customer satisfaction in the logistics and delivery domain.

### Data Description
The dataset includes survey responses with attributes such as delivery time, order accuracy, pricing satisfaction, and overall satisfaction.

### Goal(s)
Predict customer happiness based on their survey responses.

### Methodology
- Handling missing values with imputation strategies
- Data transformation with standard scaling and one-hot encoding
- Classification using various models like random forest, logistic regression, MLP, and CatBoost
- Feature importance analysis and customer segmentation using K-prototypes

### Results
Highest accuracy achieved with XGBoost and MLP using simple imputation. Significant features identified and customer segments analyzed.

---

## [Project 2: Term Deposit Subscription Prediction [rFf4wqN31Eb9WSMn]](https://github.com/pradlanka/Apziva/tree/main/rFf4wqN31Eb9WSMn)

### Introduction/Background
This project aims to predict whether customers will subscribe to a term deposit based on direct marketing efforts. The goal is to improve the success rate of marketing campaigns by identifying potential customers.

### Data Description
The dataset includes customer attributes such as age, job type, marital status, education, credit default status, balance, loan information, and contact details.

### Goal(s)
Predict whether a customer will subscribe to a term deposit.

### Methodology
- Handling missing values with imputation strategies
- Data transformation with standard scaling and one-hot encoding
- Classification using models like XGBoost, Random Forest, and CatBoost
- Feature importance analysis and customer segmentation using K-prototypes

### Results
Highest accuracy achieved with XGBoost and MLP using simple imputation. Significant features identified and customer segments analyzed.

---

## [Project 3: Potential Talents [FBAMWTp3ZsqgUXd]](https://github.com/pradlanka/Apziva/tree/main/aFBAMWTp3ZsqgUXd)

### Introduction/Background
This project aims to streamline the process of identifying and ranking talented candidates for open roles using machine learning. The system analyzes candidate profiles to predict their fit for specific roles.

### Data Description
The dataset includes attributes such as job title, location, number of connections, and a fit score indicating the candidate's suitability for the role.

### Goal(s)
Predict and rank candidates based on their fit for a given role.

### Methodology
- Text processing with tokenization and stemming
- TF-IDF vectorization and cosine similarity calculation
- One-hot encoding for connections and location matching
- Dynamic updating of the reference search vector based on starred candidates

### Results
Accurate candidate rankings with SBERT provide the most relevant results.

---
## [Project 4: MonReader [EgwgsEeWZem4iXIs]](https://github.com/pradlanka/Apziva/tree/main/EgwgsEeWZem4iXIs)

### Introduction/Background
This project leverages computer vision solutions to predict whether a single frame image indicates a page flip and whether a sequence of images contains a flipping action, using smartphone video data of page flipping.

### Data Description
Smartphone video data of page flipping, labeled as flipping or not flipping, with frames extracted and named using the structure VideoID_FrameNumber.


### Goal(s)
Achieve high classification accuracy.

### Methodology
#### Image Classification
- **Rescaling**: Data rescaled from 1920x1080 to 320x180.
- **2D CNN**: Implemented with three convolution layers, three max-pooling layers, a dense layer, and an output layer. Achieved 90.3% accuracy.
- **Transfer Learning**: Used Inception model, achieving 98.3% accuracy.

#### Sequence Classification
- **3D CNN**: Achieved 75.9% accuracy.
- **CNN-LSTM**: Achieved 96.4% accuracy.
- 
### Results
Achieved 98.3% accuracy in predicting flipping actions in single images using the Inception model and 96.4% accuracy in sequences using the CNN-LSTM model.

---

## [Project 5: ValueInvestor [Q9MwvUueFZ02RjAH]](https://github.com/pradlanka/Apziva/tree/main/Q9MwvUueFZ02RjAH)

### Introduction/Background
The goal is to develop an intelligent system that aids in our value-investing efforts using historical stock market data. Specifically, we aim to predict stock price valuations on a daily, weekly, and monthly basis to inform our BUY, HOLD, and SELL decisions. The challenge is to maximize capital returns, minimize losses, and reduce the HOLD period while using intrinsic value principles.

### Data Description
The dataset comprises trading data of 8 portfolio companies from emerging markets from 8 countries, including stock prices for 2020 Q1, Q2, Q3, Q4, and 2021 Q1. Each company’s stock data is provided in different sheets, reflecting varying operating days based on the country and market in which the stocks are traded. For this project, we will use only the 2020 data to predict with the 2021 Q1 data.

### Goal(s)
The primary goal is to predict stock price valuations on a daily basis. Based on these predictions, we will recommend BUY, HOLD, or SELL decisions to maximize capital returns and minimize losses. Ideally, the goal is to avoid any losses and minimize the HOLD period for stocks.

### Methodology
#### ARIMA Models
- Used ARIMA model to forecast the next day's closing stock price.
- Identified model order using differencing and statistical tests (ADF, KPSS, ACF, PACF).
- Checked for seasonality using Fourier transform; no significant peaks found.
- Optimal model order determined with auto.arima function.
- Extended to ARIMAX model, including exogenous variables (previous day's opening price, high, low, percentage change, stock volume, and USD exchange rate).
- Achieved best performance with ARIMAX model.

| Company                          | RMSE      | MAPE      | SMAPE     |
|----------------------------------|-----------|-----------|-----------|
| Russia - Sberbank Rossii PAO (S  | 1.188260  | 0.340182  | 0.340169  |
| Turkey - Koc Holding AS (KCHOL)  | 0.040156  | 0.176834  | 0.176852  |
| Egypt - Medinet Nasr Housing (M  | 0.017783  | 0.358459  | 0.357542  |
| Brazil - Minerva SABrazil (BEEF  | 0.108192  | 0.816683  | 0.817466  |
| Argentina - Pampa Energia SA (P  | 0.835070  | 0.835859  | 0.835907  | 
| Colombia - Cementos Argos SA (C  | 51.413962 | 0.736000  | 0.737044  |
| South Africa - Impala Platinum   | 246.191198| 1.050214  | 1.051551  |
| South Korea - Dongkuk Steel Mil  | 86.732662 | 0.897802  | 0.897001  |


#### Using DARTS Toolbox
- Implemented deep learning models with DARTS toolbox: VARIMA, N-BEATS, Block LSTM, Transformer, XGBoost.
- VARIMA: Multivariate extension of ARIMA, modeling all companies' time series simultaneously.
- Neural network models (N-BEATS, Block LSTM, Transformer): Built global models trained on multiple time series.
- Block LSTM model achieved the best performance with default parameters.

| Company                          | RMSE      | MAPE      | SMAPE     | 
|----------------------------------|-----------|-----------|-----------|
| Russia - Sberbank Rossii PAO (S  | 7.96238   | 2.735204  | 2.771913  |
| Turkey - Koc Holding AS (KCHOL)  | 0.506451  | 2.297605  | 2.330193  |
| Egypt - Medinet Nasr Housing (M  | 0.06163   | 1.191111  | 1.188003  |
| Brazil - Minerva SABrazil (BEEF  | 0.366915  | 3.336538  | 3.272202  |
| Argentina - Pampa Energia SA (P  | 2.034917  | 1.865078  | 1.851077  |
| Colombia - Cementos Argos SA (C  | 183.538271| 2.59625   | 2.639568  |
| South Africa - Impala Platinum   | 944.777482| 4.281864  | 4.406949  |
| South Korea - Dongkuk Steel Mil  | 358.544965| 403.093674| 4.549231  |

#### Trading Strategy
- Employed Bollinger Bands and custom buy-hold-sell logic to optimize trading decisions.
- Bollinger Bands calculated using moving average and standard deviations to identify potential buy and sell signals.
- Strategy buys stocks when the price crosses the lower Bollinger Band and meets a specified threshold.
- Sells stocks if the price drops below a set stop-loss level and exceeds the upper Bollinger Band with a sufficient margin or if the holding period exceeds a maximum limit.
- Aims to maximize returns, minimize losses, and optimize the holding period.

### Results
- Implemented strategy with ARIMAX and Block LSTM models, achieving average returns of USD 1024 and USD 1051, respectively, from an initial investment of USD 1000 in each company, i.e., an annualized ROI of 10 and 21%.

---
## Conclusion
These projects demonstrate various applications of machine learning and data science techniques in different domains. Each project includes detailed methodologies, results, and visualizations to provide insights and improve decision-making processes.
