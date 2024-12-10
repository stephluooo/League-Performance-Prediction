# League-Performance-Prediction
A project for DSC80 at UCSD that explores performance prediction and fairness analysis in competitive gaming data.

# Introduction

This project investigates the competitive dynamics of professional esports leagues, specifically focusing on tier-one leagues, namely the LPL, LCK, LEC, and LCS. The central question of our study is: Which tier-one league delivers the most “action-packed” games based on player performance statistics?

Our dataset comprises 18,020 rows and contains critical columns relevant to player performance and game metrics. These include:

- position: The role of the player in the game (e.g., top, jungle, mid, bot, support).
- damagetochampions: Total damage dealt to champions.
- dpm: Damage per minute, a key indicator of a player’s offensive impact.
- damageshare: The percentage of the team’s damage output contributed by the player.
- wardsplaced and controlwardsbought: Indicators of map vision control.
- earnedgold and earnedgoldshare: Metrics for resources acquired during the game.

Understanding the nuances of these metrics not only helps compare the leagues but also offers insights into team dynamics and individual player contributions. This project aims to bridge the gap in data-driven esports analysis by quantifying the “action” level of games using these performance metrics. By doing so, we hope to provide valuable perspectives for both competitive gaming enthusiasts and analysts.


# Data Cleaning and Exploratory Data Analysis

The initial dataset consisted of 150,180 rows and 161 columns, which included detailed match data from various leagues. The data cleaning process involved multiple steps to focus on relevant information and ensure the data’s quality:

1. Filtering Tier-One Leagues: Rows corresponding to the tier-one leagues, LPL, LCK, LEC, and LCS, were retained using the league column.
2.	Excluding Team-Aggregated Data: Rows where the position column was set to team were excluded, as the analysis focuses on individual players.
3.	Selecting Relevant Columns: Columns such as gameid, league, team kpm, dpm, url, and side were selected for further analysis. This step narrowed down the data to the most critical information for the project.
4.	Aggregating Data: The data was grouped by gameid, and key metrics like team kpm and dpm were summed. The first instance of league, url, and side values within each game group was retained.
5.	Renaming Columns: Columns were renamed for clarity, such as changing team kpm to game kpm and dpm to game dpm.

The cleaned dataset, df_tier_one_filtered, now contains 1,802 rows and five columns. Below is the head of the cleaned dataset:

| league   |   game kpm |   game dpm | url                                         | side   |
|:---------|-----------:|-----------:|:--------------------------------------------|:-------|
| LPL      |     0.8351 |    3099.03 | https://lpl.qq.com/es/stats.shtml?bmid=8401 | Blue   |
| LPL      |     1.2465 |    3942.17 | https://lpl.qq.com/es/stats.shtml?bmid=8401 | Blue   |
| LPL      |     0.6339 |    3251.22 | https://lpl.qq.com/es/stats.shtml?bmid=8402 | Blue   |
| LPL      |     0.502  |    3568.48 | https://lpl.qq.com/es/stats.shtml?bmid=8402 | Blue   |
| LPL      |     0.8526 |    3633.66 | https://lpl.qq.com/es/stats.shtml?bmid=8402 | Blue   |

This process ensured that only meaningful and focused data was included, reducing noise and enhancing the analysis’s relevance.

## Univariate Analysis

Damage Per Minute (DPM) Distribution

The histogram below shows the distribution of Damage Per Minute (DPM) across games, categorized by leagues (LCK, LPL, LEC, and LCS). It provides a visual representation of how “action-packed” games are in each league. Observing the plot, we see that most games cluster around a DPM of 3000–5000, indicating relatively high engagement. The LPL and LCS leagues show a slight skew towards higher DPM values, suggesting more action packed gameplay compared to other tier-one leagues.

<iframe src="dpm_histogram.html" width="800" height="600" frameborder="0"></iframe>

Kills Per Minute (KPM) Distribution

This histogram depicts the Kills Per Minute (KPM) distribution across games for each league. It illustrates how frequently kills occur during games. Most games cluster around a KPM of 0.5–1.0, with a small number of games exceeding 1.5 KPM. LPL again appears to have a slightly higher proportion of games with elevated KPM values, further emphasizing its high-paced gameplay.

<iframe src="kpm_histogram.html" width="800" height="600" frameborder="0"></iframe>

Bivariate Analysis: DPM vs. Team KPM by League

The scatter plot below depicts the relationship between Damage per Minute (DPM) and Team Kills per Minute (KPM) across the four tier-one leagues (LPL, LCK, LEC, and LCS). Points are color-coded by league, providing a visual comparison of team performance metrics. The plot reveals a clustering pattern: higher DPM values often correspond to higher KPM values, indicating more aggressive gameplay styles. Among the leagues, LPL exhibits a broader distribution in DPM and KPM, further emphasizing its fast-paced gameplay dynamics compared to others.

<iframe src="scatter_dpm_kpm.html" width="800" height="600" frameborder="0"></iframe>

Pivot Table

This pivot table is directly relevant to the research question as it compares key metrics of “action-packed” gameplay—Damage Per Minute (DPM) and Kills Per Minute (KPM)—across the tier-one leagues (LCK, LCS, LEC, and LPL). The mean values provide insights into the average intensity of gameplay, while the maximum values highlight the potential for peak activity. Notably, the LPL leads in both mean KPM (0.84) and maximum KPM (1.90), indicating it consistently features the most high-energy and engaging games. This analysis supports the hypothesis that the LPL is the most action-packed league.

|   ('mean', 'game dpm') |   ('mean', 'game kpm') |   ('max', 'game dpm') |   ('max', 'game kpm') |
|-----------------------:|-----------------------:|----------------------:|----------------------:|
|                3812.21 |               0.700287 |               6683.95 |                1.5611 |
|                3896.39 |               0.733368 |               7352.5  |                1.3486 |
|                4090.73 |               0.804023 |               6818.34 |                1.6043 |
|                4052.46 |               0.839326 |               6840.96 |                1.9029 |


# Assessment of Missingness

The url column in the dataset is likely NMAR (Not Missing At Random) because the presence of a URL might depend on factors not captured in the dataset. For example, URLs may only be included for specific games or leagues that have partnerships with certain platforms or a higher level of media coverage. This missingness cannot be explained using other variables in the dataset, such as league or gameid.

URL Missingness and League: MAR

The results of the permutation test indicate that the missingness in the url column is significantly associated with the league column. A p-value of 0.0 suggests that the observed dependency is not due to random chance. This dependency supports the assumption that missingness is Missing at Random (MAR) because the likelihood of a missing url depends on the league, a known and observed variable. For instance, some leagues may systematically lack certain data due to differences in data collection practices or standards, which can explain the missingness without additional unknown factors.

URL Missingness and Side: NMAR

In contrast, the permutation test for the side column yields a p-value of 1.0, indicating no evidence of dependency between url missingness and side. This finding implies that the missingness in the url column is not influenced by the side column (e.g., Blue or Red). If the missingness cannot be fully explained by any observed variable, it is considered Not Missing at Random (NMAR). Additional investigation into unobserved factors or processes that could explain the missingness would be required to refine this assumption.


# Hypothesis Testing

Null Hypothesis (H₀): There is no difference in the mean game DPM (Damage Per Minute) between the LPL league and other tier-one leagues.

Alternative Hypothesis (H₁): The LPL league has a higher mean game DPM compared to other tier-one leagues.

Test Statistic: The observed difference in means for game DPM between the LPL league and other leagues. The test statistic is calculated as the mean game DPM for the LPL league minus the mean game DPM for other leagues.

Significance Level (α): We use a significance level of 0.05 to evaluate the test.

Permutation Test:

- Procedure: The permutation test was conducted by shuffling the league labels 1,000 times to generate a null distribution of differences in means. The observed difference was compared to this null distribution to calculate the p-value.
- Results: The observed difference in means was 148.29, and the calculated p-value was 0.0. This means that under the null hypothesis, we observed no random permutations that produced a difference in means as extreme as the observed value.

Conclusion:
Since the p-value is less than the significance level of 0.05, we reject the null hypothesis. This result suggests that the LPL league likely has a significantly higher mean game DPM compared to other tier-one leagues. While this conclusion supports the hypothesis that the LPL league’s games are more action-packed, it is important to note that this statistical test does not account for potential confounding variables that may also contribute to the observed differences. Further analysis could provide additional insights into the causes of this difference.

# Framing a Prediction Problem

# Baseline Model

# Final Model

# Fairness Analysis