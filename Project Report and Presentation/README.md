Contents

[Summary](#summary)

[Background](#background)

[The Process](#the-process)

[Data Collection and Cleaning](#data-collection-and-cleaning)

[Exploratory Data Analysis](#exploratory-data-analysis)

[Preprocessing Pipeline](#preprocessing-pipeline)

[Modeling – Evaluation and Selection](#modeling--evaluation-and-selection)

[Conclusion](#conclusion)

# Summary

The project involved combining 5 common heart issues such as heart attack, stroke, or angina into one main target Cardiovascular event.

Several biographical and medical factors are taken in as inputs to be able to evaluate the risk of a potential cardiovascular event for the participant

The study is for members over 20 years old and is driven by 60 data files taken from the CDC website spanning over 2005 - 2016 period.

Based on approximately 35,000 participants the biggest driving factor in assessing the risk of a cardiovascular event is age, followed by a diabetes diagnosis, waist and household size, marital status, and the decision to quit smoking or not.

Out of the 1114 positive test cases, our prediction model correctly identified 948 as potential risk cases. Correctly catching risk cases 85% of the time. Leading to an inexpensive and solid initial screening method.

![A graph of a diagram Description automatically generated with medium confidence](media/e566f323ec8cc18f3320e6e29d8c4406.png)

*(Model performance during testing)*

We have a strong classification model for initial screening. The questionnaire for screening will consist of 21 input factors which are revealed in further detail during the Process section of the report. Based on this questionnaire an initial risk assessment can be performed at home or a clinic.

# Background

We live in an age where data is readily available, from smartphones to digital watches. The amount of information available at our fingertips is increasing, this project aims to make that information relevant by bringing in risk assessment features.

Taking in factors such as age, blood pressure, diabetes diagnosis, smoking habits, and others we can quickly assess if our lifestyle falls within a safe or high-risk group.

Being able to quickly analyze our heart health can lead to a more informed patient, ready to be proactive with medical needs or making changes to our habits or diet.

With 85% of risk cases being identified as a concern correctly, we can stay on top of our heart health within minutes at home or a medical clinic.

# The Process

We have followed a standard Data Science Project Methodology. After identifying the goal of the project as a classification predictive model.

We have broken down the actual data steps into the following four components.

Problem Statement: Based on demographic and rudimentary medical information, create a cardiovascular risk assessment tool that takes a list of information and classifies participants as high-risk or not.

Data Steps:

-   Data Cleaning
-   Exploratory Data Analysis (EDA)
-   Preprocessing Pipeline
-   Model Evaluations and Selection

Overall, the sequence of steps was in the above order. However, as needed the data cleaning step was revisited during the EDA and preprocessing steps.

## Data Collection and Cleaning

Our data source was a total of 60 files from the CDC website. As such, the first step was collecting and combining the data into one data frame or table. All initial column names needed an update.  
  
Key data cleaning steps:

-   As our target label is the presence of a cardiovascular event, participants 20 years or younger were removed due to the high number of missing target values in this age group. The 5 common heart issues were combined into one target column where the presence of any of the 5 issues was noted as a cardiovascular event.
-   The number of days since a person quit smoking was present in two separate columns indicating days, weeks, or months followed by the number of units. This was combined into one final column with all values in the number of days.
-   A key decision made at this point was regarding how to represent not applicable values. As an example, when it came to diabetes diagnosis age, participants who were never diagnosed with diabetes were represented using '-99' as the Not Applicable value.

At the end of the data-cleaning step, we had 34,022 entries with 29 columns (including our target label). We also note that only 11% of participants have experienced a cardiovascular event, making our data and problem an imbalanced one.

![](media/5b1e3d7ed0446c9a3e87f2d409f23637.png)

## Exploratory Data Analysis

The goal of Exploratory Data Analysis (EDA) is to gain valuable intuition of the data. This step is crucial for building an understanding of the dataset and getting a feel of the big picture, as it is harder to appreciate these relations once we dive into modeling algorithms or cross-validations.

This step was carried out in 4 parts: Statistical Summary, Numerical Features, Categorical Features, and Correlations.

We noted the average age of participants at 50. However, the most common age of participants was around 80. We also noted that the number of days since a person quit smoking is right skewed with fewer and fewer members quitting smoking for a larger number of days.

![](media/b818dbba4694b85f8822375dd25eed44.png)

During the exploration of categorical features, we noted that we have a good mix of both genders. The most common ethnicity is 'Non-Hispanic White' while the most common marital status is 'Married'.

![](media/64e6d474b42b80b94a7e0a53d48bbc16.png)

The correlation between age and heart issues jumped out in the pair plots. We also noticed a high correlation between some of the features, such as total cholesterol and bad cholesterol, or waist size and BMI. The median age of participants with heart issues lies in the late 60s, while the median age of participants without a heart issue lies in the 40s.

![A graph with a blue and orange rectangle Description automatically generated with medium confidence](media/085b5f0baafb879ffe9b7d94a125646d.png)

It was also interesting to note some other features that showed a correlation with age. As an example, the systolic blood pressure range tends to increase with age.

![A screenshot of a graph Description automatically generated](media/3c8dd63e621f962036da2b19aad4c5b1.png)

At the end of Exploratory Data Analysis, we dropped the body fat column due to a high number of missing values and the column showing a high correlation with total cholesterol. Post EDA we have 34,022 entries and 22 columns (including our target label).

## Preprocessing Pipeline

The preprocessing step started with the splitting of available data into training and testing data. 70% of the data was used to train models while 30% of data was to test them.

After the split, the following preprocessing steps were completed.

-   Imputation

The missing values were imputed using the median for numerical features and mode for categorical features.

Given the even and high volume spread for both genders, missing median and mode values were computed after grouping entries by gender.

This was to account for certain differences in certain features like income and waist size between the two gender groups.

-   Encoding

Encoding is used to convert categorical values into numerical features to prepare data for different models.

One Hot Encoding was used for ethnicity and marital status, as the categorical values are non-sequential.

On the other hand, for income brackets, Ordinal Encoding was used to retain the sequential nature of categorical values.

-   Scaling

The data values were scaled using a Standard Scaler which sets the mean for each feature at 0 and scales them to a unit variance.

![A diagram of a computer Description automatically generated](media/c1a69e694a6a258d8f7002a13ed67843.png)

## Modeling – Evaluation and Selection

The project is centered around identifying risk cases, as such missing a potential true positive will be a big drawback.

The need dictates a high recall score as such it has been used as the primary criterion.

We also created a custom evaluation metric to quickly note Recall and F1 score, while plotting Precision-Recall Curve, a confusion matrix, and a classification report.

Following classification modeling techniques were used.

| **Classification Model** | **Recall Score** |
|--------------------------|------------------|
| Logistic Regression      | 0.80             |
| LightGBM Classifier      | 0.81             |
| Support Vector Machine   | 0.83             |
| Random Forest            | 0.85             |

We used the Grid Search technique to do cross-validation and hyperparameter tuning.

The following parameters led to the best Random Forest model.

![A white background with black text Description automatically generated](media/895609f789177c2d832c3f3648a78af3.png)

Plus, age was again noted as the most important feature of this model.

Due to one hot encoding certain marital status jumped ahead in importance scale, indicating overall the relevance of marital status.

![A graph with blue and white bars Description automatically generated](media/6f50233e19322159d6c76edf1cd3bd4d.png)

Noting the test performance of our best Random Forest model below.

![](media/15167606a136ca84be0431ed5bab4ad0.png)

# Conclusion

After selecting the best model, we combine our preprocessing pipeline and classifier into one production-ready pipeline.

The new data can be entered for preprocessing and prediction straight away.

![](media/1243adb5682c72fbcb6c085665af0125.png)

We ended with a model showing a recall score of 0.85 and an f1 score of 0.36.

This is a solid performance for an imbalanced data 1:9, this imbalance was handled using the class_weight parameter of sklearn classifiers.

There is definitely a future scope to this project, adding some additional features can lead to new insights while removing low-importance features, and limiting the number of inputs can make the model lighter and more user-friendly.
