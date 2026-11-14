# Indian Premier League: data analysis and score prediction

An exploratory analysis of Indian Premier League (IPL) cricket data, and a regression model that predicts the final score of a first innings while it is still being played. Uploaded in July 2021, during my bachelor's studies in computer science. Everything is in the notebook [`IPL.ipynb`](IPL.ipynb).

## Data

The repository includes three CSV files from public IPL datasets on Kaggle:

| File | Content |
| --- | --- |
| `ipl_b_by_b.csv` | Ball-by-ball data of 816 matches, 2008 to 2020 |
| `ipl_match_result.csv` | One row per match for the same 816 matches: date, venue, teams, toss, winner and player of the match |
| `ipl_ml.csv` | The state of each first innings after every ball (runs, wickets, overs, runs and wickets in the last five overs) and its final total, 2008 to 2017 |

## Analysis

The notebook answers four questions with numbers and charts:

1. How many runs do the winning teams score in the powerplay at the start of their innings?
2. How often does the team that wins the toss also win the match?
3. How do the IPL careers of Virat Kohli and Rohit Sharma compare in matches, total runs, runs per innings and player-of-the-match awards?
4. How often does a team still win after conceding more than 50 runs in the death overs, in the first and in the second innings?

## Score prediction

- **Task:** predict the final total of a first innings from its state after any ball, once five overs have been bowled.
- **Features:** batting and bowling team (one-hot encoded), overs, runs, wickets, and runs and wickets in the last five overs. Matches with teams that no longer exist, such as Deccan Chargers and Gujarat Lions, are left out, which leaves eight teams.
- **Model:** ridge regression, with the regularisation strength chosen by 5-fold cross-validation on the training data.
- **Split by season:** trained on 2008 to 2016 (407 innings) and tested on 2017 (30 innings, 2,778 predictions).
- **Result on the 2017 test set:** a mean absolute error of 12.1 runs and a root mean squared error of 15.8 runs.

## Known limitations

- In `ipl_b_by_b.csv`, overs are numbered from 0. The filter `over <= 6` used for question 1 therefore covers the first seven overs, not the six-over powerplay, and the filter `over > 15` used for question 4 covers the last four overs.
- The player comparison in question 3 adds up `total_runs`, which also counts extras such as wides and byes while the player was on strike.
- The prediction errors are averaged over every ball of the test innings, so early predictions, which are harder, and late ones, which are nearly settled, count the same.

## Running it

```bash
pip install pandas numpy matplotlib seaborn scikit-learn notebook
jupyter notebook IPL.ipynb
```
