# 💰 Revenue Analysis

## Total Revenue
```sql
SELECT SUM(cl_price) AS total_revenue
FROM us_order_1;
```

## Revenue by Year
```sql
SELECT year, SUM(cl_price) AS total_revenue
FROM us_order_1
GROUP BY year
ORDER BY year;
```

## Year-over-Year Growth
```sql
SELECT 
    year,
    SUM(cl_price) AS revenue,
    LAG(SUM(cl_price)) OVER (ORDER BY year) AS previous_year_revenue,
    (SUM(cl_price) - LAG(SUM(cl_price)) OVER (ORDER BY year)) 
    / LAG(SUM(cl_price)) OVER (ORDER BY year) * 100 AS yoy_growth_percentage
FROM us_order_1
GROUP BY year
ORDER BY year;
ORDER BY product_contribution DESC;
```
