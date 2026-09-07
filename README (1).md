# 🛍️ Customer Shopping Insights Dashboard

End-to-end data analytics project that analyzes shopping behavior across **3,900 customer transactions** to uncover spending patterns, customer segments, product preferences, and subscription trends — from raw data to an interactive Power BI dashboard.

![Python](https://img.shields.io/badge/Python-pandas-3776AB?logo=python&logoColor=white)
![SQL Server](https://img.shields.io/badge/SQL-Server-CC2927?logo=microsoftsqlserver&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?logo=powerbi&logoColor=black)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

---

## 📌 Overview

This project follows a complete analytics workflow:

1. **Data Cleaning & Feature Engineering** — Python (pandas)
2. **Business Analysis** — SQL Server (T-SQL)
3. **Visualization** — Power BI interactive dashboard

The goal is to answer real business questions — revenue drivers, customer loyalty, discount behavior, and product performance — and turn them into actionable recommendations.

---

## 🧰 Tech Stack

| Layer | Tools |
|---|---|
| Data Cleaning & Feature Engineering | Python, pandas |
| Database | Microsoft SQL Server, SQLAlchemy, pyodbc |
| Analysis | T-SQL (CTEs, window functions, aggregations) |
| Visualization | Power BI |

---

## 📊 Dataset Summary

- **Rows:** 3,900
- **Columns:** 18
- **Missing data:** 37 values in `review_rating` (imputed using category median)

**Key features:**
- **Demographics:** Age, Gender, Location, Subscription Status
- **Purchase details:** Item Purchased, Category, Purchase Amount, Season, Size, Color
- **Shopping behavior:** Discount Applied, Promo Code Used, Previous Purchases, Frequency of Purchases, Review Rating, Shipping Type

---

## 🧹 Data Preparation (Python)

Steps performed in `notebooks/` (pandas):

1. **Import & load** the raw CSV into a DataFrame
2. **Standardize columns** to `snake_case`
3. **Handle missing data** — imputed `review_rating` nulls using the median rating per `category`
4. **Feature engineering:**
   - `age_group` — binned into Teenager / Adult / Middle-Aged / Senior
   - `purchase_cycle_days` — mapped from `frequency_of_purchases` (e.g. Weekly → 7, Monthly → 30, Annually → 365)
5. **Data integration** — loaded the cleaned DataFrame into SQL Server via SQLAlchemy for downstream SQL analysis

```python
import pandas as pd

DF = pd.read_csv('../Raw Data/customer_shopping_behavior.csv')
DF.columns = DF.columns.str.replace(' ', '_').str.lower()
DF['review_rating'] = DF.groupby('category')['review_rating'].transform(lambda x: x.fillna(x.median()))
```

---

## 🗃️ Business Analysis (SQL)

Ten business questions were answered using T-SQL against the `customer_shopping_behavior` table.

| # | Question | Key Insight |
|---|---|---|
| 1 | Revenue by gender? | Male customers generated **AED 157,890** vs. Female **AED 75,191** |
| 2 | Which discount users still spent above average? | 16 high-value customers identified for targeted upsell |
| 3 | Top 5 products by review rating? | Gloves (3.86), Sandals (3.84), Boots (3.82), Hat (3.80), Skirt (3.78) |
| 4 | Standard vs. Express shipping spend? | Express shoppers spend slightly more (AED 60.48 vs. 58.46) |
| 5 | Subscribers vs. non-subscribers? | Non-subscribers drive **73%** of total revenue |
| 6 | Most discount-dependent products? | Hat (50%), Sneakers (49.7%), Coat (49.1%) |
| 7 | Customer segmentation (New/Returning/Loyal)? | 3,116 Loyal · 701 Returning · 83 New |
| 8 | Top 3 products per category? | e.g. Clothing → Blouse, Pants, Shirt |
| 9 | Do repeat buyers (>5 purchases) subscribe more? | 958 subscribed vs. 2,518 non-subscribed repeat buyers |
| 10 | Revenue contribution by age group? | Middle-Aged customers contribute **AED 132,638** — the largest share |

<details>
<summary>Example query — Customer Segmentation</summary>

```sql
WITH Customer_Segment AS (
    SELECT customer_id, previous_purchases,
           CASE WHEN previous_purchases <= 1 THEN 'New'
                WHEN previous_purchases BETWEEN 2 AND 10 THEN 'Returning'
                ELSE 'Loyal' END AS Segment
    FROM customer_shopping_behavior
)
SELECT Segment, COUNT(*) AS Total_Customers
FROM Customer_Segment
GROUP BY Segment;
```
</details>

---

## 📈 Power BI Dashboard

An interactive dashboard consolidates all findings with slicers for **Subscription Status**, **Gender**, **Category**, and **Shipping Type**.

![Customer Shopping Insights Dashboard](https://raw.githubusercontent.com/balmahendran/Customer_Shopping_Insights_Dashboard/main/assets/dashboard.png)

**Highlights:**
- 3.9K total customers · AED 233.08K total revenue · 3.75 average review rating
- Category-wise revenue breakdown (Clothing leads at AED 104.26K)
- Payment method distribution across 6 channels
- Revenue and sales distribution by age group

---

## 💡 Business Recommendations

- **Boost Subscriptions** — Promote exclusive benefits for subscribers.
- **Customer Loyalty Programs** — Reward repeat buyers to move them into the "Loyal" segment.
- **Review Discount Policy** — Balance sales boosts with margin control.
- **Product Positioning** — Highlight top-rated and best-selling products in campaigns.
- **Targeted Marketing** — Focus efforts on high-revenue age groups and express-shipping users.

---

## 📁 Repository Structure

```
customer-shopping-insights-dashboard/
├── Raw Data/
│   └── customer_shopping_behavior.csv
├── notebooks/
│   └── data_cleaning_and_eda.ipynb
├── sql/
│   └── business_analysis_queries.sql
├── powerbi/
│   └── customer_shopping_insights.pbix
├── assets/
│   └── dashboard.png
└── README.md
```

---

## ▶️ How to Run

1. Clone the repository
   ```bash
   git clone https://github.com/<your-username>/customer-shopping-insights-dashboard.git
   ```
2. Install Python dependencies
   ```bash
   pip install pandas sqlalchemy pyodbc
   ```
3. Run the cleaning notebook to prepare the dataset and load it into SQL Server
4. Execute the queries in `sql/business_analysis_queries.sql`
5. Open `powerbi/customer_shopping_insights.pbix` in Power BI Desktop to explore the dashboard

---

## 👤 Author

**Bal Mahendran Sekar**
📧 balmahendran@gmail.com | 🔗 [LinkedIn](https://linkedin.com/in/balmahendran)
