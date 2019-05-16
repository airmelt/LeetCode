__Description__:
Given a text file file.txt that contains list of phone numbers (one per line), write a one liner bash script to print all valid phone numbers.

You may assume that a valid phone number must appear in one of the following two formats: (xxx) xxx-xxxx or xxx-xxx-xxxx. (x means a digit)

You may also assume each line in the text file must not contain leading or trailing white spaces.

**Example:**

Assume that file.txt has the following content:

987-123-4567
123 456 7890
(123) 456-7890
Your script should output the following valid phone numbers:

987-123-4567
(123) 456-7890

__题目描述__:
给定一个包含电话号码列表（一行一个电话号码）的文本文件 file.txt，写一个 bash 脚本输出所有有效的电话号码。

你可以假设一个有效的电话号码必须满足以下两种格式： (xxx) xxx-xxxx 或 xxx-xxx-xxxx。（x 表示一个数字）

你也可以假设每行前后没有多余的空格字符。

**示例：**

假设 file.txt 内容如下：

987-123-4567
123 456 7890
(123) 456-7890
你的脚本应当输出下列有效的电话号码：

987-123-4567
(123) 456-7890

__思路__:
正则表达式
^(\d{3}-|\(\d{3}\))\d{3}-\d{4}
\d{3}表示3位数字
| 表示或者
括号要用 \转义

__代码__:
__Bash__:
```
grep -P '^(\d{3}-|\(\d{3}\) )\d{3}-\d{4}$' file.txt
```
