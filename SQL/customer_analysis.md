# 👥 Customer Analysis

## Total Customers
```sql
SELECT COUNT(DISTINCT user_id) AS total_customers
FROM us_order_1;
```

## Customer Retention
#Total Repeated Order
```sql
SELECT count(*)
from order_items 
where is_primary_item is 1
```

#Total Primary Order

```sql
SELECT count(*)
from order_items 
where is_primary_item is 0
```
## Top 10 Customers Contribution
```sql
SELECT SUM(total_revenue) 
FROM (
    SELECT user_id, SUM(cl_price) AS total_revenue
    FROM us_order_1
    GROUP BY user_id
    ORDER BY total_revenue DESC
    LIMIT 10
) bb;
```
