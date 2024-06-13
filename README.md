# Analysis of League of Legends Statistics
#### **Author: Andy Huang**
___


This analysis was done as a comprehensive data science project at UCSD. The contents of this analysis will include methods such as exploratory data analysis, hypothesis testing, and baseline model creation and more.

___
## Introduction

League of Legends (LOL) is a multiplayer online battle arena (MOBA) that was developed by Riot Games. It is a highly popular game that is enjoyed by people of various ages and backgrounds and enjoys a thriving professional scene with real investments and money on the line. The data set that I will be working with is a data set on the professional LOL esport scenes from 2024, collected and aggregated by Oracle's Elixir. This data set contains a variety of features ranging from 'Kill', 'Deaths', and 'Assists' (KDA), to monster kills, and much more. 
There are many different queries and tests one could perform on this data but the question that I am proposing today is: 
> _**Between Attack Damage Carries (ADC) and Midlaners, which role tends to "carry" more often than not?**_

###### [We will define the term "carry" by their KDA ratio (kills + assists/ deaths), damage to champions (characters controlled by players) and gold income.]

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
  width="800"
  height="600"
  frameborder="0"></iframe>

###### This is a histogram showing the distribution of game lengths throughout the dataset and as we can see the curve is relatively normal thus showing that a majority of games usually last around 30 minutes. Which is an interesting data point to keep in mind as ADCs are usually considered the "scaling" position which basically means that they need time and gold in order to get strong while midlaners usually have more agency in the game being strong from around mid to late game.

### Bivariate Analysis
I have chosen to perform Bivariate Analysis on the two features of **_totalgold_** and **_position_**. This should give us quite interesting results as it will show which position makes the most gold,
which is often correlated with success and strength in game.

<iframe
  src="assets/avg_gold_income.html"
  width="800"
  height="600"
  frameborder="0"></iframe>

###### As can be seen ADCs and Midlaners have the highest average gold income among the other positions and ADC seems to come out on top, albeit not by a large margin.

#### Interesting Aggregates
As I explored the dataset I have come across some interesting aggregates that I attained by grouping by **_result_** and taking the mean of every column.

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

In these 2 dataframes I have the average aggregate statistics for ADCs and Midlaners when they win or lose. As can be seen games in which teams win have wildly higher kills, assists, damagetochampions, and totalgold than compared to games in which teams lose. This can be attributed to the fact that these statistics are from proffessional league games and thus teams that are in the lead can keep expanding their and will make little to no mistakes, ensuring that the game is as one sided as possible.

## Assessment of Missingness

### NMAR Analysis
In our data there are many different features that contain missing values, often denoted by NaN. Many of these features are MAR (Missing at Random) or MD (Missing by Design), however I believe that one of the features called **_pentakill_** is in fact NMAR (Not Missing at Random). I believe this is the case because of the fact that in league of legends, a pentakill is achieved when one person manages to kill all 5 members of the other team in a limited amount of time. Due to the difficulty of this achievement, it is not possible to achieve a pentakill in every game and thus the NaN in the data set are almost certainly not dependent on another feature and are in fact missing because that game simply did not have a pentakill occur.
This feature's missingness relies solely on itself and no other data provided would likely be able to explain the missingness and thus I believe that this feature can only be NMAR.

## Missingness Dependency
In this section I tested if the missingness of the column **_'firstbaron'_** is dependent on the columns **_'side'_** and **_'split'_**. The **significance cutoff** I have selected is 0.05 and the **test statistic** that I have chosen is Total Variation Distance (TVD).  I am going to first test if the missingness of 'firstbaron' is dependent on 'side' given these hypotheses.
> **Null Hypothesis**: The distribution of **_'side'_** is the same whether 'firstbaron' is missing or not missing.

> **Alternate Hypothesis**: The distribution of **_'side'_** is NOT the same when 'firstbaron' is missing or not missing.


<iframe
  src="assets/distr_test_side.html"
  width="800"
  height="600"
  frameborder="0"></iframe>   

###### Shown here is the counts of **_side_** when **_firstbaron_** is missing or not missing.

<iframe
  src="assets/distr_baron_side.html"
  width="800"
  height="600"
  frameborder="0"></iframe>

###### Shown here is the distribution of TVD for **_side_**.

Due to the observed statistic being 0 and the P-value being 1.0 we fail to reject the Null Hypothesis and thus **_'firstbaron'_** likely does NOT depend on **_'side'_**.

We have now seen how the counts and distribution of TVDs looks when **_firstbaron_** does NOT depend on a feature, in order to show a feature that **_firstbaron_** DOES depend on we will 
now test if the missingness of **_'firstbaron'_** is dependent on **_'split'_**. Significance cutoff is 0.05 and the test statistic is Total Variation Distance (TVD).
    
> **Null Hypothesis**: The distribution of **_'split'_** is the same whether **_'firstbaron'_** is missing or not missing.

> **Alternate Hypothesis**: The distribution of **_'split'_** is NOT the same when **_'firstbaron'_** is missing or not missing.
    
<iframe
  src="assets/distr_test_split.html"
  width="800"
  height="600"
  frameborder="0"></iframe>

###### Shown here is the counts of **_split_** when **_firstbaron_** is missing or not.

<iframe
  src="assets/distr_baron_split.html"
  width="800"
  height="600"
  frameborder="0"></iframe>

###### Shown here is the distribution of of TVD for **split**.

Due to the observed statistic being 0.28 and the P-value being 0 we reject the Null Hypothesis and thus **_'firstbaron'_** likely DOES depend on **_'split'_**.

## Hypothesis Testing
