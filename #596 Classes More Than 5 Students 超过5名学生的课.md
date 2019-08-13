__Description__:
There is a table courses with columns: student and class

Please list out all classes which have more than or equal to 5 students.

__Example:__
For example, the table:
```
+---------+------------+
| student | class      |
+---------+------------+
| A       | Math       |
| B       | English    |
| C       | Math       |
| D       | Biology    |
| E       | Math       |
| F       | Computer   |
| G       | Math       |
| H       | Math       |
| I       | Math       |
+---------+------------+
```
Should output:
```
+---------+
| class   |
+---------+
| Math    |
+---------+
 ```

__Note:__
The students should not be counted duplicate in each course.

__题目描述__:
有一个courses 表 ，有: student (学生) 和 class (课程)。

请列出所有超过或等于5名学生的课。

__示例 :__
例如,表:
```
+---------+------------+
| student | class      |
+---------+------------+
| A       | Math       |
| B       | English    |
| C       | Math       |
| D       | Biology    |
| E       | Math       |
| F       | Computer   |
| G       | Math       |
| H       | Math       |
| I       | Math       |
+---------+------------+
```
应该输出:
```
+---------+
| class   |
+---------+
| Math    |
+---------+
```

__说明:__
学生在每个课中不应被重复计算。

__思路__:
使用 GROUP BY和 HAVING条件, 用 DISTINCT筛选不重复的学生

__代码__:
__MySQL__:
```
SELECT
	class
FROM
	courses
GROUP BY
	class
HAVING
	COUNT ( DISTINCT student ) > 4
```
