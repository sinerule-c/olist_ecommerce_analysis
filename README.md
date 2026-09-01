# Olist Ecommerce Analysis

## Analysis 1: Monthly marketplace performance

Business Question:
  - How did completed order volume, merchandise value and average order value change over time?

WITH order_totals AS (
    SELECT
        order_id,
        SUM(price) AS item_value,
        SUM(freight_value) AS freight_value
    FROM order_items
    GROUP BY order_id
)

SELECT
    DATE_FORMAT(
        o.order_purchase_timestamp,
        '%Y-%m'
    ) AS order_month,

    COUNT(DISTINCT o.order_id) AS delivered_orders,

    ROUND(
        SUM(ot.item_value),
        2
    ) AS delivered_item_value,

    ROUND(
        AVG(ot.item_value),
        2
    ) AS average_order_value

FROM orders o

INNER JOIN order_totals ot
    ON o.order_id = ot.order_id

WHERE o.order_status = 'delivered'

GROUP BY order_month
ORDER BY order_month;
