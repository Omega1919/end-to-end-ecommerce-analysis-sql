# 📦 Order Analysis

## Total Orders
```sql
SELECT COUNT(order_item_id) AS total_orders
FROM s_order;
```

## Average Order Value (AOV)
```sql
SELECT AVG(price_usd) AS AOV
FROM order_items;
```

---
