# Term Deposit Marketing
## Introduction

This project aims to develop a robust machine learning system leveraging call center data to improve the success rate of customer calls for various client products. The goal is to design an evolving machine learning product that achieves high success outcomes while providing interpretability for clients to make informed decisions.

### Data Description
The data comes from direct marketing efforts of a European banking institution. The marketing campaign involved making phone calls to customers, often multiple times, to ensure a product subscription—in this case, a term deposit.

Attributes:
- **age**: Age of customer (numeric)
- **job**: Type of job (categorical)
- **marital**: Marital status (categorical)
- **education**: Level of education (categorical)
- **default**: Has credit in default? (binary)
- **balance**: Average yearly balance, in euros (numeric)
- **housing**: Has a housing loan? (binary)
- **loan**: Has a personal loan? (binary)
- **contact**: Contact communication type (categorical)
- **day**: Last contact day of the month (numeric)
- **month**: Last contact month of year (categorical)
- **duration**: Last contact duration, in seconds (numeric)
- **campaign**: Number of contacts performed during this campaign and for this client (numeric, includes last contact)

Output (desired target):
- **y**: Has the client subscribed to a term deposit? (binary)
- 
| age | job           | marital | education | default | balance | housing | loan | contact | day | month | duration | campaign | y  |
|-----|---------------|---------|-----------|---------|---------|---------|------|---------|-----|-------|----------|----------|----|
| 58  | management    | married | tertiary  | no      | 2143    | yes     | no   | unknown | 5   | may   | 261      | 1        | no |
| 44  | technician    | single  | secondary | no      | 29      | yes     | no   | unknown | 5   | may   | 151      | 1        | no |
| 33  | entrepreneur  | married | secondary | no      | 2       | yes     | yes  | unknown | 5   | may   | 76       | 1        | no |
| 47  | blue-collar   | married | unknown   | no      | 1506    | yes     | no   | unknown | 5   | may   | 92       | 1        | no |
| 33  | unknown       | single  | unknown   | no      | 1       | no      | no   | unknown | 5   | may   | 198      | 1        | no |

The data is available [here](https://drive.google.com/file/d/1EW-XMnGfxn-qzGtGPa3v_C63Yqj2aGf7).

### Goal(s)

The primary goal is to predict whether a customer will subscribe to a term deposit (variable y). Achieve an accuracy of 81% or higher, evaluated using 5-fold cross-validation and reporting the average performance score. Identify customer segments more likely to purchase the investment product, enabling clients to prioritize these segments.

## Methodology
### EDA
The percentage of people who subscribed will be lower than the people who did not subscribe, as shown below:
![image](https://github.com/user-attachments/assets/a46bcd4a-be70-48e3-bbb7-7e5e9bcd1db5)

Plotting the distribution of the categorical and numerical features available:
| ![Categorical features](https://github.com/user-attachments/assets/c7610d01-03d1-4a8b-9e7f-e04c628994c5) | ![Numerical features](https://github.com/user-attachments/assets/9cbb74a2-51f8-48f8-8604-0dedcd8fd4e7) |
|------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|

The data contains missing values for a few columns but predominantly the 'contact' column, this could be because this variable was collected later.
![image](https://github.com/user-attachments/assets/8d4210c1-ef12-4374-b14b-f7bbe7daeefd)


To address missing values, three methods were employed:

1. **Dropping Rows with Missing Values**: This resulted in a loss of 35% of the data.
2. **Dropping Columns with Missing Values**: Specifically, 'education' and 'contact' columns were dropped.
3. **Imputation Strategies**:
   - **Simple Imputer**: Assigns the mean for numerical features and the most common value (mode) for categorical features.
   - **KNN Imputer**: Uses k-nearest neighbors to impute missing values.
   - **Iterative Imputer**: Fills in missing values using predictive modeling.

### Data Transformation

- **Numerical Features**: Standard scaler transformation.
- **Categorical Features**: One-hot encoding.

### Classification

- **Initial Models**: Random forest, logistic regression, MLP, and RBF-kernel SVC achieved 43-45% balanced accuracy, indicating that dropping columns may not be ideal.
- **Advanced Models**: XGBoost, Random Forest, and CatBoost handled missing values directly, achieving accuracies of 86%, 85%, and 74%, respectively.
- **Best Performing Model**: XGBoost with simple imputation achieved 86%, and MLP with simple imputation achieved 88%. KNN and iterative imputation provided similar performance levels.

### Feature Importance

- **Key Findings**: Random forest and XGBoost indicated that 'last contact duration' is the most important feature. However, it is unclear if there is a causal relationship between call duration and term deposit subscription.

| ![Feature Importance Scores from Random Forest](https://github.com/user-attachments/assets/0e825262-5216-4a25-a0a1-20e1be1273da) | ![Feature Importance Scores from XGBoost](https://github.com/user-attachments/assets/9c1b37f1-bcb2-4bd6-bd89-22cfe2798896) |
|--------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|

### Customer Segmentation

- **Clustering Method**: K-prototypes was used to handle both categorical and numerical features.
- **Optimal Clusters**: Determined to be 5 using the elbow method.
  ![image](https://github.com/user-attachments/assets/600c55ad-1b0c-4602-bce8-f0fb44eec704)

- **Cluster Analysis**: Significant differences between high and low conversion rate clusters were found in 'previous contact duration' and 'number of contacts during the campaign'. T-tests revealed significant differences in balance, duration, and campaign. Chi-square tests showed that all categorical variables, except for 'default' and 'loan', differed significantly between high and low conversion rate clusters.
  
| ![Duration](https://github.com/user-attachments/assets/a7468674-38c6-43c2-b29c-29644efd4689) | ![Campaign](https://github.com/user-attachments/assets/1e7a7e71-b85d-4491-ba43-1adec93ca9e9) |
|------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|

## Summary
This project aims to develop a robust machine learning system leveraging call center data to improve the success rate of customer calls for various client products. The dataset, derived from a European banking institution's direct marketing efforts, includes a range of customer attributes such as age, job type, marital status, education level, credit default status, balance, loan information, and contact details. The primary goal is to predict whether a customer will subscribe to a term deposit.

To address missing values, methods such as dropping rows or columns with missing data and applying various imputation strategies were employed. Data transformation included standard scaling for numerical features and one-hot encoding for categorical features. Initial classification models like random forest, logistic regression, MLP, and RBF-kernel SVC achieved accuracies of 43-45%. Advanced models like XGBoost, Random Forest, and CatBoost improved accuracy to 86%, 85%, and 74%, respectively. The best performance was achieved with XGBoost and MLP using simple imputation, reaching 86% and 88% accuracy.

Feature importance analysis identified 'last contact duration' as the most significant feature. Customer segmentation was performed using the K-prototypes method, revealing that clusters with different conversion rates significantly differed in contact duration and the number of contacts during the campaign. Statistical tests confirmed the significance of most categorical variables, except for 'default' and 'loan', in distinguishing between high and low conversion rate clusters.
