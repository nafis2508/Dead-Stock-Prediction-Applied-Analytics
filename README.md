<img width="1024" height="1024" alt="DeadStockPrediction" src="https://github.com/user-attachments/assets/3d6eb6ac-b4a7-401d-99b4-5d0e0463ae0c" />

# Dead Stock Prediction – Applied Predictive Analytics

This repository contains my first assessment for the Applied Predictive Analytics unit (Session 2, 2025). The project builds a predictive model to identify **dead stock items** using inventory and movement data. Dead stock refers to items that no longer sell and remain in storage for extended periods, tying up capital and warehouse space.

---

## 1. Project Overview

The dataset (`Inventory.xlsx`) contains **3,000 inventory records** and includes:

* Stock quantities in different age buckets
* Monthly demand (Jan–Dec)
* Inventory turn and average monthly sales
* ABC Class, Business Area, Warehouse, State, and Item Type
* A target label: **Dead stock** (1 = dead stock, 0 = active)

The goal is to build a supervised machine learning model to predict whether an item is dead stock, and to understand which features contribute most to this outcome.

---

## 2. Data Preparation

Key preprocessing steps:

### **2.1 Dropped non-predictive columns**

* `Item No.`
* `Item Description`

These were identifiers / text fields with no predictive value.

### **2.2 Handling categorical variables**

* `State` → converted to `State_NSW` using `get_dummies(drop_first=True)`
* `Whse` → encoded into dummy variables (`Whse_1N1`, `Whse_1W0`)
* `Item Type` → grouped into four categories:

  * Finished Goods
  * Raw Materials
  * WIP Manufactured
  * Other

### **2.3 ABC Class**

ABC Class contains important categories that reflect product lifecycle:

* “Not Purchasing”
* “End of Life”
  Both are strong indicators of dead stock.

Encoded using one-hot encoding (e.g., `ABCClass_E`, `ABCClass_I`, etc.)

### **2.4 Imputation**

* **Numeric columns** → replaced missing values with **mean**
* **Categorical columns** → replaced missing values with **mode**

### **2.5 Train–Test Split**

* 80% training, 20% testing
* `random_state=31`
* `stratify=Dead stock`

### **2.6 Scaling**

Numeric predictors were standardised using `StandardScaler`.

---

## 3. Models Built

Two models were trained:

### **3.1 Logistic Regression**

* Linear, interpretable baseline
* Hyperparameter tuning across C values (0.5 → 10)
* Very stable performance regardless of C
* **Training Accuracy:** ~0.996
* **Test Accuracy:** ~0.996

### **3.2 Random Forest Classifier**

* Nonlinear model capturing feature interactions
* Best configuration:

  * `n_estimators=10`
  * `criterion="entropy"`
* **Training Accuracy:** 0.9995
* **Test Accuracy:** 1.0000

Random Forest also provided **feature importances**, useful for interpreting which variables drive dead stock risk.

---

## 4. Key Findings

### **4.1 Most Influential Features**

Top predictors (Random Forest importances):

1. **Over 2 years Qty** – long-aged stock strongly linked to dead stock
2. **ABCClass_E / ABCClass_I / ABCClass_J** – categories including “End of Life” and “Not Purchasing”
3. **% of over 2 year** – proportion of stock sitting idle
4. **Inventory Turn** – slow turnover increases dead stock risk
5. **Over 3 Years Quantity** – extreme aging is a major flag

**Interpretation:**
Dead stock is driven primarily by **stock age** and **ABC lifecycle categorisation**. Items that sit for long periods or are marked as not actively purchased naturally transition into dead stock.

---

## 5. Recommended Model

Although Logistic Regression performed very well and remained stable across hyperparameter values, the **Random Forest model achieved the highest test accuracy (100%)** and captured nonlinear interactions between aging, ABC Class, and stock movement.

For operational prediction, **Random Forest is recommended** because:

* It delivers the strongest predictive performance
* It handles complex relationships in inventory behaviour
* Feature importances clearly highlight the drivers of dead stock

Logistic Regression remains useful for explanation and decision support, but Random Forest is superior for prediction.

---

## 6. Repository Structure

```
├── Inventory.xlsx                 # Source dataset (not included if confidential)
├── notebook.ipynb                 # Full analysis, cleaning, modelling, and results
├── requirements.txt               # Python dependencies
├── README.md                      # Project documentation
```

---

## 7. Requirements

```
numpy
pandas
scikit-learn
matplotlib
seaborn
jupyter
```


