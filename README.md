# Olist Ecommerce Analysis

Dataset: https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce/data

## SQL

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

```
SELECT
	c.customer_state,
    COUNT(*) AS order_count
FROM orders o
JOIN customers c
ON o.customer_id = c.customer_id
GROUP BY c.customer_state
ORDER BY order_count DESC
LIMIT 5;
```
| customer_state | order_count |
|----------------|-------------|
| SP | 41746 |
| RJ | 12852 |
| MG | 11635 |
| RS | 5466 |
| PR | 5045 |

<br>

### Task 5: Find customers who placed more than one order. Show customer_unique_id and their order count, highest first.

```
SELECT
	c.customer_unique_id,
    COUNT(o.order_id) AS order_count
FROM customers c
JOIN orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_unique_id
HAVING COUNT(o.order_id) > 1
ORDER BY order_count DESC;
```
| customer_unique_id | order_count |
|--------------------|-------------|
| 8d50f5eadf50201ccdcedfb9e2ac8455 | 17 |
| 3e43e6105506432c953e165fb2acf44c | 9 |
| 1b6c7548a2a1f9037c1fd3ddfed95f33 | 7 |
| 6469f99c1f9dfae7733b25662e7f1782 | 7 |
| ca77025e7201e3b30c44b472ff346268 | 7 |
| 12f5d6e1cbf93dafd9dcc19095df0b3d | 6 |
| 47c1a3033b8b77b3ab6e109eb4d5fdf3 | 6 |
.
..
...

<br>

### Task 6: For each state, calculate the percentage of its orders marked 'unavailable'. Include only states with at least 1,000 total orders. Sort by percentage, highest first.

```
SELECT
	c.customer_state,
    ROUND(
		COUNT(CASE WHEN o.order_status = 'unavailable' THEN 1 END)/COUNT(*) * 100
        , 2) AS percentage_unavailable
FROM customers c
JOIN orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_state
HAVING COUNT(order_id) >= 1000
ORDER BY percentage_unavailable DESC;
```
| customer_state | percentage_unavailable |
|----------------|------------------------|
| PR | 0.79 |
| SP | 0.70 |
| MG | 0.64 |
| BA | 0.59 |
| DF | 0.56 |
| RJ | 0.53 |
| SC | 0.49 |
| GO | 0.45 |
| CE | 0.45 |
| RS | 0.44 |
| ES | 0.30 |
| PE | 0.24 |

<br>

### Task 7: Using only order_items, show each order’s item count, total item value, and total freight. Sort by total item value, highest first.

```
SELECT
    order_id,
    COUNT(*) AS item_count,
    ROUND(SUM(price), 2) AS total_item_value,
    ROUND(SUM(freight_value), 2) AS total_freight
FROM order_items
GROUP BY order_id
ORDER BY total_item_value DESC, total_freight DESC;
```
| order_id | item_count | total_item_value | total_freight |
|---|---|---|---|
| 03caa2c082116e1d31e67e9ae3700499 | 8 | 13440.00 | 224.08 |
| 736e1922ae60d0d6a89247b851902527 | 4 | 7160.00 | 114.88 |
| 0812eb902a67711a1cb742b3cdaa65ae | 1 | 6735.00 | 194.31 |
| fefacc66af859508bf1a7934eab1e97f | 1 | 6729.00 | 193.21 |
| f5136e38d1a14a4dbd87dff67da82701 | 1 | 6499.00 | 227.66 |
.
..
...

<br>

### Task 8: For delivered orders only, find the top 10 sellers by total item value. Also show their item count and average item price.

```
SELECT
    a.seller_id,
    COUNT(*) AS total_count,
    ROUND(SUM(a.price), 2) AS total_price,
    ROUND(AVG(a.price), 2) AS average_price
FROM order_items a
JOIN orders b
ON a.order_id = b.order_id
WHERE b.order_status = 'delivered'
GROUP BY a.seller_id
ORDER BY total_count DESC
LIMIT 10;
```
| seller_id | total_count | total_price | average_price |
|---|---|---|---|
| 6560211a19b47992c3666cc44a7e94c0 | 1996 | 120702.83 | 60.47 |
| 4a3ca9315b744ce9f8e9374361493884 | 1949 | 196882.12 | 101.02 |
| 1f50f920176fa81dab994f9023523100 | 1926 | 106655.71 | 55.38 |
| cc419e0650a3c5ba77189a1882b7556a | 1719 | 101090.96 | 58.81 |
| da8622b14eb17ae2831f4ac5b9dab84a | 1548 | 159816.87 | 103.24 |
| 955fee9216a65b617aa5c0531780ce60 | 1472 | 131836.71 | 89.56 |
| 1025f0e2d44d7041d6cf58b6550e0bfa | 1420 | 138208.56 | 97.33 |
| 7c67e1448b00f6e969d365cea6b010ab | 1355 | 186570.05 | 137.69 |
| ea8482cd71df3c1969d7b9473ff13abc | 1188 | 36696.76 | 30.89 |
| 7a67c85e85bb2ce8582c35f2203ad736 | 1155 | 139658.69 | 120.92 |

<br>

### Task 9: For delivered orders only, show each customer state’s distinct order count, item count, and total item value. Sort by total item value, highest first.

```
SELECT
	a.customer_state,
    COUNT(DISTINCT b.order_id) AS order_count,
    COUNT(*) AS item_count,
    ROUND(SUM(c.price), 2) AS total_item_value
FROM customers a
JOIN orders b
	ON a.customer_id = b.customer_id
JOIN order_items c
	ON b.order_id = c.order_id
WHERE order_status = 'delivered'
GROUP BY a.customer_state
ORDER BY total_item_value DESC;
```
| customer_state | order_count | item_count | total_item_value |
|-|-|-|-|
| SP | 40501 | 46448 | 5067633.16 |
| RJ | 12350 | 14143 | 1759651.13 |
| MG | 11354 | 12916 | 1552481.83 |
| RS | 5345 | 6134 | 728897.47 |
| PR | 4923 | 5649 | 666063.51 |
| SC | 3546 | 4097 | 507012.13 |
| BA | 3256 | 3683 | 493584.14 |
| DF | 2080 | 2355 | 296498.41 |


<br>

### Task 10: Calculate the average item value per delivered order for each customer state. Be careful: average item price is not average order value!
