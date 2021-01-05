# 709 To Lower Case 转换成小写字母

__Description__:
Implement function ToLowerCase() that has a string parameter str, and returns the same string in lowercase.

__Example:__

Example 1:

Input: "Hello"
Output: "hello"

Example 2:

Input: "here"
Output: "here"

Example 3:

Input: "LOVELY"
Output: "lovely"

__题目描述__:
实现函数 ToLowerCase()，该函数接收一个字符串参数 str，并将该字符串中的大写字母转换成小写字母，之后返回新的字符串。

__示例 :__

示例 1：

输入: "Hello"
输出: "hello"

示例 2：

输入: "here"
输出: "here"

示例 3：

输入: "LOVELY"
输出: "lovely"

__思路__:

遍历字符串, 将大写字符改成小写字符即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string toLowerCase(string str) 
    {
        for (int i = 0; i < str.size(); i++) if (str[i] >= 'A' and str[i] <= 'Z') str[i] += 32;
        return str;
    }
};
```

__Java__:

```Java
class Solution {
    public String toLowerCase(String str) {
        StringBuilder sb = new StringBuilder(str);
        for (int i = 0; i < str.length(); i++) if (str.charAt(i) >= 'A' && str.charAt(i) <= 'Z') sb.setCharAt(i, (char)(str.charAt(i) + 32));
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def toLowerCase(self, str: str) -> str:
        return str.lower()
```
