# Predicting Stock Movements by Analyzing News Data

## Team Members
Name | Student ID | Github ID
------------ | ------------- | -------------
Nicolas Busch | 1802010245 | [nico-busch](https://github.com/nico-busch)
Raphael Vilaseca | 1802010219 | [rvilaseca](https://github.com/rvilaseca)

## Goal of the Project
Our project aims to build a model able to predict the movement of stock prices by analyzing the content of news data. In an evergrowing sea of information we want to determine and understand the data with the most predictive power. In order to maximize the fit of our model during the limited timeframe of this project, we will focus on a few tech companies traded in the Nasdaq Stock Exchange.

Specifically, our project will try to solve the binary classification problem of predicting either a increase or decrease of the stock price of a given company on a given day based on news on that company on the same day. Therefore, the model's features are to be determined based on news data, while market data is used to label our training set.

## Description of Data
Our project makes use of two databases provided through the Wharton Research Data Services (WRDS) platform.
- News data (Source: Ravenpack News Analytics) - provides real-time structured sentiment, relevance and novelty data for entities and events detected in the unstructured text published by reputable content sources.
- Market data (Source: Compustat Global) - provides active and inactive fundamental and market data on global securities

