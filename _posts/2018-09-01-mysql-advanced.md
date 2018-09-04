---
layout: post
title: MySQL - intermediate
tags: [Computer Science]

---

### MySQL - intermediate

### 1. *Explain Plan*

#### - Explain Plan

```
EXPLAIN
SELECT
CountryCode, Language, Percentage, CEIL(Percentage)
FROM countrylanguage;
```

### 2. *Storage Engine*

#### - MyISAM

```
DROP TABLE IF EXISTS temp_user;
CREATE TABLE temp_user (
user_id INT NOT NULL AUTO_INCREMENT,
email VARCHAR(80) NULL,
username VARCHAR(45),
password VARCHAR(45) NULL,
status varchar(10) default 'inactive',
cap_time datetime NULL,
PRIMARY KEY (user_id, username)
)ENGINE=MyISAM DEFAULT CHARSET=utf8mb4;
```

#### - InnoDB

```
DROP TABLE IF EXISTS temp_user;
CREATE TABLE temp_user (
user_id INT NOT NULL AUTO_INCREMENT,
email VARCHAR(80) NULL,
username VARCHAR(45),
password VARCHAR(45) NULL,
status varchar(10) default 'inactive',
cap_time datetime NULL,
PRIMARY KEY (user_id, username)
)ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

### 3. *Index Ascending vs Descending*

#### - Index Ascending

```
SELECT
CountryCode, Language, Percentage, CEIL(Percentage)
FROM countrylanguage
order by Percentage asc;
```

#### - Index Descending

```
SELECT
CountryCode, Language, Percentage, CEIL(Percentage)
FROM countrylanguage
order by Percentage desc;
```


### 4. *Duplicate on Update*

#### - Duplicate on Update

```
INSERT INTO t1 (a,b,c) VALUES (1,2,3)
ON DUPLICATE KEY UPDATE c=c+1;
```



***

*Reference*


[MySQL Ascending index vs Descending index](http://tech.kakao.com/2018/06/19/AscendingAndDescendingIndex/)

[INSERT ... ON DUPLICATE KEY UPDATE Syntax](https://dev.mysql.com/doc/refman/8.0/en/insert-on-duplicate.html)
