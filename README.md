# SQL_Guide
Easily reference when working with SQL. 

## SQL STRUCTURE

* __SELECT__: Column you want to look at
* __FROM__: Table the data lives in
* __WHERE__: Certain condition on the data
* __GROUP BY__: Column you want to aggregate by
* __HAVING__: Certain condition on the aggregation
* __ORDER BY__: Column you want to order results by and in ASCending or DESCending order
* __LIMIT__: The maximum number of rows you want your results to contain

## Aggregators like SUM() and COUNT()

Aggregators **summarize rows** into a **single value**.

purchase table:
| name  |  tickets |
| ----- | :------: |
| Tina | 3 |
| Anna | 2 |
| Erin | 2 |
| John | 1 |

Query:
```
SELECT
  SUM(tickets) AS total_tickets, 
  COUNT(tickets) AS number_of_purchases
FROM
  purchases
```

Results:
| total_tickets |  number_of_purchases |
| :---: | :------: |
| 8 | 4 |

Adding a DISTINCT clause in your SUM() or COUNT() function lets you do an aggregation only on each distinct value of the field.
```
SELECT
  SUM(tickets) AS total_tickets, 
  SUM(DISTINCT tickets) AS total_distinct_tickets, 
  COUNT(tickets) AS number_of_purchases, 
  COUNT(DISTINCT tickets) AS number_of_distinct_purchases
FROM
  purchases
  ```
  
  Results:
| total_tickets |  total_distinct_tickets | number_of_purchases | number_of_distinct_ purchases |
| :---: | :------: |:---: |:---: |
| 8 | 6 | 4 | 3 |

In this example, it doesn’t really make sense to do a sum of distinct values. Usually DISTINCT isn't used with SUM() functions. Using DISTINCT with COUNT() functions is helpful to identifying unique cases.

## GROUP BY

