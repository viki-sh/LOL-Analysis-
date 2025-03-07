# Was it Over Before it Started?

# General Introduction 
Welcome to League of Legends! A video game published and developed by Riot Games, widely considered one of the greatest video games ever made. Furthermore, it is the worlds largest esports, with events consisting of hundreds of millions viewers. This analysis aims to analyze eight years of professional league matches, and answer the question, "Do pre-game choices have an affect on the whether a team wins or not"

In a league of legends game, two teams, consisting of 5 players each compete to destroy the enemys "nexus", a home base. The game is extremely complex, and there are many factors in a game including over 150 possible champions, a "store" to buy items to benefit a players performance, gold, jungle monsters, roles, to state a few. 

However, for the sake of our analysis, we will be focusing on the Pre-Game. Before a game begins, each player chooses a role, a champion, and a ban. The teams are also randomly assigned a side of the map, Red or Blue.

## What are the roles? 
There are 5 roles, Top, Middle, Bot, Support ADC, and Jungle. Each role typically has champions best suited for the role, for example a support champion will have a kit consisting of healing, whereas a top laner will have lots of health. There are currenltly 170 champions in League, each with their own abilities, roles, and counters. 

## What is a counter? 
In League of Legends, a "counter" refers to a champion that has a significant advantage over another champion, meaning their abilities and playstyle are particularly effective at negating the enemy champion's strengths and exploiting their weaknesses, essentially making it much harder for the enemy champion to perform well in the matchup. 

## What is a ban?
In addition to selecting a role and a repective champion, each player can also ban a champion. This can be effective when wanting to ensure a player doesnt want to play against a particular character.

## What are the sides? 
The 'Rift', aka the map a match is played on, is parallel, with three lanes and a mass jungle. The red team plays from the top, and aims to destroy the enemys nexus on the bottom, and vise versa. Typically in the league community, most players prefer to be on the blue side, as playing upwards is easier than playing downward. However, these sides are assignmed at random and there is no in game advantage for either.

## Introduction of dataset 
Our dataset comes from Oracle’s Elixir, encompassing approximately thousands of professional-level esports matches from top-tier leagues such as the LCS, LEC, LCK, and LPL. For our analysis, we chose to only use 2017, 2019, 2020, 2021, 2022, 2024, and 2025, as the data from other years only had 3 bans, and we would like to analyze across all 5. There are 86899 rows in our dataset. 

- year
- gameid
- league
- teamname
- side
- ban1 
- ban2
- ban3
- ban4
- ban5 
- pick1
- pick2
- pick3
- pick4
- pick5
- result
- num_counters_picked
- num_counters_banned 
- PGA 
- higher_PGA
- mean_champ_wr
- mean_team_wr

Additionally, we scraped the following website https://www.counterstats.net/ to create our own dataframe consisting of: 

- Champion
- counters champ
- champ counters

# Data Cleaning and Exploratory Data Analysis

## Column Creation
We added five columns to our cleaned dataframe from OracleElixer

- num_counters_picked
- num_counters_banned 
- PGA 
- higher_PGA
- mean_champ_wr
- mean_team_wr

Note that we dropped games where PGA was equivelent, because for the sake of our question of higher pga = higher likelihood of win, we wanted to compare pga between teams. We are left with xxx rows afterwards. 

Our final dataframe head is as follows: 

## Univariate Analysis 
For our univariate analysis, lets look at the distribution of PGA across all games. Note that 0 means a team did not counter ban nor counter pick, whereas 10 meant they counter picked every single champ. There are no values of PGA over 8, because there is randomness to when picking and banning. The process usually goes as follows :  

- insert univariate 

- Both teams ban 5 champions each. 
- Blue team will pick 1 champion
- Red team will pick 2 champions 
- Blue team will pick 2 champions
- Red team will pick 2 champions
- Blue team will pick 2 champion
- Red team will pick 1 champion

Note that it is done in this sense so that 
- each team has equal likelihood of picking a counter, there isnt a "blue team picks all" and then red team can counter every champion.  As a result, it is very unlikely to have a PGA of 10, as it would insinuate a team choosing to play against their counter, which is not strategic. 

- insert second univariate 

Note that there are more teams with PGA. Meaning, most teams either counter picked the enemy or banned one of their counters. \nThis leads us to believe that players are considering the affect of counter matchups


## Multivariate Analysis 
Lets now see the affect on wins. 
- insert multivariate wins and losses by PGA
It looks like a teams PGA doesnt necessarily mean its more likely to win. However, note that this only shows wins and losses of pga, not necessarily if a higher pga meant winning against their opponent

- insert higher pga one 

This visualization shows a 50/50 split. So, it looks like having a higher PGA doesnt significantly affect your ability to beat your opponent.

## Interesting Aggregates