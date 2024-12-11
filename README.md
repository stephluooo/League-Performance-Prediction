# League-Performance-Prediction
A project for DSC80 at UCSD that explores performance prediction and fairness analysis in competitive gaming data.

# Introduction

As an esports enthusiast and a league player, we want to investigates the competitive dynamics of professional esports leagues for League of Legends, specifically focusing on tier-one leagues, LPL, LCK, LEC, and LCS. The central question of our study is: Which tier-one league delivers the most “action-packed” games based on player performance statistics?

Our dataset comprises 150,180 rows and contains critical columns relevant to player performance and game metrics. These include:

- league: The league in which the player on the team
- position: The role of the player in the game (e.g., top, jungle, mid, bot, support).
- dpm: Damage per minute, a key indicator of a player’s offensive impact.
- kpm: Kills per minute

Understanding the nuances of these metrics not only helps compare the leagues but also offers insights into team dynamics and individual player contributions. This project aims to bridge the gap in data-driven esports analysis by quantifying the “action” level of games using these performance metrics. By doing so, we hope to provide valuable perspectives for both competitive gaming enthusiasts and analysts.


# Data Cleaning and Exploratory Data Analysis

The initial dataset consisted of 150,180 rows and 161 columns, which included detailed match data from various leagues. The data cleaning process involved multiple steps to focus on relevant information and ensure the data’s quality:

1. Filtering Tier-One Leagues: Rows corresponding to the four tier-one leagues, LPL, LCK, LEC, and LCS, were retained using the league column.
2. Excluding Team-Aggregated Data: Rows where the position column was set to team were excluded, as the analysis focuses on individual players.
3. Selecting Relevant Columns: Columns such as gameid, league, team kpm, dpm, url, and side were selected for further analysis. This step narrowed down the data to the most critical information for the project.
4. Aggregating Data: The data was grouped by gameid, and key metrics like team kpm and dpm were summed. The first instance of league, url, and side values within each game group was retained.
5. Renaming Columns: Columns were renamed for clarity, such as changing team kpm to game kpm and dpm to game dpm.

One of the cleaned dataset, now contains only game data and is reduced to a smaller data set of 1,802 rows and five columns. Below is the head of the cleaned dataset:

| league   |   game kpm |   game dpm | url                                         | side   |
|:---------|-----------:|-----------:|:--------------------------------------------|:-------|
| LPL      |     0.8351 |    3099.03 | https://lpl.qq.com/es/stats.shtml?bmid=8401 | Blue   |
| LPL      |     1.2465 |    3942.17 | https://lpl.qq.com/es/stats.shtml?bmid=8401 | Blue   |
| LPL      |     0.6339 |    3251.22 | https://lpl.qq.com/es/stats.shtml?bmid=8402 | Blue   |
| LPL      |     0.502  |    3568.48 | https://lpl.qq.com/es/stats.shtml?bmid=8402 | Blue   |
| LPL      |     0.8526 |    3633.66 | https://lpl.qq.com/es/stats.shtml?bmid=8402 | Blue   |

This process ensured that only meaningful and focused data was included, reducing noise and enhancing the analysis’s relevance.

## Univariate Analysis

### Damage Per Minute (DPM) Distribution

The histogram below shows the distribution of Damage Per Minute (DPM) across all games. It has a Normal distribution like many real world data.

<iframe src="dpm_histogram_uni.html" width="800" height="600" frameborder="0"></iframe>

## Bivariate Analysis

### Damage Per Minute (DPM) Distribution by League

The histogram below shows the distribution of Damage Per Minute (DPM) across games, categorized by leagues (LCK, LPL, LEC, and LCS). It provides a visual representation of how “action-packed” games are in each league. Observing the plot, we see that most games cluster around a DPM of 3000–5000, indicating relatively high engagement. The LPL and LCS leagues show a slight skew towards higher DPM values, suggesting more action packed gameplay compared to other tier-one leagues.

<iframe src="dpm_histogram.html" width="800" height="600" frameborder="0"></iframe>

### Kills Per Minute (KPM) Distribution by League

This histogram depicts the Kills Per Minute (KPM) distribution across games for each league. It illustrates how frequently kills occur during games. Most games cluster around a KPM of 0.5–1.0, with a small number of games exceeding 1.5 KPM. LPL again appears to have a slightly higher proportion of games with elevated KPM values, further emphasizing its high-paced gameplay.

<iframe src="kpm_histogram.html" width="800" height="600" frameborder="0"></iframe>

### Interesting Aggregates

This pivot table is directly relevant to the research question as it compares key metrics of “action-packed” gameplay—Damage Per Minute (DPM) and Kills Per Minute (KPM)—across the tier-one leagues (LCK, LCS, LEC, and LPL). The mean values provide insights into the average intensity of gameplay, while the maximum values highlight the potential for peak activity. Notably, the LPL leads in both mean KPM (0.84) and maximum KPM (1.90), indicating it consistently features the most high-energy and engaging games.

|   ('mean', 'game dpm') |   ('mean', 'game kpm') |   ('max', 'game dpm') |   ('max', 'game kpm') |
|-----------------------:|-----------------------:|----------------------:|----------------------:|
|                3812.21 |               0.700287 |               6683.95 |                1.5611 |
|                3896.39 |               0.733368 |               7352.5  |                1.3486 |
|                4090.73 |               0.804023 |               6818.34 |                1.6043 |
|                4052.46 |               0.839326 |               6840.96 |                1.9029 |


# Assessment of Missingness I

The 'url' column in the dataset is likely **NMAR** because the presence of a URL might depend on factors not captured in the dataset. For example, URLs may only be included for specific games or leagues that have partnerships with certain platforms or a higher level of media coverage. This missingness cannot be explained using other variables in the dataset, such as league or gameid.

On the other hand, the 'url' column could be **MAR** dependent on the 'league' column as certain leagues might have different data collection processes, visibility standards, or reporting practices that affect the availability of URL information. 

To explore whether the missingness of the ‘url’ column is dependent on the ‘league’ column, I performed a permutation test. This statistical test examines whether the observed relationship between the missingness in ‘url’ and the ‘league’ column is statistically significant, comparing the actual data to a distribution generated under the null hypothesis of no dependency. If a strong dependency is found, it would suggest that the missingness in the ‘url’ column is not random but influenced by the league.

This bar graph shows the density of leagues when missing_url is True. It reveals that missing URLs are more prevalent in the LCK and LCS leagues, indicating potential systematic differences in data collection practices across leagues.
<iframe src="true_missing.html" width="800" height="600" frameborder="0"></iframe>

This bar graph shows the density of leagues when missing_url is False. The LPL league dominates this distribution, showing that most non-missing URLs are from the LPL league.
<iframe src="false_missing.html" width="800" height="600" frameborder="0"></iframe>

### Hypothesis:

Null Hypothesis (H₀): The distribution of leagues when the url column is missing is the same as the distribution of leagues when the url column is not missing.\
Alternative Hypothesis (H₁): The distribution of leagues when the url column is missing is different from the distribution of leagues when the url column is not missing.

### Test Statistic:

Test Statistic: Total Variation Distance (TVD)
Significance Level (α): 0.05

A permutation test was chosen to assess the difference between two distributions: the league distribution when the url column is missing versus when it is not missing. The Total Variation Distance (TVD) is an appropriate metric to quantify this difference, as it measures the overall variation between two categorical distributions. The significance level of 0.05 ensures a 5% threshold for determining statistical significance.


This histogram visualizes the empirical distribution of Total Variation Distance (TVD) values generated from the permutation test. The red vertical line represents the observed TVD. The observed TVD lies significantly outside the range of permuted TVD values, suggesting strong evidence against the null hypothesis.
<iframe src="empirical_distribution.html" width="800" height="600" frameborder="0"></iframe>

The permutation test yielded a p-value close to 0, indicating that the observed difference in the distribution of leagues when the url column is missing versus not missing is statistically significant.

This result suggests that the missingness of the url column is not entirely random. Instead, the url column demonstrates a **Missing at Random (MAR)** relationship with the league column. This implies that the likelihood of a url being missing depends on the associated league.

# Assessment of Missingness II

Additionally, the 'url' column could also be **MAR** dependent on the 'side' column due to the structure of the data collection process. Specifically, if only one URL is attached to all rows corresponding to the same game, and the 'Blue' side rows consistently appear before the 'Red' side rows in the dataset for each game, then the 'Blue' side rows are more likely to have a non-missing URL. This systematic ordering could lead to a higher likelihood of missing URLs for the 'Red' side rows, making the missingness in the 'url' column conditionally dependent on the 'side' column.

### Hypothesis:

Null Hypothesis (H₀): The distribution of 'side' (Blue vs. Red) when the 'url' column is missing is the same as when it is not missing.
Alternative Hypothesis (H₁): The distribution of 'side' (Blue vs. Red) differs based on the missingness of the 'url' column.

### Test Statistic:

Test Statistic: Total Variation Distance (TVD)
Significance Level (α): 0.05

The TVD was used as the metric to quantify the difference between the distributions of 'side' (Blue vs. Red) when the 'url' column is missing versus when it is not missing. A permutation test with 1,000 permutations was conducted to calculate the empirical distribution of TVD under the null hypothesis.

This histogram illustrates the empirical distribution of TVD values from the permutation test. The red vertical line represents the observed TVD for the original data. The observed TVD lying significantly outside the range of the permuted TVD values indicates strong evidence against the null hypothesis.

<iframe src="empirical_tvd_side.html" width="800" height="600" frameborder="0"></iframe>
The permutation test yielded an observed TVD of 0.0 and a p-value of 1.0. This result indicates no significant difference in the distribution of 'side' (Blue vs. Red) based on the missingness of the 'url' column. Therefore, we fail to reject the null hypothesis, suggesting that the missingness in the 'url' column is not dependent on the 'side' column.

# Hypothesis Testing

### Null and Alternative Hypotheses

During the Exploratory Data Analysis stage, we found that the LPL League has overall an higher DPM and KPM compared to other top-one tier leagues. This may indicate that LPL considers a more "action-packed" league, so we want to investigate further this observation. 

Null Hypothesis (H₀): There is no difference in the mean game DPM (Damage Per Minute) between the LPL league and other tier-one leagues.

Alternative Hypothesis (H₁): The LPL league has a **higher mean game DPM** compared to other tier-one leagues.

Test Statistic: The observed difference in means for game DPM between the LPL league and other leagues.

Significance Level (α): We use a significance level of 0.05 to evaluate the test.

### Permutation Test

- Procedure: The permutation test was conducted by shuffling the league labels 1,000 times to generate a null distribution of differences in means. The observed difference was compared to this null distribution to calculate the p-value.
- Results: The observed difference in means was 148.29, and the calculated p-value was 0.0. This means that under the null hypothesis, we observed no random permutations that produced a difference in means as extreme as the observed value.

### Conclusion
Since the p-value is less than the significance level of 0.05, we reject the null hypothesis. This result suggests that the LPL league likely has a significantly higher mean game DPM compared to other tier-one leagues. While this conclusion supports the hypothesis that the LPL league’s games are more action-packed, it is important to note that this statistical test does not account for potential confounding variables that may also contribute to the observed differences. Further analysis could provide additional insights into the causes of this difference.

# Framing a Prediction Problem

As different position holds different skills and produce different stats during the game, the prediction problem aims to classify the position of a player (e.g., “top,” “jng,” “mid,” “bot,” or “sup”) based on their in-game performance statistics. This is a **multiclass classification problem**, as the target variable (position) has multiple categories rather than binary outcomes. The goal is to predict the role of a player based on measurable game metrics such as damage dealt, vision score, and gold spent, among others.

### Response Variable

The response variable for this classification task is position. This variable was chosen because it reflects a fundamental aspect of a player’s role in the game and has clear distinctions based on gameplay metrics. For example, certain roles like “mid” tend to have higher damage output, while “sup” roles are more focused on vision-related statistics.

### Features

The features used to predict the response variable include a variety of quantitative metrics that capture in-game player performance. These metrics include:

- Damage-related features: damagetochampions, damageshare, damagetakenperminute
- Vision-related features: wardsplaced, wpm, wardskilled, wcpw, controlwardsbought, visionscore, vspm
- Gold-related features: totalgold, earnedgold, earned gpm, earnedgoldshare, goldspent

These features were selected because they provide a comprehensive representation of a player’s contributions during a game, allowing the model to distinguish between different roles effectively.

### Evaluation Metric

The primary evaluation metric for the model is **accuracy**, chosen because the dataset is relatively balanced across the different player roles，and our goal is the gather the highest proportion of correctly predicted class. While alternative metrics like F1-score could also be useful in cases of class imbalance, accuracy provides a straightforward way to assess the overall performance of the model for this prediction problem.

### Justification

This problem was framed as a classification task due to the categorical nature of the target variable. The use of in-game metrics as features is justified by the strong association between these statistics and player roles. The selection of a **K-Nearest Neighbors (KNN) model** is appropriate for this problem because players occupying the same position are likely to exhibit similar performance patterns, forming distinct clusters in the feature space. The results of this classification task can provide valuable insights into player performance trends and can be utilized in competitive gaming analytics.

# Baseline Model

### Model Description

The baseline model uses the k-Nearest Neighbors (k-NN) algorithm to predict the role (position) of players based on their in-game statistics. The following features are included:

- Quantitative Features (12):
    - damagetochampions: Total damge dealt to champions
    - dpm: Damage per minute
    - damageshare: Percentage of team Damge
    - damagetakenperminute: Damage per minute
    - visionscore: Overall vision score
    - vspm: Vision score per minute
    - totalgold: Total gold earned
    - earnedgold: Earned gold
    - earned gpm: Earned gold per minute
    - earnedgoldshare: Percentage of gold earned
    - goldspent: Dold spent
    - league: The according league
- Ordinal Features (0):
    - None
- Nominal Features (1):
    - position (target column)

The 12 features used in the model are quantitative and span a wide range of in-game aspects. The position column (target) has been stratified during the train-test split to maintain proportional representation of each class. By incorporating these diverse metrics, the model captures a comprehensive view of a player’s playstyle, facilitating distinct clustering based on their position.

### Model Performance

- Accuracy: 60%
- Precision, Recall, F1-Score (Class-Level):
- bot: Precision = 0.49, Recall = 0.62, F1-Score = 0.55
- jng: Precision = 0.69, Recall = 0.79, F1-Score = 0.73
- mid: Precision = 0.40, Recall = 0.41, F1-Score = 0.40
- sup: Precision = 0.95, Recall = 0.92, F1-Score = 0.93
- top: Precision = 0.46, Recall = 0.28, F1-Score = 0.35

### Evaluation of Model Performance

- Strengths:
    - The model performs well for the support (sup) role, achieving an F1-score of 0.93. This may indicate that the features effectively differentiate this role from others.
    - High recall for the jng (jungle) role shows the model’s ability to identify this role in most cases.
- Weaknesses:
    - The model struggles with the mid, bot, and top roles, especially top, which has a low recall of 0.28. This suggests the model frequently misclassifies players in the top role.
    - The overall accuracy of 60% is moderate but leaves room for improvement, indicating that the current feature set and algorithm may not fully capture the distinctions between player roles.

### Conclusion

While the model demonstrates some predictive capability, especially for the sup and jng roles, its performance is uneven across roles. The current accuracy of 60% suggests that the model is a reasonable starting point but not sufficient for robust role prediction. Improvements could be made by:

- Adding more features to better capture role-specific behavior.
- Exploring different algorithms, such as decision trees or ensemble methods.
- Performing feature engineering, such as combining existing features or creating new derived metrics.

This baseline model sets a foundation for further development and comparison with more advanced models.

# Final Model

**KNN Model** is used again in the final model with updated processes. 

### Features

- Added ward metrics features based on the features from baseline model: 
    - wardsplaced: Number of wardd
    - wpm: Wards per minute 
    - wardskilled: Wumber of destroyed enemy
    - wcpm: Wards clear per minute
    - controlwardsbought: Number of control wards bought
- Nominal: position (target column).
- Ordinal: None.

Ward metrics were added to the final model to capture critical aspects of vision control and map management, which vary significantly by role. Features including wardsplaced, wpm, wardskilled, wcpm, and controlwardsbought provide insights into a player’s contribution to team strategy and their role-specific responsibilities. These metrics help distinguish between roles, particularly support and jungler, and enhance the model’s ability to cluster players based on their in-game behavior, leading to more accurate predictions.

### Model Choice

We used a K-Nearest Neighbors (KNN) classifier for its simplicity and interpretability. A pipeline was constructed to standardize the quantitative features using StandardScaler, ensuring the KNN model works efficiently with scaled data.

Hyperparameter Tuning

We used GridSearchCV to optimize hyperparameters, testing:

- Number of neighbors (k): [3, 5, 7, 9, 11]
- Weighting: ['uniform', 'distance']
- Distance metric: ['minkowski', 'manhattan']

The best hyperparameters identified were:

- k = 11
- Weighting: distance
- Metric: minkowski

The grid search evaluated these combinations using 5-fold cross-validation and selected the configuration that maximized accuracy on the training set. These hyperparameters balance model complexity and performance, maximizing accuracy without overfitting. 

### Final Model Performance

- Accuracy: 0.78
- Best performance for sup position (F1: 0.98)
- Lowest performance for mid position (F1: 0.57)
- Moderate performance for bot, jng, and top.

This represents a 18% improvement over the baseline model’s accuracy of 0.60.

### Comparison with Baseline Model

- The baseline model had an accuracy of 0.60, with lower precision and recall for key positions like mid and bot.
- Adding hyperparameter tuning and standardization improved both accuracy and the balance of metrics across classes, particularly reducing the variance in top and bot predictions.

### Analysis

- Strengths: The final model is more accurate for sup and jng positions, reflecting their distinct playstyles and metrics.
- Weaknesses: Predicting mid remains challenging due to its versatility and overlap with other roles in features like damage and gold.

Future work could explore ensemble methods or feature engineering to improve performance further for positions like mid.

# Fairness Analysis

Fairness in machine learning models is crucial to ensure equitable performance across different groups in the dataset. For this analysis, we examine whether the model’s performance varies between players from the LPL league (Group X) and players from non-LPL leagues (Group Y). Using the weighted precision score as our evaluation metric, we aim to assess if the model demonstrates any systematic bias in predicting player positions for these groups. A permutation test is employed to statistically evaluate the observed difference in performance, providing insights into the fairness and reliability of the model across diverse player populations.

### Groups and Evaluation Metric

- Group X: Players belonging to the LPL league (is_lpl = True).
- Group Y: Players belonging to non-LPL leagues (is_lpl = False).
- Evaluation Metric: Weighted precision score for classifying player positions.

### Null and Alternative Hypotheses

- Null Hypothesis (H₀): The model is fair. There is no significant difference in the weighted precision score for predicting positions between Group X (LPL players) and Group Y (non-LPL players). Any observed difference is due to random chance.
- Alternative Hypothesis (H₁): The model is unfair. There is a significant difference in the weighted precision score for predicting positions between Group X (LPL players) and Group Y (non-LPL players).

### Choice of Test Statistic and Significance Level

- Test Statistic: Observed difference in weighted precision scores between Group X and Group Y.
- Significance Level (α): 0.05.

### Permutation Test Results

- Observed Precision Difference: 0.0157.
- p-value: 0.1300.

### Conclusion
Since the p-value (0.1300) is greater than the significance level (α = 0.05), we fail to reject the null hypothesis. This suggests that the observed precision difference between LPL and non-LPL groups is not statistically significant. Therefore, we cannot conclude that the model treats these groups differently in terms of precision for position classification. While there is a slight observed difference, it does not provide sufficient evidence to infer systematic bias.

The weighted precision metric is appropriate for this task as it accounts for class imbalances, ensuring that the evaluation fairly reflects performance across all position categories. The permutation test is a robust choice for assessing the statistical significance of observed differences without making strong parametric assumptions.

<iframe src="hypothesis_test_distribution.html" width="800" height="600" frameborder="0"></iframe>