# 1179 Reformat Department Table 重新格式化部门表

__Description__:
Table: Department

```text
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| revenue       | int     |
| month         | varchar |
+---------------+---------+
```

(id, month) is the primary key of this table.
The table has information about the revenue of each department per month.
The month has values in ["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"].

Write an SQL query to reformat the table such that there is a department id column and a revenue column for each month.

__Example:__

The query result format is in the following example:

Department table:

```text
+------+---------+-------+
| id   | revenue | month |
+------+---------+-------+
| 1    | 8000    | Jan   |
| 2    | 9000    | Jan   |
| 3    | 10000   | Feb   |
| 1    | 7000    | Feb   |
| 1    | 6000    | Mar   |
+------+---------+-------+
```

Result table:

```text
+------+-------------+-------------+-------------+-----+-------------+
| id   | Jan_Revenue | Feb_Revenue | Mar_Revenue | ... | Dec_Revenue |
+------+-------------+-------------+-------------+-----+-------------+
| 1    | 8000        | 7000        | 6000        | ... | null        |
| 2    | 9000        | null        | null        | ... | null        |
| 3    | null        | 10000       | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
```

Note that the result table has 13 columns (1 for the department id + 12 for the months).

__题目描述__:
部门表 Department：

```text
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| revenue       | int     |
| month         | varchar |
+---------------+---------+
```

(id, month) 是表的联合主键。
这个表格有关于每个部门每月收入的信息。
月份（month）可以取下列值 ["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"]。

编写一个 SQL 查询来重新格式化表，使得新的表中有一个部门 id 列和一些对应 每个月 的收入（revenue）列。

__示例 :__
查询结果格式如下面的示例所示：

Department 表：

```text
+------+---------+-------+
| id   | revenue | month |
+------+---------+-------+
| 1    | 8000    | Jan   |
| 2    | 9000    | Jan   |
| 3    | 10000   | Feb   |
| 1    | 7000    | Feb   |
| 1    | 6000    | Mar   |
+------+---------+-------+
```

查询得到的结果表：

```text
+------+-------------+-------------+-------------+-----+-------------+
| id   | Jan_Revenue | Feb_Revenue | Mar_Revenue | ... | Dec_Revenue |
+------+-------------+-------------+-------------+-----+-------------+
| 1    | 8000        | 7000        | 6000        | ... | null        |
| 2    | 9000        | null        | null        | ... | null        |
| 3    | null        | 10000       | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
```

注意，结果表有 13 列 (1个部门 id 列 + 12个月份的收入列)。

__思路__:

行转列

__代码__:
__MySQL__:

```sql
# Write your MySQL query statement below
SELECT 
    id,
    MAX(IF(month = 'Jan', revenue, null)) AS "Jan_Revenue",
    MAX(IF(month = 'Feb', revenue, null)) AS "Feb_Revenue",
    MAX(IF(month = 'Mar', revenue, null)) AS "Mar_Revenue",
    MAX(IF(month = 'Apr', revenue, null)) AS "Apr_Revenue",
    MAX(IF(month = 'May', revenue, null)) AS "May_Revenue",
    MAX(IF(month = 'Jun', revenue, null)) AS "Jun_Revenue",
    MAX(IF(month = 'Jul', revenue, null)) AS "Jul_Revenue",
    MAX(IF(month = 'Aug', revenue, null)) AS "Aug_Revenue",
    MAX(IF(month = 'Sep', revenue, null)) AS "Sep_Revenue",
    MAX(IF(month = 'Oct', revenue, null)) AS "Oct_Revenue",
    MAX(IF(month = 'Nov', revenue, null)) AS "Nov_Revenue",
    MAX(IF(month = 'Dec', revenue, null)) AS "Dec_Revenue"
FROM
    Department
GROUP BY id;
```
