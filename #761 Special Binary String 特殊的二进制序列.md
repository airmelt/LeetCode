# 761 Special Binary String 特殊的二进制序列

__Description__:
Special binary strings are binary strings with the following two properties:

The number of 0's is equal to the number of 1's.
Every prefix of the binary string has at least as many 1's as 0's.
You are given a special binary string s.

A move consists of choosing two consecutive, non-empty, special substrings of s, and swapping them. Two strings are consecutive if the last character of the first string is exactly one index before the first character of the second string.

Return the lexicographically largest resulting string possible after applying the mentioned operations on the string.

__Example:__

Example 1:

Input: s = "11011000"
Output: "11100100"
Explanation: The strings "10" [occuring at s[1]] and "1100" [at s[3]] are swapped.
This is the lexicographically largest string possible after some number of swaps.

Example 2:

Input: s = "10"
Output: "10"

__Constraints:__

1 <= s.length <= 50
s[i] is either '0' or '1'.
s is a special binary string.

__题目描述__:
特殊的二进制序列是具有以下两个性质的二进制序列：

0 的数量与 1 的数量相等。
二进制序列的每一个前缀码中 1 的数量要大于等于 0 的数量。
给定一个特殊的二进制序列 S，以字符串形式表示。定义一个操作 为首先选择 S 的两个连续且非空的特殊的子串，然后将它们交换。（两个子串为连续的当且仅当第一个子串的最后一个字符恰好为第二个子串的第一个字符的前一个字符。)

在任意次数的操作之后，交换后的字符串按照字典序排列的最大的结果是什么？

__示例 :__

示例 1:

输入: S = "11011000"
输出: "11100100"
解释:
将子串 "10" （在S[1]出现） 和 "1100" （在S[3]出现）进行交换。
这是在进行若干次操作后按字典序排列最大的结果。

__说明:__

S 的长度不超过 50。
S 保证为一个满足上述定义的特殊的二进制序列。

__思路__:

递归
10 转化为 "()"
特殊字符串则是满足括号要求的字符串, 左右括号相等, 且左括号始终不小于右括号的个数
递归找到所有特殊字符串中的特殊子字符串, 即找到一个在特殊字符串的最外层括号内的子串, 将其放入一个数组中, 直到 "()"
递归的给这个数组排序, 排序完成之后子串已经有序, 逆序将数组中的元素放入结果中
时间复杂度为 O(n!), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string makeLargestSpecial(string s) 
    {
        string result;
        vector<string> cur;
        int start = 0, left = 0;
        for (int i = 0, n = s.size(); i < n; i++) 
        {
            left += s[i] == '1' ? 1 : -1;
            if (left == 0) 
            {
                string child = s.substr(start + 1, i - start);
                cur.emplace_back("1" + makeLargestSpecial(child) + "0");
                start = i + 1;
            }
        }
        sort(cur.begin(), cur.end());
        for (auto it = cur.rbegin(); it != cur.rend(); it++) result += *it;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String makeLargestSpecial(String s) {
        StringBuilder sb = new StringBuilder();
        List<String> list = new ArrayList<>();
        int start = 0, left = 0;
        for (int i = 0, n = s.length(); i < n; i++) {
            left += s.charAt(i) == '1' ? 1 : -1;
            if (left == 0) {
                String str = s.substring(start + 1, i);
                list.add("1" + makeLargestSpecial(str) + "0");
                start = i + 1;
            }
        }
        Collections.sort(list);
        for (int i = list.size() - 1; i >= 0; i--) sb.append(list.get(i));
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def makeLargestSpecial(self, s: str) -> str:
        result, cur, start, left, n = "", [], 0, 0, len(s)
        for i in range(n):
            left += 1 if s[i] == '1' else -1
            if not left:
                cur.append('1' + self.makeLargestSpecial(s[start + 1:i]) + '0')
                start = i + 1
        cur.sort()
        for c in cur[::-1]:
            result += c
        return result
```
