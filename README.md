# Pizza Sales Data Analysis using SQL

## Overview

This project involves a comprehensive analysis of pizza sales data using SQL. The objective is to uncover business-critical insights such as revenue trends, order behavior, sales distribution, and top-performing products. This analysis provides actionable intelligence for inventory planning, marketing, and operational decision-making.

## Objectives

- Analyze total revenue, orders, and quantity sold.
- Calculate average order value and pizzas per order.
- Identify daily and monthly order trends.
- Determine sales distribution by pizza category and size.
- Highlight best and worst-performing pizzas by revenue, quantity, and orders.

## Dataset

The dataset includes detailed transaction-level sales data:

- **Order Data**: Order ID, order date, quantity.
- **Product Data**: Pizza name, size, and category.
- **Sales Metrics**: Total price, order count, itemized breakdowns.

## Schema

```sql
-- Assumed schema based on queries
CREATE TABLE pizza_sales (
    order_id      INT,
    order_date    DATE,
    pizza_name    VARCHAR(255),
    pizza_size    VARCHAR(10),
    pizza_category VARCHAR(50),
    quantity      INT,
    total_price   DECIMAL(10, 2)
);
```

## Business Problems and SQL Solutions

### 1. Total Revenue

```sql
SELECT SUM(total_price) AS Total_Revenue FROM pizza_sales;
```

**Objective**: Compute the total revenue generated.

---

### 2. Average Order Value

```sql
SELECT (SUM(total_price) / COUNT(DISTINCT order_id)) AS Avg_order_Value 
FROM pizza_sales;
```

**Objective**: Measure average spend per customer order.

---

### 3. Total Pizzas Sold

```sql
SELECT SUM(quantity) AS Total_pizza_sold FROM pizza_sales;
```

**Objective**: Total number of pizzas sold.

---

### 4. Total Orders

```sql
SELECT COUNT(DISTINCT order_id) AS Total_Orders FROM pizza_sales;
```

**Objective**: Count all unique orders.

---

### 5. Average Pizzas Per Order

```sql
SELECT CAST(CAST(SUM(quantity) AS DECIMAL(10,2)) / 
CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS DECIMAL(10,2)) 
AS Avg_Pizzas_per_order 
FROM pizza_sales;
```

**Objective**: Calculate average quantity of pizzas per order.

---

### 6. Daily Trend for Total Orders

```sql
SELECT DATENAME(DW, order_date) AS order_day, COUNT(DISTINCT order_id) AS total_orders 
FROM pizza_sales 
GROUP BY DATENAME(DW, order_date);
```

**Objective**: Analyze which days see more orders.

---

### 7. Monthly Trend for Orders

```sql
SELECT DATENAME(MONTH, order_date) AS Month_Name, COUNT(DISTINCT order_id) AS Total_Orders 
FROM pizza_sales 
GROUP BY DATENAME(MONTH, order_date);
```

**Objective**: Understand seasonality and monthly demand.

---

### 8. % of Sales by Pizza Category

```sql
SELECT pizza_category, 
       CAST(SUM(total_price) AS DECIMAL(10,2)) AS total_revenue, 
       CAST(SUM(total_price) * 100.0 / (SELECT SUM(total_price) FROM pizza_sales) AS DECIMAL(10,2)) AS PCT 
FROM pizza_sales 
GROUP BY pizza_category;
```

**Objective**: Show revenue contribution by category.

---

### 9. % of Sales by Pizza Size

```sql
SELECT pizza_size, 
       CAST(SUM(total_price) AS DECIMAL(10,2)) AS total_revenue, 
       CAST(SUM(total_price) * 100.0 / (SELECT SUM(total_price) FROM pizza_sales) AS DECIMAL(10,2)) AS PCT 
FROM pizza_sales 
GROUP BY pizza_size 
ORDER BY pizza_size;
```

**Objective**: Understand size-wise performance.

---

### 10. Total Pizzas Sold by Pizza Category in February

```sql
SELECT pizza_category, SUM(quantity) AS Total_Quantity_Sold 
FROM pizza_sales 
WHERE MONTH(order_date) = 2 
GROUP BY pizza_category 
ORDER BY Total_Quantity_Sold DESC;
```

**Objective**: Month-specific category analysis.

---

### 11. Top 5 Pizzas by Revenue

```sql
SELECT TOP 5 pizza_name, SUM(total_price) AS Total_Revenue 
FROM pizza_sales 
GROUP BY pizza_name 
ORDER BY Total_Revenue DESC;
```

**Objective**: Identify top money-makers.

---

### 12. Bottom 5 Pizzas by Revenue

```sql
SELECT TOP 5 pizza_name, SUM(total_price) AS Total_Revenue 
FROM pizza_sales 
GROUP BY pizza_name 
ORDER BY Total_Revenue ASC;
```

**Objective**: Identify underperforming products by revenue.

---

### 13. Top 5 Pizzas by Quantity

```sql
SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold 
FROM pizza_sales 
GROUP BY pizza_name 
ORDER BY Total_Pizza_Sold DESC;
```

**Objective**: Find most frequently ordered pizzas.

---

### 14. Bottom 5 Pizzas by Quantity

```sql
SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold 
FROM pizza_sales 
GROUP BY pizza_name 
ORDER BY Total_Pizza_Sold ASC;
```

**Objective**: Least demanded pizzas.

---

### 15. Top 5 Pizzas by Total Orders

```sql
SELECT TOP 5 pizza_name, COUNT(DISTINCT order_id) AS Total_Orders 
FROM pizza_sales 
GROUP BY pizza_name 
ORDER BY Total_Orders DESC;
```

**Objective**: Track customer order preference.

---

### 16. Bottom 5 Pizzas by Total Orders

```sql
SELECT TOP 5 pizza_name, COUNT(DISTINCT order_id) AS Total_Orders 
FROM pizza_sales 
GROUP BY pizza_name 
ORDER BY Total_Orders ASC;
```

**Objective**: Least ordered pizza items.

---

## Findings and Conclusion

- **Revenue Trends**: Revenue is concentrated among a few top-performing pizzas.
- **Customer Behavior**: Customers typically order multiple pizzas per order, indicating group purchases.
- **Category & Size Preferences**: Certain sizes and categories outperform others.
- **Sales Patterns**: Weekends and specific months see spikes in order volumes.

This analysis enables data-driven decisions for promotions, inventory, and menu optimization.
