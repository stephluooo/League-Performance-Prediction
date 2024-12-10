## League-Performance-Prediction
A project for DSC80 at UCSD that explores performance prediction and fairness analysis in competitive gaming data.

## Introduction

This project investigates the competitive dynamics of professional esports leagues, specifically focusing on tier-one leagues, namely the LPL, LCK, LEC, and LCS. The central question of our study is: Which tier-one league delivers the most “action-packed” games based on player performance statistics?

Our dataset comprises 18,020 rows and contains critical columns relevant to player performance and game metrics. These include:

- position: The role of the player in the game (e.g., top, jungle, mid, bot, support).
- damagetochampions: Total damage dealt to champions.
- dpm: Damage per minute, a key indicator of a player’s offensive impact.
- damageshare: The percentage of the team’s damage output contributed by the player.
- wardsplaced and controlwardsbought: Indicators of map vision control.
- earnedgold and earnedgoldshare: Metrics for resources acquired during the game.

Understanding the nuances of these metrics not only helps compare the leagues but also offers insights into team dynamics and individual player contributions. This project aims to bridge the gap in data-driven esports analysis by quantifying the “action” level of games using these performance metrics. By doing so, we hope to provide valuable perspectives for both competitive gaming enthusiasts and analysts.


## Data Cleaning and Exploratory Data Analysis

The initial dataset consisted of 150,180 rows and 161 columns, which included detailed match data from various leagues. The data cleaning process involved multiple steps to focus on relevant information and ensure the data’s quality:

1. Filtering Tier-One Leagues: Rows corresponding to the tier-one leagues, LPL, LCK, LEC, and LCS, were retained using the league column.
2.	Excluding Team-Aggregated Data: Rows where the position column was set to team were excluded, as the analysis focuses on individual players.
3.	Selecting Relevant Columns: Columns such as gameid, league, team kpm, dpm, url, and side were selected for further analysis. This step narrowed down the data to the most critical information for the project.
4.	Aggregating Data: The data was grouped by gameid, and key metrics like team kpm and dpm were summed. The first instance of league, url, and side values within each game group was retained.
5.	Renaming Columns: Columns were renamed for clarity, such as changing team kpm to game kpm and dpm to game dpm.

The cleaned dataset, df_tier_one_filtered, now contains 1,802 rows and five columns. Below is the head of the cleaned dataset:

<iframe src="assets/tier_one_df.html" width="100%" height="400" frameborder="0"></iframe>

This process ensured that only meaningful and focused data was included, reducing noise and enhancing the analysis’s relevance.

## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis