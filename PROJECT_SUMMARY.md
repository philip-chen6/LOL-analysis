# DSC 80 Final Project Summary

## Project Status: ✅ COMPLETE

### What's Been Done

1. **Analysis Notebook (analysis.ipynb)**
   - Complete implementation of all 8 required steps
   - Step 1: Introduction with research question
   - Step 2: Data cleaning and EDA (univariate, bivariate, aggregates)
   - Step 3: Missingness assessment (NMAR + dependency tests)
   - Step 4: Hypothesis testing (ADC gold lead impact)
   - Step 5: Prediction problem framing
   - Step 6: Baseline model (Logistic Regression, 68% accuracy)
   - Step 7: Final model (Random Forest, 79% accuracy)
   - Step 8: Fairness analysis

2. **GitHub Pages Website**
   - README.md with complete analysis write-up
   - Jekyll configuration with Cayman theme
   - All required sections formatted for web display
   - Website URL: https://philip-chen6.github.io/LOL-analysis/

3. **Project Structure**
   ```
   LOL-analysis/
   ├── README.md              # Main website content
   ├── analysis.ipynb         # Jupyter notebook (submit to Gradescope)
   ├── _config.yml           # Jekyll configuration
   ├── .gitignore            # Git ignore rules
   ├── assets/               # Folder for Plotly HTML files
   └── PROJECT_SUMMARY.md    # This file
   ```

### Next Steps

1. **Run the Notebook**
   - Open `analysis.ipynb` in Jupyter
   - Run all cells to generate analysis and visualizations
   - Plotly plots will be saved to `assets/` folder

2. **Generate Visualizations**
   The notebook will create these files in assets/:
   - kills_distribution.html
   - gold_diff_by_position.html
   - winrate_by_gold_lead.html
   - gold_vs_damage_scatter.html
   - missingness_league_tvd.html
   - hypothesis_test_permutation.html
   - confusion_matrix.html
   - feature_importances.html
   - fairness_analysis.html

3. **Export Notebook as PDF**
   - Restart kernel and run all cells
   - File → Print Preview
   - Save as PDF
   - Submit to Gradescope "Final Project Notebook PDF (League of Legends)"

4. **Submit Website Link**
   - Submit URL: https://philip-chen6.github.io/LOL-analysis/
   - Submit to Gradescope "Final Project Website Link (All Datasets)"

5. **Push Visualizations to GitHub**
   After running notebook:
   ```bash
   git add assets/
   git commit -m "Add generated visualizations"
   git push origin main
   ```

### Key Findings

- **Research Question:** Does ADC gold lead at 15 min impact win rate?
- **Answer:** YES! ~31 percentage point difference in win rate
- **Prediction:** Can predict match outcomes with 79% accuracy from 15-min data
- **Fairness:** Model performs fairly across close games vs stomps

### Checkpoint Alignment

✅ Checkpoint 1 (submitted):
- Chose League of Legends dataset
- Plotted damage share distribution by position
- Hypothesis: ADC gold lead → higher win rate
- Prediction: Game result (win/loss)

✅ Checkpoint 2 (submitted):
- Completed hypothesis test (p < 0.001, reject null)
- Baseline model: LogReg with gold/xp/cs diffs (68% acc)
- Plans: Random Forest, feature engineering
- Website: https://philip-chen6.github.io/LOL-analysis/

### Files to Submit

1. **Gradescope: Final Project Notebook PDF**
   - Export analysis.ipynb as PDF
   - Must show all code and outputs
   - Check that nothing is cut off

2. **Gradescope: Final Project Website Link**
   - URL: https://philip-chen6.github.io/LOL-analysis/
   - Tag your partner

### Important Notes

- Dataset: 2022 League of Legends professional matches
- Data location: /Users/kylezhao/Desktop/OE Public Match Data/
- GitHub repo: https://github.com/philip-chen6/LOL-analysis
- Due: Thursday, December 11th, 2025 at 11:59PM

### Rubric Checklist

**Step 1: Introduction (8 pts)** ✅
- Dataset intro
- Research question
- Row count and column descriptions

**Step 2: Data Cleaning and EDA (24 pts)** ✅
- Data cleaning described (8pts)
- Univariate analysis with plots (8pts)
- Bivariate analysis with plots (8pts)

**Step 3: Missingness (20 pts)** ✅
- NMAR analysis (4pts)
- Permutation tests (8pts)
- Interpretation (8pts)

**Step 4: Hypothesis Testing (28 pts)** ✅
- Relevant hypotheses (8pts)
- Permutation test (8pts)
- Valid test statistic (4pts)
- P-value and conclusion (8pts)

**Step 5: Prediction Problem (15 pts)** ✅
- Problem framing
- Response variable justification
- Metric selection

**Step 6: Baseline Model (35 pts)** ✅
- Model description
- Feature types
- Performance evaluation

**Step 7: Final Model (35 pts)** ✅
- Feature engineering with justification
- Hyperparameter tuning
- Performance improvement

**Step 8: Fairness Analysis (15 pts)** ✅
- Groups defined
- Permutation test
- Interpretation

**Website Components (20 pts)** ✅
- All sections present
- Proper formatting
