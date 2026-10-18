# What Is Associated with NBA Player Improvement?

A multi-season data analysis of efficiency, usage, and age

## Overview

A common assumption is that more opportunity (higher usage) makes a player better. This project builds a year-over-year improvement score from NBA player statistics, then compares how change in efficiency, change in usage, and age relate to that score.

**Tools:** Python, pandas, Matplotlib, scikit-learn (Google Colab / Jupyter Notebook)

## Question

Which factors are most strongly associated with how much an NBA player's performance changes from one season to the next?

## Data

- **Source:** Kaggle NBA player dataset (`all_seasons.csv`): [add link to the exact dataset]
- **Coverage:** 1996-97 to [last season in the file] (one row per player per season, 22 columns)
- **Size after processing:** 10,293 player-season pairs (each season matched to the same player's following season)
- **Fields used:** player name, age, points, assists, true shooting percentage (TS%), usage rate (USG%), games played, season

The dataset is not included in this repository. Download it from the link above and upload `all_seasons.csv` when the notebook asks for it.

## Methodology

1. **Data preparation:** Dropped the CSV index column and sorted the data by player and season.
2. **Season alignment:** For each player, matched every season with the player's next recorded season using a grouped shift, and dropped seasons with no following season (a player's last season).
3. **Improvement score:** Combined year-over-year changes in four measures:

   `improvement = Δpoints + 0.5 × Δassists + 10 × ΔTS% + 0.3 × ΔUSG%`

   The weights were chosen by hand, with extra weight on efficiency.
4. **Analysis:** Plotted age and usage change against improvement, computed correlations, and fit a linear regression predicting the improvement score from age, usage change, and TS% change.

## Results

| Factor | Correlation with improvement score |
|---|---|
| Change in TS% | **0.54** |
| Change in usage | **0.30** |
| Age | **-0.28** |

- Change in TS% had the strongest correlation with the improvement score.
- Change in usage had a moderate positive correlation.
- Older players tended to have lower improvement scores.
- Change in TS% and change in usage were almost uncorrelated with each other (0.02), so they are not just measuring the same thing.

**Regression** (predicting the improvement score from age, usage change, and TS% change; R² ≈ 0.42): standardized coefficients were about **0.52** for TS% change, **0.27** for usage change, and **-0.21** for age. Standardized coefficients are used because the raw coefficients (age -0.20, usage change 28.7, TS% change 21.1) are in different units and cannot be compared directly.

## Conclusion

Under this scoring method, changes in efficiency were most strongly associated with the improvement score, followed by changes in usage. Age was the only factor not built into the score, and older players tended to score lower. These results describe how the score behaves; they do not show that efficiency matters more than opportunity for real player development. See the limitations below, especially the first one.

## Limitations

- **The score favors efficiency by design.** TS% change was given a weight of 10 and is part of the score, so a strong correlation between TS% change and the score is partly built in. Usage change is also a component (weight 0.3). The regression has the same problem, and its R² is high mostly because the predictors are ingredients of the score.
- **Points and assists changes were not analyzed.** Change in points is likely the largest part of the score and was not compared with the other factors.
- **Correlation, not causation.** A player might gain efficiency because he improved, or because of a better role, teammates, or coaching. This analysis cannot separate those.
- **Seasons are matched by player name only.** The code takes each player's next recorded season without checking that it is the next calendar season (a gap year would be missed), and two players with the same name would be mixed together.
- **No minimum-games filter.** Games played was loaded but not used to filter, so low-playing-time seasons (including seasons with 0.0 points per game) are included and add noise.
- **Selection effects.** Players with no following season (cut, retired, or left the league) are excluded.

## Future Improvements

- Rebuild the score with standardized components and equal weights, and check whether the conclusions change
- Compare change in points and assists against the other factors
- Test the findings against an outcome that is not part of the score, such as next-season minutes, win shares, or box plus/minus
- Require consecutive seasons and a minimum number of games played
- Control for minutes, position, and team context

## Files

- `NBAanalysis.ipynb`: full analysis notebook
- `README.md`: project overview and findings
