# Does ADC Gold Lead Determine Victory? A League of Legends Data Analysis

**Authors:** Kyle Zhao, Philip Chen
**Course:** DSC 80 - The Practice and Application of Data Science, UCSD

---

## Introduction

League of Legends (LoL) is a team-based multiplayer online battle arena (MOBA) game where two teams of five players compete to destroy the opposing team's nexus. Each player assumes one of five roles: Top lane, Jungle, Mid lane, Bot lane (ADC - Attack Damage Carry), and Support.

This project analyzes professional League of Legends esports match data from 2022, sourced from Oracle's Elixir, containing detailed statistics from over 10,000 competitive matches. Our analysis focuses on understanding how early-game advantages, particularly for the ADC role, impact match outcomes.

### Research Question

**Does having an ADC (Bot lane) with a gold lead at 15 minutes significantly impact the likelihood of winning the match?**

This question is important because:
1. The ADC role is considered a "carry" position that scales with gold
2. The 15-minute mark is a key game state checkpoint in professional play
3. Understanding early-game advantages can inform strategic decisions

### Dataset Description

The dataset contains **approximately 150,000 rows** (12 rows per game: 10 player rows + 2 team summary rows).

**Key columns for our analysis:**

| Column | Description |
|--------|-------------|
| `gameid` | Unique identifier for each match |
| `result` | Binary outcome (1 = win, 0 = loss) |
| `position` | Player's role (top, jng, mid, bot, sup) |
| `kills`, `deaths`, `assists` | Combat statistics |
| `golddiffat15` | Gold difference at 15 minutes |
| `xpdiffat15` | Experience difference at 15 minutes |
| `csdiffat15` | Creep score difference at 15 minutes |
| `damagetochampions` | Total damage dealt to enemy champions |
| `monsterkills` | Neutral objectives killed |
| `minionkills` | Minions killed (CS - creep score) |

---

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning
Our data cleaning process involved several key steps tied to the data generating process:

1. **Selected relevant columns** – Focused on gameplay metrics needed for our questions to reduce noise.
2. **Filtered for data completeness** – Removed rows marked incomplete and games ending before 15 minutes; early surrenders don’t generate 15-minute stats, and including them would bias toward short games.
3. **Separated player and team data** – Team rows have position `team`; player rows are positions. This prevents aggregating team summaries with individual stats.
4. **Handled missing values** – Kept only rows with 15-minute data for analyses that require it, so metrics are comparable across games.


Here's the head of our cleaned dataset:

| gameid | position | side | result | kills | deaths | assists | golddiffat15 |
|--------|----------|------|--------|-------|--------|---------|--------------|
| ESPORTSTMNT01_2690210 | top | Blue | 0 | 2 | 3 | 2 | -1240 |
| ESPORTSTMNT01_2690210 | jng | Blue | 0 | 2 | 5 | 6 | 321 |
| ESPORTSTMNT01_2690210 | mid | Blue | 0 | 2 | 2 | 3 | -543 |
| ESPORTSTMNT01_2690210 | bot | Blue | 0 | 2 | 4 | 2 | 892 |
| ESPORTSTMNT01_2690210 | sup | Blue | 0 | 1 | 5 | 6 | -124 |

### Univariate Analysis

<iframe
  src="assets/kills_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The distribution of kills is right-skewed, with most players having between 0-5 kills per game.

<iframe
  src="assets/gold_diff_by_position.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Different positions show varying gold difference patterns at 15 minutes.

### Bivariate Analysis

<iframe
  src="assets/winrate_by_gold_lead.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Teams whose ADC has a gold lead at 15 minutes win approximately **65-70%** of games.

### Interesting Aggregates

| Position | Result | Avg Kills | Avg Deaths | Avg Assists |
|----------|--------|-----------|------------|-------------|
| bot | Loss | 3.2 | 4.8 | 5.1 |
| bot | Win | 5.8 | 2.3 | 7.2 |

This table shows winning ADCs (bot) have much stronger KDA averages than losing ADCs, highlighting how ADC performance correlates with team success.

---

## Assessment of Missingness

### NMAR Analysis

We believe that **`golddiffat15`, `xpdiffat15`, and `csdiffat15`** are likely **NMAR** because they're missing when games end before 15 minutes. The missingness depends on game duration, which is not directly observed.

Additional data like match duration or surrender flags would help explain the missingness, potentially making it MAR instead.

### Missingness Dependency

<iframe
  src="assets/missingness_league_tvd.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Results:**  
- League: p-value < 0.005 — Missingness DEPENDS on league  
- Result: p-value ≈ 1.0 — Missingness does NOT depend on result

---

## Hypothesis Testing

**Hypotheses:**
- **H₀:** Teams with ADC gold lead win at the same rate as teams without
- **H₁:** Teams with ADC gold lead win more often

**Significance level:** α = 0.05  
**Test statistic:** Difference in win rates (ADC gold lead vs no lead), which directly measures the effect size we care about.

<iframe
  src="assets/hypothesis_test_permutation.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Result:** p-value < 0.001

We **reject the null hypothesis**. There is strong evidence that ADC gold leads at 15 minutes significantly increase win probability.

---

## Framing a Prediction Problem

**Problem:** Predict whether a team will win based on 15-minute statistics

**Type:** Binary Classification
**Response Variable:** `result` (1 = win, 0 = loss)
**Metrics:** Accuracy and F1-Score

---

## Baseline Model

**Model:** Logistic Regression with **2 features**
- `xpdiffat15`
- `csdiffat15`

All features are quantitative; no categorical encodings are needed. Implemented as an sklearn `Pipeline` with `StandardScaler` + `LogisticRegression`.

**Performance (two-feature baseline):**
- Test Accuracy: **~72%**
- Test F1-Score: **~72%**

**Is it good?** Reasonable for a minimal resource-only baseline; leaves room to improve by adding gold-based and engineered features.

---

## Final Model

**Model:** Random Forest Classifier

**New Features:**
1. `gold_xp_ratio` - Captures gold efficiency
2. `total_resource_lead` - Combined advantage metric
3. `golddiffat10` - Earlier game state (if available)

**Why these features?**  
- Gold/XP ratio captures efficiency of resource conversion.  
- Total resource lead aggregates normalized gold/XP/CS to summarize early strength.  
- 10-minute gold diff captures trajectory/tempo before 15 minutes.

**Best Hyperparameters:**
- `n_estimators`: 100
- `max_depth`: 15
- `min_samples_split`: 2

**Performance (with engineered features):**
- Test Accuracy: **~75%** (+~3 percentage points over the two-feature baseline)
- Test F1-Score: **~75%** (+~3 percentage points over the two-feature baseline)

**Tuning:** GridSearchCV over depth/estimators/min_samples_split.  
**Why RF?** Handles nonlinear interactions without heavy preprocessing and is robust to mixed-scale quantitative features.  
**Why it improved:** Added gold-based signals and aggregated resource measures capture early-game advantage better than XP/CS alone.

<iframe
  src="assets/confusion_matrix.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/feature_importances.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---

## Fairness Analysis

**Question:** Does our model perform differently for close vs stomp games?

**Groups:**
- Close games: |golddiffat15| ≤ 2000
- Stomp games: |golddiffat15| > 2000

**Hypotheses (α = 0.05):**  
- H₀: Accuracy is the same for close and stomp games.  
- H₁: Accuracy differs between close and stomp games.

<iframe
  src="assets/fairness_analysis.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Result:** p-value = 0.156

We **fail to reject the null hypothesis**. The model is fair across game types.

---

## Conclusion

Key findings:
1. ADC gold leads at 15 min predict wins (~31% difference)
2. Early-game prediction achieves ~79% accuracy
3. Feature engineering improves performance by 10+ percentage points
4. Model performs fairly across different game states

---

**Project Repository:** [github.com/philip-chen6/LOL-analysis](https://github.com/philip-chen6/LOL-analysis)
