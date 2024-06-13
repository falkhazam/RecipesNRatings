# RecipesNRatings: Analyzing Recipes' Calories and Nutrients

By: Fahad Alkhazam

## Introduction

### Question: What types of recipes tend to be healthier?


One of leading causes of illness and death is obesity and malnutrition. One of the main reasons for obesity is a caloric surplus. Essentially, what happens is that the body consumes more calories than it burns. This can happen due to several reasons, however, the dopamine release certain food,especially sugar, release can be addictive, causing some to overconsume foods high in calories. This begs the question: *what makes a food 'unhealthy' and is it coorelated with calories*

In this project, I will be looking into two datasets containing the nutritional value of recipes of food, and another which has the ratings of those foods. Each recipe in both datasets holds a unique id, which will be useful when merging them. 

Below are the columns of both datsets:

| Column             | Description                                                                                                                                                                                       |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `'name'`           | Recipe name                                                                                                                                                                                       |
| `'id'`             | Recipe ID                                                                                                                                                                                         |
| `'minutes'`        | Minutes to prepare recipe                                                                                                                                                                         |
| `'contributor_id'` | User ID who submitted this recipe                                                                                                                                                                 |
| `'submitted'`      | Date recipe was submitted                                                                                                                                                                         |
| `'tags'`           | Food.com tags for recipe                                                                                                                                                                          |
| `'nutrition'`      | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'`        | Number of steps in recipe                                                                                                                                                                         |
| `'steps'`          | Text for recipe steps, in order                                                                                                                                                                   |
| `'description'`    | User-provided description                                                                                                                                                                         |
| `'ingredients'`    | Text for recipe ingredients                                                                                                                                                                       |
| `'n_ingredients'`  | Number of ingredients in recipe                                                                                                                                                                   |


| Column        | Description         |
| :------------ | :------------------ |
| `'user_id'`   | User ID             |
| `'recipe_id'` | Recipe ID           |
| `'date'`      | Date of interaction |
| `'rating'`    | Rating given        |
| `'review'`    | Review text         |

---
In particular, since our questions revolves around healthiness and caloric value our columns of include are 'nutrition', and 'tags'. They also include 'minutes' and 'rating' as we wish to study the effects time to cook (minutes) and how 'good' a food tastes (rating).

## Data Cleaning and Exploratory Data Analysis

Before doing any analysis, we had to merge the two datasets. After which, we added a helper function to unpack the nutritional content (calories, proteins, etc) in the nutrition column, and push them into their own columns. We also added a 'is_healthy' column where it check for a tag of 'healthy'. This poses a weakness in which a contributer can post a healthy recipe but not add that tag, and vice versa. However, given that 'healthy' is in and of itself an arbitrary term, for the sake of this analysis, we will treat that as the metric for health.

In addition, we substitued any rating of 0 with np.nan since ratings must be on a 1-5 scale. We averaged them and added them to a seperate column. Finally, we also added 'rating_miss' column where we check if rating is missing. This will come in handy later on.

### Data Cleaning

Let's plot a histogram of calories' counts:

<iframe
  src="assets/raw_calories.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We can see that the majority lie in the 0-5000 calorie mark. However, some data is lying in the upper 40k range, which might have a negative impact on our analyses. Therefore, I set the limit to 2000 calories as this is the limit of what most reasonable recipes have in calories. I simply filtered out values with calories greater than 2000. A reason for why they might have large amounts of calories is that they might not be for one person, or the person posting could have either made a mistake or trolling. This can be seen especially in the 'minutes' column, where a contributer posted a recipe that would take millions of minutes.

Therefore, I filtered out based on these 5 points:
* calories < 2000
* carb PVD <100
* protein PVD <100
* total_fat PVD <100
* minutes < 180

### Univariate

<iframe
  src="assets/univariate/calorie_dist_2000.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We can see that the calories histogram is much cleaner.

<iframe
  src="assets/univariate/minutes_dist_180.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We can see that the minutes histogram is also much cleaner.

<iframe
  src="assets/univariate/avg_rate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We can see that the majority of ratings are 5 stars, a hint for a possible bias.

### Bivariate

<iframe
  src="assets/bivariate/calories_health.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We can see that foods tend to be labelled healthy when they are lower in calories.

<iframe
  src="assets/bivariate/carb_health.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We can see that foods tend to be labelled healthy when they are lower in carbs, but not at the very bottom where no carbs are added.

<iframe
  src="assets/bivariate/protein_health.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


We can see that foods tend to be labelled healthy when they are lower in proteins.

<iframe
  src="assets/bivariate/fat_health.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We can see that foods tend to be labelled healthy when they are lower in fats.

<iframe
  src="assets/bivariate/avg_rat_health.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We can see that rating depends very little on healthiness, though healthier items seem to be a bit more likely to be rated a 4 than a 5 compared to unhealthy items.

### Interesting Aggregates

<iframe
  src="assets/calories_pivot_min.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We decided to plot the mean, max, and min of calories against minutes. We can see that it calories varies greatly the higher the time (minutes) goes. this might be due to the fact that most foods are at the lower end, this means that single values can have a stringer influence at the higher range of time. Assuming there is no coorelation, this would mean that the variation would increase the less the count is.

## Assessment of Missingness

### NMAR

* 'desciption' is NMAR because contributers are likely to not offer a description if it is simple and self explanatory. For example, a recipe for a PB and J sandwich might not need a description due to its simplicity, popularity, and self explanatory. Therefore, they might choose to not include it.

### Missingness Dependancy

We wanted to examine the missingess of the rating column and check if it has any dependancies. So we ran a permutation test with against minutes and calories.

#### Against Minutes:

* Null Hypothesis: The missingness of ratings does not depend on the amount of minutes it takes to make a recipe.

* Alternative Hypothesis: The missingness of ratings does depend on the amount of minutes it takes to make a recipe.

* Test Statistic: Difference in mean ratings between missing and non-missing ratings.

* p-value: 0.112



<iframe
  src="assets/missingness/calories_pivot_min.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We can see the density does not vary that gretaly, however, let us runa  permutation test to be sure.

<iframe
  src="assets/missingness/perm_min.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

From this permutation test, we can see that the observed difference is not really skewed in one direction. Taking a look at the p-value, we can see that it is 0.112, which is greater than 0.05. 

* Conclusion: We fail to reject the null hypothesis since 0.112 > 0.05. Therefore, the missingness of ratings is unlikely to depend on the amount of minutes it takes to make a recipe.


#### Against Calories:

* Null Hypothesis: The missingness of ratings does not depend on the amount of calories in a recipe.

* Alternative Hypothesis: The missingness of ratings does depend on the amount of calories in a recipe.

* Test Statistic: Difference in mean ratings between missing and non-missing ratings.

* p-value: 0.000

<iframe
  src="assets/missingness/cal_miss.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


<iframe
  src="assets/missingness/perm_cal.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

From this permutation test, we can see that the observed difference is skewed in one direction. Taking a look at the p-value, we can see that it is 0.000, which is less than 0.05.

* Conclusion: We reject the null hypothesis since 0.000 < 0.05. Therefore, the missingness of ratings is likely dependant on the amount of calories in a recipe.

## Hypothesis Testing

We wanted to see if calories had any impact on the healthiness of a food. Therefore, we ran a permutation test to see if there was a difference in mean calories between healthy and unhealthy foods. 

* Null Hypothesis: Healthy foods have the same amount of calories as unhealthy food.

* Alternative Hypothesis: Healthier foods have less calories than unhealthy food.

* Test Statistic: The difference in mean between calorie count of heathy foods and unhealthy foods.

* p-value: 0.000

<iframe
  src="assets/4_null/health_cal_perm.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
We can see that our observed difference is very much skewed to the left. This is further confirmed by the p-value of 0.000, which is less than 0.05. 

* Conclusion: We reject the null hypothesis since 0.000 < 0.05. Therefore, healthier foods likely have less calories than unhealthy foods.

## Framing a Prediction Problem

We wanted to see if we could predict the calories of a food based on the nutritional content and healthiness. Therefore, we ran a linear regression model to see if we could predict the calories of a food based on the nutritional content. We already extracted all nutritional values from the nutrition string. We got calories, total fat, sugar, sodium, protein, saturated fat, and carbohydrates. We also added a column for healthiness. Since all (except healthiness) are numerical values. We can simply pass the numericals into a StandardScaler to scale them and healthiness to OneHotEncoder. We then passed them into a pipeline with a LinearRegression model. 

### Baseline Model

For our baseline model, we simply passed in the values of sodium, sugar, and healthiness into a Pipeline with a LinearRegression model. We then calculated the R^2 score and the RMSE.
 
<iframe
  src="assets/6_baseline_model/baseline_model.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

  R^2: 0.017
  RMSE: 194.4

  We can see that our R^2 value is not bad, but not the best. This might be due to the fact that sugar is a contributing factor to calorie count, however, sodium plays very little if any role in the calorie count. Healthiness also can play a factor, but it could be tha it was a pretty weak metric.

  ### Final Model

  For our final model, we passed in all the nutritional values (carbohydrates, protein, total fat, sugar, sodium) and healthiness into a Pipeline with a LinearRegression model. We then calculated the R^2 score and the RMSE.

  <iframe
  src="assets/7_final_model/final_model.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

R^2 = 0.9854
RMSE = 25.86

We can see that our R^2 value is very high, and our RMSE is very low. This is a good sign that our model is very accurate. This is likely due to the fact that all the nutritional values, especially our macronutrients (carbs, protein, fats), are very important in determining the calorie count of a food.

