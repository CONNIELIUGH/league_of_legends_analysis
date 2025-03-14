
## Introduction

This is a project from UC San Diego's Halıcıoğlu Data Science Institute's class, DSC80. 
Authors: Hannah Gonzalez (hagonzalez@ucsd.edu) and Connie Liu (col024@ucsd.edu)

#### Overview of the Game, the Dataset, and our Goal

League of Legends is a Multiplayer Online Battle Arena Game (MOBA) by Riot Games. Each game consists of two teams of five that battle out using abilities and items to destroy the enemy's base to win. This project will look into a dataset of the League of Legends game. Specifically, this dataset looks at the 2025 competitive matches data. 

Looking at the dataset, we were particularly interested in the position's columns and will be taking a deeper look at what the position column looks like and see the possible factors that can be taken into consideration to predicting the position column.

Ultimately, we will use columns to determine a player's position (Top, Jungle, Mid, Bot, or Support) because positions are not assigned or chosen randomly but can be determined based on a player's skill level, which may be reflected in other statistics within the dataset. We aim to verify this assumption by analyzing various player stats to predict their position.

#### Introduction to Columns

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

Since each position will have a different number of kills throughout the game - because some positions naturally have more opportunities to secure kills than others, and typically, bot lane ("bot") and mid laners ("mid") tend to have more kills than other positions. We would like to observe if this trend holds true. This will be done by showing the distribution of kills for the five positions, where each position is represented by a different color, and the histograms are stacked to show how kills are distributed across the positions.

Ex (change this)
<iframe src="assets/10-80-enrollment.html" width=800 height=600 frameBorder=0></iframe>

##### Bivariate Analysis

Furthermore, continuing to understand if there is some significance between the kills or damage done by a position, we created bivariate boxplots between each position and their damage per minute (DPM). This graph shows us that what we predicted was correct, "mid" and "bot" positions tended to have a higher distribution of DPM.

<iframe src="assets/dpm_by_position.html" width="100%" height="500px" frameborder="0"></iframe>

---

## Assessment of Missingness


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


---

## Framing a Prediction Problem
---

## Baseline Model

---

## Final Model
---

Fairness Analysis
---

