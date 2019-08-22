# Query

## 1. Basic Select

```sql
SELECT attribute_list 
FROM table_list;
```

e.g. Retrieve everything from a table

```sql
SELECT * 
FROM table_list;
```

e.g. Retrieve specific attributes from a table

```sql
SELECT attribute_1, attribute_2 
FROM table_list;
```



## 2. Basic Condition

```sql
SELECT attribute_list 
FROM table_list 
WHERE condition;
```

* In our query, we use the start of the `SELECT` clause to pick columns, and the `WHERE` clause to pick rows

e.g. Basic string searches

```sql
SELECT some_attributes
FROM table_list
WHERE attribute_name LIKE 'something you want to search'; 
```

* `LIKE` operator provides simple pattern matching on strings. it just takes a string with the `%` character matching any string, and `_` matching any single character. 

e.g. Working with dates

```sql
SELECT some_attributes
FROM table_list
WHERE attribute_about_date > 'YYYY-MM-DD';
```

- SQL timestamps are formatted in descending order of magnitude: `YYYY-MM-DD HH:MM:SS.nnnnnn`. We can compare them just like we might a unix timestamp, although getting the differences between dates is a little more involved (and powerful!). 

## 3. Temporary name, removing duplicates, and ordering results

```sql
SELECT DISTINCT attribute_1 AS attribute_1_new_name
FROM table_name
ORDER BY attribute_1 DESC
LIMIT a_number; 
```

- `AS` operator is simply used to label attributes or expressions, to make them display more nicely or to make them easier to reference when used as part of a subquery.
- Specifying `DISTINCT` after `SELECT` removes duplicate rows from the result set. 
- Specifying `ORDER BY` (after the `FROM` and `WHERE` clauses, near the end of the query) allows results to be ordered by a attribute or set of attributes (comma separated).
  - ASC indicates ascending order (default). 
  - DESC indicates descending order.
- The `LIMIT` keyword allows you to limit the number of results retrieved. 

## 4. Aggregation

```sql
SELECT attribute_list 
FROM table_list 
[WHERE condition] 
[GROUP BY attribute_list 
	[HAVING group_condition]] 
[ORDER BY attribute_list];
```

`GROUP BY` attribute list groups tuples for each value combination in the attribute list.

Aggregate functions can be applied to aggregate a group of attribute values into a single value, e.g., 

- `COUNT` returns the total number of argument values
- `COUNT(attribute_name)` counts the number of non-null members in the attribute.
- `COUNT(DISTINCT attribute_name)` counts the number of *different* members in the attribute.
- `AVG` returns the average of argument values 
- `MIN` returns the minimum value of the arguments
- `MAX` returns the maximum value of the arguments 
- `SUM` returns the sum of the argument values

We can use `HAVING` condition to add the condition on the groups.

`WHERE` is used to filter what data gets input into the aggregate function, while `HAVING` is used to filter the data once it is output from the function. 

e.g. Return the max value of attribute_1.

```sql
SELECT MAX(attribute_1) AS attribute_1_new_name 
FROM table_name;  
```

e.g. Return a row where attribute_3 is max.

```sql
SELECT attribute_1, attribute_2, attribute_3
	FROM table_name
	WHERE attribute_3 = 
		(select MAX(attribute_3) 
			FROM table_name);   
```

- We use a *subquery* to find out what the max [attribute name 3] is. This subquery returns a *scalar* table - that is, a table with a single attribute and a single row. Since we have just a single value, we can substitute the subquery anywhere we might put a single constant value. In this case, we use it to complete the `WHERE` clause of a query to find a given member.

## 5. Set Operations

1. SQL incorporates several set operations: `UNION` (set union) and `INTERSECT` (set intersection), and sometimes `EXCEPT` (set difference / minus).

2. Set operations result in return of a relation of tuples (no duplicates).

3. Set operations apply to relations that have the same attribute types appearing in the same order.

```sql
First query
UNION/ INTERSECT/ EXCEPT
Second query
```

- The `UNION` operator does what you might expect: combines the results of two SQL queries into a single table. The caveat is that both results from the two queries must have the same number of attributes and compatible data types.
- `UNION` removes duplicate rows, while `UNION ALL` does not. Use `UNION ALL` by default, unless you care about duplicate results.

## 6. Joins Operations

```sql
SELECT attribute_list 
FROM table_1 INNER JOIN table_2 
	ON a_attribute_in_table_1 = a_attribute_in_table_2
[WHERE condition] 
[GROUP BY attribute_list 
	[HAVING group_condition]] 
[ORDER BY attribute_list];
```

- When we want to retrieve data from more than one relations, we often need to use join operations.
- `INNER JOIN` take a left and a right table, and look for matching rows based on a join condition (`ON`). When the condition is satisfied, a joined row is produced.
- `LEFT OUTER JOIN` operates similarly, except that if a given row on the left hand table doesn't match anything, it still produces an output row. That output row consists of the left hand table row, and a bunch of **NULLS** in place of the right hand table row.
- `RIGHT OUTER JOIN` is much like the `LEFT OUTER JOIN`, except that the left hand side of the expression is the one that contains the optional data. 
- `FULL OUTER JOIN` treats both sides of the expression as optional.

## 7. Subqueries

Subqueries can be viewed as temporary tables (usually in conjunction with aliases and renaming, exist only for the query).

Subqueries can be speciﬁed within the FROM-clause.

Subqueries can also be speciﬁed within the WHERE-clause, e.g.,

* `IN` subquery tests if tuple occurs in the temporary table of the subquery.

* `EXISTS` subquery tests whether the temporary table of the subquery is empty or not.

* using `ALL`,` SOME` or `ANY` before a subquery makes subqueries usable in comparison formulae (`SOME` and `ANY` are interchangeable).

* in all these cases the condition involving the subquery can be negated using a preceding `NOT`.

### Matching against multiple possible values

```sql
SELECT attribute_list 
FROM table_list;
WHERE attribute_1 = number1 OR attribute_1 = number2 OR attribute_1 = number3;
```

* The `IN` operator takes a list of possible values, and matches them against the attribute.  It equlas:

```sql
SELECT attribute_list 
FROM table_list;
WHERE attribute_1 IN (number1,number2,number3);
```

* `IN` 后面的内容可以换成一个返回对应元素种类的 Subquery.







