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
