# Exercise 4

Two tables:

```text
customers          orders
id | name          id | customer_id | total
1  | Ana           1  | 2           | 40.00
2  | Bruno         2  | 3           | 15.00
3  | Carla         3  | 3           | 22.50
```

Ana has no orders yet.

The query below is supposed to list every customer with the number of orders
they placed. It lists two customers.

```sql
SELECT c.name, COUNT(o.id) AS orders
FROM customers c
INNER JOIN orders o ON o.customer_id = c.id
GROUP BY c.name;
```

## Task

1. Explain why Ana is missing from the result.
2. Rewrite the query with a LEFT JOIN so Ana appears with `0`.
3. Say what `COUNT(*)` would return for Ana, and why `COUNT(o.id)` is the right
   count here.
