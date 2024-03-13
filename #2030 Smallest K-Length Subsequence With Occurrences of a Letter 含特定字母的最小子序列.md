# 2030 Smallest K-Length Subsequence With Occurrences of a Letter 含特定字母的最小子序列

__Description:__

You are given a string `s`, an integer `k`, a letter `letter`, and an integer `repetition`.

Return _the __lexicographically smallest__ subsequence of_ `s` _of length_ `k` _that has the letter_ `letter` _appear __at least___ `repetition` _times_. The test cases are generated so that the `letter` appears in `s` __at least__ `repetition` times.

A subsequence is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters.

A string `a` is __lexicographically smaller__ than a string `b` if in the first position where `a` and `b` differ, string `a` has a letter that appears earlier in the alphabet than the corresponding letter in `b`.

__Example:__

Example 1:

```text
Input: s = "leet", k = 3, letter = "e", repetition = 1
Output: "eet"
Explanation: There are four subsequences of length 3 that have the letter 'e' appear at least 1 time:
```

- "lee" (from "leet")
- "let" (from "leet")
- "let" (from "leet")
- "eet" (from "leet")

The lexicographically smallest subsequence among them is "eet".

Example 2:

![2030-1](https://assets.leetcode.com/uploads/2021/09/13/smallest-k-length-subsequence.png)

```text
Input: s = "leetcode", k = 4, letter = "e", repetition = 2
Output: "ecde"
Explanation: "ecde" is the lexicographically smallest subsequence of length 4 that has the letter "e" appear at least 2 times.
```

Example 3:

```text
Input: s = "bb", k = 2, letter = "b", repetition = 2
Output: "bb"
Explanation: "bb" is the only subsequence of length 2 that has the letter "b" appear at least 2 times.
```

__Constraints:__

- `1 <= repetition <= k <= s.length <= 5 * 10 ^ 4`
- `s` consists of lowercase English letters.
- `letter` is a lowercase English letter, and appears in `s` at least `repetition` times.

__题目描述:__

给你一个字符串 `s` ，一个整数 `k` ，一个字母 `letter` 以及另一个整数 `repetition` 。

返回 `s` 中长度为 `k` 且 __字典序最小__ 的子序列，该子序列同时应满足字母 `letter` 出现 __至少__ `repetition` 次。生成的测试用例满足 `letter` 在 `s` 中出现 __至少__ `repetition` 次。

子序列 是由原字符串删除一些（或不删除）字符且不改变剩余字符顺序得到的剩余字符串。

字符串 `a` 字典序比字符串 `b` 小的定义为:在 `a` 和 `b` 出现不同字符的第一个位置上，字符串 `a` 的字符在字母表中的顺序早于字符串 `b` 的字符。

__示例:__

示例 1：

```text
输入：s = "leet", k = 3, letter = "e", repetition = 1
输出："eet"
解释：存在 4 个长度为 3 ，且满足字母 'e' 出现至少 1 次的子序列：
```

- "lee"（"leet"）
- "let"（"leet"）
- "let"（"leet"）
- "eet"（"leet"）

其中字典序最小的子序列是 "eet" 。

示例 2：

![2030-2](https://assets.leetcode.com/uploads/2021/09/13/smallest-k-length-subsequence.png)

```text
输入：s = "leetcode", k = 4, letter = "e", repetition = 2
输出："ecde"
解释："ecde" 是长度为 4 且满足字母 "e" 出现至少 2 次的字典序最小的子序列。
```

示例 3：

```text
输入：s = "bb", k = 2, letter = "b", repetition = 2
输出："bb"
解释："bb" 是唯一一个长度为 2 且满足字母 "b" 出现至少 2 次的子序列。
```

__提示：__

- `1 <= repetition <= k <= s.length <= 5 * 10 ^ 4`
- `s` 由小写英文字母组成
- `letter` 是一个小写英文字母，在 `s` 中至少出现 `repetition` 次

__思路:__

```text
单调栈
统计 s 中 letter 出现的次数 d
用 q 记录栈中 letter 出现的次数
遍历 s 中的每个字符 c
两种情况需要弹出栈中的元素:
1. 单调栈里最后的元素需要弹出，这时需要保证 stack 非空，最后一个元素比当前元素大，后面有足够的元素供结果使用即 len(stack) + n - i > k, n - i 即为剩余元素，并且要保证 letter 的数目足够多，也就是 q + d - (stack[-1] == letter) >= repetition
2. 单调栈的元素需要替换，考虑 "aaabb", 需要 2 个 b 返回长度为 3 个子序列需要返回 "abb", 如果不处理, 单调栈里会是 "aaa", 这时由于单调栈已经足够 k 个元素就需要把最后的元素替换成需要的 letter, 即在 q + k - len(stack) < repetition 时, 将最后一个元素弹出并替换为当前元素
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string smallestSubsequence(string s, int k, char letter, int repetition) 
    {
        string result;
        int n = s.size(), d = ranges::count(s, letter), q = 0;
        for (int i = 0; i < n; i++) 
        {
            while (!result.empty() and result.back() > s[i] and result.size() + n - i > k and (result.back() != letter or q + d > repetition) or q + k - result.size() < repetition) 
            {
                q -= result.back() == letter;
                result.pop_back();
            }
            if (result.size() < k) 
            {
                result += s[i];
                q += s[i] == letter;
            }
            d -= s[i] == letter;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String smallestSubsequence(String s, int k, char letter, int repetition) {
        Stack<Character> stack = new Stack<>();
        int n = s.length(), d = (int)s.chars().filter(c -> c == letter).count(), q = 0;
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && stack.peek() > s.charAt(i) && stack.size() + n - i > k && (stack.peek() != letter || q + d > repetition) || q + k - stack.size() < repetition) q -= stack.pop() == letter ? 1 : 0;
            if (stack.size() < k) {
                stack.push(s.charAt(i));
                q += s.charAt(i) == letter ? 1 : 0;
            }
            d -= s.charAt(i) == letter ? 1 : 0;
        }
        return stack.stream().map(c -> c.toString()).collect(Collectors.joining(""));
    }
}
```

__Python__:

```Python
class Solution:
    def smallestSubsequence(self, s: str, k: int, letter: str, repetition: int) -> str:
        result, n, d, q = [], len(s), s.count(letter), 0
        for i, c in enumerate(s):
            print(result)
            while result and result[-1] > c and len(result) + n - i > k and q + d - (result[-1] == letter) >= repetition or q + k - len(result) < repetition:
                q -= result.pop() == letter
            if len(result) < k:
                result.append(c)
                q += c == letter
            d -= c == letter
        return ''.join(result)
```
