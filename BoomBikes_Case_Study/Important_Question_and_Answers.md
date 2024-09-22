# Important Questions:
### Q1. From your analysis of the categorical variables from the dataset, what could you infer about their effect on the dependent variable?
- Bike demand in the year 2019 is higher as compared to 2018.
- Bike demand is high if weather is clear or with mist cloudy while it is low when there
is light rain or light snow.
- The demand for bikes is almost similar throughout the weekdays.
- Bike demand doesn’t change whether the day is a working day or not.


### Q2. Why is it important to use drop_first=True during dummy variable creation?

- In linear regression, using `drop_first=True` during dummy variable creation is
important to avoid multicollinearity issues and maintain the integrity of the regression model.
- `drop_first=True` is important to use, as it helps in reducing the extra column created during dummy variable creation. Hence it reduces the correlations created among dummy variables.
- It is important in order to achieve k-1 dummy variables as it can be used to delete extra columns while creating dummy variables.
- For eg: Let's suppose, we have three variables in a column: `Furnished`, `Semi-furnished` and `unfurnished`.
- We can only take 2 variables:
  - Furnished will be 1-0
  - Semi-furnished will be 0-1,
  - so we don’t need unfurnished as we know 0-0 will indicate un-furnished.
So we can remove it.
  - It is also used to reduce the collinearity between dummy variables.


### Q3. Looking at the pair-plot among the numerical variables, which one has the highest correlation with the target variable?
- `Temp` and `a-temp` have very high correlation with each other.
- And hence both `temp`, and `a-temp` have the highest correlation with the target variable (`cnt`).

### Q4. How did you validate the assumptions of Linear Regression after building the model on the training set?
I have validate the assumptions of Linear Regression after building the model on training set by the following:
1. **Linearity of relationship between response and predictor variables.** 
    - **Observation:-** The relationship between the model and the different predictor variables was shown in the plots. We can clearly see that the linearity was well preserved.
2. **Constant variance of the errors or Homoscedasticity**
    - **Observation:-** No such visible pattern observed in residual values, thus we can conclude homoscedasticity is well preserved.
3. **Less Multi-collinearity between features ( Low VIF)**
    - **Observation:-** Since the VIF value is less than 5 for almost all predictor variables we can conclude that the multicollinearity is quite insignificant.
4. **Normality of Errors Distribution**
    - **Observation:-**Based on the above plot we can conclude that the error is normally distributed.

### Q5. Based on the final model, which are the top 3 features contributing significantly towards explaining the demand of the shared bikes?

The top 3 features contributing significantly towards the demands of share bikes are:
1. **Light_rainsnow:**
A coefficient value of `-0.245748` indicated that a unit increase in Light_rainsnow variable, decreases the bike hire numbers by `0.245748` units (Negative Correlation).
2. **year:**
A coefficient value of `0.231064` indicated that a unit increase in year variable increases the bike hire numbers by `0.231064` units. (Positive Correlation)
3. **Temperature(temp):**
A coefficient value of `0.534547` indicated that a unit increase in temp variable increases the bike hire numbers by `0.534547` units. (Positive Correlation)


----------------------------------------

## General Subjective Questions

### Q1. Explain the linear regression algorithm in detail.
As the name suggests, it means to find the best straight line that represents the relation between things. Imagine you have a bunch of data points on a graph. Linear regression is like drawing a straight line through those points in a way that best represents the overall trend of the data.
Equation of linear Regression is:


    𝑦 = 𝑚1 * 𝑥1 +𝑚2 * 𝑥2 + 𝑚3 * 𝑥3 + ...............+ 𝑚(𝑛) * 𝑥(𝑛) + 𝑐


Let’s see how it works:
1. **Data Collection:** First, we gather data on parameters that we think might be related. For eg: In the assignment of BoomBikes, we have collected the data related to the bike bookings depending on the weather, season, temperature, etc.
2. **Plotting the Points:** We then plot each data point on a graph(scatter plot). And find the trends between variables.
3. **Finding the Line:** Linear regression is about finding the best-fitting line through those points. This line is like a summary of the relationship between the two variables. It's called a **"regression line"**.
4. **Minimising Errors:** The goal is to make the line as close as possible to all the points. But, because not every point will fall exactly on the line, there will be some error. Linear regression tries to minimise this error by adjusting the slope and intercept of the line.
5. **Making Predictions/Suggestion:** Once we have the line, we can use it to make predictions. For eg: In the assignment, it seemed that in the rainy season, sales went down. Hence, we can suggest if the company provides special offers and discounts, it might draw more customers.
6. **Understanding Relationships:** Linear regression also helps us understand the relationship between the two variables. For instance, if the line slopes upward, it means there's a positive relationship (warm and pleasant temperature usually leads to more bookings). If it slopes downward, it's a negative relationship.
So, linear regression is a method to find the best straight line that represents the relationship between two things, making it easier to understand and predict outcomes based on the data.


### Q2. Explain the Anscombe’s quartet in detail.
Anscombe's quartet is like a superstar in the world of statistics. It shows us why looking at pictures of data is super important, not just relying on numbers. There are four groups of data in this quartet, each with 11 points. Even though they seem totally different when you just look at the numbers, their averages, spreads, and how they're connected with lines are all pretty much the same. It's a big eye-opener to see that data can look different but have similar stats behind the scenes.
We can explain each of the datasets as the following:
1. **Dataset I:**
 - This dataset forms a simple linear relationship between the variables. It has a clear linear trend, and the relationship between the variables is accurately captured by a linear regression model.
2. **Dataset II:**
 - Similar to Dataset I, this dataset also exhibits a linear relationship, but with one outlier point that significantly affects the regression line. Despite the outlier, the summary statistics remain similar to those of Dataset I, highlighting the limitations of using summary statistics alone to assess the data.
3. **Dataset III:**
 - This dataset appears to follow a quadratic relationship rather than a linear one. While the data points don't align with a straight line, the summary statistics still resemble those of the previous datasets, demonstrating how summary statistics can be misleading in the presence of non-linear relationships.
4. **Dataset IV:**
- In contrast to the previous datasets, Dataset IV has an entirely different pattern, with one outlier drastically influencing the regression line. Despite this outlier and the non-linear relationship, the summary statistics remain similar to those of the other datasets.

The significance of Anscombe's quartet lies in its demonstration of the limitations of summary statistics and the importance of data visualisation.

### Q3. What is Pearson’s R?
Pearson's R, often just called "correlation coefficient". It is like a measurement of how closely two sets of numbers are related. It refers to if there's a relationship between them and if yes, then how strong that relationship is.
From the assignment, we have data like number of bike bookings vs temperature. Pearson's R would give you a number between -1 and 1:
- If it's close to 1, it means there's a strong positive relationship. So, if the temperature is a little higher(pleasant and warm), the number of booking trends will be more.
- If it's close to -1, it means there's a strong negative relationship. That would mean if the temperature is higher, the number of bookings tend to be lower.
- If it's close to 0, it means there's not much of a relationship. Temperature high or low doesn't seem to have a consistent effect on the number of bookings.

So, Pearson's R helps you figure out if there's a connection between two things you're looking at, like temperature and number of bike bookings, and how strong that connection is.

### Q4. What is scaling? Why is scaling performed? What is the difference between normalized scaling and standardized scaling?
Scaling is the process of adjusting the range of values in the datasets so that they all fall within a similar scale or range. It's like making sure all the numbers play nicely together on the same team.
For eg: Let’s say we have different types of measurements, like weight in kilograms and height in centimetres. Scaling helps put these measurements on a similar scale so that they can be compared more easily.

Following are the reason why scaling is performed:
1. **Equalising Variables:** Sometimes, the variables we are working with
might be in different units or have different ranges. Scaling helps to
make them comparable by putting them on the same scale.
2. **Improving Model Performance:** Scaling can help improve the performance of certain machine learning algorithms, including linear regression. It can make the algorithm more efficient and accurate by ensuring that no single variable dominates the others just because of
its scale.

Normalised Scaling and Standardised Scaling are the types of scaling:
- **Normalised Scaling:** This type of scaling adjusts the values of the variables so that they fall within a specific range, usually between 0 and 1. It preserves the relative relationships between the data points but scales them to fit within a consistent range.
- **Standardised Scaling:** Standardised scaling, on the other hand, adjusts the values of the variables so that they have a mean of 0 and a standard deviation of 1. This means the data is centred around 0 and spread out evenly. It doesn't change the shape of the distribution but makes it easier to compare different variables.

### Q5. You might have observed that sometimes the value of VIF is infinite. Why does this happen?
In linear regression, the Variance Inflation Factor (VIF) measures how much the variance of the estimated regression coefficients is inflated due to multicollinearity in the predictor variables. Multicollinearity occurs when two or more predictor variables in the regression model are highly correlated with each other.

When there is perfect multicollinearity, then the infinite VIF occurs. Perfect multicollinearity refers to the situation when one or more predictor variables in the regression model can be exactly predicted by a linear combination of the other predictor variables, or in simpler words one or more variables are redundant because they can be perfectly predicted from the other variable.

It is impossible to estimate distinct regression coefficients for each predictor variable in such circumstances, as the regression model becomes unstable. Consequently, for variables impacted by perfect multicollinearity, the VIF computation can produce an infinite value.

Finding and eliminating duplicate variables from the regression model is crucial to resolving this problem. You may do this by either completely removing the variables or by applying strategies like variable selection or dimensionality reduction. This enhances the regression model's interpretability and stability.

Mathematically, if there is a perfect correlation between the dependent variable and independent variable(s), the R-squared value comes out to be 1.

Hence VIF, which is calculated by:

    𝑉𝐼𝐹 = 1/(1 −𝑅2)

When **R -> 1**, then VIF tends to be **infinity(∞)**.

### Q6. What is a Q-Q plot? Explain the use and importance of a Q-Q plot in linear regression.
The **Quantile-Quantile (Q-Q) plot** is a visual aid used to evaluate whether a dataset likely conforms to a specific theoretical distribution, such as **Normal**, **exponential**, or **Uniform** distribution. Additionally, it assists in identifying if two datasets originate from populations with similar distributions.

In the context of linear regression, where we often work with distinct train and test datasets, the Q-Q plot serves to verify whether both datasets are from populations with matching distributions. This confirmation ensures the consistency and reliability of the data before proceeding with further analysis.

Q-Q plots are important in linear regression because:
- **Assumption Checking:** In linear regression, it's crucial to ensure that the residuals follow a normal distribution. A Q-Q plot visually checks if this assumption holds true by comparing the residuals to the expected normal distribution.
- **Detecting Departures from Normality:** By examining the Q-Q plot, deviations from a straight line indicate departures from normality in the data. This alerts analysts to potential issues with the normality assumption, prompting adjustments to maintain the reliability of the regression analysis.

