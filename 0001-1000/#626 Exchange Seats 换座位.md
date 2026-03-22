# 626 Exchange Seats 换座位

__Description__:
Mary is a teacher in a middle school and she has a table seat storing students' names and their corresponding seat ids.

The column id is continuous increment.

Mary wants to change seats for the adjacent students.

Can you write a SQL query to output the result for Mary?

__Example:__

```text
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Abbot   |
|    2    | Doris   |
|    3    | Emerson |
|    4    | Green   |
|    5    | Jeames  |
+---------+---------+
```

For the sample input, the output is:

```text
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Doris   |
|    2    | Abbot   |
|    3    | Green   |
|    4    | Emerson |
|    5    | Jeames  |
+---------+---------+
```

__Note:__

If the number of students is odd, there is no need to change the last one's seat.

__题目描述__:
小美是一所中学的信息科技老师，她有一张 seat 座位表，平时用来储存学生名字和与他们相对应的座位 id。

其中纵列的 id 是连续递增的

小美想改变相邻俩学生的座位。

你能不能帮她写一个 SQL query 来输出小美想要的结果呢？

__示例 :__

```text
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Abbot   |
|    2    | Doris   |
|    3    | Emerson |
|    4    | Green   |
|    5    | Jeames  |
+---------+---------+
```

假如数据输入的是上表，则输出结果如下：

```text
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Doris   |
|    2    | Abbot   |
|    3    | Green   |
|    4    | Emerson |
|    5    | Jeames  |
+---------+---------+
```

__注意:__

如果学生人数是奇数，则不需要改变最后一个同学的座位。

__思路__:

CASE WHEN

__代码__:
__MySQL__:

```sql
# Write your MySQL query statement below
SELECT (CASE 
            WHEN MOD(id, 2) = 1 AND id = (SELECT COUNT(*) FROM seat) THEN id
            WHEN MOD(id, 2) = 1 THEN id + 1
            ElSE id - 1
        END) AS id, student
FROM seat
ORDER BY id;
```
