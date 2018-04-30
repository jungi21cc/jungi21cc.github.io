---
layout: post
title: MySQL - intermediate
tags: [Computer Science]
---

### 1. *CEIL, ROUND, TRUNCATE*

#### - CEIL

```
SELECT
CountryCode, Language, Percentage, CEIL(Percentage)
FROM countrylanguage;
```

#### - ROUND

```
SELECT
CountryCode, Language, Percentage, ROUND(Percentage, 0)
FROM countrylanguage;
```

#### - TRUNCATE

```
SELECT
CountryCode, Language, Percentage, ROUND(Percentage, 0), TRUNCATE(Percentage,0)
FROM countrylanguage;
```


### 2. *Conditional statement*

#### - IF

```
SELECT
name, population, IF(population > 1000000, "big city", "small city") AS city_scale
FROM city;
```

#### - IFNULL

```
SELECT
IndepYear, IFNULL(IndepYear, 0) as IndepYear
FROM country;
```

#### - CASE

```
SELECT
name, population,
CASE
WHEN population > 1000000000 THEN "upper 1 bilion"
WHEN population > 100000000 THEN "upper 100 milion"
ELSE "below 100 milion"
END AS result
FROM country;
```


### 3. *DATE_FORMAT*

```
SELECT
DATE_FORMAT(payment_date, "%Y-%m") AS monthly, SUM(amount) AS amount
FROM payment
GROUP BY monthly;
```

### 4. *JOIN*

#### - inner join

```
SELECT
id, user.name, addr.addr
FROM user
JOIN addr
ON user.user_id = addr.user_id;
```

#### - left join

```
SELECT
id, user.name, addr.addr
FROM user
LEFT JOIN addr
ON user.user_id = addr.user_id;
```

#### - right join

```
SELECT
id, user.name, addr.addr
FROM user
RIGHT JOIN addr
ON user.user_id = addr.user_id;
```

### 5. *UNION*

```
SELECT
name
FROM user
UNION
SELECT addr
FROM addr;
```

```
SELECT
name
FROM user
UNION ALL
SELECT addr
FROM addr;
```

### 6. *sub-query*

```
SELECT
(SELECT count(name) FROM city) AS total_city,
(SELECT count(name) FROM country) AS total_country,
(SELECT count(DISTINCT(Language)) FROM countrylanguage) AS total_language
FROM DUAL;
```

```
SELECT *
FROM
(SELECT countrycode, name, population
FROM city
WHERE population > 8000000) AS city
JOIN (SELECT code, name FROM country) AS country
ON city.countrycode = country.code;
```


```
SELECT
code, name, HeadOfState
FROM country
WHERE code IN (SELECT DISTINCT(countrycode)
FROM city WHERE population > 8000000);
```

### 7. *INDEX*

```
EXPLAIN
SELECT *
FROM city
WHERE population > 1000000;
```

### 8. *VIEW*

```
CREATE VIEW code_name AS
SELECT
code, name
FROM country;
```

```
SELECT *
FROM city
JOIN code_name
ON city.countrycode = code_name.code;
```



***

*Reference*


[MySQL - 생활코딩](https://opentutorials.org/course/195)

[DATABASE2 - MySQL 생활코딩](https://opentutorials.org/course/3161)
