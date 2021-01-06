# 177 Nth Highest Salary 第N高的薪水

__Description__:
Write a SQL query to get the nth highest salary from the Employee table.

```text
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

__Example:__
For example, given the above Employee table, the nth highest salary where n = 2 is 200. If there is no nth highest salary, then the query should return null.

```text
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
```

__题目描述__:
编写一个 SQL 查询，获取 Employee 表中第 n 高的薪水（Salary）。

```text
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

__示例 :__
例如上述 Employee 表，n = 2 时，应返回第二高的薪水 200。如果不存在第 n 高的薪水，那么查询应返回 null。

```text
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
DESC

__代码__:
__MySQL__:

```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  SET N = N - 1;
  RETURN (
      # Write your MySQL query statement below.
      SELECT IFNULL((SELECT DISTINCT SALARY FROM EMPLOYEE ORDER BY SALARY DESC LIMIT N, 1), NULL)
  );
END
```
