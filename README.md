# Retail Sales Analysis My SQL Project


### 1. Database Setup

- **Database Creation**: The project starts by creating a database named "Sales".
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE Sales;

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);
```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

Q.1 **Write a SQL query to retrieve all columns for sales made on '2022-11-05**

```sql
select *
from retail_sales
where sale_date = '2022-11-05';
```

Q.2 **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 10 in the month of Nov-2022**

```sql
select *
from retail_sales
where category = 'Clothing' and
       quantity>=4 and
       month(sale_date) = 11 and
       year(sale_date) = 2022;
```

Q.3 **Write a SQL query to calculate the total sales (total_sale) for each category.**

```sql
select category,count(total_sale)as number,sum(total_sale) as sales_amount
from retail_sales 
group by category;
```

Q.4 **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**

```sql
select avg(age) as average_age
from retail_sales 
where category = 'Beauty';
```

Q.5 **Write a SQL query to find all transactions where the total_sale is greater than 1000.**

 ```sql
select * 
from retail_sales
where total_sale > 1000;
```

Q.6 **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**

```sql
select gender,category, count(*) as total_transaction
from retail_sales
group by gender,category
order by gender;
```

Q.7 **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**

```sql
select year(sale_date)as year,
	   monthname(sale_date)as month,
	   avg(total_sale) as aversge_sale
from retail_sales
group by monthname(sale_date),year(sale_date)
order by avg(total_sale) desc;
```

Q.8 **Write a SQL query to find the top 5 customers based on the highest total sales**

```sql
select customer_id,sum(total_sale) as sales_amount
from retail_sales
group by customer_id
order by sales_amount desc
limit 5;
```

Q.9 **Write a SQL query to find the number of unique customers who purchased items from each category.**

```sql             
select category,count(distinct customer_id) as customers
from retail_sales
group by category;
```

Q.10 **Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)**

```sql
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
GROUP BY shift;
```

## 🔍 **Findings**
- 📅 July 2022 had the **highest sales** with revenue of `$22195`.  
- 🛒 The **top-selling category** was *Electronics* with 678 units sold for `$311445`.  
- 👤 The **most valuable customer** was *Customer ID: 3* contributing `$38,440`.  





