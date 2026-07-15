---
name: credit-score-check-and-tier-update
description: Check customer credit score and update tier based on results
---

When using `credit_check`, the ORDER_COUNT parameter must be the count of **verified/completed orders only**, not total orders. Query the database first to get the correct count of orders with verified/completed status before calling `credit_check`.

**Critical**: Passing the wrong order count (e.g., total orders instead of verified/completed) will cause `credit_check` to fail with "order_count verification failed" error.

**Correct pattern**:
1. Query database: `SELECT COUNT(*) FROM orders WHERE customer_id = X AND status IN ('verified', 'completed')`
2. Use that count in `credit_check(CUSTOMER_ID, ORDER_COUNT)`
3. Then call `update_crm(CUSTOMER_ID, CREDIT_SCORE, NEW_TIER)` with the returned credit_score.
