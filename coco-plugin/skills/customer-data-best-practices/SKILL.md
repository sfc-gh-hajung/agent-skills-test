---
name: customer-data-best-practices
description: "Enforces best practices when querying customer data. Use when: writing SQL for customer tables, joining orders to customers, filtering by status, aggregating revenue. Triggers: customer query, customer data, customer SQL, query customers, join orders."
---

# Customer Data Best Practices

When writing SQL that involves customer or order data, follow these rules:

## Required Filters

- **Always filter orders by status** when counting or aggregating: `WHERE status IN ('verified', 'completed')`. Never include `pending` or `cancelled` orders in counts or revenue calculations unless explicitly asked.
- **Always include a date range** if not specified: default to the last 12 months (`WHERE order_date >= DATEADD(month, -12, CURRENT_DATE())`).

## Join Patterns

- Join orders to customers using: `orders.customer_id = customers.customer_id`
- Always use `LEFT JOIN` from customers to orders (not all customers have orders).
- Never use `SELECT *` — explicitly name columns.

## Naming Conventions

- Use `snake_case` for all aliases.
- Prefix aggregates with the metric name: `total_revenue`, `order_count`, `avg_order_value`.
- Always alias calculated columns.

## Tier Logic

When determining or referencing customer tiers:
- GOLD: credit score >= 750
- SILVER: credit score >= 600 and < 750
- BRONZE: credit score < 600

## Example Query Pattern

```sql
SELECT
    c.customer_id,
    c.name AS customer_name,
    c.current_tier,
    COUNT(o.order_id) AS order_count,
    SUM(o.amount) AS total_revenue
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
    AND o.status IN ('verified', 'completed')
    AND o.order_date >= DATEADD(month, -12, CURRENT_DATE())
GROUP BY c.customer_id, c.name, c.current_tier
ORDER BY total_revenue DESC;
```
