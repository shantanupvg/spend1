# 💰 SpendDNA — Financial Transaction Analysis

> A Python-based financial transaction analysis project that transforms raw transaction data into structured spending patterns, vendor insights, anomaly detection, and spending archetypes.

---

## 📌 Project Overview

**SpendDNA** analyzes six months of financial transaction data to understand:

* Where money is being spent
* Which vendors receive the most money
* Which spending categories dominate
* When spending occurs during the day
* How spending changes month-to-month
* Which transactions are unusually large
* What spending personalities/archetypes can be identified

The project was developed using **Python, Pandas, NumPy, and Jupyter Notebook**.

---

## 🎯 Objectives

The main objectives of the project are to:

1. Clean and standardize raw financial transaction data.
2. Normalize transaction types and monetary values.
3. Extract standardized vendor names from messy transaction descriptions.
4. Categorize vendors into meaningful spending categories.
5. Calculate overall financial metrics.
6. Analyze spending by category and vendor.
7. Identify time-of-day and monthly spending patterns.
8. Detect unusual transactions using category-specific Z-scores.
9. Identify multiple spending archetypes.
10. Generate a final text-based SpendDNA report.

---

## 🛠️ Tech Stack

* **Python**
* **Pandas**
* **NumPy**
* **Jupyter Notebook**
* **Statistical Analysis**
* **Data Cleaning**
* **Data Visualization using ASCII/Text-based output**

---

## 📂 Project Structure

```text
SpendDNA/
│
├── SpendDNA_shantanu.ipynb
├── Bank_Statement.csv
├── README.md
```

---

## 🧹 Data Cleaning

The raw transaction data contained several inconsistencies that needed to be standardized.

### Date Standardization

Multiple date formats were handled using Pandas datetime conversion.

```python
df["Date"] = pd.to_datetime(
    df["Date"],
    errors="coerce",
    dayfirst=True,
    format="mixed"
)
```

### Amount Cleaning

Currency symbols and commas were removed before converting the column into a numerical field.

Examples:

```text
₹1,250       → 1250
Rs. 2,500    → 2500
1500.00      → 1500.00
```

### Transaction Type Standardization

Different representations were standardized:

```text
DR      → debit
Debit   → debit
CR      → credit
Credit  → credit
```

### Missing Values

Missing transaction modes were preserved as missing rather than inventing information.

---

## 🏪 Vendor Standardization

One of the major challenges was extracting meaningful vendor names from messy transaction descriptions.

For example:

```text
UPI-SWIGGY-6560@HDFCBANK
        ↓
      Swiggy
```

and:

```text
BUNDL Tech P L
        ↓
      Swiggy
```

Vendor aliases were handled using keyword-based matching.

Examples:

| Description Keyword | Standardized Vendor |
| ------------------- | ------------------- |
| SWIGGY              | Swiggy              |
| BUNDL               | Swiggy              |
| ZOMATO              | Zomato              |
| GROFERS             | Blinkit             |
| BLINKIT             | Blinkit             |
| ZEPTO               | Zepto               |
| AMZN                | Amazon              |
| FKART               | Flipkart            |
| FSN E-COMMERCE      | Nykaa               |
| ZERODHA             | Zerodha             |
| GROWW               | Groww               |

The original `Description` column was preserved and a separate `Vendor` column was created.

---

## 🗂️ Spending Categories

Standardized vendors were mapped into spending categories.

The project uses categories such as:

* Food Delivery
* Quick Commerce
* E-commerce
* Transport
* Cafe
* Restaurants
* Subscriptions
* Utilities
* Groceries
* Investments
* Fuel
* Entertainment
* Personal Transfer
* Cash Withdrawal
* Unknown

This creates a useful hierarchy:

```text
Description
     ↓
Vendor
     ↓
Category
     ↓
Spending Analysis
```

---

## 📊 Spending Overview

The project calculates:

### Total Credits

Total incoming money.

### Total Debits

Total outgoing money.

### Net Change

```text
Net Change = Credits − Debits
```

### Savings Rate

```text
Savings Rate =
(Credits − Debits) / Credits × 100
```

### Current Analysis Results

| Metric                    |      Result |
| ------------------------- | ----------: |
| Total Credits             |   ₹5,09,774 |
| Total Debits              |  ₹16,78,901 |
| Net Change                | −₹11,69,127 |
| Savings Rate              |     −229.3% |
| Transactions              |       1,310 |
| Recognized Unique Vendors |          32 |

---

## 🏆 Top Spending Categories

The current analysis identified the following major spending categories:

| Category      |  Spending | % of Debits |
| ------------- | --------: | ----------: |
| E-commerce    | ₹6,03,877 |       36.0% |
| Unknown       | ₹3,28,370 |       19.6% |
| Investments   | ₹2,48,160 |       14.8% |
| Food Delivery | ₹1,50,839 |        9.0% |
| Fuel          |   ₹89,303 |        5.3% |

E-commerce is the largest identified spending category.

---

## 🏪 Top Vendors

The highest-spending recognized vendors were:

| Rank | Vendor   |  Spending |
| ---: | -------- | --------: |
|    1 | Amazon   | ₹3,28,530 |
|    2 | Zerodha  | ₹2,10,000 |
|    3 | Flipkart | ₹1,77,510 |
|    4 | Swiggy   |   ₹95,523 |
|    5 | Myntra   |   ₹69,529 |

---

## 🕐 Time-of-Day Analysis

The transaction `Time` column was converted into an hour value from **0–23**.

A **Category × Hour** spending matrix was then created.

The project uses a text/ASCII-based heatmap rather than Matplotlib or Seaborn.

Example:

```text
Category          18  19  20  21  22  23
Food Delivery      ▒   ▓   █   █   ▓   ▒
Cafe               ░   ▒   ▓   ▒   ░   .
Transport          ▒   ▓   ▒   ░   .   .
```

This makes it possible to identify periods of high spending without relying on graphical plotting libraries.

---

## 📅 Monthly Trend Analysis

Spending was analyzed from January to June 2024.

For example, Food Delivery spending was:

| Month    | Spending |
| -------- | -------: |
| January  |  ₹22,633 |
| February |  ₹23,740 |
| March    |  ₹24,803 |
| April    |  ₹27,756 |
| May      |  ₹25,408 |
| June     |  ₹26,499 |

The largest increase between January and June was observed in **E-commerce**.

---

## 🚨 Anomaly Detection

Instead of calculating one global mean and standard deviation, the project calculates statistics **separately for each spending category**.

For each transaction:

```text
z = (transaction amount − category mean)
    / category standard deviation
```

A transaction is flagged as an anomaly when:

```text
z > 2
```

This allows a transaction to be evaluated relative to its own category.

For example, a ₹2,000 transaction could be unusual in one category but normal in another.

### Current Result

**41 transactions** were identified as anomalies using the `z > 2` rule.

The top anomalies were sorted from the highest Z-score to the lowest.

---

## 🧠 Spending Archetypes

The project evaluates multiple spending personalities independently.

This is important because a person can have **more than one spending archetype**.

The implemented archetypes include:

1. The Foodie
2. The Quick Commerce Junkie
3. The Shopaholic
4. The Investor
5. The Late-Night Snacker
6. The Cab Commuter
7. The Subscription Lover
8. The YOLO Spender

Based on the current analysis, the identified archetypes are:

```text
→ THE SHOPAHOLIC
→ THE YOLO SPENDER
```

### Example Rules

**The Foodie**

```text
Food Delivery + Restaurants + Cafe > 25%
of total debits
```

**The Quick Commerce Junkie**

```text
Quick Commerce > 15%
of total debits
```

**The Shopaholic**

```text
E-commerce > 15%
of total debits
```

**The Investor**

```text
Investments > 15%
of total debits
```

**The YOLO Spender**

```text
Savings Rate < 10%
```

---

## 💡 Key Findings

The current analysis indicates:

* **E-commerce** is the largest spending category.
* **Amazon** is the highest-spending recognized vendor.
* **March** recorded the highest overall monthly spending.
* Food Delivery spending is concentrated around evening/night hours.
* Category-specific Z-score analysis identified unusually large transactions.
* The current archetype analysis identifies **The Shopaholic** and **The YOLO Spender**.

---

## 📈 Skills Demonstrated

This project helped demonstrate practical skills in:

### Python

* Functions
* Loops
* Dictionaries
* Conditional logic
* String processing

### Pandas

* DataFrame manipulation
* Filtering
* `groupby()`
* `agg()`
* `map()`
* `apply()`
* `pivot_table()`
* Sorting
* Datetime operations

### Data Analysis

* Aggregation
* Percentage analysis
* Time-based analysis
* Category analysis
* Vendor analysis
* Trend analysis

### Statistics

* Mean
* Standard deviation
* Z-score
* Category-specific anomaly detection

### Data Cleaning

* Missing-value handling
* Date standardization
* Currency cleaning
* Text normalization
* Entity/vendor standardization

---

## 🚀 Future Improvements

Possible improvements include:

* Interactive dashboard using Streamlit
* Automated vendor classification using NLP
* Better handling of unknown transactions
* Advanced anomaly detection
* Spending forecasting
* Budget recommendations
* Recurring-payment detection
* Automated monthly financial summaries
* Interactive category and vendor visualizations

---

## 📌 Project Status

**Completed — Portfolio Project**

The project demonstrates an end-to-end workflow from **raw financial transaction data → data cleaning → feature engineering → statistical analysis → behavioral insights → final financial report**.

---

## 👨‍💻 Author

**shantanu**

Electronics & Telecommunication Engineering Student

Interested in:

* Python
* Data Analytics
* Artificial Intelligence
* Machine Learning
* Electronics & Technology

---

⭐ If you found this project interesting, feel free to explore the notebook and share your feedback.
