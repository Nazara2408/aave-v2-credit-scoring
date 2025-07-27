# Wallet Risk Scoring Model (Compound V2/V3)

the aim is to present a two-fold approach to assess the **risk scores (0–1000)** of DeFi wallets interacting with Compound protocols (V2/V3). The primary goal is to analyze wallet behavior and assign a risk score based on either on-chain activity or simulated data when real data is insufficient.

## Project Structure

The repository contains two main notebooks:

### 1. `compound_scoring_rule_based_approach.ipynb`
Applied a rule-based scoring logic to wallets using real on-chain data from Compound V2/V3.
Compound API is the Data source.
Due to API limitations, **only 3 wallets returned valid transaction data**, while others failed (API returned empty or error responses) that can be seen in the Notebook.
Method Used: 
  - Extracted borrow/supply/repay data.
  - Applied predefined rules to assign risk scores based on activity.

### 2. `compound_Scoring_ml_approach.ipynb`
Applied dummy fallback for wallets where API data was unavailable. Then applied Machine Learning Algorithms to predict the risk score.

- Models Tried:
  1) RandomForestRegressor: Initial model, but performance (R² ~ 0.65–0.75) was not optimal.
  2) XGBoostRegressor: Final selected model with improved performance (R² ~ 0.84).

- Output:
  1) wallet_risk_scores_ml.csv — ML-based final scores for all dummy wallets.
  2) wallet_risk_scores_rule_based.csv — Rule-based scores (only 3 valid).

## 📦 Output Files

1) rule_based_wallet_risk_scores.csv - Rule-based risk scores (3 valid wallets)
2) rf_wallet_risk_scores.csv         - Random Forest output (all dummy wallets)
3) xgb_wallet_risk_scores.csv        - Final ML output using XGBoost              

# Limitations

- Compound API is deprecated or partially functional, limiting real-world data availability.
- Rule-based approach was limited to a very small subset (3/100 wallets).
- Machine Learning model uses **simulated data**, which may differ from real on-chain wallet behavior.


