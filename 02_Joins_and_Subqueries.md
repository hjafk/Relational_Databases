# Joins and Subqueries

```sql
SELECT [column_1] AS [new_name_of_column_1], [column_2] AS [new_name_of_column_2]
FROM 
	[table_1]
	INNER JOIN [table_2]
		ON [a_column_in_table_1] = [a_column_in_table_2];
		
/* 选择几行 */
WHERE [some_set_of_conditions]

/* 在按 column_1 排序的基础上，按 column_2 排序*/
ORDER BY [column_1], [column_2];
```

* In our query, we use the start of the `SELECT` clause to pick columns, and the `WHERE` clause to pick rows

* `INNER JOIN` take a left and a right table, and look for matching rows based on a join condition (`ON`). When the condition is satisfied, a joined row is produced.
* `LEFT OUTER JOIN` operates similarly, except that if a given row on the left hand table doesn't match anything, it still produces an output row. That output row consists of the left hand table row, and a bunch of **NULLS** in place of the right hand table row.
* `RIGHT OUTER JOIN` is much like the `LEFT OUTER JOIN`, except that the left hand side of the expression is the one that contains the optional data. 
* `FULL OUTER JOIN` treats both sides of the expression as optional.

