__Description__:
Write a SQL query to get the second highest salary from the Employee table.
```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```
For example, given the above Employee table, the query should return 200 as the second highest salary. If there is no second highest salary, then the query should return null.
```
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
```

__题目描述__:
编写一个 SQL 查询，获取 Employee 表中第二高的薪水（Salary） 。
```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```
例如上述 Employee 表，SQL查询应该返回 200 作为第二高的薪水。如果不存在第二高的薪水，那么查询应返回 null。
```
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
```

__思路__:
DISTINCT
LIMIT
IFNULL

__代码__:
__MySQL__:
```
# Write your MySQL query statement below
SELECT IFNULL((SELECT DISTINCT(Salary) FROM Employee ORDER BY Salary DESC LIMIT 1, 1), NULL) AS SecondHighestSalary
```
