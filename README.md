# 🛍️ Retail Sales Analysis using SQL

This project explores and analyzes retail sales transaction data using SQL. 
From cleaning the data to answering real business questions, it demonstrates the power of SQL in extracting insights from raw data.

---

## 📌 Project Goals

- Create and clean a structured retail sales database
- Use SQL to explore and analyze customer behavior and sales performance
- Extract actionable insights for decision-making

---

## 🧼 Data Cleaning

```sql
DELETE FROM RETAIL_SALES
WHERE 
	TRANSACTIONS_ID IS NULL
	OR
	SALE_DATE IS NULL
	OR
	SALE_TIME IS NULL
	OR
	CUSTOMER_ID IS NULL
	OR
	GENDER IS NULL
	OR
	AGE IS NULL
	OR
	CATEGORY IS NULL
	OR
	QUANTIY IS NULL
	OR
	PRICE_PER_UNIT IS NULL
	OR
	COGS IS NULL
	OR
	TOTAL_SALE IS NULL
```

**Action**: Deleted all rows with NULLs to ensure clean, complete data.

---

## 🔍 DATA EXPLORATION

### 1. How many total sales do we have?

```sql
SELECT COUNT(*) AS TOTAL_SALES FROM RETAIL_SALES;
```

---

### 2. How many unique customers do we have?

```sql
SELECT COUNT(DISTINCT CUSTOMER_ID) AS TOTAL_CUSTOMERS FROM RETAIL_SALES;
```

---

### 3. How many unique product categories do we have?

```sql
SELECT COUNT(DISTINCT CATEGORY) AS TOTAL_CATEGORY FROM RETAIL_SALES;
```

# DATA ANALYSIS

---

### 1. All the Sales made on '2022-11-05'

```sql
SELECT * FROM RETAIL_SALES
WHERE SALE_DATE = '2022-11-05';
```

---

### 2. All of 'Clothing' in category column sold with more than 2 units quantity in Nov 2022

```sql
SELECT * FROM RETAIL_SALES
WHERE CATEGORY = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND QUANTIY >= '2';
```

---

### 3. Total sale for each category

```sql
SELECT 
	CATEGORY,
	SUM(TOTAL_SALE) AS TOTAL_SALE
FROM RETAIL_SALES
GROUP BY 1;
```

---

### 4. Average age of customers in the Beauty category

```sql
SELECT
	ROUND(AVG(AGE), 3) AS AVG_AGE
FROM RETAIL_SALES
WHERE CATEGORY = 'Beauty'
```

---

### 5. Transactions with total sale > ₹1000

```sql
SELECT *
FROM RETAIL_SALES
WHERE TOTAL_SALE > 1000
```

---

### 6. Total transactions by gender in each category

```sql
SELECT 
	CATEGORY,
	GENDER,
	COUNT(TOTAL_SALE) AS TOTAL_SALE_PER_CATEGORY
FROM RETAIL_SALES
GROUP BY 2, 1
ORDER BY 1
```

---

### 7. Average monthly sale & best-selling month of each year

```sql
SELECT YEAR_, MONTH_, TOTAL_SALE_PER_MONTH FROM
(
	SELECT
		EXTRACT(YEAR FROM SALE_DATE) AS YEAR_,
		EXTRACT(MONTH FROM SALE_DATE) AS MONTH_,
		ROUND(AVG(TOTAL_SALE)::NUMERIC, 3) AS TOTAL_SALE_PER_MONTH,
		RANK() OVER(PARTITION BY EXTRACT(YEAR FROM SALE_DATE) ORDER BY AVG(TOTAL_SALE) DESC) AS RANKS
	FROM RETAIL_SALES
	GROUP BY 1, 2
) AS T1
WHERE RANKS = 1
```

---

### 8. Top 5 customers by total sales

```sql
SELECT
	CUSTOMER_ID,
	SUM(TOTAL_SALE) AS TOTAL_S
FROM RETAIL_SALES
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```

---

### 9. Unique customers per category

```sql
SELECT 
	CATEGORY,
	COUNT(DISTINCT(CUSTOMER_ID)) AS UNIQUE_CUSTOMERS
FROM RETAIL_SALES
GROUP BY 1
```

---

### 10. Create 3 shifts (Morning, Afternoon, Evening) and count orders

```sql
WITH HOURLY_SALE AS (
SELECT *,
	CASE
		WHEN EXTRACT(HOUR FROM SALE_TIME) < 12 THEN 'MORNING'
		WHEN EXTRACT(HOUR FROM SALE_TIME) BETWEEN 12 AND 17 THEN 'AFTERNOON'
		ELSE 'EVENING'
	END AS SHIFT
FROM RETAIL_SALES)

SELECT 
	SHIFT,
	COUNT(*) AS SHIFT_COUNT
FROM HOURLY_SALE
GROUP BY 1
```

---

## ✅ Conclusion

SQL can turn raw retail transactions data into actionable business intelligence. The insights help in identifying customer preferences, high-performing categories, top customers, and sales trends over time.

---

## 👨‍💻 Author

**Aakash**  
M.Sc. Physics, IIT Bombay  
Business & Data Analyst | SQL Enthusiast  
📧 aakash.1attri@gmail.com  
