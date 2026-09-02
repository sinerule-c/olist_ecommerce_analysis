# Olist Ecommerce Analysis

### Task 1: Count orders for each order_status. Show the most common status first.
```
SELECT
	order_status,
    COUNT(*) AS total_orders,
    ROUND(COUNT(*)/(SELECT COUNT(*) FROM orders) * 100, 2) AS percentage_status
FROM orders
GROUP BY order_status
ORDER BY total_orders DESC;
```

| order_status | total_orders   | percentage_status |
|----|--------|------------|
| delivered | 96478 | 97.02 |
| shipped | 1107 | 1.11 |
| canceled | 625 | 0.63 |
| unavailable | 609 | 0.61 |
| invoiced | 314 | 0.32 |
| processing | 301 | 0.30 |
| created | 5 | 0.01 |
| approved | 2 | 0.00 |

<br>

### Task 2: Show total orders, delivered orders, and the percentage of orders marked 'delivered'. Round the percentage to 2 decimal places.

```
SELECT
	COUNT(*) AS total_orders,
    (SELECT 
		COUNT(*) 
        FROM orders 
        WHERE order_status = 'delivered') AS delivered_orders,
    ROUND(
		COUNT(CASE WHEN order_status = 'delivered' THEN 1 END)/ COUNT(*) * 100, 
        2) AS percentage_delivered
FROM orders;
```

| total_orders | delivered_orders | percentage_delivered |
|--------------| ---------------- | -------------------- |
| 99441 | 96478 | 97.02 |

<br>

### Task 3: Count delivered orders by purchase month using order_purchase_timestamp. Keep different years separate and show the earliest month first.

```
SELECT
	EXTRACT(YEAR FROM order_purchase_timestamp) AS purchase_year,
	EXTRACT(MONTH FROM order_purchase_timestamp) AS purchase_month,
	COUNT(*) AS delivered_orders
FROM orders
WHERE order_status = 'delivered'
GROUP BY purchase_month, purchase_year
ORDER BY purchase_year ASC, purchase_month ASC;
```

| purchase_year | purchase_month | delivered_orders |
|---------------|----------------|------------------|
| 2016 | 9 | 1 |
| 2016 | 10 | 265 |
| 2016 | 12 | 1 |
| 2017 | 1 | 750 |
| 2017 | 2 | 1653 |
| 2017 | 3 | 2546 |
| 2017 | 4 | 2303 |
| 2017 | 5 | 3546 |
| 2017 | 6 | 3135 |
| 2017 | 7 | 3872 |
| 2017 | 8 | 4193 |
| 2017 | 9 | 4150 |
| 2017 | 10 | 4478 |
| 2017 | 11 | 7289 |
| 2017 | 12 | 5513 |
| 2018 | 1 | 7069 |
| 2018 | 2 | 6555 |
| 2018 | 3 | 7003 |
| 2018 | 4 | 6798 |
| 2018 | 5 | 6749 |
| 2018 | 6 | 6099 |
| 2018 | 7 | 6159 |
| 2018 | 8 | 6351 |

<br>

### Task 4: Find the top 5 customer states by number of orders. Show customer_state and the order count.

<br>

### Task 5: Find customers who placed more than one order. Show customer_unique_id and their order count, highest first.

<br>

### Task 6: For each state, calculate the percentage of its orders marked 'unavailable'. Include only states with at least 1,000 total orders. Sort by percentage, highest first.
