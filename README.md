# Housing-EDA
EDA on Housing Data for Ames, Iowa

# Overview
This repository features EDA on the [House Prices Advanced Regression Techniques](https://www.kaggle.com/c/house-prices-advanced-regression-techniques/overview) kaggle data set, which features data related to home sales in Ames, Iowa. The goal of this EDA is to understand potential predictors of house values.

# Investigate the marginal distribution of house values
The IQR for home sales falls between $130,000 and $214,000 with a mean of $180,000. There are 61 outliers above the IQR, with a max of $755,000. A historgram displays this long and heavy right tailed distribution with a skewness of 1.88. The distribution also features a high peak, with a kurtosis of 6.53.

# Investigate missing data and outliers
16 columns featured more than 30 null values, with four columns featuring more than 1,000. Most of these columns have null values because a given house does not have that feature. IE 94% of houses in Ames, Iowa probably do not abut an alley. I could look at the distribution of values of these columns to see if replacing null values with zeros or "false" would make sense.

17 columns featured more than 30 outliers, with five columns featuring more than 100. Three of those five columns have a lot of outliers because the IQR is zero, indicating most houses again do not have that feature. The overall conditin column seems intuitively related to price, so I would investigate outliers more when building a model. MSSubClass seems like it should be an object data type with numeric codes representing the dwelling type.

# Investigate potential predictors of house values
10 numeric variables showed a correlation of at least .5 with house sale prices. I explore overall quality (.79), above grade living area (.71), garage cars (.64), and total basement square feet (.61) further. A pair plot of these four variables shows intuitive positive correlation between pairs like above grade living area and garage cars. Boxplots of overall quality reflect a moderately strong positive correlation to sale price with some outliers. A garage spaces box plot shows home prices rise with more car spaces, but drops back down from three to four car garages. All four variables show significant linear relationships with sale price at the 1% level.

# Engage in feature creation
Categorical variables such as kitchen quality and exterior condition were examined for potential feature creation but did not reveal any findings. As we saw when looking at outliers, houses will have "0" for a given feature if they do not have that feature. I created a total feature score column that standardizes and then totals numeric columns. This incorporate both area metrics (square feet) and count metrics (IE number of baths), providing a single metric that rates the diversity and scale of a house's features. This "feature score" column shows a significant correlation of .82 with sale price, making it the strongest linear relationship (overall quality is .79). Scatterplots show potential relationships between feature score, sale price, and the other four predictors examined. For example, overall quality and feature score both increase with sale price.

# Perform min-max and standard scaling to prepare for model training
I drop the ID column to prepare my training data as it is just an index column. Categorical columns are replaced with dummy variables/columns prior to scaling.

# Model Outlook
Overall, moderately strong correlations with sale price were identified and explored among four variables and one engineered feature. We should be able to reasonably predict sale price with machine learning models.
