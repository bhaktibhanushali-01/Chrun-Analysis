# Customer Churn Analysis

## 📊 Project Overview

This project analyzes **customer churn** using customer, subscription, and support data. The analysis is performed in Python using **Pandas, NumPy, SQLite, Matplotlib, and Seaborn**.

The project covers data import, data cleaning, feature engineering, table joins, churn KPIs, customer tenure, revenue at risk, escalation analysis, correlation analysis, visualizations, and pivot tables.

## 🎯 Objectives

- Understand customer churn and retention.
- Clean and standardize customer, subscription, and support data.
- Combine multiple data sources using customer ID.
- Create useful churn-related features.
- Calculate key business KPIs.
- Analyze churn across plans and states.
- Examine the relationship between escalations and churn.
- Visualize churn trends and customer segments.
- Practice SQL and SQLite operations using Python.

## 🗂️ Data Sources

The project uses three related data tables:

### 1. Customer Data — `db_customer`

Contains customer-level information such as:

- Customer ID
- Name
- Country
- State
- Gender
- Date of Birth
- Interests
- Pincode

### 2. Subscription Data — `db_subscription`

Contains subscription and churn-related information:

- Customer ID
- Subscription start date
- Subscription type
- Renewal date
- Plan type
- Contract type
- Cancellation date
- Cancellation reason
- Monthly charges
- CLTV
- Churn score

### 3. Support Data — `db_support`

Contains customer support information:

- Customer ID
- Complaint date
- Escalations
- CSAT score
- Support comments

The supplied Excel workbook contains **21 rows in each of the three sheets**, while the SQLite database stores these sheets as separate tables.

## 🛠️ Technologies Used

- **Python**
- **Pandas**
- **NumPy**
- **SQLite**
- **Matplotlib**
- **Seaborn**
- **Jupyter Notebook**
- **Excel**

## 🔄 Project Workflow

```text
Customer / Subscription / Support Data
                ↓
          SQLite Database
                ↓
          Data Import
                ↓
          Data Cleaning
                ↓
       Feature Engineering
                ↓
         DataFrame Joins
                ↓
        Churn KPI Analysis
                ↓
          Visualization
                ↓
        Correlation Analysis
                ↓
           Pivot Tables
```

## 🧹 Data Cleaning

The notebook performs several data-cleaning steps.

### Customer table

- Renames `name` to `customer_name`.
- Removes `interests` and `pincode`.
- Converts `dob` to datetime.
- Standardizes gender values:
  - `Men` → `Male`
  - `Women` → `Female`
- Fills missing country values using the corresponding state-to-country mapping.

### Subscription table

Converts these columns to datetime:

- `subscription_start_date`
- `renewal_date`
- `cancellation_date`

### Support table

- Removes unused columns `col_1` and `comment`.
- Converts `complaint_date` to datetime.
- Calculates complaint count per customer.
- Sorts support records by complaint date and keeps the latest record for each customer before the final merge.

## ⚙️ Feature Engineering

### Churn Flag

A `churn_flag` is created from the cancellation date:

```python
df_db_subscription['churn_flag'] = np.where(
    df_db_subscription['cancellation_date'].notna(), 1, 0
)
```

- `1` → Customer has a cancellation date
- `0` → Customer has no cancellation date

### Customer Tenure

Customer tenure is calculated in days using:

- Cancellation date − subscription start date for churned customers.
- Current date − subscription start date for active customers.

### Churn Risk

Customers are grouped into three churn-risk categories using their `churn_score`:

| Churn Score | Risk |
|---|---|
| `< 50` | Low |
| `50–69` | Medium |
| `≥ 70` | High |

## 🔗 Data Integration

The three datasets are joined using `customerid`.

```python
df = (
    df_db_subscription
    .merge(df_db_customer, on='customerid', how='left')
    .merge(df_db_support, on='customerid', how='left')
)
```

Before merging, the support data is processed to calculate complaint counts and retain the latest support record per customer.

The final merged dataset is exported as:

```text
exported_churn_data.csv
```

## 📈 Key Business KPIs

The notebook calculates:

### 1. Churn Rate

Percentage of customers with a churn flag of `1`.

### 2. Retention Rate

```text
Retention Rate = 100 - Churn Rate
```

### 3. Churn by Plan Type

Churn rate is calculated separately for each plan type.

### 4. ARPU

Average Revenue Per User is calculated using average `monthly_charges`.

### 5. Average Customer Tenure

Average number of days customers have used the service.

### 6. Revenue at Risk

Total monthly charges associated with churned customers.

### 7. Escalation Rate

Percentage of support records with an escalation value of `Y`.

### 8. Average Complaints per User

Total complaints divided by the number of unique customers.

### 9. Escalation vs Churn Correlation

The notebook converts escalation values to numeric form and calculates the correlation between escalation and churn.

## 📊 Visualizations

The project includes:

- Monthly Churn Trend
- Churn Rate by Plan Type
- Churn by State
- Correlation Heatmap
- Pair Plot
- Categorical/Facet Plot
- Pivot-table based analysis

The visualizations are created using **Matplotlib** and **Seaborn**.

## 📋 Pivot Table Analysis

The notebook also uses Pandas pivot tables to analyze:

- Churn rate by plan type
- Total monthly charges by plan
- Unique customer count by plan
- Average churn flag by plan

## 🗄️ SQL with Python

The notebook includes a separate SQLite practice section demonstrating:

- Creating a SQLite database
- Creating a table
- Inserting records
- Reading SQL query results into Pandas
- Aggregation using `GROUP BY`
- Closing database connections

Example:

```sql
SELECT country, SUM(budget) AS total_budget
FROM users
GROUP BY country;
```

## 📁 Project Structure

```text
Chrun-Analysis/
│
├── churn_analysis.ipynb
├── customer_churn.db
├── customer_churn_data_raw.xlsx
├── exported_churn_data.csv
└── README.md
```

> `exported_churn_data.csv` is generated by the notebook after the data integration step.

## ▶️ How to Run the Project

### 1. Clone the repository

```bash
git clone https://github.com/bhaktibhanushali-01/Chrun-Analysis.git
cd Chrun-Analysis
```

### 2. Install dependencies

```bash
pip install pandas numpy matplotlib seaborn openpyxl jupyter
```

### 3. Open Jupyter Notebook

```bash
jupyter notebook
```

Open:

```text
churn_analysis.ipynb
```

### 4. Run the notebook

Keep the following files in the same project directory:

```text
churn_analysis.ipynb
customer_churn.db
customer_churn_data_raw.xlsx
```

The notebook can use the existing SQLite database directly. If the database is unavailable, the notebook also contains code to recreate `customer_churn.db` from the Excel workbook.

## 💡 Key Takeaway

This project demonstrates an end-to-end **Customer Churn Analysis workflow**, from raw customer and subscription data through cleaning, integration, feature engineering, KPI calculation, visualization, and business-oriented analysis.

It showcases practical skills in:

`Python` · `Pandas` · `NumPy` · `SQL` · `SQLite` · `Matplotlib` · `Seaborn` · `EDA` · `Data Cleaning` · `Feature Engineering` · `Data Visualization`

## 👩‍💻 Author

**Bhakti Bhanushali**
