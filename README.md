# Protein Prediction Power

by Yuancheng Cao (yuc094@ucsd.edu), Grace Chen

Credit: [UC San Diego DSC 80 Winter 2023 Course Project 5 Instruction](https://dsc80.com/project5/) 🙏

Our exploratory data analysis on this dataset can be [found](https://cao1224.github.io/recipeRatings_EDA/).😉

## Problem Identification

Continuing from our previous research, we thought it would be an interesting investigation to estimate the protein content of a recipe.Our prediction problem is: **How to predict protein content of a recipe from other nutrition information and related tags?** 🤨 We will produce a regression model to predict quantitative responses. The response variable we chose is protein in PDV (percentage of daily value). 

We have noticed that many recipes have "low fat", "low carb", "low calories" in their title, possibly a means of attracting browsing. From EDA, we observed that 1512 recipes contain "low fat" in their title; 1042 recipes contain "low carb" in their title; 254 recipes contain "low cal" in their title. In reality, there are also many recipes that only advertise about lower fat, lower carbohydrates, and lower calories and only mark these nutrition information😢. But we believe proteins are equally important for healthy eaters, and should be predicted if not explicitly given. The information available to us at the “time of prediction” will be a combination of nutrition information: calories (#), total fat (PDV), carbohydrates (PDV) and other related verbal discription of the recipes, in this case, we will use information in the tags column. Although these pieces of information about a recipe are the most commonly provided, we hope that our model can also predict the protein content of recipes for people who needs it. Regression models can take into account the relationships between the input variables, such as calories, fat content, and carbohydrate content, and the output variable, protein content.

To evaluate our the metric, we decided to use RMSE to measure the error in prediction. RMSE is a measure of the average magnitude of the errors in the predictions made by the model. A lower RMSE indicates a better fit. We also used R squared when evaluating our models. A high R-squared indicates that the model is a good fit to the data and explains a large proportion of the variation in the dependent variable. Both RMSE and R squared are commonly used metrics to evaluate the performance of regression models. However, there are several reasons why we believe RMSE will better predit our model, which makes it easier to understand and interpret. One of the advantages of RMSE is that it has the same unit as the target variable. In addition, R-squared may overestimate the goodness-of-fit of the model, if parameters in a model are highly correlated with each other. On the other hand, RMSE is more robust to multicollinearity. This allows us to better understand the model's predictive performance.


## Baseline Model

The baselind model is a linear regression model with degree of 2 polynomial features, I chose 3 featuress related to protein content.
- `calories`
  - Calorie content affects the protein content of a recipe, since high-calorie foods are often high protein. For example, meat🥩 annd dairy🧀 products are high in calories and protein. However, it should be noted that not all high-calorie foods are high-protein, and not all high-protein foods are high-calorie. Some 🫘plant-based proteins are also high!
- `total fat` (PDV)
  - Protein content in a recipe can be impacted by fat content because fat can mute the protein in a dish. For example, a recipe that is high in fat but low in protein will have a lower protein-to-fat ratio than a recipe that is high in protein but low in fat. The protein flavor of a dish can also be masked by fat, making it less prominent as fat can provide a more satisfying overall flavor experience.
- `carbohydrates` (PDV)
  - Carbohydrates can also affect protein content by affecting the overall nutritional content of a recipe. For example, recipes high in carbohydrates may be low in other nutrients like protein or fats, which can affect the overall protein content of the meal.
  
**Note:** PDV stands for “percentage of daily value”

At first, polynomial regression was used with polynomial features of degree 1 - 15, and the training and testing errors were visualized to observe any trends (refer figure below). It was found that the training and test errors for polynomials 1-4 were almost identical, making it difficult to choose which polynomial should be used for protein prediction and model evaluation. 

<iframe src="assets/polynomialFeaturesTrends.html" width=700 height=400 frameBorder=0></iframe>

Hence, k-fold cross-validation was used to find the best fit polynomial. The 5-fold cross-validation results showed that a degree of 1 was the best fit because it has the lowest average RMSE across all the folds of cross-validation. Additionally, the degree of polynomial features used in each fold was recorded, with the first and third folds using a degree of 1, and the second, fourth, and fifth folds using a degree of 2. This indicates that a second-degree polynomial may provide a better fit for the data in certain cases. But, there is a problem with using polynomial 1 or 2 with negative protein content in both testing and training datasets. So look at the RMSE and R-squared values for each degree of polynomial used in the model to gain insight into how well the model fits the data, and whether overfitting or underfitting occurs.

- **Degree of 1**
  - RMSE of training is 16.25; RMSE of testing is 16.17
  - Training accuracy is 0.89; Testing accuracy is 0.88
- 👀 **Degree of 2**
  - RMSE of training is 15.42; RMSE of testing is 15.39
  - Training accuracy is 0.90; Testing accuracy is 0.89

Based on the results, both models seem to perform similarly, with the degree of 2 polynomial having slightly lower RMSE values and slightly higher accuracy scores. However, the negative protein content in both the training and testing datasets suggests that there may be some issues with the accuracy of the model. The model with degree of 2 may be considered "good" but there is definitely room for improvement.


## Final Model
The dataframe has 10 columns including vegetarian, cooking_method, red_meat, calories, total_fat, sugar, sodium, saturated_fat, carbohydrates, and protein. The columns vegetarian, cooking_method, and red_meat represent the number of tags related to specific ingredients or methods and are already numeric, so no transformer is needed for these columns.

**🆕 New Features without Transformer**:
- `vegetarian`: number of tags related to beans, lentils, and soy-tofu
  - The variable `vegetarian` was included in the model as it is believed that recipes with a higher number of vegetarian-related tags may have a higher protein content due to the inclusion of protein-rich plant-based ingredients such as beans and lentils. 
- `cooking_method`: number of tags related to oven and steam
  - We included this variable as different cooking methods may have different effects on the nutritional content of the recipe, such as the protein content.
- `red_meat`: number of tags related to beef-organ-meats, beef, and roast_beef
  - We included this variable as it may impact the protein content of the recipe. Red meat is a common source of protein, and recipes that include red meat may have higher protein content compared to those that do not.

**🆕 New and Old Nutritional Information with Transformer `StandardScaler`**
We added the following features to the dataframe: `sugar`, `sodium`, `saturated fat`. We believe that these features improved our model's performance because they provide more nutritional information about the recipe and how it may affect the protein content. We used the `StandardScaler` transformer on the columns of calories, total fat, sugar, sodium, saturated fat, and carbohydrates. This transformer was used to scale the data to a mean of zero and a variance of one. This was done to ensure that all the features had equal importance in the model and that no feature would dominate the prediction. Overall, we believe that these features and transformer improved the performance of the model by providing more information about the recipe and standardizing the data for better accuracy.

The following **3 combincations** of varaibles can be used to predict protein content:
1. `vegetarian`, `red_meat`, `calories`, `total_fat`, `carbohydrates`
  - Linear Regression: RMSE of training is 16.25; RMSE of testing is 16.17
2. `vegetarian`, `cooking_method`, `red_meat`, `calories`, `total_fat`, `carbohydrates`, `sugar`, `sodium`
  - Linear Regression: RMSE of training is 16.12; RMSE of testing is 16.06
3. `cooking_method`, `red_meat`, `calories`, `total_fat`, `sugar`, `sodium`, `saturated_fat`, `carbohydrates`
  - Linear Regression: RMSE of training is 16.13; RMSE of testing is 16.08

**Linear Regression and Degree 1 Polynomial Regression**
I used GridSearchCV with cv=5 and polynomial degree from 1 to 5 to determine the best hyperparameters for the Polynomial Features model. The best hyperparameter was found to be a degree of 1 for the Polynomial Regression with Linear Regression combination. Interestingly, all three combinations of Polynomial Features of Linear Regression with degrees 1, 2, and 3 produced the same result as Linear Regression. This suggests that the addition of Polynomial Regression did not improve the accuracy of the model.

**Random Forest Regression 🌲**

Based on the large size of our dataset with over 200,000 finding the optimal combination of hyperparameters for our Random Forest Regression model can be computationally expensive. As a result, we will set the number of trees in the forest and the maximum depth of the tree as small as possible to efficiently search for the best parameters. We used GridSearchCV to tune the hyperparameters of the model, including the number of estimators [100], the maximum depth of the tree [3, 4, 5, 10, 100]. Using GridSearch CV to search for the best hyperpameters for combination 1, we found that the number of trees in the forest and the maximum depth were both 100. We then trained the model on 70% of the dataset, with the remaining 30% used for testing, and with these hyperparameters and obtained a training error of 6.45 and a testing error of 11.60, resulting in a training accuracy of 0.98 and testing accuracy of 0.94. Based on these results, we decided to manually set different values for n_estimators and max_depth to see their effect on the model's performance, without using GridSearchCV.

**Note:** First tuple is training and testing error, second tuple is accuracy of training and testing, last tuple is minimum value of training and testing dataset.
- 🏆 **Combination 1: `n_estimators` is 100 and `max_depth` is 100**
  - [(6.45, 11.60), (0.98, 0.94), (0, 0)]
- Combination 2: `n_estimators` is 200 and `max_depth` is 100
  - [(5.26, 14.39), (0.99, 0.90), (0, 0)]
- Combination 3: `n_estimators` is 200 and `max_depth` is 150
  - [(6.01, 13.74), (0.98, 0.91), (0, 0)]

The accuracy of the model is evaluated using the training dataset and testing dataset. For Combination 1, the training accuracy is 0.98 and the testing accuracy is 0.93, which indicates that the model fits the data well and is also able to generalize to new data. However, for all three combinations, the test error is higher than the training error, which may indicate overfitting. But only Combination 1 has the smallest difference between test error and training error. This could be due to the higher number of estimators used in Combination 2 and the deeper trees in Combination 3, which may have led to overfitting the training data. Therefore, we decided to stick with the original hyperparameters of n_estimators = 100 and max_depth = 100. The Combination 1 mode performed reasonably well in predicting protein content based on the given features. However, there is still room for improvement as the testing error was relatively high. Additionally, it may be beneficial to explore additional features or different types of models to improve the accuracy of the predictions.

Compared to the baseline model, which was a linear regression model with degree 2 polynomial features, the Combination 1 random forest model showed significant improvement in both training and testing accuracy. The baseline model had a training accuracy of 0.90 and a testing accuracy of 0.89, with RMSE of training as 15.42 and RMSE of testing as 15.39.

**Back to Baseline Model Issue**

Linear regression model with degree of 2 polunomial features have negative protein content in the training and testing datasets, while random forest regression does not have this problem. The reason may be that the linear regression model with degree of 2 polunomial features overfits the data, which means it is too complex and captures noisse rather than the true underlying pattern. Random forest regression is a nonparametric model that does not make assumptios about the functional form of the relationship between the predictor and target variables. Instead, it uses  a combination of decision trees to create predictions. This can make it more robust to outliers and nonlinear relationships between variables. The fact that random forest regression has a higher test error than training error, as this is common in complex models. However, it is important to monitor the gap between the training error and the test error to ensure that the model does not overfit the data.


## Fairness Analysis
**Does our model perform better for recipes with high ratings than it does for recipes with low ratings?** 🤔 We will define recipes with a 4.0 rating or higher as a recipe with high ratings, there are 206,983 total recipes from the dataframe that we considered as recipes with high ratings. We will define recipes with less than 4.0 rating as a recipe with low ratings, there are 12,410 total recipes from the dataframe that we considered as recipes with low ratings. 

Now, we will perform a permutation test to answer our question. Null Hypothesis: Our model is fair. Its RMSE for high rating recipes and low rating recipes are roughly the same, and any differences are due to random chance. Alternative Hypothesis: Our model is unfair. Its RMSE for high rating recipes is lower than its RMSE for low rating recipes.

We expected RMSE for low rating recipes to be higher, so we will use: RMSE for low rating recipes - RMSE for high rating recipes as our test statistics. We choose a significane level at ɑ = 0.05


From 100 repetitions, we got a p-value of 0.56, this is greater than the significane level at ɑ = 0.05. Therefore, we fail to reject the null hypothesis. From the permutation test, we can conclude that our model is likely fair. Specifically, RMSE for high rating recipes and low rating recipes are roughly the same, and any differences are due to random chance. In fact, from the negative observed statistic, we saw that RMSE for high rating recipes is actually slightly higher than RMSE for low rating recipe! This is not what we expected! 😲

From the plot, we can also summarize that an observed difference of -1.9 is totally reasonable.

