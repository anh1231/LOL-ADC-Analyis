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

In terms of data cleaning I first converted **_'gamelengths'_** values from seconds to minutes for easier use. I will use all of these features later in further analysis and model building 
'<table border="1" class="dataframe">  <thead>    <tr style="text-align: right;">      <th></th>      <th>gameid</th>      <th>result</th>      <th>gamelength</th>      <th>side</th>      <th>position</th>      <th>kills</th>      <th>deaths</th>      <th>assists</th>      <th>firstbaron</th>      <th>team kpm</th>      <th>damagetochampions</th>      <th>totalgold</th>    </tr>  </thead>  <tbody>    <tr>      <th>0</th>      <td>10660-10660_game_1</td>      <td>0</td>      <td>31</td>      <td>Blue</td>      <td>top</td>      <td>1</td>      <td>3</td>      <td>1</td>      <td>NaN</td>      <td>0.1</td>      <td>7092</td>      <td>11083</td>    </tr>    <tr>      <th>1</th>      <td>10660-10660_game_1</td>      <td>0</td>      <td>31</td>      <td>Blue</td>      <td>jng</td>      <td>0</td>      <td>4</td>      <td>3</td>      <td>NaN</td>      <td>0.1</td>      <td>7361</td>      <td>8636</td>    </tr>    <tr>      <th>2</th>      <td>10660-10660_game_1</td>      <td>0</td>      <td>31</td>      <td>Blue</td>      <td>mid</td>      <td>0</td>      <td>2</td>      <td>0</td>      <td>NaN</td>      <td>0.1</td>      <td>10005</td>      <td>10743</td>    </tr>    <tr>      <th>3</th>      <td>10660-10660_game_1</td>      <td>0</td>      <td>31</td>      <td>Blue</td>      <td>bot</td>      <td>2</td>      <td>4</td>      <td>0</td>      <td>NaN</td>      <td>0.1</td>      <td>10892</td>      <td>12224</td>    </tr>    <tr>      <th>4</th>      <td>10660-10660_game_1</td>      <td>0</td>      <td>31</td>      <td>Blue</td>      <td>sup</td>      <td>0</td>      <td>3</td>      <td>3</td>      <td>NaN</td>      <td>0.1</td>      <td>6451</td>      <td>7221</td>    </tr>  </tbody></table>'
