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
- **Number of Rows:** 86899
- **Key Columns:**
- `year`: Records the year a game was played  
- `gameid`: A unique identifier for each game. Shared by two teams
- `league` : Specifies the league tournament in which the match took place. There are 110 unique leagues in our dataset. 
- `teamname` : The name of the team competing. There are 1479 unique teams in our dataset
- `side` : The side distinguishes the 'red' and 'blue' teams 
- `ban1` : The first ban of the respective team, in the form of a champion's name
- `ban2`: The second ban of the respective team, in the form of a champion's name
- `ban3`: The third ban of the respective team, in the form of a champion's name
- `ban4`: The fourth ban of the respective team, in the form of a champion's name
- `ban5` : The fifth ban of the respective team, in the form of a champion's name
- `pick1`: The first pick of the respective team, in the form of a champion's name
- `pick2`: The first pick of the respective team, in the form of a champion's name
- `pick3`: The first pick of the respective team, in the form of a champion's name
- `pick4`: The first pick of the respective team, in the form of a champion's name
- `pick5`: The first pick of the respective team, in the form of a champion's name
- `result`: The outcome of a match. 1 indicates the team won, 0 indicates the team lost.
- `num_counters_picked` : The number champion picks that counter any of the opposing team picks, between 1-5. (Equivelent to choosing your opponent's weakness)
- `num_counters_banned` : The number champion bans that counter any of the respective team picks, between 1-5. (Equivelent to removing your own weakness)
- `PGA` : "Pre-game Advantage", The sum of `num_counters_picked` and `num_counters_banned`, between 1-10. 
- `higher_PGA` : Indication of which team has a higher `PGA`. 1 if yes, 0 if no. There are no ties, as those rows have been dropped from the dataset
- `mean_champ_wr`: The mean win rate of all picks in a team
- `mean_team_wr`: The mean win rate of the team playing 

Additionally, we scraped the following website https://www.counterstats.net/ to create our own dataframe consisting of: 

- `Champion` : Name of champion
- `counters champ` : The top 5 champions that counter `Champion`

# Data Cleaning and Exploratory Data Analysis

## Data Cleaning 
Our dataset comes from Oracle’s Elixir, encompassing thousands of professional-level esports matches from top-tier leagues such as the LCS, LEC, LCK, and LPL. For our analysis, we chose to only use 2017, 2019, 2020, 2021, 2022, 2024, and 2025, as the data from other years only had 3 bans, and we would like to analyze across all 5.

We dropped columns that referenced the in-process game statistics, such as kills, gold, and game objectives, since we are interested in the pre-game. 

Additionally, we dropped all rows that had the same PGA for both teams. This way, we could accurately gauge if having a **higher** PGA increased a teams ability to win their match. 

We added five columns to this dataset : 

- `num_counters_picked` : Referencing our webscraped data containing champions and their top five counters, we observed each opponents picks, and incremented by 1 if any of the teams picks were in the opponents counters. 
- `num_counters_banned` : Referencing our webscraped data containing champions and their top five counters, we observed each teams picks, and incremented by 1 if any of the teams bans were a counter to a pick. 
- `PGA` : This amount was obtained by adding `num_counters_picked` and `num_counters_banned`, and is an indicator of how strong the overall pre-game choices are for a team
- `higher_PGA` : This value was obtained to be able to indicate which team had a stronger pre-game advantage, and was used to see the relationship between winning and having a higher pga than your opponent. 
- `mean_champ_wr` : Using our cleaned dataframe, we obtained a wr for each champ by calculating the number of winning games a champion was played in, divided by the total number of games the champion was played. Then, each pick's respective win rate was added and divided by 5 to obtain the mean. 
- `mean_team_wr` : Using our cleaned dataframe, we obtained a wr for each team by calculating the number of winning games a team has, divided by the total number of games the team played. 

Our head of our final dataframe is as follows: 

|   year | gameid    | league   | teamname            | side   | ban1    | ban2     | ban3     | ban4       | ban5    | pick1   | pick2   | pick3      | pick4   | pick5      |   num_counters_picked |   num_counters_banned |   PGA |   higher_PGA |   mean_champ_wr |   mean_team_wr |   result |
|-------:|:----------|:---------|:--------------------|:-------|:--------|:---------|:---------|:-----------|:--------|:--------|:--------|:-----------|:--------|:-----------|----------------------:|----------------------:|------:|-------------:|----------------:|---------------:|---------:|
|   2017 | 1506-1540 | LPL      | I May               | Blue   | Syndra  | Malzahar | Ashe     | Karma      | Poppy   | Maokai  | Kha'Zix | Cassiopeia | Varus   | Tahm Kench |                     0 |                     2 |     2 |            1 |        0.508666 |       0.438776 |        1 |
|   2017 | 1506-1540 | LPL      | Royal Never Give Up | Red    | Camille | Rengar   | Zyra     | Elise      | Rek'Sai | Kled    | Lee Sin | Ryze       | Caitlyn | Nautilus   |                     0 |                     0 |     0 |            0 |        0.493086 |       0.598582 |        0 |
|   2017 | 1506-1541 | LPL      | I May               | Blue   | Syndra  | Malzahar | Ashe     | Rek'Sai    | Kha'Zix | Maokai  | Lee Sin | Corki      | Caitlyn | Thresh     |                     0 |                     0 |     0 |            0 |        0.510833 |       0.438776 |        1 |
|   2017 | 1506-1541 | LPL      | Royal Never Give Up | Red    | Rengar  | Camille  | Varus    | Cassiopeia | Orianna | Trundle | Rumble  | Ryze       | Jhin    | Zyra       |                     1 |                     0 |     1 |            1 |        0.492558 |       0.598582 |        0 |
|   2017 | 1507-1544 | LPL      | Invictus Gaming     | Blue   | Jayce   | Elise    | Malzahar | Kha'Zix    | Lee Sin | Singed  | Rengar  | LeBlanc    | Varus   | Tahm Kench |                     0 |                     1 |     1 |            0 |        0.49238  |       0.523126 |        1 |

## Univariate Analysis 
For our univariate analysis, lets look at the distribution of PGA across all games. Note that 0 means a team did not counter ban nor counter pick, whereas 10 meant they counter picked every single champ. 


<iframe
  src="C:\LOL-Analysis-\uni.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

There are no values of PGA over 8, because there is randomness to when picking and banning. The process usually goes as follows :  

- Both teams ban 5 champions each

- **Blue** team will pick 1 champion
- **Red** team will pick 2 champions 
- **Blue** team will pick 2 champions
- **Red** team will pick 2 champions
- **Blue** team will pick 2 champion
- **Red** team will pick 1 champion

This process ensures each team has equal likelihood of picking a counter; there isnt a "blue team picks all" and then the red team can counter every champion.  As a result, it is very unlikely to have a PGA of 10, as it would insinuate a team choosing to play against their counter, which is not strategic. 

<iframe
  src="uni2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


It seems that that there are more teams with PGA of at least 1, than none at all. Meaning, most teams either counter picked the enemy or banned one of their counters. This leads us to believe that players are considering the affect of counter matchups


## Multivariate Analysis 
Lets now see the affect on wins. 



It looks like a teams PGA doesnt necessarily mean its more likely to win. However, note that this only shows wins and losses of pga, not necessarily if a higher pga meant winning against their opponent. Lets see that here :


This visualization shows a 50/50 split. So, it looks like having a higher PGA doesnt significantly affect your ability to beat your opponent.

## Interesting Aggregates