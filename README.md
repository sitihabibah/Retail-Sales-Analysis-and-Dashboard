# Retail Sales Analytics & Insights (SQL + Tableau Project)

## Project Overview

This project focuses on implementing an end-to-end data analysis workflow, bridging relational database management with interactive business intelligence. Designed to simulate the real-world responsibilities of a Data Analyst, this case study covers database schema creation, data cleaning, and advanced SQL querying. To maximize business value, the SQL database was directly connected to Tableau to build an interactive executive dashboard, translating raw queries into visual, actionable insights for data-driven decision-making.

**Role**: Data Analysis  
**Tools Used**: PostgreSQL, Tableau Desktop  
**Key Skills**: DDL/DML, Data Cleansing, Window Functions `(RANK)`, CTEs, Database Integration, Dashboard Design, & KPI Data Visualization.

## Project Objectives

1. **Database Architecture**: Design and deploy a structured relational schema to efficiently store retail transaction records.
2. **Data Cleansing**: Identify, audit, and handle missing values (NULL data) while rectifying typographical schema errors.
3. **Exploratory Data Analysis (EDA)**: PComprehend dataset characteristics, including total sales volume, unique customer reach, and product category distributions.
4. **Business Intelligence Queries**: Formulate and execute 10 advanced SQL queries to solve specific retail business problems and generate management reports.
5. **Tableau Integration & Visualization**: Connect the processed SQL data to Tableau and build a comprehensive dashboard to visualize high-level sales trends and consumer demographics.

## Project Architecture & Workflow

### 1. Database Setup & Schema Design

The initial phase involves setting up the database instance `sql_project_p2` and defining the structural boundaries for the `retail_sales` table using optimized data types

```sql
CREATE DATABASE sql_project_p2;

CREATE TABLE retail_sales (
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(15),
    quantiy INT, -- Typo from raw source (rectified during data cleaning)
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
```

### 2. Data Cleaning & Quality Assurance

To prevent skewed metrics and guarantee data integrity prior to analysis:
- **Schema Correction**: Standardized the mislabeled column `quantiy` to `quantity` using an `ALTER TABLE` statement.
- **Null Value Eradication**: Filtered and audited all primary operational columns. Records containing vital missing information (`NULL`) were purged to ensure aggregation accuracy.

### Data Visualization & Executive Dashboard

The cleaned SQL database was connected directly to Tableau to create the Retail Sales Dashboard, which serves as a single source of truth for executive stakeholders.

### 1. Core KPIs (Key Performance Indicators)

The dashboard highlights three high-level business metrics at a glance:
- **Total Sales**: $459K, representing the gross revenue generated.
- **Total Profit**: $216K, indicating healthy profit margins across product lines.
- **Customer Footprint**: 147 unique customers driving the operational volume.

### 2. Core KPIs (Key Performance Indicators)

The dashboard highlights three high-level business metrics at a glance:
- **Total Sales**: $459K, representing the gross revenue generated.
- **Total Profit**: $216K, indicating healthy profit margins across product lines.
- **Customer Footprint**: 147 unique customers driving the operational volume.

### 2. Visual Breakthroughs & Charts

The analytical framework was structured to decode key management requirements:
- **Sales by Month (Trend Analysis)**: A line chart mapping the sales velocity from January to December. It visually showcases performance consistency with a noticeable growth peak starting in September.
- **Sales by Product (Category Performance)**: A horizontal bar chart evaluating the top-performing categories (Beauty, Clothing, Electronics) against the average benchmark line. Clothing and Electronics emerge as dominant revenue drivers.
- **Sales by Gender (Demographic Split)**: A donut chart breaking down market share by gender, revealing a highly balanced customer base with Males contributing $234K and Females contributing $225K to total sales.
- **Sales by Age Group (Stacked Breakdown)**: A stacked column chart bucketing customers into 5 generational segments (Teen, Young, Adult, Mid-Age, Senior). The Senior (50+) segment leads gross revenue at $149K, followed closely by Adults (30-39) at $105K

### Key Insights & Business Findings
- **Consumer Behavior Patterns**: Time-of-day and seasonal line trends reveal explicit operational peaks (such as the Q3-Q4 sales climb), allowing for optimized inventory prep and targeted marketing campaigns.
- **Demographic Powerhouse**: While sales are evenly split by gender, purchasing power is heavily concentrated in the Senior (50+) and Adult (30-39) age brackets, suggesting marketing spend should be tailored toward these high-yield groups.
- **Product Alignment**: Product optimization should prioritize Clothing and Electronics, which consistently meet or exceed average sales expectations across retail branches

### Complete SQL Script (Portfolio Ready)

```sql
-- ====================================================================
-- 1. DATABASE & TABLE SETUP
-- ====================================================================
CREATE DATABASE sql_project_p2;

CREATE TABLE retail_sales (
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(15),
    quantiy INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);

-- Quick Data Verification
SELECT * FROM retail_sales LIMIT 10;
SELECT COUNT(*) FROM retail_sales;

-- ====================================================================
-- 2. DATA CLEANING & SCHEMA CORRECTION
-- ====================================================================
ALTER TABLE retail_sales RENAME COLUMN quantiy TO quantity;

DELETE FROM retail_sales
WHERE 
    transactions_id IS NULL OR sale_date IS NULL OR customer_id IS NULL OR
    sale_time IS NULL OR gender IS NULL OR age IS NULL OR category IS NULL OR
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL OR total_sale IS NULL;

-- ====================================================================
-- 3. BUSINESS ANALYSIS & INSIGHTS (CONNECTED TO TABLEAU)
-- ====================================================================

 -- Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05

SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';


-- Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022

SELECT 
  *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4


-- Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.

SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1

-- Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.

SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty'


-- Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.

SELECT * FROM retail_sales
WHERE total_sale > 1000


-- Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.

SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP 
    BY 
    category,
    gender
ORDER BY 1


-- Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year

SELECT 
       year,
       month,
    avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales
GROUP BY 1, 2
) as t1
WHERE rank = 1
    
-- ORDER BY 1, 3 DESC

-- Q.8 Write a SQL query to find the top 5 customers based on the highest total sales 

SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5

-- Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.


SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category



-- Q.10 Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)

WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift

-- End of project

```

### Contact & Portfolio
- **LinkedIn**: [Connect with me professionally](https://www.linkedin.com/in/sitihabibah071)
- **Email**: [Contact me](sitihabibah071@gmail.com)
