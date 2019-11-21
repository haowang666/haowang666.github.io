---
layout: post
title: "LeetCode SQL"
date: 2019-10-20
output: html_document
share: true
categories: blog
excerpt: "leetcode sql questions and answers"
tags: [SQL]
---

LeetCode 刷题中



# Leetcode 部分



##  175. Combine Two Tables 

Write a SQL query for a report that provides the following information for each person in the Person table, regardless if there is an address for each of those people:

```
FirstName, LastName, City, State
```

```sql
# Write your MySQL query statement below
SELECT FirstName, LastName, City, State
FROM Person LEFT JOIN Address
ON Person.PersonId = Address.PersonId

```

## 176. Second Highest Salary

Write a SQL query to get the second highest salary from the `Employee` table.

```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

For example, given the above Employee table, the query should return `200` as the second highest salary. If there is no second highest salary, then the query should return `null`.

```
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
```

UNION: combine results of different queries, without repetition 

UNION ALL: combine results of different queries, with repetition

```SQL
# Write your MySQL query statement below
SELECT DISTINCT Salary AS SecondHighestSalary FROM Employee 
UNION ALL 
SELECT NULL FROM Employee
ORDER BY SecondHighestSalary DESC LIMIT 1 OFFSET 1;
```

## 177. Nth Highest Salary 

```sql
SELECT name, salary 
FROM #Employee e1
WHERE 3-1 = (SELECT COUNT(DISTINCT salary) FROM #Employee e2
WHERE e2.salary > e1.salary)

Result:
name salary
John 4000
```

**Explanation :**The keyword is there to deal with duplicate salaries in the table. In order to find the Nth highest salary, we are only considering unique salaries. Highest salary means no salary is higher than it, Second highest means only one salary is higher than it, 3rd highest means two salaries are higher than it, similarly Nth highest salary means N-1 salaries are higher than it.



```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  SET N = N - 1;
  RETURN (
      # Write your MySQL query statement below
      SELECT DISTINCT Salary AS SecondHighestSalary FROM Employee 
      UNION ALL 
      SELECT NULL FROM Employee
      ORDER BY SecondHighestSalary DESC LIMIT 1 OFFSET N
  );
END
```



## 181. Employees Earning More than Managers

The `Employee` table holds all employees including their managers. Every employee has an Id, and there is also a column for the manager Id.

```
+----+-------+--------+-----------+
| Id | Name  | Salary | ManagerId |
+----+-------+--------+-----------+
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | NULL      |
| 4  | Max   | 90000  | NULL      |
+----+-------+--------+-----------+
```

Given the `Employee` table, write a SQL query that finds out employees who earn more than their managers. For the above table, Joe is the only employee who earns more than his manager.

```
+----------+
| Employee |
+----------+
| Joe      |
+----------+
```

```sql
SELECT name as Employee FROM Employee
WHERE Salary > (
SELECT max(Salary) From Employee as E2
    WHERE Employee.ManagerID = E2.ID
);
```



## 183. Customers Who Never Order

```sql
Select Name as Customers 
from Customers
where Customers.Id not in
(select distinct CustomerId from Orders)

```



## 184. Deaprtment Highest Salary



`The `Employee` table holds all employees. Every employee has an Id, a salary, and there is also a column for the department Id.

```
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Jim   | 90000  | 1            |
| 3  | Henry | 80000  | 2            |
| 4  | Sam   | 60000  | 2            |
| 5  | Max   | 90000  | 1            |
+----+-------+--------+--------------+
```

The `Department` table holds all departments of the company.

```
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
```

Write a SQL query to find employees who have the highest salary in each of the departments. For the above tables, your SQL query should return the following rows (order of rows does not matter).

```
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Jim      | 90000  |
| Sales      | Henry    | 80000  |
+------------+----------+--------+
```

```sql
SELECT
    Department.name AS 'Department',
    Employee.name AS 'Employee',
    Salary
FROM
    Employee
        JOIN
    Department 
    ON Employee.DepartmentId = Department.Id
WHERE
    (Employee.DepartmentId , Salary) IN
    (   SELECT
            DepartmentId, MAX(Salary)
        FROM
            Employee
        GROUP BY DepartmentId
	)
;
```

## 185. SELECT TOP 3 salaries

**Algorithm**

A top 3 salary in this company means there is no more than 3 salary bigger than itself in the company.

```sql
select e1.Name AS 'Employee', e1.Salary
from Employee AS e1
where 3 >
(
    select count(distinct e2.Salary)
    from Employee AS e2
    where e2.Salary > e1.Salary
)
;
```

In this code, we count the salary number of which is bigger than e1.Salary. So the output is as below for the sample data.

```
| Employee | Salary |
|----------|--------|
| Henry    | 80000  |
| Max      | 90000  |
| Randy    | 85000  |
```

Then, we need to join the **Employee** table with **Department** in order to retrieve the department information.

**MySQL**

```sql
SELECT
    d.Name AS 'Department', e1.Name AS 'Employee', e1.Salary
FROM
    Employee e1
        JOIN
    Department d ON e1.DepartmentId = d.Id
WHERE
    3 > (SELECT
            COUNT(DISTINCT e2.Salary)
        FROM
            Employee e2
        WHERE
            e2.Salary > e1.Salary
                AND e1.DepartmentId = e2.DepartmentId
        )
;
```



## 262. Trips and Users

The `Trips` table holds all taxi trips. Each trip has a unique Id, while Client_Id and Driver_Id are both foreign keys to the Users_Id at the `Users` table. Status is an ENUM type of (‘completed’, ‘cancelled_by_driver’, ‘cancelled_by_client’).

```
+----+-----------+-----------+---------+--------------------+----------+
| Id | Client_Id | Driver_Id | City_Id |        Status      |Request_at|
+----+-----------+-----------+---------+--------------------+----------+
| 1  |     1     |    10     |    1    |     completed      |2013-10-01|
| 2  |     2     |    11     |    1    | cancelled_by_driver|2013-10-01|
| 3  |     3     |    12     |    6    |     completed      |2013-10-01|
| 4  |     4     |    13     |    6    | cancelled_by_client|2013-10-01|
| 5  |     1     |    10     |    1    |     completed      |2013-10-02|
| 6  |     2     |    11     |    6    |     completed      |2013-10-02|
| 7  |     3     |    12     |    6    |     completed      |2013-10-02|
| 8  |     2     |    12     |    12   |     completed      |2013-10-03|
| 9  |     3     |    10     |    12   |     completed      |2013-10-03| 
| 10 |     4     |    13     |    12   | cancelled_by_driver|2013-10-03|
+----+-----------+-----------+---------+--------------------+----------+
```

The `Users` table holds all users. Each user has an unique Users_Id, and Role is an ENUM type of (‘client’, ‘driver’, ‘partner’).

```
+----------+--------+--------+
| Users_Id | Banned |  Role  |
+----------+--------+--------+
|    1     |   No   | client |
|    2     |   Yes  | client |
|    3     |   No   | client |
|    4     |   No   | client |
|    10    |   No   | driver |
|    11    |   No   | driver |
|    12    |   No   | driver |
|    13    |   No   | driver |
+----------+--------+--------+
```

Write a SQL query to find the cancellation rate of requests made by unbanned users (both client and driver must be unbanned) between **Oct 1, 2013** and **Oct 3, 2013**. The cancellation rate is computed by dividing the number of canceled (by client or driver) requests made by unbanned users by the total number of requests made by unbanned users.

For the above tables, your SQL query should return the following rows with the cancellation rate being rounded to *two* decimal places.

```
+------------+-------------------+
|     Day    | Cancellation Rate |
+------------+-------------------+
| 2013-10-01 |       0.33        |
| 2013-10-02 |       0.00        |
| 2013-10-03 |       0.50        |
+------------+-------------------+
```



```sql
SELECT Request_at as Day, ROUND(SUM(CASE WHEN Status!="completed" THEN 1 ELSE 0 END)/COUNT(*),2) as "Cancellation Rate" FROM Trips
WHERE Client_Id NOT IN
(
    SELECT Users_Id FROM Users
    WHERE Banned="Yes"
)
AND Request_at>="2013-10-01" AND Request_at<="2013-10-03"
GROUP BY Request_at;
```



## 627. Swap Salary

**Example:**

```
| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | m   | 2500   |
| 2  | B    | f   | 1500   |
| 3  | C    | m   | 5500   |
| 4  | D    | f   | 500    |
```

After running your **update**

```
| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | f   | 2500   |
| 2  | B    | m   | 1500   |
| 3  | C    | f   | 5500   |
| 4  | D    | m   | 500    |
```

```sql
UPDATE salary
SET
    sex = CASE sex
        WHEN 'm' THEN 'f'
        ELSE 'm'
    END;
```

# 面经部分



## 活动运营数据分析

表1——订单表*orders*，大概字段有（*user_id‘用户编号’, order_pay‘订单金额’ , order_time‘下单时间’*）。

表2——活动报名表*act_apply*，大概字段有（*act_id‘活动编号’, user_id‘报名用户’,act_time‘报名时间’*）

**要求：**

1. 统计每个活动对应所有用户在报名后产生的总订单金额，总订单数。（每个用户限报一个活动,***题干默认用户报名后产生的订单均为参加活动的订单\***）。
2. 统计每个活动从开始后到当天（考试日）平均每天产生的订单数，活动开始时间定义为最早有用户报名的时间。（涉及到时间的数据类型均为：datetime）。

参考答案：

1. （表的基本连接，比较简单)

```sql
select act_id as '活动编号', COUNT(*) as '订单数', SUM(order_pay) as '总金额'
from orders LEFT JOIN act_apply on orders.user_id=act_apply.user_id 
where order_time >= act_time
GROUP BY act_id
```

2. (这里我认为‘活动最早报名的时间’并不是‘最早有人下单的时间’这样计算平均的底数会不同，这样可以作为分析提前几天预上架活动最合适，提前太早可能报名后忘记下单）

```sql
SELECT act_id as '活动编号', COUNT(*)/DATEDIFF(NOW(),act_start) AS '活动开始后平均每天下单数'
FROM orders a
LEFT JOIN
(SELECT user_id ,act_id ,act_time, min(act_time) over(PARTITION by act_id) as 'act_start'
FROM act_apply
) b
ON a.user_id=b.user_id
WHERE order_time>=act_time
GROUP BY act_id,act_start
```

## 用户行为路径分析

表1——用户行为表*tracking_log*，大概字段有（*user_id‘用户编号’,opr_id‘操作编号’,log_time‘操作时间’*）

要求：

1. 计算每天的访客数和他们的平均操作次数。
2. 统计每天符合以下条件的用户数：A操作之后是B操作，AB操作必须相邻。

参考答案：

1. （用group嵌套，刚好可以把人数和人次分解开）

```sql
SELECT b.date ,  COUNT(b.user_id) as '访客数' , AVG(op) AS '平均操作次数'
FROM
(SELECT user_id , COUNT(opr_type) as 'op', CONVERT(log_time,date) as 'date' FROM tracking_log
GROUP BY user_id,date) b
GROUP BY b.date
```

2. （这里用到了开窗函数over和分析函数lead）

```sql
SELECT a.Date , COUNT(*) as 'A-B路径用户计数'
FROM(
SELECT DISTINCT user_id as 'User',opr_type as '1st', CONVERT(log_time,date) as 'Date', lead(opr_type,1) 
over(PARTITION by user_id,CONVERT(log_time,date) ORDER BY log_time) as '2nd'
FROM tracking_log
) a
WHERE a.1st = 'A' and a.2nd ='B'
GROUP BY a.Date
```

## 用户新增留存分析

表1——用户登陆表*user_log*，大概字段有（*user_id‘用户编号’，log_time‘登陆时间’*）

要求：

1.每天新增用户数，以及他们第2天、30天的回访比例

参考答案：

1. (找出每个用户第一次登陆时间，再聚合时间得到每一天新增用户，时间要聚合到天)

```sql
SELECT a.first_time as '日期' ,a.new as '新增用户', concat(round(100*b.2_back/a.new,2),'%')  as '第二天回访率',concat(round(100*c.30_back/a.new,2),'%') as '第30天回访率'
FROM
(
SELECT aa.first_time, COUNT(distinct aa.user_id) as new
FROM
(SELECT  user_id,CONVERT(MIN(log_time) over(PARTITION by user_id ),date)as first_time
FROM user_log) aa
GROUP BY aa.first_time
) a
LEFT JOIN
(
SELECT bb.first_time, COUNT(distinct bb.user_id) as 2_back
FROM
(SELECT  user_id,CONVERT(MIN(log_time) over(PARTITION by user_id ),date)as first_time,log_time
FROM user_log) bb
WHERE DATEDIFF(log_time,first_time)=1
GROUP BY bb.first_time
) b
on a.first_time=b.first_time
LEFT JOIN
(
SELECT cc.first_time, COUNT(distinct cc.user_id) as 30_back
FROM
(SELECT  user_id,CONVERT(MIN(log_time) over(PARTITION by user_id ),date)as first_time,log_time
FROM user_log) cc
WHERE DATEDIFF(log_time,first_time)=29
GROUP BY cc.first_time
) c
on a.first_time=c.first_time
```

## 至少选两门选修课

```sql
select 学号，count（课程号）as 选修课课程数
from score
group by 学号
having count（课程号）>1;
```



## 查询每门课平均成绩

结果按照平均成绩排序，平均成绩相同时，按照课程号降序排序

```SQL
SELECT 课程号，avg（成绩） as 平均成绩
from score
group by 课程号
order by 平均成绩 asc，课程号 desc
```



## 查询没有学全所有课的学生的学号，姓名

没有选全课：选修课程数小于总课程数

考察子查询

```sql
select 学号，姓名
from student
where 学号 in（
select 学号
from score
group by 学号
having count（课程号） <（select count（课程号）from course）
）；
```



## 查询每门课的及格人数和不及格人数

```SQL
select 课程号，
sum（case when 成绩》= 60 then 1
else 0 
end）as 及格人数，
sum（case when 成绩 《 60 then 1
else 0
end） as 不及格人数
from score；
```







# 牛客网题目



















