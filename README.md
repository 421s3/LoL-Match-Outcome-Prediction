# LoL Match Outcome Prediction

Logistic regression on 110,521 ranked League of Legends matches: which objectives predict wins, and does that change by rank or game length? Built in Python (main analysis), with the original analysis in R.

Author: Xayden Kamei ([@421s3](https://github.com/421s3)).

## Read this first: what the 97.6% accuracy means

The model predicts the winner from **end-of-game** statistics (the difference in kills, towers, dragons, Barons and Heralds between the two teams). Those statistics largely decide the winner, so the high accuracy mostly confirms the result from the final score. It is **not** a forecast of a game in progress. A model that works from early-game statistics (the 10- and 15-minute marks) would be the real prediction problem and is listed as future work.

## Results

| Question | Finding |
|---|---|
| Does blue side have a win-rate advantage? | Not a meaningful one. Blue wins 47.5% to 50.0% of games from Iron to Diamond, and 50.9% to 53.1% from Master up (Challenger has only 433 games). |
| Which objectives predict the winner? | Tower difference is strongest (correlation 0.87 with the outcome), then kills (0.84), dragons (0.65), Barons (0.52) and Heralds (0.34). Game duration is not significant (p = 0.73). |
| How accurate is the model? | 97.63% on a held-out 20% test set (22,105 games); sensitivity 97.81%, specificity 97.46%. Fit on all games it scores 97.60%, nearly identical, so no sign of overfitting. |
| Does objective importance change by rank? | Yes. The tower coefficient rises from 0.72 (Iron) to 1.53 (Challenger). Challenger's small sample makes the top-rank estimates noisy. |
| Does it change with game length? | Yes. Baron goes from irrelevant in early games (0.00) to the second-strongest objective in mid games (0.84, behind towers at 0.96). Kills matter most early. Accuracy drops to 90.9% in late games. |

## Method

- **Data:** ranked Summoner's Rift matches (`QueueType == CLASSIC`, Iron to Challenger) lasting at least 15 minutes, to drop remakes. Three tables are joined: matches, team statistics and rank names.
- **Features:** blue minus red differences in kills, dragons, towers, Barons and Heralds, plus game duration.
- **Validation:** VIF for multicollinearity (values up to 3.7 with a design-matrix VIF; 1.0 to 1.1 with the coefficient-covariance VIF used by R's `car::vif`), an 80/20 train/test split (`random_state=42`), and an unpenalized statsmodels fit for coefficients and p-values.
- **Extensions:** separate models per rank (10 models) and per game-length segment (early 15-25 min, mid 25-35, late 35+).

## Limitations

- End-of-game predictors, as described above.
- Coefficients are per unit and the objective differences are on different scales, so comparing coefficient sizes across objectives is only a rough guide.
- Each rank and segment model uses a single train/test split, so small differences are noise. Challenger (433 games) is especially noisy.

## Repository contents

```
LoL_Match_Outcome_Prediction.ipynb   Main analysis (Python), with outputs
LoL_Match_Outcome_Prediction.pdf     PDF export of the notebook
requirements.txt                     Python dependencies
data/README.md                       Where to get the data (not included)
r_original/                          Original analysis in R
    LoL_Match_Analysis.Rmd
    LoL_Match_Analysis.pdf
```

## How to run

1. Download the data (see [`data/README.md`](data/README.md)) into a `data/` folder.
2. `pip install -r requirements.txt`
3. Open `LoL_Match_Outcome_Prediction.ipynb` and run all cells. It runs top to bottom and reproduces the numbers above.

For the R version, knit `r_original/LoL_Match_Analysis.Rmd` (needs `tidyverse`, `corrplot` and `car`). Its accuracy (97.6%) is measured on the same games the model was fit to, with no train/test split.

## Data

The data is **not included** in this repository. It is Nathan Smallcalder's Kaggle dataset, collected from the Riot Games API. See [`data/README.md`](data/README.md) for the source, the files used and their terms.

> This project is not endorsed by Riot Games and does not reflect the views or opinions of Riot Games or anyone officially involved in producing or managing Riot Games properties. Riot Games and League of Legends are trademarks or registered trademarks of Riot Games, Inc.

## License

Code and write-up are released under the MIT License (see `LICENSE`). The license does not cover the dataset.

---
*Verified: the notebook was re-run end to end on Python 3.13 (pandas 3.0, scikit-learn 1.9, statsmodels 0.15) and reproduces every number above.*
