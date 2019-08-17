# Aggregation

##### Count 

```sql
SELECT COUNT(*) 
FROM [table_name] 
/* 如果需要，可以再加上WHERE */
WHERE [condition];          
```

- `COUNT(*)` simply returns the number of rows
- `COUNT([column_name])` counts the number of non-null members in the column.
- Finally, `COUNT(DISTINCT column_name)` counts the number of *different* members in the column.

```sql
SELECT COUNT(DISTINCT [column_name]) 
FROM [table_name] 
```

* Find the total number of members  in [column_name]

##### Group

```sql
SELECT [column_name], COUNT(*) 
		FROM [table_name] 
		WHERE [condition]
		GROUP BY [column_name]
		HAVING [condition]
ORDER BY [column_name]; /* 排序 */
```

* In this case, we're saying 'for each distinct members in [column_name], get me the number of times that members appears'.

* Remember that aggregation happens after the `WHERE` clause is evaluated: we thus use the `WHERE` to restrict the data we aggregate over.

* The behaviour of `HAVING` is easily confused with that of `WHERE`. The best way to think about it is that in the context of a query with an aggregate function, `WHERE` is used to filter what data gets input into the aggregate function, while `HAVING` is used to filter the data once it is output from the function. 



