### **CAR PRICE PREDICTION PROJECT**

This is a project to predict Car Prices using different Machine Learning techniques.

It features:

- Linear Regression
- Ridge Regression
- Lasso Regression
- Polynomial Regression 

Dataset used is obtained from [Kaggle](https://www.kaggle.com/datasets/sukhmandeepsinghbrar/car-price-prediction-dataset/data).

Dataset columns:name, year, selling price, kms driven,fuel type, seller type, transmission, owner, mileage, engine, max power, seats

#### **DATA CLEANING AND PREPROCESSING**

---

**i. Data Cleaning**

The first section tackles data loading and cleaning. We explore our data; how it has been structured, the data types, missing values, duplicates, skewness and fix the inconsistencies present.

**a. Data types and Skewess**

- The data types are in order except for max power which is of type *object* instead of *numeric*. Corrected by stripping any white spaces and converting to numeric. 
- Visualize engine, max power, seats and selling price to check for skewness. The data is skewed to the left. This informs how we deal with missing values

**b. Missing values**
 
- Missing values: columns: max power, engine, seats, mileage have missing values.
- Solution: They are filled with the *median* values because of the skewed nature of the data.

Filling the mising values in this case is better than dropping because in the process of dropping, we might end up getting rid of important data. 
 
There are no missing values in our target column which is selling price, this is good because we can't train a model that has missing target column values. 

**c. Duplicates**

- 1205 Duplicate rows are removed from the data set. Duplicate rows can cause confusion in our model leading to predictions that are inaccurate.

**ii. Data Preprocessing**

This section involves exploring, extracting additional information from our data and visualizing our data to understand it better. 

**a. Unique values and outliers**

- We check for the unique values in the data set and for specific columns just in case there are inconsistencies. For example: the fuel column. It has 4 unique values which are: Diesel, Petrol, CGN, LPG.

**Remember:**
In a categorical column, it is important to know all categories present because they might have been entered incorrectly to be separate categories yet they represent the same category.

Check for outliers in the target column (selling_price) using a box plot. The box plot reveals the prescence of outliers which are dealt with by filtering data to remain with columns that have selling_price value <= 2500000 as the threshold. 

Outliers affect model accuracy by causing the model to put more weight on the outliers which leads to a bad model thus the outliers are removed before modelling. 

**b. Calculated columns**
- Car age is obtained by calculating the difference between current year and year of manufucture.
- Another point of interest is the price per kilometer, divide the selling price and mileage to obtain the values.

After cleaning and preprocessing, the index is properly reset considering we made changes to the data.

#### EXPLORATORY DATA ANALYSIS

---

This section involves finding averages, grouping data, visualizing

**Findings:**

i. Selling price
- Average selling price: 476857
- Most cars cost between 0-600,000.
- 2018 cars have the highest average selling price across the years.
- Selling prices had a steady trend over the years, the peak was between 2015-2018 and a great decline in 2020.


ii. Fuel
- The most used fuel type is Diesel - 3681 cars.
- Least used fuel type is LPG - 38 cars
- Diesel cars sell at an average price 578937.

iii. Transmission
- Most of the cars have a manual transmission type.
- Automatic cars are generally cheaper.

iv. Mileage
- Highest mileage is 33.44(km/ltr/kg) by Maruti Alto 800 CNG LXI Optional. 

v. Age
- The relationship between car age and price is that the greater the age of the car, the higher the price of the car.

vi. Brand
- Maruti is the most frequent 2165 cars 
- Peugot is the least frequent 1 car.

**Correlation matrix between selling price and other columns**

Highest positive correlation: price per km.
- The two have an almost perfect correlation (0.94) because you can calculate price per km from selling price and kms driven.

Highest negative coreelation: age(-0.55).
- A unit increase in age leads to a (0.55) unit decline in the seeling price.
- This makes sense because older cars sell for less. 

Age and year
- Equal correlation with price, difference is the direction.
- They tell the same story.

---


#### MACHINE LEARNING

MODELS: Linear Regression, Ridge Regression, Lasso Regression

| Model | Intercept |Mean Square Error | R2 Score|
| ----------- | ----------- | ----------- | ----------- |
| Linear Regression | 602734 | 32958214396 | 0.71615 |
| Lasso | 602738 | 32958190681 | 0.71615 |
| Ridge | 607318 | 32954842230 | 0.71618 |

##### **Evaluation:**

Mean Squared Error

- Calculates the mean squared distance between the the actual data points and the line of best fit. It is best when the distance is as minimal as possible.

R2 Score

- Explains how much of the variation in the output that our model can explain

**i. Linear Regression** 

Assumptions

a). Multicollinearity

- The heatmap reveals levels of correlation between variables. Age and year have a perfect negative correlation. Other columns of concern are engine with max power and seats, max power with price per km.

b). Linearity

- Visualized using a pairplot. The variables do not have a linear relationship with selling price.

c). Homoskedasticity 

- A plot of residual vs fitted values reveals a pattern in the residuals. The variance is not equal across the line so the data has heteroskedasticity.

Our data does not satisfy the Linear regression assumptions as it is and thus Linear regression might not be the best technique to model the data.

MSE: The model has the highest MSE thus it is not the best performing model of the three.

R2 Score:It has the lowest score.

**ii. Lasso Regression**

- Lasso regression is used for regularizarion. It works by getting rid of correlated features where it drops one and keeps the other and also by dropping irrelevant variables. 

MSE: It has a lower MSE compared to Linear Regression.

R2 Score: The model has a higher score as compared tp Linear Regression.

**iii. Ridge Regression**

- It is also used for purposes of regularization.Works differently from Lasso such that: when you have too many variables, it shrinks all of them and when you have correlated features, it shrinks them rather than getting rid of others. 

MSE: It has the lowest MSE which means it is the better performing of the three.

R2 Score: It has the highest of the three.

However, a plot of the actual vs predited values reveals that our model does not do a good job especially for the more expensive cars. The values become less clustered around the line of best fit as the selling price increases. 

**Hyperparameter in Ridge and Lasso Regression**

- In as much as we have obtained these results, it is worth to remember that the alpha value set for both affaects the results that you obtain. 
- Alpha controls the degree of the penalty in the model and thus it should be set just right in order to get the best possible reults.
- Used GridSearch to tune the value of alpha for ridge regression.

**iv. Polynomial Regression**

- The relationship between X and y is modelled to the nth degree which basically means that it is more of a curve than a linear relationship.

The polynomial model performed better than linear regression with a lower MSE and higher R2 score at nth degree=2. 

#### SUMMARY

---

Our dataset:
- Non-linear relationship present
- Outliers present

Best performing model:

-The polynomial model performed best out of all the models. 

Recommended improvements for a better model:

- For better predictions the selling price should be transformed to account for the outliers. 
- Random forest regressor for better and more reliable predictions