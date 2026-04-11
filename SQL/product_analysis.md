# 🛍️ Product Analysis

## Product Performance
```sql
SELECT 
    product_name, 
    SUM(cl_price) AS total_revenue,
    COUNT(order_order_id) AS total_orders
FROM us_order_1
GROUP BY product_name
ORDER BY total_revenue DESC;
```

## Product Contribution
```sql
SELECT 
    product_name,
    SUM(cl_price) AS total_revenue,
    SUM(cl_price) * 100 / (SELECT SUM(cl_price) FROM us_order_1) AS product_contribution
FROM us_order_1
GROUP BY product_name
ORDER BY product_contribution DESC;
```
