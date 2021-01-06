# 180 Consecutive Numbers 连续出现的数字

__Description__:
Write a SQL query to find all numbers that appear at least three times consecutively.

```text
+----+-----+
| Id | Num |
+----+-----+
| 1  |  1  |
| 2  |  1  |
| 3  |  1  |
| 4  |  2  |
| 5  |  1  |
| 6  |  2  |
| 7  |  2  |
+----+-----+
```

__Example:__

For example, given the above Logs table, 1 is the only number that appears consecutively for at least three times.

```text
+-----------------+
| ConsecutiveNums |
+-----------------+
| 1               |
+-----------------+
```

__题目描述__:
编写一个 SQL 查询，查找所有至少连续出现三次的数字。

```text
+----+-----+
| Id | Num |
+----+-----+
| 1  |  1  |
| 2  |  1  |
| 3  |  1  |
| 4  |  2  |
| 5  |  1  |
| 6  |  2  |
| 7  |  2  |
+----+-----+
```

__示例 :__

例如，给定上面的 Logs 表， 1 是唯一连续出现至少三次的数字。

```text
+-----------------+
| ConsecutiveNums |
+-----------------+
| 1               |
+-----------------+
```

__思路__:

DISTINCT
INNER JOIN

__代码__:
__MySQL__:

```sql
# Write your MySQL query statement below
SELECT DISTINCT a.num AS ConsecutiveNums FROM Logs a 
INNER JOIN Logs b ON b.id - 1  = a.id AND a.num = b.num 
INNER JOIN Logs c ON c.id - 2 = a.id AND a.num = c.num
```
