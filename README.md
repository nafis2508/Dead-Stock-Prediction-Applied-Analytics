Dead Stock Prediction – Applied Predictive Analytics (BUSA8001)

This repository contains my full solution for Programming Task 1 in the Applied Predictive Analytics unit (BUSA8001, Session 2, 2025).
The project focuses on predicting dead stock in a manufacturing inventory system using Python, pandas, NumPy, and scikit-learn.

Project Overview

A mid-sized manufacturing retailer observed that a subset of inventory items across Australian warehouses had not moved for extended periods. The goal of this analysis was to predict which items are likely to be classified as dead stock (1 = yes, 0 = no) to support decisions around markdowns, liquidation, storage optimisation, and warehouse planning.

The assignment required:

Cleaning and preparing 3,000 inventory records

Treating categorical, nominal, and numeric variables

Imputing missing values using mean/mode

One-hot encoding several categorical variables

Preparing X and y arrays

Training and evaluating:

Logistic Regression (linear classifier)

Random Forest (nonlinear classifier)

Interpreting model accuracy and feature importance

Recommending the most suitable model

Key Steps & Methods
1. Data Cleaning & Imputation

Dropped non-predictive text fields (e.g., Item Description).

Numeric features → imputed using mean

Categorical features → imputed using mode

Re-grouped Item Type into 4 consolidated categories

Used get_dummies() to encode:

Warehouse (Whse)

State

Base Unit

Business Area

ABC Class

Converted Dead stock to numeric using LabelEncoder.

2. Feature Matrix (X) and Target Vector (y)

Used first 80% of observations for initial X and y arrays.

Train-test split performed with:

test_size=0.2

random_state=31

stratify=y

Standardised numeric features using StandardScaler.

3. Models Trained
Logistic Regression
lr = LogisticRegression(C=1, random_state=31, solver='lbfgs')


Training Accuracy: 99.64%

Test Accuracy: 99.58%

Very stable across different C values

Captures linear relationships well

Random Forest
rf = RandomForestClassifier(n_estimators=10, criterion='entropy', random_state=31)


Training Accuracy: 99.94%

Test Accuracy: 100%

Strongest model overall

Captures nonlinear interactions and hierarchical splits

4. Best Model & Recommendation

While Logistic Regression performed extremely well and demonstrated strong stability, Random Forest slightly outperformed it, achieving perfect test accuracy under its best configuration (10 trees, entropy criterion).

Random Forest was recommended because:

It consistently captured complex interactions between stock age, ABC classification, turnover behaviour, and warehouse attributes.

It achieved the highest predictive accuracy.

Feature importance allowed clear interpretation of which variables drive dead stock risk.

5. Important Predictors

Based on Random Forest feature importance:

Over 2 Years Qty

ABC Class (especially "Not Purchasing" & "End of Life")

% of Over 2 Years

Inventory Turn

Over 3 Years Quantity

These reflect stock age + ABC categorisation, the strongest business logic indicators of unsellable inventory.

Code Structure
├── data/ (not included for confidentiality)
├── notebook/
│   └── dead_stock_prediction.ipynb
├── README.md
└── requirements.txt (optional)

Technologies Used

Python

pandas

NumPy

scikit-learn

Jupyter Notebook
