# Olist Ecommerce Analysis

## Task 1: Count orders for each order_status. Show the most common status first.
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
