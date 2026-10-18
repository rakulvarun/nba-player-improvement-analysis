# What Is Associated with NBA Player Improvement?

A multi-season data analysis of efficiency, usage, scoring, and age, with a check on how much the weights matter

## Overview

A common assumption is that more opportunity (higher usage) makes a player better. This project builds a year-over-year improvement score from NBA player statistics and asks which factors are associated with it. It then re-runs the analysis with a different weighting to test whether the first answer holds up. **It did not hold up, and that is the main finding** (see Results).

**Tools:** Python, pandas, Matplotlib, scikit-learn (Google Colab / Jupyter Notebook)

## Question

Which factors are most strongly associated with how much an NBA player's performance changes from one season to the next, and do the conclusions depend on how "improvement" is defined?

## Data

- **Source:** Kaggle NBA player dataset (`all_seasons.csv`): [add link to the exact dataset]
- **Coverage:** 1996-97 to [last season in the file] (one row per player per season, 22 columns)
- **Sample:** 10,293 player-season pairs in the first run; [N] after the games filter in the second run
- **Fields used:** player name, age, points, assists, true shooting percentage (TS%), usage rate (USG%), games played, season

The dataset is not included in this repository. Download it from the link above and upload `all_seasons.csv` when the notebook asks for it.

## Methodology

1. **Preparation:** Dropped the CSV index column and sorted by player and season.
2. **Season alignment:** Matched each season with the same player's next recorded season using a grouped shift, and dropped seasons with no following season.
3. **Run 1: hand-weighted score.** `improvement = Δpoints + 0.5 × Δassists + 10 × ΔTS% + 0.3 × ΔUSG%`. The weights were chosen by hand, with extra weight on efficiency. No games filter was applied.
4. **Run 2: equal-weight score.** Standardized (z-scored) the four changes and added them with equal weight. Kept only player-seasons with at least 20 games played in both the current and next season.
5. **Analysis:** Computed correlations between each factor and the score in both runs, and fit a linear regression in Run 1.

## Results

Correlation of each factor with the improvement score:

| Factor | Run 1: hand-weighted, no games filter | Run 2: equal-weight, 20+ games |
|---|---|---|
| Change in points | not computed | **0.90** |
| Change in assists | not computed | **0.69** |
| Change in usage | **0.30** | **0.62** |
| Change in TS% | **0.54** | **0.54** |
| Age | **-0.28** | **-0.30** |

- **The ranking changed with the weights.** In Run 1, TS% change looked strongest and usage looked weak. In Run 2, change in points is by far the strongest, usage roughly doubles to 0.62, and TS% change is the weakest of the four components. The first conclusion ("efficiency matters most") came from the weights, not from the data.
- **Age is the stable result.** Age is not part of the score, and its correlation was about -0.3 in both runs: older players tended to have lower improvement scores.
- **Efficiency and usage changes were nearly independent** in Run 1 (correlation 0.02).
- **Run 1 regression** (improvement score on age, usage change, and TS% change; R² ≈ 0.42): standardized coefficients were about 0.52 (TS% change), 0.27 (usage change), and -0.21 (age). This regression was not repeated for Run 2.

## Conclusion

This analysis does **not** show that efficiency matters more than opportunity. The factor that looked strongest depended on how the score was weighted. Because every component is part of the score, the correlations mostly reflect how the score was built, so Run 2 also does not show that scoring change "matters most." The one result that held across both versions is that older players tended to improve less. A real answer needs an outcome that is not made from the same statistics (see Future Improvements).

## Limitations

- **Circular measurement.** The factors compared (changes in points, assists, usage, and TS%) are the ingredients of the score, so their correlations with it are partly built in. This is why the results change when the weights change.
- **Arbitrary choices.** The weights and the 20-game cutoff are judgment calls and were not tested across other values.
- **Correlation, not causation.** A player might gain efficiency because he improved, or because of a better role, teammates, or coaching. This analysis cannot separate those.
- **Seasons are matched by player name only.** The code takes each player's next recorded season without checking that it is the next calendar season, and two players with the same name would be mixed together.
- **Selection effects.** Players with no following season (cut, retired, or left the league) are excluded, as are low-games seasons in Run 2.

## Future Improvements

- Test against an outcome that is not made from the score's inputs, such as next-season minutes, win shares, or box plus/minus
- Repeat Run 2 with other games cutoffs (for example 10 and 40) and other weightings
- Require consecutive seasons and match players by ID instead of name
- Control for minutes, position, and team context

## Files

- `NBAanalysis.ipynb`: full analysis notebook
- `README.md`: project overview and findings
