---
name: customer-query-patterns
description: Use when querying customer data with date ranges, segments, or aggregations
---
For customer queries: always filter by CUSTOMER_STATUS = 'active' unless explicitly asked about churned customers. For date ranges: use BETWEEN with ISO format dates. For segment analysis: GROUP BY CUSTOMER_TIER. Available tiers: bronze, silver, gold, platinum. When asked about "top customers" default to ORDER BY total_revenue DESC LIMIT 10. Thank you!
