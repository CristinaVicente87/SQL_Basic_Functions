# SQL_Basic_Functions

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

New purchase table:
| occasion |  name | tickets | 
| :---: | :------: |:---: |
| fun | Tina | 5 | 
| date | Anna | 2 |
| date | Erin | 2 |
| fun | John | 3 |

Query:
```
SELECT
occasion, 
  SUM(tickets) AS total_tickets, 
  COUNT(tickets) AS number_of_purchases
FROM
  purchases
GROUP BY
  occasion
```

Results:
| occasion |  total_tickets | number_of_purchases | 
| :---: | :------: |:---: |
| fun | 8 | 2 | 
| date | 4 | 2 |

## Having

The HAVING clause is used to make filters on your aggregations and has to be paired with a GROUP BY clause.

purchase table:
| occasion |  name | tickets | 
| :---: | :------: |:---: |
| fun | Tina | 5 | 
| date | Anna | 2 |
| date | Erin | 2 |
| fun | John | 3 |

Query:
```
SELECT
  occasion, 
  SUM(tickets) AS total_tickets, 
  COUNT(tickets) AS number_of_purchases
FROM
  purchases
GROUP BY
  occasion
HAVING
  SUM(tickets) > 5
  ```
  
  Results:
| occasion |  total_tickets | number_of_purchases | 
| :---: | :------: |:---: |
| fun | 8 | 2 | 

## ORDER BY

The ORDER BY clause helps  organize the results. It goes at the end of the SQL query and it is the very last clause to use unless you have a LIMIT clause.

A slightly modified version of the purchase table:
| name  |  tickets |
| ----- | :------: |
| Tina | 3 |
| Anna | 5 |
| Erin | 2 |
| John | 4 |

Query:
```
SELECT
  name, 
  tickets
FROM
  purchases
ORDER BY 
  tickets DESC
```

Results:
| name  |  tickets |
| ----- | :------: |
| Anna | 5 |
| John | 4 |
| Tina | 3 |
| Erin | 2 |


## LIMIT

The LIMIT clause is helpful when you only want to work with a select number of rows.

Query:
```
SELECT
  name, 
  tickets
FROM
  purchases
ORDER BY 
  tickets DESC
 LIMIT 3 --top 3 results only
```

Results:
| name  |  tickets |
| ----- | :------: |
| Anna | 5 |
| John | 4 |
| Tina | 3 |

## CASE statement

CASE statements are most commonly used as labels within the dataset. CASE statements can be used to label rows that meet a certain condition as X and rows that meet another condition as Y.

The MovieTheater table:
| genre  |  movie_title |
| ----- | :------: |
| horror | Silence of the Lambs |
| comedy | Jumanji |
| family | Frozen 2 |
| documentary | 13th |

Let’s say I want to group these movies into the movies that I will watch and movies that I will not watch, and count the number of movies that fall into each category. Your query would be:

```
SELECT
CASE
  WHEN genre = ‘horror’ THEN ‘will not watch’
  ELSE ‘will watch’
  END AS watch_category, --creating your own category
COUNT(movie_title) AS number_of_movies
FROM
  MovieTheater
GROUP BY
  1 --when grouping by CASE, use position numbers or type entire CASE statement here
```

Results:
| watch_category  |  number_of_movies |
| ----- | :------: |
| Will not watch | 1 |
| Will  watch | 3 |


## JOINs

Most used are **INNER JOINs or LEFT JOINs**.

Favorite_ Colors:
| name  |  color |
| ----- | :------: |
| Tina | blue |
| Anna | green |
| Erin | red |
| John | orange |

Favorite_ movie:
| name  |  movie |
| ----- | :------: |
| Tina | Avengers |
| Anna | Despicable Me |
| Erin | Frozen |

INNER JOINs are used to see the data where the JOIN key exists in both tables

```
SELECT
  friend, 
  color, 
  movie
FROM Favorite_Colors AS c 
INNER JOIN Favorite_Movies AS m ON c.friend = m.friend
```

Results:
| name  |  color | movie |
| ----- | :------: | :-----: |
| Tina | blue | Avengers |
| Anna | green | Despicable Me |
| Erin | red | Frozen |

LEFT JOIN

```
SELECT
  friend, 
  color, 
  movie
FROM Favorite_Colors AS c 
LEFT JOIN Favorite_Movies AS m ON c.friend = m.friend
```

Results:
| name  |  color | movie |
| ----- | :------: | :-----: |
| Tina | blue | Avengers |
| Anna | green | Despicable Me |
| Erin | red | Frozen |
| John | orange | *null* |
