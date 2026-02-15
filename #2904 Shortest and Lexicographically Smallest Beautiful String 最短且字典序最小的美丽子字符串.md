# 2904 Shortest and Lexicographically Smallest Beautiful String 最短且字典序最小的美丽子字符串

__Description:__

You are given a binary string `s` and a positive integer `k`.

A substring of `s` is __beautiful__ if the number of `1`'s in it is exactly `k`.

Let `len` be the length of the __shortest__ beautiful substring.

Return _the lexicographically __smallest__ beautiful substring of string_ `s` _with length equal to_ `len`. If `s` doesn't contain a beautiful substring, return _an __empty__ string_.

A string `a` is lexicographically __larger__ than a string `b` (of the same length) if in the first position where `a` and `b` differ, `a` has a character strictly larger than the corresponding character in `b`.

- For example, `"abcd"` is lexicographically larger than `"abcc"` because the first position they differ is at the fourth character, and `d` is greater than `c`.

__Example:__

Example 1:

```text
Input: s = "100011001", k = 3
Output: "11001"
Explanation: There are 7 beautiful substrings in this example:
1. The substring "100011001".
2. The substring "100011001".
3. The substring "100011001".
4. The substring "100011001".
5. The substring "100011001".
6. The substring "100011001".
7. The substring "100011001".
The length of the shortest beautiful substring is 5.
The lexicographically smallest beautiful substring with length 5 is the substring "11001".
```

Example 2:

```text
Input: s = "1011", k = 2
Output: "11"
Explanation: There are 3 beautiful substrings in this example:
1. The substring "1011".
2. The substring "1011".
3. The substring "1011".
The length of the shortest beautiful substring is 2.
The lexicographically smallest beautiful substring with length 2 is the substring "11".
```

Example 3:

```text
Input: s = "000", k = 1
Output: ""
Explanation: There are no beautiful substrings in this example.
```

__Constraints:__

- `1 <= s.length <= 100`
- `1 <= k <= s.length`

__题目描述:__

给你一个二进制字符串 `s` 和一个正整数 `k` 。

如果 `s` 的某个子字符串中 `1` 的个数恰好等于 `k` ，则称这个子字符串是一个 __美丽子字符串__ 。

令 `len` 等于 __最短__ 美丽子字符串的长度。

返回长度等于 `len` 且字典序 __最小__ 的美丽子字符串。如果 `s` 中不含美丽子字符串，则返回一个 __空__ 字符串。

对于相同长度的两个字符串 `a` 和 `b` ，如果在 `a` 和 `b` 出现不同的第一个位置上， `a` 中该位置上的字符严格大于 `b` 中的对应字符，则认为字符串 `a` 字典序 __大于__ 字符串 `b` 。

- 例如， `"abcd"` 的字典序大于 `"abcc"` ，因为两个字符串出现不同的第一个位置对应第四个字符，而 `d` 大于 `c` 。

__示例:__

示例 1：

```text
输入：s = "100011001", k = 3
输出："11001"
解释：示例中共有 7 个美丽子字符串：
1. 子字符串 "100011001" 。
2. 子字符串 "100011001" 。
3. 子字符串 "100011001" 。
4. 子字符串 "100011001" 。
5. 子字符串 "100011001" 。
6. 子字符串 "100011001" 。
7. 子字符串 "100011001" 。
最短美丽子字符串的长度是 5 。
长度为 5 且字典序最小的美丽子字符串是子字符串 "11001" 。
```

示例 2：

```text
输入：s = "1011", k = 2
输出："11"
解释：示例中共有 3 个美丽子字符串：
1. 子字符串 "1011" 。
2. 子字符串 "1011" 。
3. 子字符串 "1011" 。
最短美丽子字符串的长度是 2 。
长度为 2 且字典序最小的美丽子字符串是子字符串 "11" 。
```

示例 3：

```text
输入：s = "000", k = 1
输出：""
解释：示例中不存在美丽子字符串。
```

__提示：__

- `1 <= s.length <= 100`
- `1 <= k <= s.length`

__思路:__

```text
1. 暴力
找到每一个包括 k 个 1 的子串
按照长度和大小排序之后选择最小的
时间复杂度为 O(N ^ 3), 空间复杂度为 O(N ^ 2)
2. 滑动窗口
保证窗口中有 k 个 '1'
如果没有直接结束返回空字符串
如果窗口中的字符 '1' 大于 k 个
移动 left 直到遇到下一个 '1'
更新结果
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string shortestBeautifulSubstring(string s, int k) 
    {
        if (ranges::count(s, '1') < k) return "";
        string result = s, t = "";
        for (int cur = 0, left = 0, right = 0, n = s.size(); right < n; right++) 
        {
            cur += s[right] - '0';
            while (cur > k or s[left] == '0') cur -= s[left++] - '0';
            if (cur == k) if ((t = s.substr(left, right - left + 1)).size() < result.size() or t.size() == result.size() and t < result) result = t;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String shortestBeautifulSubstring(String s, int k) {
        if (s.replace("0", "").length() < k) return "";
        String result = s, t = "";
        for (int cur = 0, left = 0, right = 0, n = s.length(); right < n; right++) {
            cur += s.charAt(right) - '0';
            while (cur > k || s.charAt(left) == '0') cur -= s.charAt(left++) - '0';
            if (cur == k) if ((t = s.substring(left, right + 1)).length() < result.length() || t.length() == result.length() && t.compareTo(result) < 0) result = t;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def shortestBeautifulSubstring(self, s: str, k: int) -> str:
        return a[0] if (a := sorted([s[i:j + 1] for i in range(len(s)) for j in range(i, len(s)) if s[i:j + 1].count('1') == k], key=lambda x: (len(x), x))) else ''
```
