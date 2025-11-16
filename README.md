# ML Model Comparison for Stock Index Performance Classification

Data Science Institute - Cohort 7 - Team ML 16 Project

## Short description
This project compares several machine learning models to classify short-term stock index price trends (e.g., whether the next day's price moves up or down) using historical open/close price data. The goal is to identify which model(s) perform best for the selected index(es) and provide recommendations for practical use.

## Table of contents
- [Project objective](#project-objective)
- [Dataset](#dataset)
- [Scope](#scope)
- [Methodology](#methodology)
- [Models compared](#models-compared)
- [Evaluation metrics](#evaluation-metrics)
- [Repository structure](#repository-structure)
- [Getting started](#getting-started)
- [Members](#members--roles)
- [Results & recommendations](#results--recommendations)
- [License](#license)
- [Contact](#contact)

## Project objective
- Predict short-term (next-day or consecutive-day) stock index direction from historical open/close prices.
- Compare multiple models and recommend the best-performing model(s) with their caveats.
- Produce reproducible experiments and clear documentation for model selection.

## Dataset
- Dataset used: "Stock Exchange Data" (https://www.kaggle.com/datasets/mattiuzc/stock-exchange-data).
- Raw data is included in this repo.

## Scope
### Realistic scenario
- Focus on one index to determine which ML model best forecasts consecutive-day price trends using only open/close features.

### Optimistic scenario
- Extend experiments to all 14 indices and compare performance differences across indices.

## Methodology
- Data ingestion and cleaning: handle missing values, create standardized fields (per index).
- Feature engineering examples: percentage change, rolling means, day-of-week, lag features (previous open/close returns), volatility measures.
- Train / validation / test: use time-series-aware splitting (no random shuffling); consider walk-forward validation for robust estimates.
- Reproducibility: set random seeds, record package versions, log experiments.

## Models compared
- Baseline: Logistic Regression.
- Advanced models: XGBoost, Decision Tree, KNN.
- Rationale: baseline vs tree-based vs distance-based methods to cover a range of model families.

## Evaluation metrics
- For classification: accuracy, precision, recall, F1-score, ROC-AUC, confusion matrix.
- For probabilistic models: calibration and log loss can be informative.
- Reports: per-index metrics and aggregated comparisons (tables and plots).

## Repository structure
- data/                 # raw and processed datasets
- notebooks/            # exploratory data analysis and experiments (one notebook per index)
- src/                  # scripts for processing, training, evaluation
- models/               # saved model artifacts
- results/              # evaluation outputs, figures, and tables
 - src/                  # scripts and notebooks
 - data/                 # raw and processed datasets
 - model/                # saved model artifacts 
 - reports/              # evaluation outputs, figures, and tables
 - README.md

## Getting started
1. Clone the repository:
   - git clone https://github.com/JesusSolisR/Stock-Index-ML-Model-Comparison.git

2. Create environment and install dependencies:
   - Python 3.11 recommended.
   - Use environment.yml for environment setup.

3. Data:
   - indexInfo.csv - exploratory information about the RAW dataset.
   - indexData.csv - RAW dataset
   - indexProcessed.csv - Processed dataset

4. Quick Start Guide:
   - Create and activate the Python environment from `environment.yml` (example using conda):
      - conda env create -f environment.yml
      - conda activate <env-name>

## Members & roles
### Jesus Solis 
- GitHub: https://github.com/JesusSolisR
### Abeer Khetrapal
- GitHub: https://github.com/abeerkhe
### Mark Kuriy 
- GitHub: https://github.com/xqzv
### Mingxia Zeng
- GitHub: https://github.com/luoyaqifei  
- Audio Reflection (3 min): [🎧 listen here](https://drive.google.com/file/d/1Ifkt016SNXM0K9X0gkrPASk9essI5Nj8/view?usp=sharing)

## Conclusion
### Results
**Random Baseline Model**: a random walk approach was correct `52.24%` of the time. The recall of `0.5405` indicates the model correctly classified `54%` of days where index prices rose over time, and a ROC AUC of `0.5202` indicates index prices have a propensity to increase over time and are not truly random. The data is biased towards increasing, `1`, and randomly guessing shows positive performance over time. 

**Logistic Regression**: a linear probabilistic approach was correct `53.68%` of the time. The recall of `0.7452` indicates the model correctly classified over `74%` of days where index prices rose, and a ROC AUC of `0.5226` indicates its ability to discriminate between "Up" and "Down" days was marginally (`+0.24%`) better than the random baseline. This model is strongly biased towards predicting "Up," catching most positive days (high recall) at the cost of being less precise (lower precision) than the random model. Ultimately, the model learned the positive bias of the dataset. 

**KNN**: a pattern-matching approach was correct only `48.16%` of the time. The recall of `0.4786` indicates the model failed to identify over half of the days where index prices rose, and a ROC AUC of `0.4918` indicates its predictions were actively worse than a random guess. This model likely overfit the training data, as its performance on unseen data is less effective than the biased random baseline.

**Decision Tree**: a rule-based flowchart approach was correct only `46.58%` of the time. The recall of `0.4190` indicates the model failed to identify nearly `58%` of the days where index prices rose, and a ROC AUC of `0.4712` indicates it performed significantly worse than a random guess, showing notably (`-4.91%`) worse performance than the random baseline. This model is the worst performer, suggesting an overfit towards of the training data. 

**EXGBoost**: an advanced ensemble approach was correct exactly `50.00%` of the time. The recall of `0.3476` is extremely low, indicating the model correctly identified only `35%` of days where index prices rose; however, it had the highest precision (`0.5794`) and an ROC AUC of `0.5206`, showing a marginal (`+0.03%`) improvement over the random baseline. This model is highly conservative, only predicting "Up" when very confident (high precision), but its cautiousness causes it to miss many increasing, `1` days (low recall).

### Recommendations
The historical data, variables, and features available do not make for good predictors of future market behaviour. Historical trends are not suggestive of future market behaviour (except for inherent bias in the data) for classification--regardless of theoretical model robustness--with the limited post-hoc data elements available (e.g., *Open* and *Close* prices). The variables available fail to capture critical underlying scenarios which can predict market behaviour, and this behaviour propogates to features engineered from these variables. While helpful for trend analysis, none of the tested models facilitate effectively 'beating the market'. 

The recommendation is for aggressive investors to leverage **Logistic Regression** models, for conservative investors to leverage **XGBoost** models, and for carefree investors to flip a coin. 

## Contact
For questions contact the team members listed above.
