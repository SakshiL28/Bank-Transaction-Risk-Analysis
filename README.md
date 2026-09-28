# 🏦 Bank Transaction Risk Analysis

## 📌 Overview

Banks handle thousands of transactions every day, and a small share of them show unusual behavior. This project builds a **rule-based risk scoring model** that flags suspicious transactions, then visualizes the results so a risk team can filter, explore, and act on them.

🎯 **Goal:** Identify high-risk transactions using clear, explainable signals, and make the findings easy to explore in a dashboard.

---

## 📂 Dataset

**Source:** Bank Transaction Dataset for Fraud Detection (Kaggle)

**Size:** 2,512 transactions, 16 columns

### 🔑 Key Columns

* TransactionAmount
* TransactionDate
* TransactionType
* Location
* Channel
* CustomerAge
* CustomerOccupation
* TransactionDuration
* LoginAttempts
* AccountBalance
* PreviousTransactionDate

---

## 🛠️ Tools and Technologies

### 🐍 Python

* Pandas
* NumPy
* Matplotlib
* Seaborn

### 📓 Development

* Jupyter Notebook

### 📊 Power BI

* DAX measures
* Interactive visuals
* Slicers

---

## 🔄 Project Workflow

### 1️⃣ Data Loading and Inspection

Checked shape, data types, and missing values.

### 2️⃣ Data Cleaning

* Converted date columns
* Removed duplicates
* Filled missing occupation values with Unknown

### 3️⃣ Feature Engineering

Created risk signals.

### 4️⃣ Risk Scoring

Combined the signals into a weighted score and grouped transactions into Low, Medium, and High risk.

### 5️⃣ Exploratory Data Analysis

Created charts for:

* Risk distribution
* Risk by channel
* Amount by risk level
* Correlation heatmap

### 6️⃣ Export

Saved the scored data as:

**transactions_with_risk_score.csv**

### 7️⃣ Dashboard

Built an interactive Power BI report on top of the scored data.

---

## ⚠️ Risk Scoring Model

Each transaction earns points for every warning sign it triggers.

**Maximum Score: 80**

| Risk Signal               | Rule                                             | Points |
| ------------------------- | ------------------------------------------------ | -----: |
| 💰 Unusual amount         | Absolute z-score of the amount is greater than 2 |     25 |
| 🔐 High login attempts    | More than 3 login attempts                       |     20 |
| ⚡ Very short duration     | Transaction duration in the bottom 5%            |     15 |
| 💳 Large share of balance | Amount is more than 70% of the account balance   |     20 |

---

## 🚦 Risk Levels

| Risk Level |      Score |
| ---------- | ---------: |
| 🔴 High    | 45 or more |
| 🟡 Medium  |   20 to 44 |
| 🟢 Low     |   Below 20 |

---

## 📈 Results

| Risk Level | Transactions |    Share |
| ---------- | -----------: | -------: |
| 🟢 Low     |        2,212 |    88.1% |
| 🟡 Medium  |          259 |    10.3% |
| 🔴 High    |           41 |     1.6% |
| **Total**  |    **2,512** | **100%** |

The model flagged **41 high-risk transactions out of 2,512** for closer review.

---

## 📝 Note on Model Validation

An earlier version of the model included a **"rapid repeat transaction"** flag based on the days since the previous transaction.

During review I found that **PreviousTransactionDate was later than TransactionDate** in this dataset, which made the calculated gap negative and caused the flag to fire on almost every row.

I removed the flag and recalibrated the thresholds to the values above.

---

## 📊 Power BI Dashboard

The dashboard includes:

### 📌 KPI Cards

* Total transactions
* Total value
* Average risk score
* High-risk count

### 📈 Visuals

* Risk gauge
* Risk level distribution chart
* Transaction trend over time by risk level
* High-risk transactions by channel
* Table of top flagged transactions with color-coded risk badges

### 🎛️ Slicers

* Date range
* Channel
* Risk level

---

## ⚠️ Limitations and Future Work

The score is **rule-based**, and the weights and thresholds are set by judgment rather than learned from labeled fraud cases.

The dataset has **no confirmed fraud label**, so accuracy cannot be measured.

### 🚀 Future Improvements

* Try an unsupervised model such as **Isolation Forest** to compare against the rule-based flags
* Add **device and IP-based signals**
* Test the thresholds against **labeled data**
