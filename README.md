# Retail Sales Data Engineering & Interactive Dashboard 🛍️📊

An end-to-end data engineering and business intelligence project showcasing advanced database querying using **PostgreSQL** and dynamic executive reporting using **Tableau**. 

The project converts raw transactional retail data into a refined, decision-ready visual dashboard tracking key store performance metrics, customer demographics, and product category trends.

---

## 📌 Business Scenario (STAR Framework)

* **Situation:** A retail business captures daily point-of-sale (POS) transaction records containing customer demographic parameters, product metadata, individual prices, and the cost of goods sold (COGS). However, the raw database suffers from minor formatting anomalies, structural misspellings, and empty records that obscure operational insights.
* **Task:** Act as the core Data Analyst to build a robust data transformation script to sanitize the warehouse tables, conduct comprehensive Exploratory Data Analysis (EDA) to resolve 10 critical business questions, and build an interactive Executive Dashboard.
* **Action:** Programmed structural data engineering updates and analytics calculations in SQL. Connected the output cleanly into Tableau to deliver a unified, interactive filtering experience.
* **Results:** Designed a centralized hub monitoring `$459K` in aggregate sales across distinct consumer age classifications, leading to clear operational recommendations on shifts and localized inventory management.

---

## 🛠️ Data Engineering & Pipeline (PostgreSQL)

The database development process inside `SQL - Retail Sales Analysis_utf .csv` followed standard database procedures:

### 1. Table Schema Creation
Established a strict data structure with appropriate numerical, date, and text constraints to maintain relational warehouse consistency:
```sql
CREATE TABLE retail_sales (
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(15),
    quantiy INT, -- Later altered to quantity
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
```

### 2. Data Cleaning Routine
* **Typo Rectification:** Fixed a structural table misspelling by altering `quantiy` to `quantity` using database commands (`ALTER TABLE retail_sales RENAME COLUMN quantiy TO quantity;`).
* **Null Value Handling:** Isolated and removed corrupt transactional entries containing null markers across critical computation rows using conditional deletion routines (`DELETE FROM retail_sales WHERE ... IS NULL;`).

---

## 🔍 Exploratory Data Analysis (10 Core Business Queries)

The following advanced analytical inquiries were implemented to guide the business planning phase:

* **Q1 (Specific Date Audit):** Filtered all metrics for transactions completed on `'2022-11-05'`.
* **Q2 (High-Volume Apparel Tracking):** Isolated bulk transactions (`Quantity >= 4`) inside the `Clothing` category specifically during `November 2022`.
* **Q3 (Category Aggregations):** Calculated cumulative net sales and order volumes per category using `SUM`, `COUNT`, and `GROUP BY`.
* **Q4 (Demographic Profiling):** Leveraged `ROUND(AVG(age), 2)` to discover the average age profile of customers buying from the `Beauty` department.
* **Q5 (High-Value Premium Orders):** Filtered transactions with a `total_sale` breaking the `$1,000` threshold.
* **Q6 (Cross-Sectional Gender Orders):** Grouped transaction density across gender splits for each separate business vertical.
* **Q7 (Window Function / Best Months):** Built an advanced subquery utilizing the `RANK() OVER (PARTITION BY ... ORDER BY ... DESC)` window function to identify the single highest-grossing month for each calendar year.
* **Q8 (VIP Customer Loyalty):** Extracted the top 5 customers driving maximum profit margins using aggregated order limits (`ORDER BY 2 DESC LIMIT 5`).
* **Q9 (Unique Footfall Metrics):** Evaluated unique brand penetration per vertical via `COUNT(DISTINCT customer_id)`.
* **Q10 (Operational Shift Windowing):** Used a Common Table Expression (`WITH hourly_sale AS ...`) combined with a conditional `CASE WHEN` statement to segregate operational logs into **Morning (<12)**, **Afternoon (12-17)**, and **Evening (>17)** windows to monitor shift volume distribution.

---

## 📊 Tableau Executive Dashboard Insights

The final presentation layout, showcased in `Dashboard 3 - Copy.png`, provides cross-filtered corporate visibility:

* **High-Level Summary Tiles (KPIs):** Displays macro metrics highlighting **$459K Total Sales**, **$216K Total Net Profits**, and **147 Unique Active Customers**.
* **Sales by Month (Trend Line Chart):** Displays revenue fluctuations over the calendar year, showcasing steady growth throughout Q1-Q3 with a major performance breakout beginning in **September** and peaking through **December**.
* **Sales by Product Category (Bar Chart):** Ranks performance across major segments. **Electronics** and **Clothing** run neck-and-neck as the core revenue pillars, both surpassing the store-wide average threshold line, while **Beauty** functions as a smaller secondary category.
* **Sales by Age Group & Product Stack (Stacked Bar Chart):** Segmented into demographics: *Teen (<20)*, *Young (20-29)*, *Adult (30-39)*, *Mid Age (40-49)*, and *Senior (50+)*. **Seniors (50+) represent the highest-value cohort** contributing `$149K`, driven heavily by Electronics and Clothing options.
* **Sales by Gender (Donut Chart):** Shows a balanced market equilibrium, with **Male buyers representing $234K (51%)** and **Female buyers representing $225K (49%)** of overall gross returns.

---

## 📂 Repository Blueprint
* **`SQL - Retail Sales Analysis_utf .csv`**: Cleaned structural tabular data holding complete records used during database querying and visualization tracking.
* **`project_p2.sql`**: Comprehensive script housing table creation, table modification, cleaning steps, and the solutions to the 10 core business questions.
* **`Dashboard 3 - Copy.png`**: High-resolution Tableau visualization canvas mapping the business summary and cross-filtered demographic charts.

---
*Developed by [Siti Habibah](https://github.com/sitihabibah/) as part of a Professional Data Analytics Portfolio.*
