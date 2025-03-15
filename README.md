
## Introduction

This is a project from UC San Diego's Halıcıoğlu Data Science Institute's class, DSC80. 
Authors: Hannah Gonzalez (hagonzalez@ucsd.edu) and Connie Liu (col024@ucsd.edu)

#### Overview of the Game, the Dataset, and our Goal

League of Legends is a Multiplayer Online Battle Arena Game (MOBA) by Riot Games. Each game consists of two teams of five that battle out using abilities and items to destroy the enemy's base to win. This project will look into a dataset of the League of Legends game. Specifically, this dataset looks at the 2025 competitive matches data. 

Looking at the dataset, we were particularly interested in the position's columns and will be taking a deeper look at what the position column looks like and see the possible factors that can be taken into consideration to predicting the position column.

Ultimately, we will use columns to determine a player's position (Top, Jungle, Mid, Bot, or Support) because positions are not assigned or chosen randomly but can be determined based on a player's skill level, which may be reflected in other statistics within the dataset. We aim to verify this assumption by analyzing various player stats to predict their position.

#### Introduction to the Columns

We obtained the most recent 2025 match dataset from the website Oracle's Elixir, which contains 19,692 rows and 161 columns. The 2025 LoL Esports Match Data provides detailed information about team statistics as well as individual player statistics.

We picked some key columns from the dataset that are essential for our analysis:
1. position – The role or lane that a player occupies in the game (e.g., Top, Jungle, Mid, ADC, Support).
2. kills – The number of enemy champions a player in a specific position eliminated during a game.
3. deaths – The number of times a player in a given position was defeated by an opponent.
4. assists – The number of times a player contributed to an enemy champion's elimination without landing the final blow.
5. doublekills – The number of times a player secured two kills in quick succession.
6. triplekills – The number of times a player secured three kills in quick succession.
7. quadrakills – The number of times a player secured four kills in quick succession.
8. pentakills – The number of times a player secured five kills in quick succession, achieving a "Pentakill."
9. firstblood – A binary indicator (1 or 0) representing whether the player secured the first kill of the game.
10. barons – The number of Baron Nashor objectives secured by the player's team.
11. opp_barons – The number of Baron Nashor objectives secured by the opposing team.
12. inhibitors – The number of enemy inhibitors destroyed by the player's team.
13. opp_inhibitors – The number of inhibitors destroyed by the opposing team.
14. dpm (Damage Per Minute) – The average amount of damage a player deals to enemies per minute, reflecting sustained damage output.
15. wpm (Wards Placed per Minute) – The number of vision or control wards placed by a player per minute.
16. wcpm (Wards Cleared per Minute) – The number of enemy wards removed by the player per minute.
17. vspm (Vision Score per Minute) – A measure of a player's contribution to vision control, based on wards placed, wards cleared, and other vision-related actions.
18. earned gpm (Earned Gold Per Minute) – The amount of in-game gold a player earns per minute through kills, objectives, farming, and other sources.
19. total cs (Total Creep Score) – The total number of minions and neutral monsters a player has slain in the game.
20. minionkills – The number of lane minions a player has killed during a match.
21. monsterkills – The number of neutral monsters (jungle camps) a player has slain in the game.
22. cspm (Creep Score Per Minute) – The average number of minions and jungle monsters a player kills per minute.

---

## Cleaning and EDA

#### Data Cleaning

We first extracted only the key columns from the original datasets. We then combined the "barons" and "opp_barons" columns, as well as the "inhibitors" and "opp_inhibitors" columns, by adding them together. Since our goal is to predict each player's position, we wanted to consider whether individual players contributed to taking down Barons or inhibitors, rather than analyzing it from a team perspective.

Additionally, we merged the "doublekills," "triplekills," "quadrakills," and "pentakills" columns into a single metric, as they all measure a player's ability to secure consecutive kills. Combining them helps reduce redundancy while maintaining accuracy.

Lastly, we removed rows representing team statistics (rows where the "position" column has the value "team"), as our predictions focus solely on individual players rather than entire teams.

Take a look at the head of the cleaned DataFrame:
<iframe src="assets/index.html" width=800 height=600 frameBorder=0></iframe>

#### Exploratory Data Analysis

##### Univariate Analysis

Before looking into the cross reference between the position column and other columns, let's look at the position column distribution overall. Below is a graph of the distribution of the counts per position. As you can see, the distribution between each position is the same. This makes sense because while it's not required to have only one of each position, it's extremely beneficial for lane balance and team synergy. Furthermore, since this data comes from competitive matches, it's more likely that they would sit to this typical type of structure.

<iframe src="assets/10-80-enrollment.html" width=800 height=600 frameBorder=0></iframe>


In a different sense, we should also consider the kill's column distribution. This will allow us to see what is considered the average and how each position may reflect differently based on the typical kill distribution. Below is the graph of the kill distribution.


<iframe src="assets/10-80-enrollment.html" width=800 height=600 frameBorder=0></iframe>



##### Bivariate Analysis

Since each position will have a different number of kills throughout the game - because some positions naturally have more opportunities to secure kills than others, and typically, bot lane ("bot") and mid laners ("mid") tend to have more kills than other positions. We would like to observe if this trend holds true. This will be done by showing the distribution of kills for the five positions, where each position is represented by a different color, and the histograms are stacked to show how kills are distributed across the positions.

Ex (change this)
<iframe src="assets/10-80-enrollment.html" width=800 height=600 frameBorder=0></iframe>


Furthermore, continuing to understand if there is some significance between the kills or damage done by a position, we created bivariate boxplots between each position and their damage per minute (DPM). This graph shows us that what we predicted was correct, "mid" and "bot" positions tended to have a higher distribution of DPM.

<iframe src="assets/dpm_by_position.html" width="100%" height="500px" frameborder="0"></iframe>

##### Interesting Aggregates

We grouped the data by the "position" column. As seen in the table above, each position has different statistics for each attribute (column), further supporting that the columns we selected for predicting a player's position are valid, as they vary across different positions.

---

## Assessment of Missingness

##### NMAR Analysis
In our dataset, the columns "ban1", "ban2", "ban3", "ban4", and "ban5" are classified as NMAR (Not Missing At Random) because each of these columns has a different number of missing values, and the missing values appear in different rows for each column. There is no evidence to suggest that the missingness in these columns is dependent on other columns in the dataset. The reasoning behind the missing values is tied to the missingness itself, as in the actual League of Legends game, players independently decide whether or not to ban a champion.

An additional column we would like to include is one that indicates the likelihood of a player banning another champion, with 1 representing "yes" and 0 representing "no." If the value in this new column is 0, then the five "ban" columns ("ban1", "ban2", "ban3", "ban4", "ban5") should likely be missing.

##### Missing Dependency
We would like to examine if the missingness of "killsat25" columns depends on the columns "league". The significance level we choose for this permutation test is 0.5.

Null Hypothesis: Distribution of "league" when "killsat25" is missing is the same as distribution of "league" when "killsat25" isn't missing. 

Alternative Hypothesis: Distribution of "league" when "killsat25" isn't missing is NOT the same as distribution of "league" when "killsat25" is missing. 

<iframe src="assets/10-80-enrollment.html" width=800 height=600 frameBorder=0></iframe>

Since our league is a categorical vlaue, we will use TVD as test statistics to compare categorical distributions between when killsat25 is missing and when killsat25 isn't missing. We obtained a p value of 0 thus we reject the null hypothesis. 

Ex table (change)
```py
print(counts[['Quarter', 'Count']].head().to_markdown(index=False))
```

| Quarter     |   Count |
|:------------|--------:|
| Fall 2020   |       3 |
| Winter 2021 |       2 |
| Spring 2021 |       6 |
| Summer 2021 |       4 |
| Fall 2021   |      55 |

---

## Hypothesis Testing

Since the goal of this project is to predict each player's position, the statistics for each role (e.g., kills, deaths, DPM, etc.) differ from one another. Through our research, we found that the "bot" and "mid" positions tend to have higher kill counts than other roles. In our hypothesis test, we aim to specifically analyze the "bot" position and determine whether it has a higher average kill count compared to the average kill count of all positions. Our test statistic will be the TVD (Total Variation Distance), as we are comparing two distributions, which could reflect potential differences in the distribution of kills. We will choose a significance level of 5% and calculate the p-value.

Null Hypothesis (H0): The distribution of kills for the "bot" position is the same as the overall kills distribution across all positions.

Alternative Hypothesis (H1): The distribution of kills for the "bot" position is different from the overall kills distribution.

<iframe src="assets/10-80-enrollment.html" width=800 height=600 frameBorder=0></iframe>

After performing our hypothesis testing, we found a p value of 0, thus we reject the null hypothesis in favor of the alternative hypothesis that the distribution of kills for the "bot" position is different from the overall kills distribution.

---

## Framing a Prediction Problem

From our previous hypothesis testing section, we found that each position has a distinct statistical distribution. This suggests that by analyzing different performance metrics, we can identify the position each player played during the game. To tackle this problem, we can utilize machine learning techniques, particularly mutlticlass classification algorithms, to predict a player's role based on key features from the dataset. By selecting relevant statistical attributes such as kills, deaths, damage per minute (dpms), and other performance indicators, we aim to build an accurate model for position prediction. 

We will use accuracy score to evaluate our models since the primary goal is simply to determine whether the predicted position for each player is correct. In this case, we are interested in the overall percentage of correct predictions rather than balancing precision and recall, which is what the F1 score emphasizes. Given that the task is a straightforward classification of positions, accuracy provides a clear and direct measure of how well the model is performing in terms of correct predictions.

To prevent overfitting, we will split the data into 80% for training and 20% for testing, and evaluate the model using the accuracy score.

---

## Baseline Model

For Baseline Model, we decide to use a Random Forest Classifier, the features we are going to use (selected from the key columns extratcetd from the original dataframes) are: 
- quantitative: "kills", "deaths", "assists", "total cs", "dpm", "wpm", "wcpm", "vspm", "earned gpm", "cspm"
- nominal: "firstblood"

We have decided to encode four columns — 'kills', 'deaths', 'assists', and 'total cs' — using the StandardScaler. The reason for this is that each game in our dataset can have a different length, which in turn affects the values in these columns. For example, longer games might naturally result in higher counts for kills, deaths, assists, and total CS, making these raw values harder to compare across games. Applying the StandardScaler ensures that the values are on a consistent scale, which can improve the performance and accuracy of our model by allowing it to better interpret the relationships between these features and the target variable, regardless of the varying game lengths.

<iframe src="assets/10-80-enrollment.html" width=800 height=600 frameBorder=0></iframe>

After training our model with the training dataset, we evaluated it on the testing data and achieved an accuracy score of 73.5%. We believe this score is relatively high and accurate enough, as we have already incorporated a decent number of features. However, there is still room for improvement by including more relevant features and removing those that do not vary across positions.

---

## Final Model

In our final model, we added two additional features: 'monsterkills', 'minionkills', and 'total_kills' (a column that combines 'doublekills', 'triplekills', 'quadrakills', and 'pentakills'). We decided to include these features because we believe that different positions have varying amounts of monster kills and minion kills, as well as differing abilities to perform consecutive kills. For example, the jungle position typically has higher monster and minion kills due to stronger damage abilities, which allow them to kill more monsters and farm gold and experience more efficiently than other positions. Therefore, we believe that adding these three features will enhance our predictions.
We also decided to remove the columns 'total_cs' and 'wpm' because each position tends to have similar statistics for these two features. In fact, removing these columns did not affect the model's performance at all.

We tuned the parameters of our Random Forest Classifier using GridSearchCV. The two parameters we focused on were max_depth and n_estimators. We tested various combinations of values for max_depth, ranging from 3 to 200 in steps of 10, and for n_estimators, ranging from 10 to 200 in steps of 10. The best hyperparameters we found were a max_depth of 173 and n_estimators of 190. With these parameters, the accuracy score on the test data increased to 77.3%, showing a 4% improvement compared to our baseline model. We believe this is a significant accomplishment, especially considering the randomness involved in player position choices, which could be influenced by personal preference and chance.

<iframe src="assets/10-80-enrollment.html" width=800 height=600 frameBorder=0></iframe>

---

## Fairness Analysis

For our Fairness Analysis, we are going to assess parity of our predictive final model developed above is fair among different groups. Our question is: "does our model achieve accuracy parity predicting positions when kills are less than or equal to 10 and when kills are greater than 10?". To answer this question, we performed a permutation test and examined the difference in accuracy between the two groups. 

Group A represents players with kills less than or equal to 10 and Group B represents players with kills greater than 10. Our evaluation metric is accuracy score and we will use the significance level of 5%.

Null Hypothesis: Our model is fair. Its accuracy for Group A and Group B are roughly the same, and any differences are due to random chance.

Alternative Hypothesis: Our model is unfair. Its accuracy for Group A is higher than for Group B.

Test statistics: Difference between Group B accuracy and Group A accuracy. (Group B accuracy - Group A accuracy)

<iframe src="assets/10-80-enrollment.html" width=800 height=600 frameBorder=0></iframe>

---

