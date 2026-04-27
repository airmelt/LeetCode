# 2953 Count Complete Substrings 统计完全子字符串

__Description:__

You are given a string `word` and an integer `k`.

A substring `s` of `word` is __complete__ if:

- Each character in `s` occurs __exactly__ `k` times.
- The difference between two adjacent characters is __at most__ `2`. That is, for any two adjacent characters `c1` and `c2` in `s`, the absolute difference in their positions in the alphabet is __at most__ `2`.

Return _the number of __complete__ substrings of_ `word`.

A substring is a non-empty contiguous sequence of characters in a string.

__Example:__

Example 1:

```text
Input: word = "igigee", k = 2
Output: 3
Explanation: The complete substrings where each character appears exactly twice and the difference between adjacent characters is at most 2 are: igigee, igigee, igigee.
```

Example 2:

```text
Input: word = "aaabbbccc", k = 3
Output: 6
Explanation: The complete substrings where each character appears exactly three times and the difference between adjacent characters is at most 2 are: aaabbbccc, aaabbbccc, aaabbbccc, aaabbbccc, aaabbbccc, aaabbbccc.
```

__Constraints:__

- `1 <= word.length <= 10 ^ 5`
- `word` consists only of lowercase English letters.
- `1 <= k <= word.length`

__题目描述:__

给你一个字符串 `word` 和一个整数 `k` 。

如果 `word` 的一个子字符串 `s` 满足以下条件，我们称它是 __完全字符串:__

- `s` 中每个字符 __恰好__ 出现 `k` 次。
- 相邻字符在字母表中的顺序 __至多__ 相差 `2` 。也就是说， `s` 中两个相邻字符 `c1` 和 `c2` ，它们在字母表中的位置相差 __至多__ 为 `2` 。

请你返回 `word` 中 __完全__ 子字符串的数目。

子字符串 指的是一个字符串中一段连续 非空 的字符序列。

__示例:__

示例 1：

```text
输入：word = "igigee", k = 2
输出：3
解释：完全子字符串需要满足每个字符恰好出现 2 次，且相邻字符相差至多为 2 ：igigee, igigee, igigee 。
```

示例 2：

```text
输入：word = "aaabbbccc", k = 3
输出：6
解释：完全子字符串需要满足每个字符恰好出现 3 次，且相邻字符相差至多为 2 ：aaabbbccc, aaabbbccc, aaabbbccc, aaabbbccc, aaabbbccc, aaabbbccc 。
```

__提示：__

- `1 <= word.length <= 10 ^ 5`
- `word` 只包含小写英文字母。
- `1 <= k <= word.length`

__思路:__

```text
滑动窗口
先将相邻元素不超过 2 的子字符串提取出来
对于子字符串
一共有 26 种小写字母的组合
从 1 遍历到 26 种组合
检查子字符串中是否有对应的组合
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countCompleteSubstrings(string word, int k) 
    {
        int result = 0, n = word.size();
        auto helper = [&](const auto& s) -> int
        {
            int result = 0;
            for (int m = 1, n = s.size(); m <= 26 and k * m <= n; m++) 
            {
                int d[26]{ 0 };
                for (int right = 0, left = 0; right < n; right++) 
                {
                    ++d[s[right] - 'a'];
                    if ((left = right + 1 - k * m) > -1) 
                    {
                        bool flag = true;
                        for (int i = 0; i < 26; i++) 
                        {
                            if (d[i] and d[i] != k) 
                            {
                                flag = false;
                                break;
                            }
                        }
                        if (flag) ++result;
                        --d[s[left] - 'a'];
                    }
                }
            }
            return result;
        };
        for (int i = 0, start = i; i < n; start = i) 
        {
            for (i++; i < n and abs(word[i] - word[i - 1]) <= 2; i++) ;
            result += helper(word.substr(start, i - start));
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countCompleteSubstrings(String word, int k) {
        int result = 0, n = word.length();
        for (int i = 0, start = i; i < n; start = i) {
            for (i++; i < n && Math.abs(word.charAt(i) - word.charAt(i - 1)) <= 2; i++) ;
            result += helper(word.substring(start, i).toCharArray(), k);
        }
        return result;
    }

    private int helper(char[] s, int k) {
        int result = 0;
        for (int m = 1, n = s.length; m <= 26 && k * m <= n; m++) {
            var d = new int[26];
            for (int right = 0, left = 0; right < n; right++) {
                ++d[s[right] - 'a'];
                if ((left = right + 1 - k * m) > -1) {
                    boolean flag = true;
                    for (int i = 0; i < 26; i++) {
                        if (d[i] > 0 && d[i] != k) {
                            flag = false;
                            break;
                        }
                    }
                    if (flag) ++result;
                    --d[s[left] - 'a'];
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countCompleteSubstrings(self, word: str, k: int) -> int:
        def helper(s: str) -> int:
            result = 0
            for m in range(1, 27):
                if k * m > len(s):
                    break
                d = Counter()
                for right, c in enumerate(s):
                    d[c] += 1
                    left = right + 1 - k * m
                    if left > -1:
                        result += all(not c or c == k for c in d.values())
                        d[s[left]] -= 1
            return result

        n, result, i = len(word), 0, 0
        while i < n:
            start = i
            i += 1
            while i < n and abs(ord(word[i]) - ord(word[i - 1])) <= 2:
                i += 1
            result += helper(word[start:i])
        return result
```
