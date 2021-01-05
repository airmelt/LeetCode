# 171 Excel Sheet Column Number Excel表列序号

__Description__:
Given a column title as appear in an Excel sheet, return its corresponding column number.

For example:

```text
    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28
    ...
```

__Example__:

Example 1:
Input: "A"
Output: 1

Example 2:
Input: "AB"
Output: 28

Example 3:
Input: "ZY"
Output: 701

__题目描述__:
给定一个Excel表格中的列名称，返回其相应的列序号。

例如，

```text
    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28
    ...
```

__示例__:

示例 1:
输入: "A"
输出: 1

示例 2:
输入: "AB"
输出: 28

示例 3:
输入: "ZY"
输出: 701

__思路__:

相当于 26进制转化为 10进制
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int titleToNumber(string s) 
    {
        int result = 0;
        for (auto c : s) 
        {
            result *= 26;
            result += c - 'A' + 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution 
{
    public int titleToNumber(String s) 
    {
        int result = 0;
        for (char c : s.toCharArray()) {
            result *= 26;
            result += c - 'A' + 1;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def titleToNumber(self, s: str) -> int:
        result = 0
        for i in s:
            result *= 26
            result += ord(i) - ord('A') + 1
        return result
```
