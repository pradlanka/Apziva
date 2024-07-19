# Happy Customers Project
## Introduction
We have customer feedback survey data from a logistics and delivery company conducted among its selected customer cohort. A subset of this data is provided to us, with the remaining data reserved as a private test set.

### Data Description
The dataset includes the following attributes:

Y: Target attribute indicating customer happiness (0 for unhappy, 1 for happy).
X1: My order was delivered on time (values range from 1 to 5).
X2: Contents of my order were as I expected (values range from 1 to 5).
X3: I ordered everything I wanted to order (values range from 1 to 5).
X4: I paid a good price for my order (values range from 1 to 5).
X5: I am satisfied with my courier (values range from 1 to 5).
X6: The app makes ordering easy for me (values range from 1 to 5).
Attributes X1 to X6 represent customer responses on a scale of 1 to 5, where a higher number indicates a more favorable response.

The data can be downloaded [here](https://drive.google.com/open?id=1KWE3J0uU_sFIJnZ74Id3FDBcejELI7FD).

| Y | X1 | X2 | X3 | X4 | X5 | X6 |
|---|----|----|----|----|----|----|
| 0 | 3  | 3  | 3  | 4  | 2  | 4  |
| 1 | 3  | 2  | 3  | 5  | 4  | 3  |
| 2 | 5  | 3  | 3  | 3  | 3  | 5  |
| 3 | 5  | 4  | 3  | 3  | 3  | 5  |
| 4 | 5  | 4  | 3  | 3  | 3  | 5  |

### Goals
The primary goal is to predict whether a customer is happy or not based on their survey responses. Secondly, we should also identify which questions/features are most important for predicting customer happiness.

## Methodology
### Classification
The initial classification was performed using a decision tree classifier, achieving an accuracy of 55%. After parameter optimization, the accuracy improved to 60.2%. The random forest model yielded an accuracy of 58.7%, while logistic regression achieved 53.7%.
Given that the survey responses are on a Likert scale and represent ordinal values, we applied a one-hot encoding transformation to augment the scaled scores. This approach led to improved accuracies: 61.6% with logistic regression and 58% with random forest. Additionally, the CatBoost classifier achieved an accuracy of 61.5%.

### Feature Importance
CatBoost provides feature importance scores, which indicate that "X1: My order was delivered on time" is the most important feature, highlighting the significance of timely order delivery in customer satisfaction. Similarly, using SHAP scores with the random forest model confirmed that X1 is the most important feature, further emphasizing its impact on predicting customer happiness.

The figure below shows the feature importance from CatBoost and SHAP scores:

| ![Feature importance scores from CatBoost](https://github.com/user-attachments/assets/43298369-1d1f-4047-a415-b6150ba463a4) | ![SHAP scores from Random Forest](https://github.com/user-attachments/assets/9343a9c3-0a23-4d84-916a-c7fd92be159b) |
|----------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|



## Summary
This project aimed to predict customer happiness based on survey responses from a logistics and delivery company's selected customer cohort. The dataset included ordinal responses on a Likert scale related to various aspects of the service. We applied several classification models, including decision trees, random forests, logistic regression, and CatBoost. Initial accuracies ranged from 53.7% to 60.2%, with improvements achieved through parameter optimization and one-hot encoding of ordinal features, resulting in accuracies up to 61.6%. Feature importance analysis, using CatBoost and SHAP scores, identified "X1: My order was delivered on time" as the most critical factor in determining customer satisfaction. This insight highlights the importance of timely delivery in enhancing customer happiness and guiding future operational improvements.
