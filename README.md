# Credit Default Classification - EDFB Hackathon

## Overview
This project builds a machine learning classifier to predict loan defaults using credit data from the Lending Club dataset.

## Objective
Predict whether a borrower will default on a loan based on their credit profile and loan characteristics.

## Dataset
- **Source**: Lending Club loan data
- **Size**: 20,000+ loan records
- **Target Variable**: Binary classification (0 = Repaid, 1 = Default)

## Features
- **Numerical**: loan amount, interest rate, installment, annual income, DTI, FICO score, etc.
- **Categorical**: loan grade, home ownership, purpose, verification status, term

## Model
**Logistic Regression** with balanced class weights

### Why Logistic Regression?
1. **Interpretability**: Critical for regulatory compliance in credit risk
2. **Probability Estimates**: Provides default probabilities for risk-based pricing
3. **Industry Standard**: Widely accepted in financial institutions
4. **Computational Efficiency**: Fast predictions for production systems

## Key Findings
1. **Interest Rate Pattern**: Higher interest rates strongly correlate with increased default risk
2. **Loan Grade Pattern**: Clear risk stratification from Grade A (lowest risk) to Grade E (highest risk)
3. **Model Performance**: Good discriminative ability with AUC-ROC > 0.65

## Project Structure
```
├── hackathon_prep.ipynb          # Main analysis notebook
├── Hackathon_prep_excel.csv      # Loan dataset
├── lending_club_clean_dict.csv   # Data dictionary
└── README.md                      # This file
```

## Usage
1. Ensure you have Python 3.x installed
2. Install required packages: `pip install pandas numpy matplotlib seaborn scikit-learn`
3. Open and run `hackathon_prep.ipynb` in Jupyter or VS Code

## Results
The model achieves solid performance on the test set with balanced precision and recall, suitable for credit risk assessment applications.

## Date
December 15, 2025
