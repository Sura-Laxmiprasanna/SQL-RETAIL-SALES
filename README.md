Retail Sales Data Analysis using SQL

<img width="990" height="374" alt="Screenshot 2<img width="1001" height="323" alt="Screenshot 2025-08-20 000656" src="https<img width="751" height="549" alt="Screenshot 2025-08-20 001038" src="https://github.com/user-attachments/assets/d32457c4-a030-484b-8b37-5458bc2be108" />
://github.com/user-attachments/assets/fc6c59df-bd11-4e00-8a17-0a8<img width="988" height="422" alt="Screenshot 2025-08-20 000917" src="https://github.com/user-attachments/assets/a07f6ff0-656e-4f96-a1bf-6fef3dc820d4" />
1<img width="441" height="474" alt="Screenshot 2025-08-20 000814" src="https://github.com/user-attachments/assets/ff853d41-b6e4-4814-8056-7017bda19541" />
0c06f01b" />
025-08-20 000443" src="https://github.com/user-attachments/assets/6e68dd30-33e8-4b23-afd6-5e0bb262c400" />Retail Sales Data Analysis using SQL

This project demonstrates end-to-end data analysis using SQL on a retail sales dataset. It covers data cleaning, exploration, and business-focused analysis, providing actionable insights for decision-making.
<img width="997" height="273" alt="Screenshot 2025-08-19 235816" src="https://github.com/user-attachments/assets/534569ca-2d92-4464-bb95-1630d16178ce" />


Project Overview

The goal is to analyze sales transactions to:

Identify trends in revenue and customer behavior.

Understand which product categories perform best.

Discover high-value customers and seasonal patterns.

Support business decisions with data-driven recommendations.

<img width="372" height="536" alt="Screenshot 2025-08-20 000103" src="https://github.com/user-attachments/assets/2b871391-e5c3-446f-90ae-c11f87a0f77d" />

Dataset Schema

The project uses a SQL table named retail_sales with the following columns:

Column Name	Data Type	Description
transactions_id	INT (PK)	Unique transaction ID
sale_date	DATE	Date of sale
sale_time	TIME	Time of sale
customer_id	INT	Customer identifier
gender	VARCHAR(15)	Gender of customer
age	INT	Age of customer
category	VARCHAR(15)	Product category
quantiy*	INT	Number of units sold (*typo in column name)
price_per_unit	FLOAT	Price per unit
cogs	FLOAT	Cost of goods sold
total_sale	FLOAT	Total sale amount

Steps Performed
1. Data Cleaning![Uploading Screenshot 2025-08
2. <img width="799" height="427" alt="Screenshot 2025-08-20 000550" src="https://github.com/user-attachments/assets/5de387b4-ed6f-4a8f-a92e-679890b47758" />
-20 000443.png…]()


Checked for missing or NULL values.

Removed invalid transactions:

---DELETE FROM retail_sales
WHERE transactions_id IS NULL
   OR sale_date IS NULL
   OR sale_time IS NULL
   OR gender IS NULL
   OR category IS NULL
   OR quantiy IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;
   ---

2. Data Exploration

Total number of transactions

Unique customers

Product categories

Key SQL Queries
Q1. Sales on a specific date
---SELECT * FROM retail_sales 
WHERE sale_date = '2022-11-05';---

Q2. Clothing transactions with quantity >4 in Nov 2022
---SELECT * FROM retail_sales
WHERE category='clothing'
  AND TO_CHAR(sale_date,'YYYY-MM')='2022-11'
  AND quantiy>=4;---

Q3. Total sales per category
SELECT category, SUM(total_sale) AS net_sale, COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;

Q4. Average age of Beauty category customers
SELECT ROUND(AVG(age),2) AS avg_age
FROM retail_sales
WHERE category='Beauty';

Q5. High-value transactions (>1000)
SELECT * FROM retail_sales
WHERE total_sale>1000;

Q6. Transactions by gender and category
SELECT category, gender, COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;

Q7. Best-selling month (per year)
SELECT year, month, avg_sale
FROM (
    SELECT year, month, avg_sale,
           RANK() OVER (PARTITION BY year ORDER BY avg_sale DESC) AS rnk
    FROM (
        SELECT EXTRACT(YEAR FROM sale_date) AS year,
               EXTRACT(MONTH FROM sale_date) AS month,
               AVG(total_sale) AS avg_sale
        FROM retail_sales
        GROUP BY 1,2
    ) sub
) ranked
WHERE rnk = 1;

Q8. Top 5 customers by total sales
SELECT customer_id, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC 
LIMIT 5;

Q9. Unique customers per category
SELECT category, COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;

Q10. Orders by time shift
WITH hourly_sale AS (
    SELECT *,
           CASE
               WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'morning'
               WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'afternoon'
               ELSE 'evening'
           END AS shift
    FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;

Insights & Recommendations

Data Quality: Missing records were removed to ensure reliable results.

Customer Demographics: Specific categories (e.g., Beauty) attract a defined age group.

Seasonality: Best-selling months can be targeted for promotions and stock planning.

High-Value Customers: Top spenders can be engaged through loyalty programs.

Time-of-Day Trends: Morning vs. evening sales insights can optimize staffing and marketing campaigns.

How to Run the Project

Create a database in PostgreSQL/MySQL (or any RDBMS).

Use the CREATE TABLE script to set up the schema.

Insert your dataset (CSV/SQL dump).

Run each query in sequence to reproduce the analysis.

End of Project

This SQL project demonstrates a complete data workflow: cleaning, exploration, and deriving actionable business insights using structured queries.# SQL-RETAIL-SALES-QUERY
