by Hillary Chang

## Introduction
In an era where mindful eating and wellness are in the spotlight, there's a growing emphasis on understanding the nutritional content of our meals. The calorie count of a recipe isn't just a number—it has evolved into a crucial piece of information, guiding individuals to make informed decisions about their dietary choices. For athletes, health enthusiasts, and even individuals living a normal lifestyle, having awareness of protein count and calorie content in recipes is essential for weight management, reaching fitness objectives, and fostering mindful decision-making. Our specific aim is to investigate the existence of a correlation between protein and calorie content in recipes.

Our dataset is curated from food.com and contains recipes and reviews. The data is separated into two datasets, the recipes dataset and ratings dataset. The recipes dataset contains columns like ```name```, ```id```, ```minutes```, ```contributor_id```, ```submitted tags```, ```nutrition```, ```n_steps```, ```steps```, ```description```, ```ingredients```, ```n_ingredients```, representing the recipe name, recipe ID, the amount of recipes, ID of the person who contributed the recipe, and nutritional information, including the fats, sugar, sodium, protein, saturated fat, and carbohydrates. The ratings dataset includes te columns, user_id, recipe_id, date, rating, and review. This represents the ID of the user who left the review, the recipe ID the user left the review for, the date, and the rating and review the reviewer left for the recipe. The dataframe for recipes has 83782 rows, which represents 83782 unique recipes. The dataframe for ratings has 731927 rows, which represents 731927 reviews.

---


## Data Cleaning
### Merging DataFrames:
We created our DataFrame by performing a left merge on the ‘recipes’ and ‘interactions’ DataFrames on the ```id``` and ```recipe_id``` columns respectively. In doing so, we are able to see the information necessary to our data analysis on one DataFrame.

### Fill 0's with NaN Value:
After merging our data sets into one DataFrame, we filled all the 0 values in the ```rating``` column with np.NaN. This was done in order to preserve accurate calculations for distribution statistics, such as the mean and median, as these functions will drop np.NaN when calculating. In this way, we can calculate accurate distribution statistics with values that count towards the total, rather than including values that would not contribute to the overall calculation.

### Add Average Rating Column:
Next, we added an ```avg_rating``` column to the DataFrame by creating a new DataFrame that consists of 1 column, indexed by the ```name``` column of the original DataFrame. The values of this column were found by performing a groupby in the ```name``` column of the original DataFrame and finding the mean. After making this auxiliary DataFrame, we then merged it with the original DataFrame with an inner join on the ```name``` column of both. 

### Convert Nutrition Column to List and Assign Individual Columns
The ```nutrition``` column on the original DataFrame has important information regarding the caloric and nutrient content of each recipe. We felt that it was imperative to sort these individual values into their respective columns on the DataFrame. 

First, we imported the ast package, which allowed us to utilize the literal_eval() function in order to convert the object data types of the column into Python lists that we can more easily access the individual values. Afterwards, we assigned the columns ```calories```, ```total_fat```, ```sugar```, ```sodium```, ```protein```, ```saturated_fat```, and ```carbs``` with the values corresponding to each label from the ```nutrition``` column. This was done through a simple indexing of the values of the ```nutrition``` column.

### Remove duplicate recipe entries 
Next, we noticed that there were duplicate recipe names in the ```name``` column. To manage this, we used a simple groupby on the ```name``` column in order to group each value by the name of the recipe. 

### Look at outliers 
Throughout our analysis of the data, we noticed that some of the calorie counts were extremely high (ie. 40000). We decided to exclude these outliers due to the fact that most diets range from 0-2000 calories a day, making anything above that range unrealistic.

### The first few 5 rows of our cleaned and merged dataframe is shown below (with only important columns selected for display).

| name                                 | id      | rating | calories | protein |
|--------------------------------------|---------|--------|----------|---------|
| 0 carb 0 cal gummy worms             | 283618.0| 4.75   | 384.7    | 159.0   |
| 0 point ice cream only 1 ingredient  | 480558.0| 5.0    | 304.1    | 13.0    |
| 0 point soup ww                      | 391705.0| 4.78   | 26.8     | 2.0     |
| 0 point soup crock pot               | 416980.0| 5.0    | 40.7     | 4.0     |
| 007 martini                          | 429524.0| 5.0    | 146.5    | 0.0     |


---


## Exploratory Data Analysis
## Univariate Analysis
In the univariate analysis, we analyzed the distribution of calorie count in the recipes. Because there were several recipes with extremely high calorie counts, we split the analysis into two graphs and created a distribution for recipes with calories greater than the recommended calorie intake of 2000 and a second one for recipes with calories less than or equal to the recommended calorie intake of 2000 to remove the outliers for each distribution.
<iframe src="assets/calorie_below.html" width=800 height=600 frameBorder=0></iframe>
This plot shows that the distribution of recipes with a calorie count from 0 to 2000 could be approximated as a right skewed gaussian distribution. The graph is centered around 200, meaning that most recipes below the recommended calorie intake of 2000 calories have around 200 calories.

<iframe src="assets/calorie_above.html" width=800 height=600 frameBorder=0></iframe>
This plot shows that the distribution of recipes with a calorie count from 2000 and up is also a right skewed gaussian distribution. Compared to the plot with a calorie count from 0 to 2000, this graph has a lot more outliers, with several recipes that have extremely high calorie count. The graph is centered around 2500, meaning that most recipes above the recommended calorie intake of 2000 calories have around 2500 calories.

## Bivariate Analysis
In the bivariate analysis, we analyzed the distribution of calorie count in the recipes with the protein count. Because there were several recipes with extremely high calorie counts, we split the analysis into two graphs and created a distribution for recipes with calories greater than the recommended calorie intake of 2000 and a second one for recipes with calories less than or equal to the recommended calorie intake of 2000 to remove the outliers for each distribution.
<iframe src="assets/bivariate" width=800 height=600 frameBorder=0></iframe>
This plot shows that there is a positive correlation between protein count and calorie count for recipes with a calorie count equal to or below 2000 calories. It is also interesting to note that recipes with a very high calorie count can have 0 grams of protein or very high counts of protein.

<iframe src="assets/bivariate_above" width=800 height=600 frameBorder=0></iframe>
This plot also shows that there is a positive correlation between protein count and calorie count for recipes with a calorie count above 2000 calories. It is also interesting to note in this plot that recipes with a very high calorie count can have very low or very high counts of protein, as shown in the plot from the recipe with a count of 36,188 calories with 329 grams of protein, compared to the recipe with 45,609 calories with 4356 grams of protein.

## Interesting Aggregates
In the aggregates analysis, we studied the calorie and protein count. We looked at the mean for each, and noticed recipes with high counts of protein tend to have high counts of calories, whereas recipes with low counts of protein tend to have low counts of calories

| mean calories | mean protein |
|---------------|--------------|
| 4356.000      | 45609.000    |
| 360.000       | 28930.200    |
| 446.000       | 26604.400    |
| 3605.000      | 21497.800    |
| 329.000       | 18927.250    |
| ...           | ...          |
| 1.250         | 91.925       |
| 7.250         | 87.775       |
| 0.035714      | 82.625       |
| 7.500         | 54.400       |
| 0.500         | 53.425       |

This is the pivot table for the ```calories``` and ```protein```

---



## Assessment of Missingness
State whether you believe there is a column in your dataset that is NMAR. Explain your reasoning and any additional data you might want to obtain that could explain the missingness (thereby making it MAR). Make sure to explicitly use the term “NMAR.”
Present and interpret the results of your missingness permutation tests with respect to your data and question. Embed a plotly plot related to your missingness exploration; ideas include:
The distribution of column Y when column X is missing
the distribution of column Y when column X is NOT missing
The empirical distribution of the test statistic used in one of your permutation tests, along with the observed statistic.
	
### NMAR Analysis
A column of our DataFrame that we believe to be NMAR is the ```rating``` column. Someone is more likely to rate a recipe if they believe that it is either extremely good (5.0) or extremely bad (0.0), resulting in more neutral ratings not being recorded. Thus, reviewers fail to report ratings for recipes they believe are average.

### Missingness Dependency
The column of missingness that we will be analyzing is the ```review``` column. To test our hypothesis, we performed a repeated TVD test on ‘review’ and the columns of ```contributer_id```, ```rating```, and ```n_steps``` of the original DataFrame.

### ‘Contributor_id’ and ‘review’
Null hypothesis: the distribution of ```contributor_id``` when ```review``` is missing is the same as the distribution of the ```contributor_id``` when ```review``` is not missing 

Alternative hypothesis: the distribution of ```contributor_id``` when ```review``` is missing is different from the distribution of ```contributor_id``` when ```review``` is not missing 

Observed Statistics: The total variation distance (TVD) between these two distributions.

To perform this experiment, we used permutation testing to shuffle the missingness of ‘contributor_id’ 500 times and get 500 experimental TVDs to compare against the observed TVD. 

<iframe src="assets/review_contributor_id" width=800 height=600 frameBorder=0></iframe>

Conclusion: From this experiment, we calculated the p-value to be 0.006. When using a significance threshold of 0.05, since 0.006 < 0.05, we reject the null hypothesis that ‘review’ is not dependent on ‘contributor_id.’ Based on our test result, we can see that the missingness of the ‘review’ is MAR because the missingness of ‘review’ is dependent on ‘contributor_id.’ This is likely due to the fact that someone who has left a review on a single recipe is more likely to leave more reviews on other recipes. 

### ‘N_steps’ and ‘review’ 
Null hypothesis: the distribution of ‘n_steps’ when ‘review’ is missing is the same as the distribution of the ‘n_steps’ when ‘review’ is not missing 

Alternative hypothesis: the distribution of ‘n_steps’ when ‘review’ is missing is different from the distribution of ‘n_steps’ when ‘review’ is not missing. 

Observed Statistics: The total variation distance (TVD) between these two distributions.

To perform this experiment, we used permutation testing to shuffle the missingness of ‘n_steps’ 500 times and get 500 experimental TVDs to compare against the observed TVD. 

<iframe src="assets/review_n_steps" width=800 height=600 frameBorder=0></iframe>

Conclusion: From this experiment, we calculated the p-value to be 0.156. When using a significance threshold of 0.05, since 0.156 > 0.05, we fail to reject the null hypothesis that ```review``` is not dependent on ```n_steps```. Based on our test result, we can see that the missingness of the ```review``` is MCAR because the missingness of ```review``` is not dependent on ```n_steps```.

---


## Hypothesis Testing
Hypothesis Test Question: 
Is there a significant relationship between protein level in recipes and their calorie counts?

### Setting Up the Testing
Null Hypothesis: 
The protein level has no association with calorie count in recipes.

Alternative Hypothesis: 
There is an association between protein level and calorie count in recipes.

We only selected useful columns, including ```protein```, ```calories```, and created a new column named ```above_threshold```, which is True for protein counts above the mean protein level of 33.13 grams and False if the protein count is below or equal to the mean protein level of 33.13 grams.

| name                                | above_threshold | protein | calories |
|-------------------------------------|-----------------|---------|----------|
| 0 carb 0 cal gummy worms            | True            | 159.0   | 384.7    |
| 0 point ice cream only 1 ingredient | False           | 13.0    | 304.1    |
| 0 point soup ww                     | False           | 2.0     | 26.8     |
| 0 point soup crock pot              | False           | 4.0     | 40.7     |
| 007 martini                         | False           | 0.0     | 146.5    |

We are using the mean difference in calorie content between recipes with protein levels above and below their mean thresholds as the test statistic. Because calorie count is numerical data, it is appropriate to use the difference in means as our test statistics. We are interested in comparing the calorie content for high protein levels (protein level above mean protein level) and low protein levels (protein level equal to or below mean protein level). We define "high protein level" as the protein level above mean protein level, and "low protein level" as the protein level equal to or below mean protein level.

We have chosen a significance level of 0.05 for this analysis. This is because a significance level of 0.05 is commonly used to represent the threshold for considering results statistically significant.

| above_threshold | protein    | calories   |
|------------------|------------|------------|
| False            | 11.140109  | 273.151118 |
| True             | 73.587422  | 718.447829 |

The observed difference in means is 445.29.

### Permutation Testing
<iframe src="assets/protein-calorie" width=800 height=600 frameBorder=0></iframe>


### Association between Protein and Calorie Count: 
The resulting p-value based on the observed difference and the distribution of differences obtained through permutation testing is 0.0.

With a p-value of 0.0 for the permutation test for protein, we reject the null hypothesis that the protein level has no association with calorie count in recipes given that we set our significance level at 0.05. This p-value suggests that there is an association between the protein level and calorie content for the recipes, which is reasonable because recipes with more protein tend to have more calories, which is also evident in the plots in the exploratory data analysis section.

---


## Framing the Prediction Problem
Our prediction problem for the Recipes and Ratings data set is predicting total calories based on the features of the data set, such as the number of ingredients per recipe ```n_ingredients``` and how the recipe is rated on a scale from 1 to 5 ```rating```. To model our prediction, we will be training a regression model. 

**Response Variable**:\
Our response variable will be an engineered column titled ```calories``` that we created during the exploratory data analysis process of this project. We believe that the ability to predict ```calories``` can be extremely useful for the food and health industry in a variety of different ways, such as in the development of new recipes and in the understanding of how different types of ingredients affect the overall caloric count of a recipe. This would not only allow manufacturers to maximize the nutritional value and quality of their products, but would also shift the industry towards a more health-conscious standard that would benefit consumers.

**Evaluation Metrics**:\
We will be using the root mean square error (RMSE) in tandem with the coefficient of determination (R^2) as metrics to evaluate our regression model. 

RMSE, by definition, shows us the average error between our predictions from the actual values of ```calories```. Thus, the lower the RMSE of the model is, the more accurate the predictions it makes and the better the model is. By using RMSE to evaluate our model, we can tune it to have a relatively low RMSE, which means that it can reliably predict `calories.`

R^2, on the other hand, represents the proportion of the variance for our dependent variable ```calories``` that is predictable from the independent variables ```n_ingredients```, ```n_steps```, ```total_fat```, ```sugar```, ```carbs```, ```sodium```, ```protein```, ```saturated_fat```, ```minutes```. This is an essential metric for our model, as it provides an intuitive measure of how much of the variation in our predictions can be explained by our model. For example, a high R^2 indicates that our model can explain a large proportion of the variability in the `calories`, which is what we are training the model to do, while a low R^2 indicates the opposite, which is what we do NOT want to do. By using R^2, we can understand how closes our predictions align with the features in out model and swap out specific features depending on its value. 

**Information Known**:\
At the time of prediction, our data set will have all of its original columns from the merged data ```name```, ```minutes```, ```review```, etc. as well as columns that we engineered during the exploratory data analysis process from our exploratory data analysis in Project 3 ```avg_rating```, ```total_fat```, ```protein```, ```total_fat```, ```sugar```, ```carbs```, ```sodium```, ```saturated_fat```. 


## Base Model
**Introduction**:\
The model that we will be using to solve our prediction problem will be the RandomForestRegressor. It will be trained on the`total_fat` and `sugar` features of the data set. Both of these features are quantitative.

**Data Encoding**:\
For the `total_fat` feature, we applied Binarizer() with the threshold at 31.9, which is the mean of all the `total_fat` column. For the `sugar` feature, we applied StdScaler() to standardize the `sugar` for all recipes. 

**Model Description and Performance**:\
We decided to use the RandomForestRegressor as our model of choice for this regression problem with default values on the parameters of the model.

Our model achieved a RMSE of 345.72485986758636 and a R^2 of 0.6556359621651207. Based on the RMSE alone, the model is very unreliable in predicting `calories` from our features, as the mean for the `calories` column is 419.52887014831776, which is relatively close to the RMSE we are achieving. Additionally, the R^2 implies that only 65.56% of the variability in the predicted `calories` can be explained by the `total_fat` and `sugar`. For our model to be considered “good,” we need to reduce the RMSE significantly and raise our R^2 to at least 90%.

| Metric            | Test Score                    |
| ------------------| ------------------------------|
| RMSE              | 345.72485986758636           |
| R^2               | 0.6556359621651207           |


## Final Model
**Introduction**:\
For our final model, we decided to stick with the RandomForestRegressor, as we believe that it is flexible for adding new features as well as good at preventing overfitting of features due to the majority voting process when deciding predictions.

**Added Features**:\
`carbs` - The carbohydrate count of a recipe often correlates with a higher calorie count, as seen in dishes like pasta and bread. 

`sodium` - The inclusion of excess sodium is often correlated with highly-processed foods or foods with higher calorie counts, such as instant noodles and preserved meat.

`protein` - The presence of protein in a dish is somewhat correlated with the calorie count of that dish. The use of dairy products in a dish usually contributes to higher calorie counts than dishes without dairy. 

`saturated_fat` - Saturated fat is a large contributor to the caloric content of a recipe. 

`n_steps` - The number of steps in a recipe could have an impact on the complexity of the dish and therefore on the caloric 
content of the recipe. 

`minutes` - The number of minutes per recipe could be correlated to the total calories of the recipe. Longer cooking times are often linked to more complex dishes than shorter cooking times, which could result in higher calorie counts for recipes that take a longer time to prepare.

We used the nutritional information, such as total fat (```total_fat```), sugar (```sugar```), carbs (```carbs```), sodium (```sodium```), protein (```protein```), saturated_fat (```saturated_fat```), the number of steps (```n_steps```), and the number of minutes (```minutes```) in our recipe dataset.

To fit our model, we performed transformations on the new features introduced to the model. We used StdScaler() to standardize `carbs`, `sodium`, `protein`, `saturated_fat`, `n_steps`. For the `minutes` feature, we used QuantileTransformer to transform the values. In doing these steps, we are able to better fit the model, as it balances the weights of each feature relative to one another.

| Metric            | Test Score                    |
| ------------------| ------------------------------|
| RMSE              | 47.97538413056438            |
| R^2               | 0.9933687791149496           |


**Hyperparameters**:\
For the RandomForestRegressor, there are two hyperparameters that we are interested in: `max_depth` and `n_estimators`. These two hyperparameters are imperative for optimizing each training tree for a more accurate prediction in the end. To find these optimal hyperparameters we performed a GridSearchCV with a cv value of 3 and our parameter grid listed below:

param_grid = {
    'random forest__n_estimators': [50, 100, 200],
    'random forest__max_depth': [None, 10, 20],
}

After the GridSearch, our optimal hyperparameters are:
max_depth = None
N_estimators = 100

| Metric            | Test Score                    |
| ------------------| ------------------------------|
| RMSE              | 39.92424849043112            |
| R^2               | 0.9954077018865699            |

By introducing new features to our model, our ability to predict `calories` greatly improved. The new R^2 value is very close to 1, indicating that almost 100% of the variations in the predictions made by the model are due the features fit by the model. The RMSE of the model also greatly decreased compared to the RMSE found in our baseline model.

Based on the increased R^2 value and the decreased RMSE value, our final model is significantly better at predicting `calories` accurately. 


## Fairness Analysis
We want to see whether our final model demonstrates a difference in performance between recipes with n_step less than or equal to 10 and recipes with n_step greater than 10. We will explore this question with a permutation test, shuffling the n_step groupings to assess the model's accuracy.

Our two groups are recipes that have `n_step` less than or equal to 10 and recipes with `n_step` greater than 10. This is because 10 is the mean n_step in the recipes dataset.

**Null Hypothesis**:
Our model is fair. Its accuracy among recipes with n_step less than or equal to 10 is roughly the same as its accuracy among recipes with n_step greater than 10

**Alternative Hypothesis**:
Our model is not fair. Its accuracy among recipes with n_step less than or equal to 10 is different from its accuracy among recipes with n_step greater than 10.

**Test Statistic**:
In our test statistics, we examine the difference in root mean square errors (RMSE) between recipes in Group X (recipes with `n_step` less than or equal to 10) and Group Y (recipes with `n_step` greater than 10). 

Our chosen significance threshold is 0.01. The observed statistic represents the actual difference in RMSE between Group X and Group Y. To assess our hypothesis, we conduct a permutation test by shuffling the `n_step` column and utilizing our pre-trained model to predict the corresponding calories. Subsequently, we calculate the RMSE differences for the two groups and repeat this process 250 times. The resulting p-value from these 250 simulations is nearly 0. Consequently, we reject the null hypothesis, indicating that the performance of our model in predicting calories for recipes in Group X significantly differs from that in Group Y. This suggests that the model's predictive fairness is compromised concerning recipes with different `n_step` values. This suggests that our model is not fair, and that it is stastically biased towards the recipes with n_step less than or equal to 10, but this does not signify absolute conclusion.
