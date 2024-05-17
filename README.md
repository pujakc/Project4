# Project4 - Saving the Movie Industry in a Streaming World
by David Kanevsky and Puja KC
General Assembly, Data Science Immersive
May 2024

## Problem Statement
With the rise of streaming platforms and changing viewer habits, the movie industry is facing new challenges in ensuring profitability and sustainibility. To help Hollywood continue producing profitable movies and creating jobs across the industry, we need to identify the key features that drive movie profitability and develop predictive models to guide future movie production decisions.

## Project Overview

This project aims to analyze movie data from 1980 to 2020 to understand the features that contribute to movie profitability and likelihood of a movie making profit. By adjusting financial figures for inflation and iteratively refining our models with various features, we will provide actionable insights to support data-driven decision making in the movie industry. In particular, we will focus on the years from 2007 to 2019 since that corresponds with when Netflix started offering the ability to stream movies and TV shows on demand, which could impact a movie's gross and profit by offering audiences another alternative to watching movies without going to the theater and buying a ticket.

## Key Objectives
1. Adjust movie budgets, gross and profit for inflation, since otherwise more recent movies would be seen as more profitable.
2. Predict Movie Profitability. Develop a regression mocdel to predict net profit of movies
3. Feature Importance Analysis: Identify the key features that significantly impact movie profitability and provides for recommendation for movie production.

## Background
The movie industry has traditionally relied on box office performane to gauge movie's success. However, with the advancement in the streaming platforms and shifting viewer preferences, the dynamics of movie profitability has evolved. Understanding these changes and identifying the factors that drive profitability in the new landscape is very crucial in order to maintain thriving movie industry. This project leverages a comprehensive movie dataset and inflation adjustment financial data to build predictive models and uncover insights itno the profibality drivers of movies.

## Detailed Project Overview

Data was aquired from two sources. The first source was a movie dataset from [Kaggle](https://www.kaggle.com/datasets/danielgrijalvas/movies/data) that included 7,688 movies released between 1980 and 2020. The second source was Consumer Price Index (CPI) data from the [Bureau of Labor Statistics](https://data.bls.gov/timeseries/CUUR0000SA0), covering the same years as the movie data set. 

The workbooks should be read in the following order

1. 01_Cleaning_Movies_Data.ipynb

Reading in the movies.csv that came from the Kaggle data set, the following cleaning was applied.

-- There were 189 movies were the gross was missing. Since the gross was a key variable we are trying to predict, we removed them from the data set.

-- There were 3 movies were the writer was missing and 1 were the country was missing. We could not find the writer or production company for those movies, so we removed them from the data set rather than imputing them since it was so few cases.

-- There was 1 movie were the country was missing, so it was dropped since none of the other identifying features (the director, writer, star and production company) appeared in the data set to help determine where the movie was made.

-- The rating category was simplified since it had a mix of ratings for foreign movies and TV movies like TV-MA. The ratings were recoded as an ordinal variable with 1 for G, 2 for PG or TV-PG, 3 for PG-13 or TV-14, 4 for R, TV-MA or Approved, 5 for X or NC-17, and 6 for Unrated or Not Rated.

-- There were 10 movies were the production company was missing. We found that data on Wikipedia and had it entered into the data set. In all but 2 cases, those production companies had also produced other movies that are in the data set.

-- We created a column for the movie's month by taking the first word in the released column. However, there were 7 movies where there was just a year and no month under released. For those 7 movies where the month was missing, we filled in the data based on what we found on Wikipedia and IMDB.

-- This data was then exported as clean.csv
   
2. 02_a_EDA.ipynb

-- The clean.csv data was read in and additional EDA was done to help understand the various features in the data set.

3. 02_Imputation

The clean.csv data was read in to do some additional imputation.

-- After some initial cleaning, there were still 1 were the runtime was missing, 53 movies were the rating was missing, and 2040 were the movie's budget was missing. For the runtime that was missing, manually coded it based on the IMDB data.

-- Since some EDA showed that ratings and budget impacted the gross, and factors like genre and country impact the budget, we went about imputing the missing data for those features. 

-- We ran a Grid Search over a Gradient Boosting Classifier (best parameters were max depth of None and N_estimators = 100) to impute the rating based on the genre, country, company and month. 

-- To impute the budget, we ran a Linear Regression based on the movie's rating, genre, country, company and month. Since we are trying to predict a movie's profit (which is dependent on it's gross), we intentionally did not include the gross when imputing the budget since we did not want the dependent variable we are trying to predict to influence some of the features we are trying to impute.

-- Once all the features were imputed, this data was exported as imputed.csv.
   
4. 03_Budget_adjustment

-- The imputed.csv data set was read in along with the BLS data set (SeriesReport-20240509191709_137c61.xlsx). We calculated the inflation factor by taking the CPI data for the last year and month in our data set (December 2020, which was 260.474) and then dividing it by the CPI/Index for each month/year to get the inflation adjusted value. For example, in January of 1980, the CPI was 77.8, so the inflation factor is 3.34 (260.474 / 77.8). The inflation adjusted factor was then mapped onto each movie for it's respective month and year of it's release.

-- The inflation adjusted factor was then multiplied against the movie's budget and it's gross to calculate it's inflation adjusted budget and gross in December 2020 dollars. The movie's profit, both in real and inflation adjusted values, was calculated by subtracting the movie's gross from it's budget.

-- The data with the inflation adjusted value's for gross, budget and profit was then exported as final_inflation_adj_data.csv.

   
5. 03_a_Additional_EDA_Post_Adjustment.ipynb

-- The final_inflation_adj_data.csv was read in to analyze the number of movies per year, as well as the mean and median values per year. 

-- While the data covered movies from 1980 to 2020, the data showed that the number of movies and movie's profitability declined in 2020, due to the COVID pandemic shutting down most movie theaters and companies choosing not to release most movies in theaters. As a result, further analysis excluded 2020 from the data set since it was such a clear outlier.

-- The analysis also shows that while the **median** movie's gross has trended downwards since 2007, the **mean** movie's gross has increased since 2007, even after adjusting for inflation. This suggests that the industry's profitability is driven by a few blockbusters that are generating larger grosses even as more films are less profitable than they used to be. This is also reflected in the fact that the percent of movies that are profitable has declined from its peak in 2008 at 73% to just 56% in 2019, which is the lowest its been since 2001.

-- It is not expenditures driving down profitability, but revenue. The **median** movie's budget has declined since 2007 in real dollars, indicating studios are generally spending less per movie than they have. Even the **mean** budget is flat since 2007. But there is a positive linear relationship between a movie's budget and it's profit, suggesting that holding all other factors constant, spending more money on a movie should increase it's profit.

6. 04_Modeling.ipynb

In this workbook, we ran various models on some basic features to determine which models to select as we do additional feature engineering. We started with some basic features of the movie's rating, it's runtime, it's inflation adjusted budget, genre, company and month, and ran several models, including a Linear Regression, a Ridge Regression, a LASSO Regression, a Random Forest Regressor, an AdaBoost Regressor, a Graident Boosting Regressor, and Bagging Regressor.

Of these models, the Random Forest Regressor had both the highest testing R2 and the lowest Root Mean Squared Error, with the Ridge Regression performing the second best on both those metrics.
 
7. 05_Important_Features_Best_model.ipynb

Having settled on using the Random Forest regressor, we ran through several variations to determine whether adding or subtracting certain features improved the model. The model that produced the highest R2 score and the lowest RMSE score included the features of the genre, company, country, month, star, rating, runtime and budget (but did not include the writer). 

When budget is included in the model, it is consistently the top feature. 

## Data Sets and Data Dictionary
|Feature|Type|Description|
|---|---|---|
|name|object|The name of the movie from the Kaggle data set|
|rating|float|The rating of the movie as an ordinal variable, with 1 for G, 2 for PG, 3 for PG-13, 4 for R, 5 for NC-17 and 6 for Unrated|
|genre|object|The movie's genre based on the Kaggle data set|
|year|int|The year the movie was released|
|score|int|The movie's IMDB rating at the time the data was collected|
|votes|float64|The number of times the movie has been rated on IMDB at the time the data was collected|
|director|object|The movie's primary director|
|writer|object|The movie's primary writer|
|star|object|The movie's primary star|
|country|object|The country were the movie was produced|
|budget|float|The movie's budget at the time of its release|
|gross|float|The movie's gross at the time of its release |
|company|object|The name of the distribution company that released the movie|
|runtime|float|The length of the movie in minutes|
|month|object|The month the movie was released, as calculated from the Kaggle released column|
|rating_imputed|float|1 if the rating was imputed and 0 if it came from the Kaggle data set|
|budget_imputed|float|1 if the budget was imputed and 0 if it came from the Kaggle data set|
|inf_adj_value|float|The inflation adjusted value by taking the CPI index for December 2020 and dividing it by the CPI Index for the year and month the movie was release |
|budget_adj|float|The result from multiplying the budget * the inf_adj_value to determine the movie's budget in inflation-adjusted dollars |
|gross_adj|float|The result from multiplying the gross * the inf_adj_value to determine the movie's gross in inflation-adjusted dollars |
|profit|float|Calculating the movie's profit by subtracting it's gross from it's budget |
|profit_adj|float|The result from multiplying the movie's profit * the inf_adj_value to determine the movie's profit in inflation-adjusted dollars |

## Analysis, Conclusions and Recommendations

The aphorism "you got to spend money to make money" holds up when it comes to helping predict what the most profitable movies are, particularly in the post-2007 streaming era. This is also reflected in the data showing that the **median** movie profit is down, while the **mean** movie profit is up, reinforcing how it's the few big blockbusters that are driving up movie profits, rather than a larger number of smaller and mid-budget.

However, while this analysis focused on maximizing profit, further analysis should be done on rate of returns. While investing more in big-budget movies can help maximize profit, it also could potentially create additional risk exposure in losses. Further analysis should look at the percent profitability to see what has the best rate of return in investment. Addiitonally, since the number of movies that are profitable has been declining since the streaming error, further analysis should look into a classification model to predict if a movie is profitable or not. This can help us understand that if the same features that help maximize profit in raw dollars also can help generate the best rate of return while minimizing risk of losses.

It is also worth collecting additional data on movie revenue from 2021 onwards to see how trends on movie grosses and budgets have faced in a post-pandemic landscape.