---
layout: post
title: "SQL自学笔记"
date: 2019-01-20
output: html_document
share: true
categories: blog
excerpt: "learning basic SQL"
tags: [SQL]
---

计划两周内做到能用, 主要依据DataCamp课程

## 2/5/2019


**SQL: Structured Query Language**

- 数据库: 数据库是一些关联表的集合。
- 数据表: 表是数据的矩阵。在一个数据库中的表看起来像一个简单的电子表格。
- 列: 一列(数据元素) 包含了相同的数据, 例如邮政编码的数据。
- 行：一行（=元组，或记录）是一组相关的数据，例如一条用户订阅的数据。
- 冗余：存储两倍数据，冗余降低了性能，但提高了数据的安全性。
- 主键：主键是唯一的。一个数据表中只能包含一个主键。你可以使用主键来查询数据。
- 外键：外键用于关联两个表。
- 复合键：复合键（组合键）将多个列作为一个索引键，一般用于复合索引。
- 索引：使用索引可快速访问数据库表中的特定信息。索引是对数据库表中一列或多列的值进行排序的一种结构。类似于书籍的目录。
- 参照完整性: 参照的完整性要求关系中不允许引用不存在的实体。与实体完整性是关系模型必须满足的完整性约束条件，目的是保证数据的一致性。

**Notation:**
- use ';' to separate lines
- key words: SELECT, FROM are not case sensitive 

**Basic grammar: **
SELECT A,B,C FROM D;
SELECT * FROM D
LIMIT 10;

**Keywords:**
LIMIT: set the maximum number of rows
DISTINCT: select unique values
COUNT: return the number of rows; COUNT(DISTINCT ABC)
WHERE: set restrictions (=, <>, <, >) use logical combination 'and' 'or', can use as many as possible
IN/BETWEEN: IN(a,b,c,d...) good for selecting discrete restrictions 


## 2/7/2019

**Special key words**
- NULL is a special case, standing for missing value. Null is also a key word, which means it is not case-sensitive.
- LIKE and NOT LIKE： SELECT name FROM companies WHERE name LIKE 'Data%';

**Aggregate functions**
- AVG: average calculation
- MAX
- SUM: summation function 



## 2/8/2019