# League_Predictions
This is Project 5 for DSC 80. It was authored by Andrew Peng.
All the data was sourced from a public dataset created by Oracle's Elixer found here: https://oracleselixir.com/tools/downloads.

---

## League Of Legends: Do Stats Make the Role?

**Name(s)**: Andrew Peng

**Website Link**: https://haibenac.github.io/League_Predictions/
---
### Introduction and the Problem

Within the game, League of Legends, the dominant (and the only) strategy is for the 5 players on each team to split into the same 5 roles every time. Those roles are top lane, mid lane, jungle, support, and bottom lane. However, on a sub-average League of Legends player such as myself, I find it difficult to truly understand what the difference between these roles is. Do they have a true difference or are they just fancy names to their titles that are indistinguishable from one another? Can I correctly classify what role a player played given their advanced stats, or are all of these metrics truly indistinguishable? Do players simply play the same game where the only thing that differs is where they play on the map? 

To truly have a grasp and understand the patterns and characteristics of each of these roles, I will train a model that takes in advanced metrics of player data using the data gathered by Oracle's Elixer, specifically every professional match from 2023. Because I will be building a *multiclass classification* model that will take in a dataset and predict the role in which the given player played. The suitable metric to use to evaluate my model will be *Accuracy* as every single team is always made up of a single support player, a jungle player, a mid-laner, a top-laner, and a bottom-laner. 

To make a more rational prediction, I cleaned my dataset by removing large swaths of data that were related more to a team's success in the game rather than the individuals themselves. This will hopefully reduce the amount of variation between the roles. For example, if a team is defeating another team by a large margin, I would expect stats like "gold difference at the 15-minute mark" to be a useless metric in determining the role that the current player is playing. Thus, I focused on features that were more team-orientated.

| side   | position   | teamname      | champion   |   kills |   deaths |   assists |   teamkills |   teamdeaths |   barons |   damageshare |   damagemitigatedperminute |    wpm |   earnedgoldshare |   cspm |   vspm |   total cs |   minionkills |   monsterkills |   gamelength |
|:-------|:-----------|:--------------|:-----------|--------:|---------:|----------:|------------:|-------------:|---------:|--------------:|---------------------------:|-------:|------------------:|-------:|-------:|-----------:|--------------:|---------------:|-------------:|
| Blue   | top        | Klanik Esport | Jax        |       4 |        0 |         6 |          13 |            7 |        0 |     0.150027  |                    878.913 | 0.4594 |         0.295868  | 9.1654 | 1.1256 |        399 |           367 |             32 |         2612 |
| Blue   | jng        | Klanik Esport | Poppy      |       2 |        2 |         4 |          13 |            7 |        1 |     0.0653236 |                   1513.97  | 0.4135 |         0.14464   | 3.6524 | 1.4012 |        159 |            23 |            136 |         2612 |
| Blue   | mid        | Klanik Esport | Taliyah    |       2 |        2 |        11 |          13 |            7 |        0 |     0.283899  |                    323.729 | 0.5283 |         0.225914  | 7.7412 | 1.1256 |        337 |           287 |             50 |         2612 |
| Blue   | bot        | Klanik Esport | Ezreal     |       5 |        1 |         7 |          13 |            7 |        0 |     0.441215  |                    234.372 | 0.3905 |         0.261862  | 8.4992 | 1.0796 |        370 |           345 |             25 |         2612 |
| Blue   | sup        | Klanik Esport | Karma      |       0 |        2 |        10 |          13 |            7 |        0 |     0.0595359 |                    284.15  | 1.1945 |         0.0717161 | 0.4824 | 2.4349 |         21 |            17 |              4 |         2612 |

## Baseline Model
### Model Description
### Features
#### Quantitative
#### Ordinal
#### Nominal
### Encodings
### Model Performance

## Final Model
### Added Features
### Model
