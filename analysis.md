# Project Overview

This project focuses on building a wallet risk scoring system using Compound V2/V3 on-chain transaction data. The goal is to assign a score between 0 and 1000 to each wallet, representing its risk level. Due to limited valid API data, the project is divided into two parts: a rule-based scoring model using actual API data (for only a few wallets), and a dummy fallback machine learning model using synthetically generated wallet data.

The project contains two main Jupyter Notebook files:
- compound_scoring_rule_based_approach.ipynb : Implements the rule-based scoring logic for wallets fetched from the API.
- compound_Scoring_ml_approach.ipynb : Builds a machine learning model using dummy wallet data for fallback scoring, as most API responses were incomplete or missing.

# Approaches Used

1) Rule-Based Scoring (compound_rule_based.ipynb)

In the rule-based model, actual wallet data was fetched using the Cobvalent API. However, due to deprecation and poor API response, only three wallet addresses returned valid data. For these, rule-based conditions were applied to assign risk scores based on logic such as:

- High borrowing and low collateral results in higher risk
- High health factor and good collateral results in lower risk

The rule-based scoring was effective only for those three valid wallets. For all others, the API returned empty or invalid data, leading to the fallback approach.

2) ML-Based Fallback Scoring (compound_ml_based.ipynb)

Due to the lack of valid real data, a fallback approach was developed using dummy but realistic wallet data. Synthetic data included key features that reflect wallet behavior in Compound. These were used to train a regression model that predicts a wallet’s risk score.

Initially, a Random Forest Regressor was used, but it did not provide satisfactory performance. Later, an XGBoost Regressor was applied, which gave better results in terms of R2 score and RMSE.

FOR RANDOM FOREST :
R² Score: 0.7390127847033894
RMSE: 101.62796895552546

FOR XGBOOST :
R² Score: 0.8432804032886826
RMSE: 78.7526691520918

The final model was used to predict risk scores for the full dummy dataset. The outputs of both the Random Forest and XGBoost models were saved separately for comparison.

# Error Details

While running the rule-based file, API fetching resulted in faults for most wallet addresses. The common error shown was an API fetch fault or empty result. This highlighted the limitation of relying solely on real-time on-chain data from deprecated subgraphs.

To handle this, the fallback ML model was built to ensure scoring could still be applied even without API data.

# COMPARISON BETWEEN RULE BASED AND ML APPROACH

Below is a comparison of the first three wallet scores obtained from both models:

Rule-Based Output:
1. 0x0039f22efb07a647557c7c5d17854cfd6d489ef3 - Score: 1000
2. 0x06b51c6882b27cb05e712185531c1f74996dd988 - Score: 1000
3. 0x0795732aacc448030ef374374eaae57d2965c16c - Score: 700

XGBoost Model Output:
1. 0x0039f22efb07a647557c7c5d17854cfd6d489ef3 - Score: 560.73
2. 0x06b51c6882b27cb05e712185531c1f74996dd988 - Score: 950.4
3. 0x0795732aacc448030ef374374eaae57d2965c16c - Score: 750.73

This comparison shows that while the rule-based model assigns more rigid, extreme values (like 1000), the ML model is capable of more realistic scoring. For example, wallet 1 was scored 1000 in the rule-based method but 560.73 in the ML model, which might better reflect the wallet's moderate risk based on multiple contributing factors.

Output Files

- rule_based_wallet_risk_scores.csv : Contains risk scores for three valid wallets from the API
- rf_wallet_risk_scores.csv : Contains risk scores from the Random Forest model using dummy data
- xgb_wallet_risk_scores.csv : Contains risk scores from the XGBoost model using the same dummy data

Each file includes the wallet address and its corresponding predicted risk score (scaled between 0 and 1000).
