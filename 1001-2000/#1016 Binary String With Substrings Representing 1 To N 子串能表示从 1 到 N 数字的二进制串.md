# 1016 Binary String With Substrings Representing 1 To N 子串能表示从 1 到 N 数字的二进制串

__Description__:
Given a binary string s and a positive integer n, return true if the binary representation of all the integers in the range [1, n] are substrings of s, or false otherwise.

A substring is a contiguous sequence of characters within a string.

__Example:__

Example 1:

Input: s = "0110", n = 3
Output: true

Example 2:

Input: s = "0110", n = 4
Output: false

__Constraints:__

1 <= s.length <= 1000
s[i] is either '0' or '1'.
1 <= n <= 10^9

__题目描述__:
给定一个二进制字符串 s 和一个正整数 n，如果对于 [1, n] 范围内的每个整数，其二进制表示都是 s 的 子字符串 ，就返回 true，否则返回 false 。

子字符串 是字符串中连续的字符序列。

__示例 :__

示例 1：

输入：s = "0110", n = 3
输出：true

示例 2：

输入：s = "0110", n = 4
输出：false

__提示:__

1 <= s.length <= 1000
s[i] 不是 '0' 就是 '1'
1 <= n <= 10^9

__思路__:

模拟
由于 n << 1 一定包含 n
只用检查大于 n << 1 的一半即可
时间复杂度为 O(nm), 空间复杂度为 O(1), 其中 m 为 s 的长度

__代码__:
__C++__:

```C++
class Solution
{
public:
    bool queryString(string s, int n) 
    {
        for (int i = (n >> 1) + 1; i <= n; i++) 
        {
            string cur = bitset<32>(i).to_string();
            cur.erase(0, cur.find('1'));
            if (s.find(cur) == -1) return false;
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean queryString(String s, int n) {
        for (int i = (n >>> 1) + 1; i <= n; i++) if (!s.contains(Integer.toBinaryString(i))) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def queryString(self, s: str, n: int) -> bool:
        return all('{:b}'.format(i) in s for i in range(1, n + 1))
```
