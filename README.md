# Analysis of League of Legends ADC and Midlane Statistics
#### **Author: Andy Huang**
___


This analysis was done as a comprehensive data science project at UCSD. The contents of this analysis will include methods such as exploratory data analysis, hypothesis testing, and baseline model creation. 

___
## Introduction

League of Legends (LOL) is a multiplayer online battle arena (MOBA) that was developed by Riot Games. It is a highly popular game that is enjoyed by people of various ages and backgrounds and enjoys a thriving professional scene with real investments and money on the line. The data set that I will be working with is a data set on the professional LOL esport scenes from 2024, collected and aggregated by Oracle's Elixir. This data set contains a variety of features ranging from 'Kill', 'Deaths', and 'Assists' (KDA), to monster kills, and much more. 
The question that I am proposing today is: 
> _**Between Attack Damage Carries (ADC) and Midlaners, which role tends to "carry" more often than not?**_

###### [We will define the term "carry" by their KDA ratio (kills + assists/ deaths), damage to champions (characters controlled by players) and gold income.]

The reason as to why I am asking this question is due to the fact that LOL is a 5 player team based game but certain positions in the game can affect the outcomes of the game more than others. By showing that a position is more likely to "carry" a game more than others teams can devote more time to practicing strategies that can maximize the benefits of that role and perhaps increase their chances of winning as such. 
I will use the answers of these statistics to fit a predictive model that can predict the results of a game from a position's statistics and this model can be used to enhance a teams strategy, decision-making and overall impact their chances of winning.

### Chosen features
The chosen features that I will be working with today 
