# Analysis of League of Legends Statistics
#### **Author: Andy Huang**
___


This analysis was done as a comprehensive data science project at UCSD. The contents of this analysis will include methods such as exploratory data analysis, hypothesis testing, and baseline model creation and more.

___
## Introduction

League of Legends (LOL) is a multiplayer online battle arena (MOBA) that was developed by Riot Games. It is a highly popular game that is enjoyed by people of various ages and backgrounds and enjoys a thriving professional scene with real investments and money on the line. The data set that I will be working with is a data set on the professional LOL esport scenes from 2024, collected and aggregated by Oracle's Elixir. This data set contains a variety of features ranging from **_'Kill'_**, **_'Deaths'_**, and **_'Assists'_** (KDA), to **_'monster kills'_**, and much more. 
There are many different queries and tests one could perform on this data but the question that I am proposing today is: 
> _**Between Attack Damage Carries (ADC) and Midlaners, which role tends to "carry" more often than not, and given the role that tends to "carry" the most, is it possible to predict the game's results from their statistics?**_
###### We will define the term "carry" by their KDA ratio (kills + assists/ deaths), damage to champions (characters controlled by players) and gold income.

The reason as to why I am asking this question is due to the fact that LOL is a 5 player team based game but certain positions in the game can affect the outcomes of the game more than others. By showing that a position is more likely to "carry" a game more than others teams can devote more time to practicing strategies that can maximize the benefits of that role and perhaps increase their chances of winning as such. 
I will use the answers of these statistics to fit a predictive model that can predict the results of a game from a position's statistics and this model can be used to enhance a teams strategy, decision-making and overall impact their chances of winning.

___
## Data Cleaning and Exploratory Data Analysis

### Chosen features

In order to clean the data I will be selecting specific features to  work with. The chosen features that I will be working with today are:
- **_'gameid'_** : Unique game IDs
- **_'result'_** : The result of a game (0 = Loss, 1 = Win)
- **_'gamelength'_** : The length of a game in minutes
- **_'side'_** : The side of a particular team (Blue, Red)
- **_'position'_** : The position of a particular player (top, jng, mid, bot, sup, team)
- **_'kills'_** : The total number of kills for a particular player
- **_'deaths'_** : The total number of deaths for a particular player
- **_'assists'_** : The total number of assists for a particular player
- **_'firstbaron'_** : Notes if a team secured first baron or not (0 = No, 1 = Yes)
- **_'team kpm'_** : The average kills per minute of a team
- **_'damagetochampions'_** : The total damage a particular player has done in a game
- **_'totalgold'_** : The total gold a particular player has earned in a game

In terms of data cleaning I first converted **_'gamelengths'_** values from seconds to minutes for easier use. I will use all of these features later in further analysis and model building.
I also separated the data into two separate dataframes, one containing statistics solely for ADCs and the other containing statistics solely for Midlaners.

| gameid             |   result |   gamelength | side   | position   |   kills |   deaths |   assists |   firstbaron |
|:-------------------|---------:|-------------:|:-------|:-----------|--------:|---------:|----------:|-------------:|
| 10660-10660_game_1 |        0 |           31 | Blue   | top        |       1 |        3 |         1 |          nan |
| 10660-10660_game_1 |        0 |           31 | Blue   | jng        |       0 |        4 |         3 |          nan |
| 10660-10660_game_1 |        0 |           31 | Blue   | mid        |       0 |        2 |         0 |          nan |
| 10660-10660_game_1 |        0 |           31 | Blue   | bot        |       2 |        4 |         0 |          nan |
| 10660-10660_game_1 |        0 |           31 | Blue   | sup        |       0 |        3 |         3 |          nan |

###### The first couple of columns and rows of the data frame containing all positions.

| gameid             |   result |   gamelength | side   | position   |   kills |   deaths |   assists |   firstbaron |
|:-------------------|---------:|-------------:|:-------|:-----------|--------:|---------:|----------:|-------------:|
| 10660-10660_game_1 |        0 |           31 | Blue   | bot        |       2 |        4 |         0 |          nan |
| 10660-10660_game_1 |        1 |           31 | Red    | bot        |       7 |        1 |         5 |          nan |
| 10660-10660_game_2 |        0 |           31 | Blue   | bot        |       0 |        2 |         2 |          nan |
| 10660-10660_game_2 |        1 |           31 | Red    | bot        |       4 |        0 |         5 |          nan |
| 10660-10660_game_3 |        1 |           22 | Blue   | bot        |       3 |        0 |         4 |          nan |

###### The first couple of columns and rows of the data frame containing only ADCs.


| gameid             |   result |   gamelength | side   | position   |   kills |   deaths |   assists |   firstbaron |
|:-------------------|---------:|-------------:|:-------|:-----------|--------:|---------:|----------:|-------------:|
| 10660-10660_game_1 |        0 |           31 | Blue   | mid        |       0 |        2 |         0 |          nan |
| 10660-10660_game_1 |        1 |           31 | Red    | mid        |       4 |        0 |         7 |          nan |
| 10660-10660_game_2 |        0 |           31 | Blue   | mid        |       1 |        4 |         2 |          nan |
| 10660-10660_game_2 |        1 |           31 | Red    | mid        |       5 |        1 |         9 |          nan |
| 10660-10660_game_3 |        1 |           22 | Blue   | mid        |       8 |        0 |         8 |          nan |

###### The first couple of columns and rows of the data frame containing only Midlaners.

### Univariate Analysis
In this section I will perform some Univariate Analysis on the feature of **_'gamelength'_**.

<iframe
  src="assets/distr_game_length.html"
  width="700"
  height="600"
  frameborder="0"></iframe>
###### This is a histogram showing the distribution of game lengths throughout the dataset and as we can see the curve is relatively normal thus showing that a majority of games usually last around 30 minutes. Which is an interesting data point to keep in mind as ADCs are usually considered the "scaling" position which basically means that they need time and gold in order to get strong while midlaners usually have more agency in the game being strong from around mid to late game.

### Bivariate Analysis
I have chosen to perform Bivariate Analysis on the two features of **_'totalgold'_** and **_'position'_**. This should give us quite interesting results as it will show which position makes the most gold,
which is often correlated with success and strength in game.

<iframe
  src="assets/avg_gold_income.html"
  width="700"
  height="600"
  frameborder="0"></iframe>
###### As can be seen ADCs and Midlaners have the highest average gold income among the other positions and ADC seems to come out on top, albeit not by a large margin.

#### Interesting Aggregates
As I explored the dataset I have come across some interesting aggregates that I attained by grouping by **_'result'_** and taking the mean of every column.

|   gamelength |   kills |   deaths |   assists |   team kpm |   damagetochampions |   totalgold |
|-------------:|--------:|---------:|----------:|-----------:|--------------------:|------------:|
|      31.5437 | 2.8799  |  3.50163 |   3.61254 |   0.295688 |             18936.8 |     12647.1 |
|      31.5478 | 6.22261 |  1.63021 |   7.62306 |   0.626788 |             23892.4 |     15053.2 |

###### (ADC Aggregates)

|   gamelength |   kills |   deaths |   assists |   team kpm |   damagetochampions |   totalgold |
|-------------:|--------:|---------:|----------:|-----------:|--------------------:|------------:|
|      31.5437 | 2.47692 |  3.52533 |   3.84518 |   0.295688 |             19239.7 |     12225.1 |
|      31.5478 | 4.97976 |  1.71934 |   8.53802 |   0.626788 |             23397   |     14382.5 |

###### (Midlaner Aggregates)

In these 2 dataframes I have the average aggregate statistics for ADCs and Midlaners when they win or lose. As can be seen games in which teams win have wildly higher **_'kills'_**, **_'assists'_**, **_'damagetochampions'_**, and **_'totalgold'_** than compared to games in which teams lose. 
This can be attributed to the fact that these statistics are from professional league games and thus teams that are in the lead can keep expanding their and will make little to no mistakes, ensuring that the game is as one sided as possible.

## Assessment of Missingness

### NMAR Analysis
In our data there are many different features that contain missing values, often denoted by NaN. Many of these features are MAR (Missing at Random) or MD (Missing by Design), however I believe that one of the features called **_pentakill_** is in fact NMAR (Not Missing at Random). 
I believe this is the case because of the fact that in league of legends, a pentakill is achieved when one person manages to kill all 5 members of the other team in a limited amount of time. Due to the difficulty of this achievement, it is not possible to achieve a pentakill in every game and thus the NaN in the data set are almost certainly not dependent on another feature and are in fact missing because that game simply did not have a pentakill occur.
This feature's missingness relies solely on itself and no other data provided would likely be able to explain the missingness and thus I believe that this feature can only be NMAR.

## Missingness Dependency
In this section I tested if the missingness of the column **_'firstbaron'_** is dependent on the columns **_'side'_** and **_'split'_**. The **significance cutoff** I have selected is `0.05` and the **test statistic** that I have chosen is `Total Variation Distance (TVD)`.  I am going to first test if the missingness of **_'firstbaron'_** is dependent on **_'side'_** given these hypotheses.

> **Null Hypothesis**: The distribution of **_'side'_** is the same whether 'firstbaron' is missing or not missing.

> **Alternate Hypothesis**: The distribution of **_'side'_** is NOT the same when 'firstbaron' is missing or not missing.


<iframe
  src="assets/distr_test_side.html"
  width="700"
  height="600"
  frameborder="0"></iframe>   
###### Shown here is the counts of **_'side'_** when **_'firstbaron'_** is missing or not missing.

<iframe
  src="assets/distr_baron_side.html"
  width="700"
  height="600"
  frameborder="0"></iframe>
###### Shown here is the distribution of TVD for **_'side'_**.

Due to the observed statistic being `0` and the P-value being `1.0` we fail to reject the Null Hypothesis and thus **_'firstbaron'_** likely does NOT depend on **_'side'_**.

We have now seen how the counts and distribution of TVDs looks when **_'firstbaron'_** does NOT depend on a feature, in order to show a feature that **_'firstbaron'_** DOES depend on we will 
now test if the missingness of **_'firstbaron'_** is dependent on **_'split'_**. Significance cutoff is `0.05` and the test statistic is `Total Variation Distance (TVD)`.
    
> **Null Hypothesis**: The distribution of **_'split'_** is the same whether **_'firstbaron'_** is missing or not missing.

> **Alternate Hypothesis**: The distribution of **_'split'_** is NOT the same when **_'firstbaron'_** is missing or not missing.
    
<iframe
  src="assets/distr_test_split.html"
  width="700"
  height="600"
  frameborder="0"></iframe>
###### Shown here is the counts of **_'split'_** when **_'firstbaron'_** is missing or not.

<iframe
  src="assets/distr_baron_split.html"
  width="700"
  height="600"
  frameborder="0"></iframe>
###### Shown here is the distribution of TVD for **'split'**.

Due to the observed statistic being `0.28` and the P-value being `0` we reject the Null Hypothesis and thus **_'firstbaron'_** likely DOES depend on **_'split'_**.

## Hypothesis Testing
Now that I have finished with my early analysis of the data, it is time to move on to formulating and testing a hypothesis on our data set.
Before I get into the question I will give a bit of background information first on the term "ADC". 
Due to the nature of the name Attack Damage Carry (ADC), one can be expected to assume that they are in fact, "carrying" their games harder than all the other roles. 

In this section I will try to see if ADCs "carry" harder than Midlaners by performing a hypothesis test on the metric of "carry" that I have defined, that which being KDA ratio (kills + assists / death), damage to champions, and gold income. I believe that these 3 statistics are important when considering how hard a position "carries" because they are 3 statistics that numerically shows how much of an impact that role has had in that game. A high KDA ratio means that role has had high kill participation while having low deaths, a high damage to champion stat shows how much they are contributing in a team fight, and a high gold income shows how far they are ahead of their opponent's economy.

Thus with all 3 of these statistics combined, this will give us a way to numerically show how hard a certain position "carried" a game in relations to other positions.
    
The test statistic is `difference in means` and the **significance cutoff** is `0.05`.
    
> **Null Hypothesis**: ADCs unfortunately do NOT live up to their moniker and midlaners "carry" harder than them according to KDA ratio, damage to champions, and gold income.
    
> **Alternate Hypothesis**: ADCs live up to their moniker and do in fact "carry" harder than midlaners according KDA ratio, damage to champions, and gold income.
    
<iframe
  src="assets/distr_diff_means.html"
  width="700"
  height="600"
  frameborder="0"></iframe>

###### Shown here is the resulting distribution of our permutation tests.

We tested our hypothesis by doing a permutation test on a dataframe that contains all 3 of these statistics combined for both ADC and Midlaners. By doing this we attained a _p-value_ of `0.0154` which is less than our _significance cutoff_ of `0.05` thus we reject the Null Hypothesis in favor for the Alternate Hypothesis. This would suggest that ADCs do in fact "carry" harder than Midlaners which means our inital assumption was correct.

## Framing a Prediction Problem

### Problem Identification
It is a well known fact that ADCs are considered a "scaling" character. This means that they do better the longer the game goes on and the more gold and items they get. 
On paper, gold is super important as they are what allows players to buy items, get stronger, take objectives and ultimately win them the game. We have also shown before that when games are won, ADCs tend to do better on average than most other positions. 
Thus I want to see if it is possible to predict the result of a game given statistics for an ADC.

> That is, given the statistics of an ADC, is it possible to accurately predict if the game was won or lost?

## Baseline Model

I am going to be building a _Binary Classification model_ that uses the metric of _F-1 score_ and _Accuracy_ in order to predict the results of the games. 
The reason as to why I have decided to choose the _F-1 score_ and _Accuracy_ is due to the fact that the _F-1 score_ prioritizes precision and recall, leading to a more balanced model while Accuracy gives us an idea of how accurate our predictions are.

Using both of these metrics will allows us to see how well our model performs while also giving us a check in case there are imbalances to our data that we may not be aware of. 

The features used for prediction will be **_'totalgold'_** and **_'gamelength'_**. These 2 features are available at the end of every game and thus are perfect to be used as predictors for the games result.

To start with we will first use a _Logistic Regression model_ as our baseline classifier and we will use the 2 features of **_'totalgold'_** (Quantitative) and **_'gamelength'_** (Quantitative) both of which are quantitative features and thus we will not need to do any categorical encoding on our features. 
We will first utilize the _StandardScaler Transformer_ in order to normalize our 2 features in order to minimize data bias and make the data all be in the same scale. We then fit our model with these normalized features in order to attain our _F-1 score_ and _Accuracy score_ which are `0.7930682976554536` and `0.7926455566905005` respectively. 

As can be seen here both of our scores are decent, meaning that around 79% of the time we can predict the games outcome based on the length of the game and gold income of an ADC. However I believe that this model can be improved upon and I will attempt to do so in the next section by changing models, increasing features and tuning hyperparameters.

## Final Model
In order to achieve the best final model I have decided to change the model from a _LogisticRegression model_ to a _RandomForestClassifier_ and I will add 3 new features **_'kills'_** (Quantitative) and **_'assists'_** (Quantitative) and **_'deaths'_** (Quantitative).
I decided to change models because I believe that the _LogisticRegression model_ was too simple and in order to improve our predictions I needed to have a model that can have a greater depth and complexity to it and thus I have decided on a _RandomForestClassifier_ model. 
Moving on to the features, the reason why I chose these features is to do with the fact that kills, deaths and assists are a baseline for how well the ADC (or any other role) performed in a game. Having high kills and assists with low deaths means that an ADC has played very well and they have managed to have very high impact in the game while making as few of mistakes as possible.
Vice versa, low kills and assists and high deaths means that they have been unable to attribute much to the game and have had very little impact on the game.
   
These 3 features are quantitative and thus no categorical encoding is required for them, however I do still need to normalize these features by applying the _StandardScaler_ transformation to them before I can fit the model to these features. 
In terms of hyperparameters I chose 2 different hyperparameters to tune, _number of estimators_ and _max depth_. I optimized these two hyperparameters by utilizing the _GridSearchCV_ technique and testing _number of estimators_ from a range of 100 to 300 with steps of 50 and testing _max depth_ from a range of 2 to 12 with steps of 2. 
After doing this I found that the best _number of estimators_ and _max depth_ to be `150` and `10` respectively.
    
Our final model has improved greatly with an _F1 score_ and _Accuracy score_ of `0.88`. This new score is better than our old model's score by 0.11, which translates to our new model being able to accurately predict the results of a game based on an ADC's statistics better by 11%, a significant increase in performance.

<iframe
  src="assets/final_model_bar_plot.html"
  width="700"
  height="600"
  frameborder="0"></iframe>
###### Shown here is a bar plot showing the weights of each feature used to fit the model.

<iframe
  src="assets/final_model_heat_map.html"
  width="700"
  height="600"
  frameborder="0"></iframe>
###### Shown here is a heat map of the _GridSearchCV_ results.

## Fairness Analysis
In this final section I will attempt to perform a fairness analysis on my model with the question of: 

> "Does my model perform worse on _**'gamelengths'**_ of **less than 25** (below average game length) versus _**'gamelengths'**_ of **greater than or equal to 25** (above average game length)? 

We will perform this fairness analyis by performing a permutation test a compared the resulting _F-1 scores_ between the two groups with a _significance threshold_ of `0.05`. Group 1 is our _**'gamelengths'**_ **less than 25** and Group 2 is our _**'gamelengths'**_ **greater than or equal to 25**.
    
> Null Hypothesis: Our model is fair. It's _F-1 score_ for _**'gamelengths'**_ **less than 25** and for _**'gamelengths'**_ **greater than or equal to 25** are roughly the same, and any differences are due to random chance.


> Alternate Hypothesis: Our model is NOT fair. It's _F-1 score_ for _**'gamelengths'**_ **less than 25** is lower than the _F-1 score_ for _**'gamelengths'**_ **greater than or equal to 25**.
    
As we attained a _p-value_ of `0.003` we can see that our model is indeed NOT fair based on the column of _**'gamelengths'**_. 
From a logical standpoint this makes sense as we have made clear before that ADCs require time to "scale" and be strong so in the games that ended quickly ADC were likley unable to contribute as much as they probably should have versus the games that went on for a long time where they probably contributed the amount they should or even over contributed.

<iframe
  src="assets/distr_diff_f1.html"
  width="700"
  height="600"
  frameborder="0"></iframe>
###### Shown here is the distribution of _F1 scores_ in our fairness analysis model.