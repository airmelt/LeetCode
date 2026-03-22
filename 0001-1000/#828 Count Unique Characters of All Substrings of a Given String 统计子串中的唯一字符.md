# 828 Count Unique Characters of All Substrings of a Given String 统计子串中的唯一字符

__Description__:
Let's define a function countUniqueChars(s) that returns the number of unique characters on s.

For example if s = "LEETCODE" then "L", "T", "C", "O", "D" are the unique characters since they appear only once in s, therefore countUniqueChars(s) = 5.
Given a string s, return the sum of countUniqueChars(t) where t is a substring of s.

Notice that some substrings can be repeated so in this case you have to count the repeated ones too.

__Example:__

Example 1:

Input: s = "ABC"
Output: 10
Explanation: All possible substrings are: "A","B","C","AB","BC" and "ABC".
Evey substring is composed with only unique letters.
Sum of lengths of all substring is 1 + 1 + 1 + 2 + 2 + 3 = 10

Example 2:

Input: s = "ABA"
Output: 8
Explanation: The same as example 1, except countUniqueChars("ABA") = 1.

Example 3:

Input: s = "LEETCODE"
Output: 92

__Constraints:__

1 <= s.length <= 10^5
s consists of uppercase English letters only.

__题目描述__:
我们定义了一个函数 countUniqueChars(s) 来统计字符串 s 中的唯一字符，并返回唯一字符的个数。

例如：s = "LEETCODE" ，则其中 "L", "T","C","O","D" 都是唯一字符，因为它们只出现一次，所以 countUniqueChars(s) = 5 。

本题将会给你一个字符串 s ，我们需要返回 countUniqueChars(t) 的总和，其中 t 是 s 的子字符串。注意，某些子字符串可能是重复的，但你统计时也必须算上这些重复的子字符串（也就是说，你必须统计 s 的所有子字符串中的唯一字符）。

由于答案可能非常大，请将结果 mod 10 ^ 9 + 7 后再返回。

__示例 :__

示例 1：

输入: s = "ABC"
输出: 10
解释: 所有可能的子串为："A","B","C","AB","BC" 和 "ABC"。
     其中，每一个子串都由独特字符构成。
     所以其长度总和为：1 + 1 + 1 + 2 + 2 + 3 = 10

示例 2：

输入: s = "ABA"
输出: 8
解释: 除了 countUniqueChars("ABA") = 1 之外，其余与示例 1 相同。

示例 3：

输入：s = "LEETCODE"
输出：92

__提示:__

0 <= s.length <= 10^4
s 只包含大写英文字符

__思路__:

1. 按照位置模拟
对每一个 i 向前找到第一个 s[i] == s[j] 下标, 向后找到第一个 s[i] == s[k] 的下标
依据乘法原理 i 的贡献度为 (i - j) * (k - i)
2. 按照字符模拟
对每个字符找到其位置, 然后向后搜索到第一个出现的位置
利用乘法原理进行累加
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int uniqueLetterString(string s) 
    {
        int n = s.size(), mod = 1e9 + 7, result = 0, j = 0, k = 0;
        for (int i = 0; i < n; i++) 
        {
            for (j = i - 1; j > -1 and s[j] != s[i]; j--);
            for (k = i + 1; k < n and s[k] != s[i]; k++);
            result += (i - j) * (k - i);
        }
        return result % mod;
    }
};
```

__Java__:

```Java
class Solution {
    public int uniqueLetterString(String s) {
        Map<Character, List<Integer>> index = new HashMap();
        int result = 0, n = s.length(), prev = 0, next = 0, mod = 1_000_000_007;
        for (int i = 0; i < s.length(); i++) index.computeIfAbsent(s.charAt(i), x-> new ArrayList<Integer>()).add(i);
        for (List<Integer> list: index.values()) {
            for (int i = 0, m = list.size(); i < m; i++) {
                prev = i > 0 ? list.get(i - 1) : -1;
                next = i < m - 1 ? list.get(i + 1) : n;
                result += (list.get(i) - prev) * (next - list.get(i));
            }
        }
        return result % mod;
    }
}
```

__Python__:

```Python
class Solution:
    def uniqueLetterString(self, s: str) -> int:
        n, mod, result, i = len(s), 10 ** 9 + 7, 0, 'A'
        while i <= 'Z':
            j, l, r = 0, -1, -1
            while j < n:
                if s[j] == i:
                    l, r = r, j
                result = (result + r - l) % mod
                j += 1
            i = chr(ord(i) + 1)
        return result % mod
```
