# Basic

## 1.Select

```sql
SELECT [some set of columns] 
FROM [some table or group of tables]
```

##### Retrieve everything from a table

```sql
SELECT * 
FROM [table name];
```

##### Retrieve specific columns from a table

```sql
SELECT [column name 1, column name 2] 
FROM [table name];
```



## 2.Control which rows are retrieved

```sql
SELECT [some columns] 
FROM [table name]
WHERE [condition1] AND [condition2];  
```

##### Basic string searches

```sql
SELECT [some columns] 
FROM [table name]
WHERE [column name] LIKE ['something you want to search']; 
```

* `LIKE` operator provides simple pattern matching on strings. it just takes a string with the `%` character matching any string, and `_` matching any single character. 

##### Matching against multiple possible values

```sql
SELECT [some columns] 
FROM [table name]
WHERE [column name] IN (number1,number2,number3);
```

* The `IN` operator takes a list of possible values, and matches them against the column.  It equlas:

```sql
SELECT [some columns] 
FROM [table name]
WHERE [column name] = number1 OR [column name] = number2 OR [column name] = number3;
```

##### Classify results into buckets

```sql
SELECT [column name 1], 
	CASE 
		WHEN ([condition_1 in column_name_2]) 
			THEN 'result_expression_1'
		WHEN ([condition_2 in column_name_2]) 
			THEN 'result_expression_2'
		ELSE 
			'result_expression_3'
	END 
	AS [new column name 2]  /*give column_2 a new name*/
FROM [table name];  
```

* `CASE` is effectively like if/switch statements in other languages, with a form as shown in the query. To add a 'middling' option, we would simply insert another `when...then` section.

* `AS` operator is simply used to label columns or expressions, to make them display more nicely or to make them easier to reference when used as part of a subquery.

##### Working with dates

```sql
SELECT [some columns] 
FROM [table name]
WHERE [column about date] > 'YYYY-MM-DD';
```

* SQL timestamps are formatted in descending order of magnitude: `YYYY-MM-DD HH:MM:SS.nnnnnn`. We can compare them just like we might a unix timestamp, although getting the differences between dates is a little more involved (and powerful!). 

##### Removing duplicates, and ordering results

```sql
SELECT DISTINCT [column name 1]  
FROM [table name]
ORDER BY [column name 1]  
LIMIT [a number]; 
```

* Specifying `DISTINCT` after `SELECT` removes duplicate rows from the result set. 

* Specifying `ORDER BY` (after the `FROM` and `WHERE` clauses, near the end of the query) allows results to be ordered by a column or set of columns (comma separated).

* The `LIMIT` keyword allows you to limit the number of results retrieved. 

##### Combining results from multiple queries

```sql
SELECT [column name 1]   
	FROM [table name 1]
UNION
SELECT [column name 2]  
	FROM [table name 2];
```

* The `UNION` operator does what you might expect: combines the results of two SQL queries into a single table. The caveat is that both results from the two queries must have the same number of columns and compatible data types.

* `UNION` removes duplicate rows, while `UNION ALL` does not. Use `UNION ALL` by default, unless you care about duplicate results.

##### Simple aggregation

```sql
SELECT MAX([column name 1]) AS [new column name 1] 
FROM [table name];  
```

* The `MAX` aggregate function here is very simple: it receives all the possible values for joindate, and outputs the one that's biggest. 

##### More aggregation

```sql
/*return a row where [column name 3] is max.*/
SELECT [column name 1], [column name 2], [column name 3]
	FROM [table name]
	WHERE [column name 3] = 
		(select MAX([column name 3]) 
			FROM [table name]);   
```

* We use a *subquery* to find out what the max [column name 3] is. This subquery returns a *scalar* table - that is, a table with a single column and a single row. Since we have just a single value, we can substitute the subquery anywhere we might put a single constant value. In this case, we use it to complete the `WHERE` clause of a query to find a given member.

