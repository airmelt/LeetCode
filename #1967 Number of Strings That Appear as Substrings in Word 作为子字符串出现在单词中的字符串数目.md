# 1967 Number of Strings That Appear as Substrings in Word 作为子字符串出现在单词中的字符串数目

__Description:__

Given an array of strings `patterns` and a string `word`, return _the __number__ of strings in_ `patterns` _that exist as a __substring__ in_ `word`.

A substring is a contiguous sequence of characters within a string.

__Example:__

Example 1:

```text
Input: patterns = ["a","abc","bc","d"], word = "abc"
Output: 3
Explanation:
```

- "a" appears as a substring in "abc".
- "abc" appears as a substring in "abc".
- "bc" appears as a substring in "abc".
- "d" does not appear as a substring in "abc".
3 of the strings in patterns appear as a substring in word.

Example 2:

```text
Input: patterns = ["a","b","c"], word = "aaaaabbbbb"
Output: 2
Explanation:
```

- "a" appears as a substring in "aaaaabbbbb".
- "b" appears as a substring in "aaaaabbbbb".
- "c" does not appear as a substring in "aaaaabbbbb".
2 of the strings in patterns appear as a substring in word.

Example 3:

```text
Input: patterns = ["a","a","a"], word = "ab"
Output: 3
Explanation: Each of the patterns appears as a substring in word "ab".
```

__Constraints:__

- `1 <= patterns.length <= 100`
- `1 <= patterns[i].length <= 100`
- `1 <= word.length <= 100`
- `patterns[i]` and `word` consist of lowercase English letters.

__题目描述:__

给你一个字符串数组 `patterns` 和一个字符串 `word` ，统计 `patterns` 中有多少个字符串是 `word` 的子字符串。返回字符串数目。

子字符串 是字符串中的一个连续字符序列。

__示例:__

示例 1：

```text
输入：patterns = ["a","abc","bc","d"], word = "abc"
输出：3
解释：
```

- "a" 是 "abc" 的子字符串。
- "abc" 是 "abc" 的子字符串。
- "bc" 是 "abc" 的子字符串。
- "d" 不是 "abc" 的子字符串。
patterns 中有 3 个字符串作为子字符串出现在 word 中。

示例 2：

```text
输入：patterns = ["a","b","c"], word = "aaaaabbbbb"
输出：2
解释：
```

- "a" 是 "aaaaabbbbb" 的子字符串。
- "b" 是 "aaaaabbbbb" 的子字符串。
- "c" 不是 "aaaaabbbbb" 的字符串。
patterns 中有 2 个字符串作为子字符串出现在 word 中。

示例 3：

```text
输入：patterns = ["a","a","a"], word = "ab"
输出：3
解释：patterns 中的每个字符串都作为子字符串出现在 word "ab" 中。
```

__提示：__

- `1 <= patterns.length <= 100`
- `1 <= patterns[i].length <= 100`
- `1 <= word.length <= 100`
- `patterns[i]` 和 `word` 由小写英文字母组成

__思路:__

```text
1. 暴力法
逐个检查 patterns 中的字符串是否是 word 的子字符串
时间复杂度为 O(MN), 空间复杂度为 O(1)
2. KMP 算法
时间复杂度为 O(M + N), 空间复杂度为 O(M)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numOfStrings(vector<string>& patterns, string word) 
    {
        auto check = [](const string& pattern, const string& word) -> bool
        {
            int m = pattern.size(), n = word.size();
            for (int i = 0; i + m <= n; i++)
            {
                bool flag = true;
                for (int j = 0; j < m; j++)
                {
                    if (word[i + j] != pattern[j])
                    {
                        flag = false;
                        break;
                    }
                }
                if (flag) return true;
            }
            return false;
        };
        int result = 0;
        for (const auto& pattern : patterns) result += check(pattern, word);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numOfStrings(String[] patterns, String word) {
        int result = 0;
        for (String s : patterns) if (word.contains(s)) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numOfStrings(self, patterns: List[str], word: str) -> int:
        return sum(p in word for p in patterns)
```
