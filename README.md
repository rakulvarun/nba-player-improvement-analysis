# What Drives NBA Player Improvement?

### A Multi-Factor Data Analysis

##  Overview

This project analyzes NBA player's performance across multiple seasons to identify the key factors that drive player improvement. While a common assumption is that increased opportunity (such as higher usage) leads to better performance, this analysis evaluates whether efficiency, usage, or age plays the most significant role.

---

##  Problem

What factors actually influence how NBA players improve over time?

---

##  Data

* Source: Kaggle NBA player dataset (multi-season player statistics)
* Includes:

  * Player information (name, age, team)
  * Performance metrics (points, assists, rebounds)
  * Advanced statistics (true shooting percentage, usage rate)
  * Seasonal data across multiple years

---

##  Methodology

### 1. Data Preparation

* Cleaned dataset and removed irrelevant columns
* Sorted data by player and season
* Filtered out low-game seasons to reduce inconsistency

### 2. Season Alignment

* Matched each player's stats with their following season using grouped shifting
* Created “next season” variables to measure year-to-year changes

### 3. Improvement Metric

Developed a custom improvement score combining:

* Scoring (points)
* Playmaking (assists)
* Efficiency (true shooting %)
* Role/opportunity (usage rate)

### 4. Analysis

* Visualized relationships between variables
* Computed correlation coefficients
* Built a linear regression model to evaluate factor importance

---

##  Key Findings

* **Efficiency (TS%) showed the strongest relationship with improvement**

  * Correlation: **0.54**

* **Usage (opportunity) had a moderate effect**

  * Correlation: **0.30**

* **Age had a negative relationship with improvement**

  * Correlation: **-0.28**

* **Efficiency and usage were largely independent**

  * Near-zero correlation between TS% change and usage change

---

##  Conclusion

Player improvement is influenced by multiple factors, but **improvement in efficiency is the strongest indicator of development**. While increased opportunity (usage) contributes to improvement, it's not the primary driver.

These findings suggest that **skill development (how well a player performs) matters more than opportunity alone (how much a player is used).**

---

## 🤖 Model Insights

A linear regression model was used to evaluate the combined effect of variables:

* Age → small negative impact
* Usage change → strong positive contribution
* Efficiency change → strong positive contribution

This confirms that both efficiency and usage matter, but efficiency remains the most consistently predictive factor.

---

##  What I Learned

* How to work with real-world, imperfect datasets
* Techniques for aligning time-series data across entities
* The difference between correlation and causation
* How to extract meaningful insights from statistical models

---

##  Future Improvements

* Incorporate additional advanced metrics (BLK, +/- rating, etc.)
* Expand dataset to include more historical seasons
* Improve predictive modeling techniques
* Analyze differences between player types (rookies vs veterans, roles, etc.)

---

##  Project Structure

* `analysis.ipynb` — main notebook containing full analysis
* `README.md` — project overview and findings
* `data/` — dataset (or link to source)

---

##  Final Note

This project demonstrates how data science can be used to challenge assumptions and uncover deeper insights in sports analytics. Rather than relying on surface-level trends, it highlights the importance of structured analysis, statistical reasoning, and clear communication of results.
