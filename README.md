
[Open My Notebook - Linear Regression Models](https://colab.research.google.com/drive/1ztBEbLpyelwc1jqE4kADUz1vnltZPTF6?usp=sharing)

# Housing-EDA and Model Development
EDA on Housing Data for Ames, Iowa

# Overview
This repository features EDA and prediction on the [House Prices Advanced Regression Techniques](https://www.kaggle.com/c/house-prices-advanced-regression-techniques/overview) kaggle data set, which features data related to home sales in Ames, Iowa. The goal of this EDA and model development is to understand and predict house values.

# Sale Price Distribution
The IQR for home sales falls between $130,000 and $214,000 with a mean of $180,000. There are 61 outliers above the IQR, with a max of $755,000. A historgram displays this long and heavy right tailed distribution with a skewness of 1.88. The distribution also features a high peak, with a kurtosis of 6.53.

# Missing Data and Outliers
16 columns featured more than 30 null values, with four columns featuring more than 1,000. Most of these columns have null values because a given house does not have that feature. IE 94% of houses in Ames, Iowa probably do not abut an alley. I could look at the distribution of values of these columns to see if replacing null values with zeros or "false" would make sense.

17 columns featured more than 30 outliers, with five columns featuring more than 100. Three of those five columns have a lot of outliers because the IQR is zero, indicating most houses again do not have that feature. The overall conditin column seems intuitively related to price, so I would investigate outliers more when building a model. MSSubClass seems like it should be an object data type with numeric codes representing the dwelling type.

# Predictors of Sale Price
10 numeric variables showed a correlation of at least .5 with house sale prices. I explore overall quality (.79), above grade living area (.71), garage cars (.64), and total basement square feet (.61) further. A pair plot of these four variables shows intuitive positive correlation between pairs like above grade living area and garage cars. Boxplots of overall quality reflect a moderately strong positive correlation to sale price with some outliers. A garage spaces box plot shows home prices rise with more car spaces, but drops back down from three to four car garages. All four variables show significant linear relationships with sale price at the 1% level.

# Feature Creation
Categorical variables such as kitchen quality and exterior condition were examined for potential feature creation but did not reveal any findings. As we saw when looking at outliers, houses will have "0" for a given feature if they do not have that feature. I created a total feature score column that standardizes and then totals numeric columns. This incorporate both area metrics (square feet) and count metrics (IE number of baths), providing a single metric that rates the diversity and scale of a house's features. This "feature score" column shows a significant correlation of .82 with sale price, making it the strongest linear relationship (overall quality is .79). Scatterplots show potential relationships between feature score, sale price, and the other four predictors examined. For example, overall quality and feature score both increase with sale price.

A feature counting the total number of full and half baths was also added. Further, a feature counting total livable square feet was added.

# Data Formatting
In a second round of formatting, categorical variables displaying order were manually and numerically ranked. Remaining categorical features were transformed into dummy variables, and numeric features were standardized.

# Model Development
Overall, 6 fold cross validation was implemented to evaluate linear, ridge, and lasso regression models. Each model produced an RMSLE within .004 of one another, and the Lasso regression model performed the best on the Kaggle test set. Both lasso and ridge regression performed better than the base linear regression model, indicating overfitting may have been reduced. Robost scaling was also used for each model to limit sensitivity to outliers.

# Model Evaluation and Assumptions
Features that seemed intuitively related to sale price, such as pool quality, overall quality, overall condition, and garage car spaces each had relatively large coefficients in my linear model. Street had the largest coefficient in my linear model. Given nearly all houses had "paved" street access, this variable should be dropped for future models as the related value is more reflective of an intercept for my model. 

Residual plots (predicted less actual sale prices) of my linear and lasso regression models did not reveal any clear shape or trend. This indicates there is a fair amount of equality of variance between features, adhering to basic linear regression assumptions. Sale price was log 10 transformed. A histogram of residuals did reflect a high peak, just as the distribution of sale price variable did. 

# Model Tuning
The ridge model was tuned to an alpha of 10, implying model performance could be improved by shrinking predictor coeficients/applying significant regularization. This should reduce overfitting. Similarly, the elastic net alpha was tuned to .001, where a value of zero would be equivalent to performing ridge regresstion. But, the lasso model was tuned to an alpha of .0001, indicating not much regularization is needed. So, we can conclude there are many variables present that add a small amount of predictive power to our model. IE our model performs better by including these variables with low coefficients (via ridge with a high alpha) over removing them completely (via lasso with a high alpha).

# Model Performance
The tuned ridge model got the highest kaggle test score of .13307. This model featured an R-Square of .951 on training data, explaining just over 95% of the variance in sale price. Differences between model results were minimal.
