# Protein Prediction Power

by Yuancheng Cao (yuc094@ucsd.edu), Grace Chen

Credit: [UC San Diego DSC 80 Winter 2023 Course Project 5 Instruction](https://dsc80.com/project5/) 🙏

## Problem Identification



## Baseline Model

The baselind model is a linear regression model with degree of 3 polynomial features, I chose 3 featuress related to protein content.
- `calories`
  - Calorie content affects the protein content of a recipe, since high-calorie foods are often high protein. For example, meat🥩 annd dairy🧀 products are high in calories and protein. However, it should be noted that not all high-calorie foods are high-protein, and not all high-protein foods are high-calorie. Some 🫘plant-based proteins are also high!
- `total fat` (PDV)
  - Protein content in a recipe can be impacted by fat content because fat can mute the protein in a dish. For example, a recipe that is high in fat but low in protein will have a lower protein-to-fat ratio than a recipe that is high in protein but low in fat. The protein flavor of a dish can also be masked by fat, making it less prominent as fat can provide a more satisfying overall flavor experience.
- `carbohydrates` (PDV)
  - Carbohydrates can also affect protein content by affecting the overall nutritional content of a recipe. For example, recipes high in carbohydrates may be low in other nutrients like protein or fats, which can affect the overall protein content of the meal.
  
Note: PDV stands for “percentage of daily value”

At first, polynomial regression was used with polynomial features of degree 1 - 15, and the training and testing errors were visualized to observe any trends (refer figure below). It was found that the training and test errors for polynomials 1-4 were almost identical, making it difficult to choose which polynomial should be used for protein prediction and model evaluation. 

<iframe src="assets/polynomialFeaturesTrends.html" width=700 height=400 frameBorder=0></iframe>

Hence, k-fold cross-validation was used to find the best fit polynomial. The 5-fold cross-validation results showed that a degree of 1 was the best fit because it has the lowest average RMSE across all the folds of cross-validation. Additionally, the degree of polynomial features used in each fold was recorded, with the first and third folds using a degree of 1, and the second, fourth, and fifth folds using a degree of 2. This indicates that a second-degree polynomial may provide a better fit for the data in certain cases. But, there is a problem with using polynomial 1 or 2 with negative protein content in both testing and training datasets. So look at the RMSE and R-squared values for each degree of polynomial used in the model to gain insight into how well the model fits the data, and whether overfitting or underfitting occurs.

- **Degree of 1**
  - RMSE of training is 16.25; RMSE of testing is 16.17
  - Training accuracy is 0.89; Testing accuracy is 0.88
- **Degree of 2**
  - RMSE of training is 15.42; RMSE of testing is 15.39
  - Training accuracy is 0.90; Testing accuracy is 0.89

Based on the results, both models seem to perform similarly, with the degree of 2 polynomial having slightly lower RMSE values and slightly higher accuracy scores. However, the negative protein content in both the training and testing datasets suggests that there may be some issues with the accuracy of the model. The model with degree of 2 may be considered "good" but there is definitely room for improvement.


## Final Model

Since plant-based protein may have a different nutrient profile compared to animal-based protein, this can affect the amount of protein in a recipe. I decided to add tags about plant-based protein and animal-based protein as features.

## Fairness Analysis
