# End_to_End_Linear_Regression
This function, run_regression_analysis, is a end-to-end solution for performing linear regression analysis with several diagnostics, transformations, and model fitting options. 

Function Definition
Parameters:
X: The predictor variables matrix (can be a DataFrame or numpy array).
y: The continuous response variable (also can be a Series or numpy array).
purpose: Specifies whether the analysis is for "prediction" or "explanation" (default is "prediction").
transformation: Specifies the transformation to apply to the response variable (y). Options are:
"none" (no transformation),
"log" (logarithmic transformation),
"sqrt" (square root transformation),
"boxcox" (Box-Cox transformation).
cv_folds: The number of folds for cross-validation (default is 5).
Returns:
A dictionary containing the model metrics, chosen model parameters, and diagnostic plots.
Preprocessing
Handling Missing Values:
The function starts by dropping any rows in X that have missing values (X.dropna()).
It also ensures that y aligns with the cleaned X index (y.loc[X.index]), meaning y is subset to match the predictors that remain after dropping X rows with missing data.
Diagnostics Before Transformation:
Before any transformation, the function fits an OLS (Ordinary Least Squares) model to check some important assumptions of linear regression.
Diagnostics
These are statistical tests and visual checks that help assess whether the linear regression assumptions hold and that we can actually use linear regression
1. Normality of Residuals (Shapiro-Wilk Test):
Shapiro-Wilk Test: This test checks if the residuals (errors) follow a normal distribution. The null hypothesis is that the residuals are normally distributed. The result gives a statistic (W) and a p-value. If the p-value is less than a significance threshold (e.g., 0.05), we reject the null hypothesis and conclude that the residuals are not normally distributed.
Q-Q Plot: A graphical tool that plots quantiles of the residuals against theoretical quantiles from a normal distribution. If the points lie close to a straight line, the residuals are approximately normal.
2. Homoscedasticity (Breusch-Pagan Test):
Breusch-Pagan Test: This test checks for heteroscedasticity, which refers to the residuals having non-constant variance. A significant p-value (less than 0.05) suggests that the residuals are heteroscedastic, violating one of the key assumptions of linear regression.
3. Linearity Check (Residuals vs. Fitted Values Plot):
This plot helps to check for the assumption of linearity. A random scatter of residuals around zero suggests that the relationship between the predictors and the outcome is linear. If there is a clear pattern (e.g., a curve), it suggests a non-linear relationship.
Transformation of Response Variable (y)
Depending on the transformation argument, the function applies different transformations to the response variable y:
Log Transformation: This transformation is useful when the response variable has a skewed distribution or wide-ranging values. If y contains non-positive values, the function adjusts by adding a constant before applying the log transformation.
Square Root Transformation: Similar to the log transformation, square root transformation is applied to stabilize variance. Negative values in y are handled by adding a constant.
Box-Cox Transformation: This is a more flexible transformation that includes the log transformation as a special case. The Box-Cox method finds the best transformation parameter (lambda) for y to make it as close to normal as possible. If there are non-positive values in y, a constant is added before applying Box-Cox.
No Transformation: The response variable y is left unchanged.
After the transformation, the function re-checks the diagnostics (normality, homoscedasticity, and linearity) with the transformed y.
Outliers, Leverage, and Influential Points
Outliers and influential points can significantly affect regression models. This section evaluates them using the following diagnostics:
1. Cook's Distance:
Cook's Distance is a measure of the influence of each data point on the fitted regression model. Points with high Cook's distance are considered influential and may disproportionately affect the model. A plot of Cook's Distance helps visualize these points.
2. Leverage vs. Cook's Distance Plot:
Leverage measures how far an observation’s predictor values are from the mean predictor values. Points with high leverage can have a large influence on the regression model. The plot helps identify observations with high leverage that also have high Cook's Distance.
F-test for Nested Models
F-test: This test is used to compare the fit of two models, one nested within the other. The full model includes all predictors, while the reduced model excludes one or more predictors. The F-statistic compares the residual sum of squares (RSS) from both models and tests whether the excluded predictors significantly improve the model fit. A small p-value indicates that the full model is significantly better.
Fitting Ridge and Lasso Models
Ridge Regression and Lasso Regression are forms of regularized linear regression used to handle multicollinearity and overfitting. The function uses cross-validation to tune the regularization parameter (alpha), testing a range of values from 10^-3 to 10^3. GridSearchCV is used to find the best model for each.
Ridge Regression penalizes the sum of squared coefficients.
Lasso Regression penalizes the absolute sum of the coefficients, which can also result in zero coefficients (i.e., feature selection).
Purpose of the Analysis
Prediction: If the purpose is "prediction", the function returns metrics like Mean Squared Error (MSE), AIC, BIC, and Adjusted R² for the OLS, Ridge, and Lasso models. These metrics help assess the predictive accuracy and the goodness-of-fit of the models.
Explanation: If the purpose is "explanation", the function prints out the summary of the OLS model, which includes information about the coefficients, their significance, and overall model statistics like R², p-values, etc. This is useful for understanding the relationships between the predictors and the response variable.
Model Metrics
The function calculates and prints out several metrics for each of the models (OLS, Ridge, and Lasso):
MSE (Mean Squared Error): Measures the average squared difference between the actual and predicted values. Lower MSE indicates better predictive accuracy.
AIC (Akaike Information Criterion): A measure of model quality that penalizes for the number of predictors. Lower AIC values suggest a better model.
BIC (Bayesian Information Criterion): Similar to AIC, but it includes a stronger penalty for models with more predictors.
Adjusted R²: Adjusted R² accounts for the number of predictors and provides a more accurate measure of the model's explanatory power.
