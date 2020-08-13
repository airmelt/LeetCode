__Description__:
Given a text file file.txt, transpose its content.

You may assume that each row has the same number of columns and each field is separated by the ' ' character.

__Example:__

If file.txt has the following content:

name age
alice 21
ryan 30
Output the following:

name alice ryan
age 21 30

__题目描述__:
给定一个文件 file.txt，转置它的内容。

你可以假设每行列数相同，并且每个字段由 ' ' 分隔.

__示例 :__

假设 file.txt 文件内容如下：

name age
alice 21
ryan 30
应当输出：

name alice ryan
age 21 30

__思路__:
awk ——格式化输出
NF ——列数
NR ——已读的行数
END{} ——文件扫描结束后要执行的操作。

__代码__:
__Bash__:
```
# Read from the file file.txt and print its transposed content to stdout.
awk '{for (i=1; i<=NF; i++){if(NR==1){res[i]=$i;}else{res[i]=res[i] " " $i}}}END{for (i=1; i<=NF; i++){print res[i]}}' file.txt
```