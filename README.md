# Predicting Stock Movements by Analyzing News Data

## Team Members
Name | Student ID | Github ID
------------ | ------------- | -------------
Nicolas Busch | 1802010245 | [nico-busch](https://github.com/nico-busch)
Raphael Vilaseca | 1802010219 | [rvilaseca](https://github.com/rvilaseca)

## Goal of the Project

Our project aims to build a model able to predict the movement of stock prices by analyzing the content of news data. In an evergrowing sea of information we want to determine and understand the data with the most predictive power. In order to maximize the fit of our model during the limited timeframe of this project, we will focus on a subset of US-listed companies traded on the Dow Jones Industrial Average.

Specifically, our project will try to solve the binary classification problem of predicting either an increase or decrease of the abnormal return of a given company stock on a given day based on news on that company on the same day. Therefore, the model's features are to be determined based on news data, while market data is used to label our training set.

## Description of Data

Our project makes use of two databases provided through the Wharton Research Data Services (WRDS) platform.
o News data (Source: Ravenpack News Analytics) - provides real-time structured sentiment, relevance and novelty data for entities and events detected in the unstructured text published by reputable content sources.
o Market data (Source: Compustat Global) - provides financial market information such as opening price, closing price, trading volume, calculated returns, etc.

## Pre-Processing

The news data and the stock market data are merged on company name and news arrival time in such a way that we consider the current daily return if the news is published during normal trading hours (from 9:30 a.m. until 4 p.m.). Otherwise we consider the next trading day return. This specification is important especially if some company news is published during week-ends or US national holidays because the market impact can only be measure on the next market opening day. 

Next, we use the S&P daily return retrieved from Yahoo! Finance and compute the market beta of each Dow Jones Industrial Average stock. We proceed by calculating beta as the covariance between the market and the individual stock return divided by the market return variance. Once the beta is computed we can apply the CAPM formula and find the daily stock abnormal return. 

We will use the abnormal return as our target variable in the subsequent Machine Learning binary classification exercise. More precisely, we will try to predict if abnormal return is positive or negative following a news announcement so for that purpose, we add a “abnormal_return_flag” variable to our dataset. 

Before starting data exploration, we further clean or dataset dropping columns which are not exploitable inside our machine learning classifiers or not directly related to the news analytics package. We finally remove rows where 25% of important features are missing. The final dataset is composed of 42 546 observations and our output variable is well-balanced between positive and negative abnormal returns. 

## Data exploration

In a first step, we plot the histogram of abnormal returns. We notice it follows a bell-shaped distribution with an average around zero which is in line with expectations.

![abnormal](/images/abnormal.png)

Our final feature matrix is composed of the following variables (see the Raven Pack user guide section "Data Field Descriptions" for more information):

o	RELEVANCE
o	GROUP
o	ESS
o	AES
o	AEV
o	ENS
o	ENS_SIMILARITY_GAP
o	ENS_ELAPSED
o	G_ENS
o	G_ENS_SIMILARITY_GAP
o	G_ENS_ELAPSED
o	NEWS_TYPE
o	RP_STORY_EVENT_INDEX
o	RP_STORY_EVENT_COUNT
o	CSS
o	NIP
o	PEQ
o	BEE
o	BMQ
o	BAM
o	BCA
o	MCQ

We create dummies for each categorical nominal variable in the dataset to be able to exploit their predictive power during model training. Furthermore, we plot violin plots for the variables GROUP and NEWS_TYPE to have a better idea of their impact on abnormal returns. 

![group](/images/group.png)

![news_type](/images/news_type.png)

## Model Training

We are interested first in assessing feature importance in predicting the sign of abnormal return. To that end, we fit a random forest classifier with 500 decision trees and use the functionality " forest. feature_importances” to sort features by importance. 

![features](/images/features.PNG)

Top features include AEV, AES and G_ENS_SIMILARITY_GAP.

Next, we run a PCA to see how many Principal Components would be needed to explain most of the dispersion in the data. We conclude that at least 10 principal components should be use in the model selection pipeline if we want to keep some consistency with the data and reduce computation time.

![pca](/images/pca.png)

Now we proceed to the construction of our scikit-learn pipeline which we will use for model selection. Inside the pipeline we perform standardization of all the features and PCA decomposition. What’s more, we add interaction terms to take into consideration the non-linearity of our classification problem. In order to assess the performance of various models, we decide to use K-fold cross-validation (K=5). It’s worth noting that we shuffle the observations before splitting the dataset to allow a better convergence of linear classifiers. Finally, we can fit several classifiers and retrieve performance metrics to select the best model. 

![models](/images/models.PNG)

Based on the output table, we conclude that the Random Forest classifier is the best model to predict the sign of abnormal returns (highest test accuracy average, second lowest test accuracy standard deviation). 


## Hyperparameter tuning 

A last step before running our final model is to use a Grid Search in order to perform hyperparameters optimization on our Random Forest classifier. The list of parameters we wish to optimize includes: “max_features”, “min samples split”, “bootstrap” and “criterion”. Using the optimized parameters, we fit another Random Forest classifier and get a final test accuracy of 68% (higher accuracy after hyperparameter tuning).

![result](/images/result.PNG)
