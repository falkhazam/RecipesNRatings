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





<iframe
  src="assets/file-name.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
