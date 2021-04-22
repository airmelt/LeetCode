# 28 Implement strStr() 实现strStr()

__Description__:
Implement [strStr()](http://www.cplusplus.com/reference/cstring/strstr/).

Return the index of the first occurrence of needle in haystack, or **-1** if needle is not part of haystack.

**Example:**

**Example 1:**

**Input:** haystack = "hello", needle = "ll"
**Output:** 2

**Example 2:**

**Input:** haystack = "aaaaa", needle = "bba"
**Output:** -1

**Clarification:**

What should we return when `needle` is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when `needle` is an empty string. This is consistent to C's [strstr()](http://www.cplusplus.com/reference/cstring/strstr/) and Java's [indexOf()](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#indexOf(java.lang.String)).

__题目描述__:
实现 [strStr()](https://baike.baidu.com/item/strstr/811469) 函数。

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  **-1**。

__示例__:

**示例 1:**

**输入:** haystack = "hello", needle = "ll"
**输出:** 2

**示例 2:**

**输入:** haystack = "aaaaa", needle = "bba"
**输出:** -1

**说明:**

当 `needle` 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 `needle` 是空字符串时我们应当返回 0 。这与C语言的 [strstr()](https://baike.baidu.com/item/strstr/811469) 以及 Java的 [indexOf()](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#indexOf(java.lang.String)) 定义相符。

__思路__:

1. 在每一个haystack字符上遍历needle
时间复杂度O(nm), 空间复杂度O(1), 其中n为haystack长度, m为needle长度
2. [KMP算法](http://www.ruanyifeng.com/blog/2013/05/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm.html)
时间复杂度O(n), 空间复杂度O(m), 其中n为haystack长度, m为needle长度
3. 当然最偷懒的算法是直接调用strstr()(C++)/indexOf()(Java)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    // 求next数组
    void ComputePrefix(string s, int next[]) 
    {
        int len = s.length();
        // i为字符串下标, j为最大前/后缀长度
        int i, j;
        // next数组初始一定为0, 即字符串第一个字符是没有前/后缀的
        next[0] = 0;
        // 从第二个字符开始求取next数组
        for (i = 0, j = 1; j < len; j++) 
        {
            // 求取s[0]~s[j]相同的前/后缀长度
            /* 例子:
               "AB"
               前缀为["A"]
               后缀为["B"]
               无相同元素, 长度为0
               "ABCDAB"
               前缀为["A", "AB", "ABC", "ABCD", "ABCDA"]
               后缀为["BCDAB", "CDAB", "DAB", "AB", "B"]
               相同的元素为 "AB", 长度为2
            */
            /* i > 0, 是因为第一个字符是没有前/后缀的
               而且查找失败, i要退回next[i - 1]
            */
            /* 对于已经求取的next[j] = i, 表示在字符串下标为 j处有长度为 i的前/后缀相等
               比如 A -> AB
               即 next[0] = 0
               对下次while循环, 直接跳出
               ABCDA -> ABCDAB
               可以得到有一个前/后缀相等
               即next[4] = 1
               即
               ABCD A
                    |
                    A BCDA
               此时 next[4] = 1, j = 4, i = 1
               ABCD AB
                    ||
                    AB CDAB
               此时 next[5] = 2, j = 5, i = 2
            */
            // 每次查找不匹配, i退回next[i - 1]
            while (i > 0 and s[i] != s[j]) i = next[i - 1];
            if (s[i] == s[j]) i++;
            next[j] = i;
        }
    }
 public:
 int strStr(string haystack, string needle) 
    {
        if (!needle.length()) return 0;
        if (!haystack.length()) return -1;
        int next[needle.length()];
        ComputePrefix(needle, next);
        int j = 0;
        for (int i = 0; i < haystack.length(); i++) 
        {
            // 每次查找不匹配, j退回next[j - 1]
            while (j > 0 and needle[j] != haystack[i]) j = next[j - 1];
            if (needle[j] == haystack[i]) j++;
            if (j == needle.length()) return i - j + 1;
        }
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int strStr(String haystack, String needle) {
        return haystack.indexOf(needle);
    }
}
```

__Python__:

```Python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        if not needle:
            return 0
        for i in range(len(haystack)):
            j = i + len(needle)
            if j <= len(haystack) and haystack[i:j] == needle:
                return i
        return -1
```
