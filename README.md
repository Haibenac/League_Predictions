# League_Predictions
This is Project 5 for DSC 80. It was authored by Andrew Peng.
All the data was sourced from a public dataset created by Oracle's Elixer found here: https://oracleselixir.com/tools/downloads.

---

## League Of Legends: Do Stats Make the Role?

**Name(s)**: Andrew Peng (apeng@ucsd.edu)

**Website Link**: [Link](https://haibenac.github.io/League_Predictions/) 
**Exploratory Data Analysis Link (also done on this dataset): [Link](https://haibenac.github.io/League-Of-Legends-2023-Analysis/)

---
## Introduction and the Problem

Within the game, League of Legends, the dominant (and the only) strategy is for the 5 players on each team to split into the same 5 roles every time. Those roles are top lane, mid lane, jungle, support, and bottom lane. However, on a sub-average League of Legends player such as myself, I find it difficult to truly understand what the difference between these roles is. Do they have a true difference or are they just fancy names to their titles that are indistinguishable from one another? Can I correctly classify what role a player played given their advanced stats, or are all of these metrics truly indistinguishable? Do players simply play the same game where the only thing that differs is where they play on the map? Thus, **I want to understand if I can predict what role a player was playing given their advanced stats after the game has ended using a multiclass classifier**. As this prediction would be analyzing post-game stats, all information will be known to me apart from my response variable (position) which I will be trying to predict.

To truly have a grasp and understand the patterns and characteristics of each of these roles, I will train a model that takes in advanced metrics of player data using the data gathered by Oracle's Elixer, specifically every professional match from 2023. Because I will be building a **multiclass classification** model that will take in a dataset and predict the role in which the given player played. The suitable metric to use to evaluate my model will be ***Accuracy*** as every single team is always made up of a single support player, a jungle player, a mid-laner, a top-laner, and a bottom-laner. **All the classes are balanced.** Thus, the number of False Negatives and False Positives will eventually add up to be the same value, causing every value to be relatively similar.

Additionally, the ***position*** column will be used as the **response variable** as it is already nicely encoded with each player's role within the game. since this is a multiclass classification model, this column will not be changed.

To make a more rational prediction, I cleaned my dataset by removing large swaths of data that were related more to a team's success in the game rather than the individuals themselves. This will hopefully reduce the amount of variation between the roles. For example, if a team is defeating another team by a large margin, I would expect stats like "gold difference at the 15-minute mark" to be a useless metric in determining the role that the current player is playing. Thus, I focused on features that were more team-orientated.


| side   | position   | teamname      | champion   |   kills |   deaths |   assists |   teamkills |   teamdeaths |   barons |   damageshare |   damagemitigatedperminute |    wpm |   earnedgoldshare |   cspm |   vspm |   total cs |   minionkills |   monsterkills |   gamelength |
|:-------|:-----------|:--------------|:-----------|--------:|---------:|----------:|------------:|-------------:|---------:|--------------:|---------------------------:|-------:|------------------:|-------:|-------:|-----------:|--------------:|---------------:|-------------:|
| Blue   | top        | Klanik Esport | Jax        |       4 |        0 |         6 |          13 |            7 |        0 |     0.150027  |                    878.913 | 0.4594 |         0.295868  | 9.1654 | 1.1256 |        399 |           367 |             32 |         2612 |
| Blue   | jng        | Klanik Esport | Poppy      |       2 |        2 |         4 |          13 |            7 |        1 |     0.0653236 |                   1513.97  | 0.4135 |         0.14464   | 3.6524 | 1.4012 |        159 |            23 |            136 |         2612 |
| Blue   | mid        | Klanik Esport | Taliyah    |       2 |        2 |        11 |          13 |            7 |        0 |     0.283899  |                    323.729 | 0.5283 |         0.225914  | 7.7412 | 1.1256 |        337 |           287 |             50 |         2612 |
| Blue   | bot        | Klanik Esport | Ezreal     |       5 |        1 |         7 |          13 |            7 |        0 |     0.441215  |                    234.372 | 0.3905 |         0.261862  | 8.4992 | 1.0796 |        370 |           345 |             25 |         2612 |
| Blue   | sup        | Klanik Esport | Karma      |       0 |        2 |        10 |          13 |            7 |        0 |     0.0595359 |                    284.15  | 1.1945 |         0.0717161 | 0.4824 | 2.4349 |         21 |            17 |              4 |         2612 |


---
## Baseline Model
### Model Description
This Baseline model will be a **Decision Tree Classifier** used to predict a player's role given their advanced stats post-game. The features that have been selected for this model are ‘kills’, ‘deaths’, ‘assists’, 'champion', and 'vspm' (Vision Score Per Minute).
### Features
#### Quantitative
The quantitative features in this model are Kills, Deaths, Assists, and VSPM (Vision Score Per Minute). Normally, kills, assists, and deaths are an easy way to see a player's performance. These quantitative values were kept raw for simplicity. They will be used in conjunction with the following nominal feature for the baseline Decision Tree Classifier model.
#### Nominal
The nominal feature in this model is Champion. Given that there are over 160 champions in League of Legends, this column will be **One Hot Encoded** using a pipeline.
### Model Performance
The results of this model produced the following stats:

Precision: 0.9227042645126411


Recall: 0.9224848222029488


F1: 0.9225250395700931


Accuracy: 0.9224848222029488

These are the stats run on the test data after the data was split. In general, I believe this model to be a good model overall. With a naive prediction (that being guessing every position as support or ADC), we would've seen an accuracy of exactly 20% because each position will always be one of the five roles required on the team. Thus, an accuracy of 0.92 is incredible in comparison. A large part of this model having high accuracy likely has to do with the Champion One Hot Encoder. Normally, Champions rarely fall outside of a single possible role. For example, Nautilus is almost exclusively used as a support champion While Aatrox is strongest when he is used in the Top Lane. This fact allows for a high initial accuracy. 

However, this model can still be improved. As of right now, none of the current features are truly normalized to account for game length. Additionally, I can fine-tune hyperparameters to strengthen the accuracy that is returned.

---
## Final Model
### Model Description
To more accurately account for statistics within a game without the variable of time getting in the way, I had to first transform a few new features that involved normalizing kills, deaths, assists, minon kills, and monster kills by the game length. In the end, the dataframe for this final step looked like the following dataframe.


| side   | position   | teamname      | champion   |   kills |   deaths |   assists |   teamkills |   teamdeaths |   barons |   damageshare |   damagemitigatedperminute |    wpm |   earnedgoldshare |   cspm |   vspm |   total cs |   minionkills |   monsterkills |   gamelength |         kpt |         dpt |        apt |      minpt |      monpt |
|:-------|:-----------|:--------------|:-----------|--------:|---------:|----------:|------------:|-------------:|---------:|--------------:|---------------------------:|-------:|------------------:|-------:|-------:|-----------:|--------------:|---------------:|-------------:|------------:|------------:|-----------:|-----------:|-----------:|
| Blue   | top        | Klanik Esport | Jax        |       4 |        0 |         6 |          13 |            7 |        0 |     0.150027  |                    878.913 | 0.4594 |         0.295868  | 9.1654 | 1.1256 |        399 |           367 |             32 |         2612 | 0.00153139  | 0           | 0.00229709 | 0.140505   | 0.0122511  |
| Blue   | jng        | Klanik Esport | Poppy      |       2 |        2 |         4 |          13 |            7 |        1 |     0.0653236 |                   1513.97  | 0.4135 |         0.14464   | 3.6524 | 1.4012 |        159 |            23 |            136 |         2612 | 0.000765697 | 0.000765697 | 0.00153139 | 0.00880551 | 0.0520674  |
| Blue   | mid        | Klanik Esport | Taliyah    |       2 |        2 |        11 |          13 |            7 |        0 |     0.283899  |                    323.729 | 0.5283 |         0.225914  | 7.7412 | 1.1256 |        337 |           287 |             50 |         2612 | 0.000765697 | 0.000765697 | 0.00421133 | 0.109877   | 0.0191424  |
| Blue   | bot        | Klanik Esport | Ezreal     |       5 |        1 |         7 |          13 |            7 |        0 |     0.441215  |                    234.372 | 0.3905 |         0.261862  | 8.4992 | 1.0796 |        370 |           345 |             25 |         2612 | 0.00191424  | 0.000382848 | 0.00267994 | 0.132083   | 0.00957121 |
| Blue   | sup        | Klanik Esport | Karma      |       0 |        2 |        10 |          13 |            7 |        0 |     0.0595359 |                    284.15  | 1.1945 |         0.0717161 | 0.4824 | 2.4349 |         21 |            17 |              4 |         2612 | 0           | 0.000765697 | 0.00382848 | 0.00650842 | 0.00153139 |



### Added Features
Apart from the ***One Hot Encoded*** feature and the VSPM feature, every other feature was modified or added. First off, the KILLS, DEATHS, and ASSISTS columns were transformed by the GAMELENGTH column before being added back in as *kpt*, *dpt*, and *apt* (kills per time, deaths per time, and assists per time). This was done to prevent outliers from affecting the data. These three features were then *transformed* by using a **standard scaler** to pick out potential outliers that would help classify roles more accurately. 

In addition to the old features, many new features were added.

- Damageshare: This feature was added because it created a clear break between two main groups: The laners versus everyone else. Theoretically, if everyone played the same, this value would be 20% for all players. However, there is a clear drop off in damage for both junglers and support players as their medians are fair below the 20% mark. **The feature was not encoded and was kept as is.**


<iframe src="assets/dmgbypercent.html" width=800 height=600 frameBorder=0></iframe>


- Minpt: Also known as minion kills per time, this stat was used and created a much larger distinction between the two groups than damageshare did. Here, we see the two groups clearly outlined below. Once again, both junglers and support players have few to no minion kills when it is normalized by time. In comparison, the laners tend to have the lion's share of minion kills. This gives a clear split and helps the model more accurately define laners from other roles. **This feature was encoded by the standard scaler.**

  
<iframe src="assets/minionkills.html" width=800 height=600 frameBorder=0></iframe>


- Monpt: Also known as jungle monster kills per time. This stat clearly let junglers stand out as they were the clear dominant force within this stat. This gives a clear split and helps the model more accurately define junglers. **This feature was encoded by the standard scaler.**


<iframe src="assets/junglemonsterkills.html" width=800 height=600 frameBorder=0></iframe>


- Damagemitigatedperminute: While I thought that support players and top laners would be the main tanks, this was not the case. Instead, while I was correct with my top laner assumption, junglers tended to be the other main tanks of the dataset. This creates another correlation between a feature and a new group consisting of top laners and jungles that helps distinguish the categories again. **This feature was encoded by the standard scaler.**


<iframe src="assets/dmgmiti.html" width=800 height=600 frameBorder=0></iframe>

In short, these features were chosen because they were either individually based (and not impacted by being clearly ahead of the enemy team) or a value that compared the player to the other members of their team. Each of these features splits the classes by giving them features that stand out for their respective role. For example, junglers will likely be easily identified by the Monster Kills by game time statistic. Top laners will likely be identified by the fact that they have high minion kills by time, but also high damage mitigated per minute. **These added features helped separate the classes I am trying to predict**.

### Hyper Parameters
The hyperparameters I want to tune are:
- min samples split
    - A smaller value allows the tree to make more fine-grained splits, to potentially capture more complex relationships between the role and some of the data points. Tuning it helps find the right balance between capturing patterns and avoiding overfitting.
- max depth
    - A deeper tree can capture more complex relationships in the training data. Most of these would likely be to sift through the different one hot encoded champions to find the correct splits to accurately assess what role to assign to the data.
- number of estimators
    - I chose estimators to help find the right balance between computational cost and improved performance, especially for the ensemble effect.

To select the best hyperparameters, I performed a GridSearchCV, which determined that the most optimal outcomes are:

**max_depth = 150**
**min_samples_split = 5**
**n_estimators = 80**

### Model Performance
The model I settled on to be used here was a **Random Forest Classifier** which is better at sifting through data and tuning the hyperparameters to their optimal values than a Decision Tree Classifier. After fitting the model, I compared and found the accuracy that the model had on the test data that had been split earlier. The stats are as follows:

Precision: 0.9704266477493233


Recall: 0.9704032957502168


F1: 0.9704140817600516


Accuracy: 0.9704032957502168

Although the initial model was already very strong, I still managed to improve the model by 5% compared to the baseline model. This final model improved the accuracy from 92% to 97%. Much of this was likely to improve the features that were used within the final model. They helped create more defined splits between the five roles and more accurately predict the role a player played given their post-game advanced statistics. 

---
## Fairness Analysis

While this model is incredibly accurate, I still wonder if some roles are more similar to each other than others. For supports, they are expected to have lower kills per game and higher vision scores. For junglers, find themselves with the most amount of monster kills. However, for the other laners, it becomes increasingly difficult to distinguish the three. ADCs are expected to have the highest damage share, but that may not be accurate if the game does not reach the late-game required for the ADC to power spike. As for the top and mid laners, they are both solo laners that are expected to perform similarly to each other by controlling their lanes, farming minion kills, and generally staying alive. Thus, does this final model perform better for support players and jungle players (who have very defined stats) than it does for the three lane players?

Group 0 will be the junglers and the support players. Group 1 will consist of the three lane players (bot, mid, top).

Null: The model's accuracy is the same for both groups, and any observed variation is due to chance.

Alternative: The model's accuracy is significantly greater and more accurate for the group pertaining to the support player and the jungler player than it is for the three laners.

Test Statistic: The difference in proportions of correct predictions between the two groups (Group 0 - Group 1).

Significance level: 1%

After running a permutation test 1,000 times, I found a p-level of 0.0, meaning that the observed value found is indeed statistically significant. Thus, I can reject the null hypothesis. And look towards the alternative hypothesis. From the sample I used (League of Legends 2023 professional matches), it seems that my multiclass classifier struggles with correctly classifying group 1 in comparison to group 0. Perhaps my initial confusion is, in a way, justified: defining what role a player played given their post-game stats is confusing as some of the roles seem very similar to one another compared to other roles.

<iframe src="assets/tvd.html" width=800 height=600 frameBorder=0></iframe>
