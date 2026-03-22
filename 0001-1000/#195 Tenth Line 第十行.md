# 195 Tenth Line 第十行

__Description__:
Given a text file file.txt, print just the 10th line of the file.

**Example:**

Assume that file.txt has the following content:

```text
Line 1
Line 2
Line 3
Line 4
Line 5
Line 6
Line 7
Line 8
Line 9
Line 10
```

Your script should output the tenth line, which is:

Line 10

__Note__:

1. If the file contains less than 10 lines, what should you output?
2. There's at least three different solutions. Try to explore all possibilities.

__题目描述__:
给定一个文本文件 file.txt，请只打印这个文件中的第十行。

**示例：**

假设 file.txt 有如下内容：

```text
Line 1
Line 2
Line 3
Line 4
Line 5
Line 6
Line 7
Line 8
Line 9
Line 10
```

你的脚本应当显示第十行：

Line 10

__思路__:

awk命令

__代码__:
__Bash__:

```bash
awk -F: "NR==10{print}" file.txt
```
