# 1400 Construct K Palindrome Strings 构造K个回文字符串

__Description:__

Given a string s and an integer k, return true if you can use all the characters in s to construct k palindrome strings or false otherwise.

__Example:__

Example 1:

Input: s = "annabelle", k = 2
Output: true
Explanation: You can construct two palindromes using all characters in s.
Some possible constructions "anna" + "elble", "anbna" + "elle", "anellena" + "b"

Example 2:

Input: s = "leetcode", k = 3
Output: false
Explanation: It is impossible to construct 3 palindromes using all the characters of s.

Example 3:

Input: s = "true", k = 4
Output: true
Explanation: The only possible solution is to put each character in a separate string.

__Constraints:__

1 <= s.length <= 10^5
s consists of lowercase English letters.
1 <= k <= 10^5

__题目描述:__

给你一个字符串 s 和一个整数 k 。请你用 s 字符串中 所有字符 构造 k 个非空 回文串 。

如果你可以用 s 中所有字符构造 k 个回文字符串，那么请你返回 True ，否则返回 False 。

__示例:__

示例 1：

输入：s = "annabelle", k = 2
输出：true
解释：可以用 s 中所有字符构造 2 个回文字符串。
一些可行的构造方案包括："anna" + "elble"，"anbna" + "elle"，"anellena" + "b"

示例 2：

输入：s = "leetcode", k = 3
输出：false
解释：无法用 s 中所有字符构造 3 个回文串。

示例 3：

输入：s = "true", k = 4
输出：true
解释：唯一可行的方案是让 s 中每个字符单独构成一个字符串。

示例 4：

输入：s = "yzyzyzyzyzyzyzy", k = 2
输出：true
解释：你只需要将所有的 z 放在一个字符串中，所有的 y 放在另一个字符串中。那么两个字符串都是回文串。

示例 5：

输入：s = "cr", k = 7
输出：false
解释：我们没有足够的字符去构造 7 个回文串。

__提示：__

1 <= s.length <= 10^5
s 中所有字符都是小写英文字母。
1 <= k <= 10^5

__思路:__

```text
贪心
首先 s 的长度要足够分成 k 份
然后偶数个字符可以任意分配给 k 份中的任何一个部分
所以只需要统计出现次数为奇数的字符
这些字符出现的次数需要不大于 k
否则根据抽屉原理无法将两个出现奇数的字符分给同一个部分
时间复杂度为 O(N), 空间复杂度为 O(1), 空间需要记录字符的数目, 本题中为 26
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool canConstruct(string s, int k) 
    {
        int n = s.size(), odd = 0, count[26];
        if (n < k) return false;
        memset(count, 0, sizeof(count));
        for (const auto& c : s) ++count[c - 'a'];
        for (const auto& i : count) odd += i & 1;
        return odd <= k;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canConstruct(String s, int k) {
        int n = s.length(), odd = 0, count[] = new int[26];
        if (n < k) return false;
        for (char c : s.toCharArray()) ++count[c - 'a'];
        for (int i : count) if ((i & 1) != 0) ++odd;
        return odd <= k;
    }
}
```

__Python__:

```Python
class Solution:
    def canConstruct(self, s: str, k: int) -> bool:
        return len(s) >= k and sum(map(lambda x: x % 2, Counter(s).values())) <= k
```
