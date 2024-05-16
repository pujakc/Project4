# Project4 - Saving the Movie Industry in a Streaming World

## Problem Statement
With the rise of streaming platforms and changing viewer habits, the movie industry is facing new challenges in ensuring profitability and susttainibility. To help Hollywood continue producing profitable movies and maintaining employment across the industry, we need to identify the key factors that drive movie profitability and develop predictinve models to guide future movie production decisions.

## Project Overview

This project aims to analyze movie data from 1980 to 2020 to understand the features that contribute to movie profitability and likelihood of a movir making profit. By adjusting financial figures for inflation and iteratively refining our models with various features, we will provide actionable insights for support data-driven decision making in the movie industry.

## Key Objectives
1. Predict Movie Profitability. Develop a regression mocdel to predict net profit of movies
2. Feature Importance Analysis: Identify the key features that significantly impact movie profitability and provides for recommendation for movie production.
3. Contemporary Relevance: Focus on recent movies to ensure that findings are relevant to the current industry context influenced by streaming platforms.
4. Classify Profitability Potential: Create a classification model to predict whether a movie will generate gross income higher than its budget.

## Background
The movie industry has traditionally relied on box office performane to gauge movie's success. However, with the advancement in the streaming platforms and shifting viewer preferences, the dynamics of movie profitability has evolved. Understanding these changes and identifying the factors that drive profitability in the new landscape is very crucial in order to maintain thriving movie industry. This project leverages a comprehensive movie dataset and inflation adjustment financial data to build predictive models and uncover insights itno the profibality drivers of movies.

## Detailed Project Overvies

1. Data Collection and Preprocessing
    .Data Sources
        . Movie Dataset from Kaggle
        . Consumer Price Index(CPI) Data from BLS
    .Data Cleaning 
    .Missing Vlaues: Imputing missing values for the features that have missing valued. Appropriate imputation methodology will be
    utilized.
    . Inflation AdjustmentL Adjust movie budgets and gross incomes for inflation using CPI Data to ensure comparability across years.

2. Exploratory Data analysis(EDA)
    . Conduct descriptive statistics and visualize data distributions
    . Analyze correlaton between features and teh target variable
    
3. Regresssion Model Development
    . Start with a simplet linear regression model to predict net income
    . Iteratively add features(genre, rating, runtime, release month and assess their impact on model performance
    . Evaluate Model performance using mmetrics like RMSE and R2 Square
    . Explore ensemble models Random Forest, AdaBoost, Gradient boosting to improve the predictive bias and variance.
4. Recent Movies Analysis:
    . Subset the data to focus on movies form past 10-20 years.
    . Rebuild and evaluate models for check consistency and relevance of feature importance.
 5. Feature Importance and Intepretation:
     . Identify key features influencing profitability using model interpretation techniques
     . Provide actionable recommendations based on feature importance analysis.
    