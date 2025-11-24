# 2781 Length of the Longest Valid Substring 最长合法子字符串的长度

__Description:__

You are given a string `word` and an array of strings `forbidden`.

A string is called __valid__ if none of its substrings are present in `forbidden`.

Return _the length of the __longest valid substring__ of the string_ `word`.

A substring is a contiguous sequence of characters in a string, possibly empty.

__Example:__

Example 1:

```text
Input: word = "cbaaaabc", forbidden = ["aaa","cb"]
Output: 4
Explanation: There are 11 valid substrings in word: "c", "b", "a", "ba", "aa", "bc", "baa", "aab", "ab", "abc" and "aabc". The length of the longest valid substring is 4. 
It can be shown that all other substrings contain either "aaa" or "cb" as a substring.
```

Example 2:

```text
Input: word = "leetcode", forbidden = ["de","le","e"]
Output: 4
Explanation: There are 11 valid substrings in word: "l", "t", "c", "o", "d", "tc", "co", "od", "tco", "cod", and "tcod". The length of the longest valid substring is 4.
It can be shown that all other substrings contain either "de", "le", or "e" as a substring.
```

__Constraints:__

- `1 <= word.length <= 10 ^ 5`
- `word` consists only of lowercase English letters.
- `1 <= forbidden.length <= 10 ^ 5`
- `1 <= forbidden[i].length <= 10`
- `forbidden[i]` consists only of lowercase English letters.

__题目描述:__

给你一个字符串 `word` 和一个字符串数组 `forbidden` 。

如果一个字符串不包含 `forbidden` 中的任何字符串，我们称这个字符串是 __合法__ 的。

请你返回字符串 `word` 的一个 __最长合法子字符串__ 的长度。

子字符串 指的是一个字符串中一段连续的字符，它可以为空。

__示例:__

示例 1：

```text
输入：word = "cbaaaabc", forbidden = ["aaa","cb"]
输出：4
解释：总共有 11 个合法子字符串："c", "b", "a", "ba", "aa", "bc", "baa", "aab", "ab", "abc" 和 "aabc"。最长合法子字符串的长度为 4 。
其他子字符串都要么包含 "aaa" ，要么包含 "cb" 。
```

示例 2：

```text
输入：word = "leetcode", forbidden = ["de","le","e"]
输出：4
解释：总共有 11 个合法子字符串："l" ，"t" ，"c" ，"o" ，"d" ，"tc" ，"co" ，"od" ，"tco" ，"cod" 和 "tcod" 。最长合法子字符串的长度为 4 。
所有其他子字符串都至少包含 "de" ，"le" 和 "e" 之一。
```

__提示：__

- `1 <= word.length <= 10 ^ 5`
- `word` 只包含小写英文字母。
- `1 <= forbidden.length <= 10 ^ 5`
- `1 <= forbidden[i].length <= 10`
- `forbidden[i]` 只包含小写英文字母。

__思路:__

```text
滑动窗口
将数组转化为哈希表加快查找速度
对于每个窗口的右边界
由于哈希表中字符串最长为 10
窗口最多往回找 10
找到一个子字符串在禁止列表中就停止查找
此时最长的字符串为 [i + 1:right + 1], 并令 left 为 i + 1
下一次从 i + 1 开始查找
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestValidSubstring(string word, vector<string>& forbidden) 
    {
        int result = 0, left = 0, n = word.size();
        unordered_set<string> fb(forbidden.begin(), forbidden.end());
        for (int right = 0; right < n; right++) 
        {
            for (int i = right; i > max(right - 10, left - 1); i--) 
            {
                if (fb.count(word.substr(i, right - i + 1))) 
                {
                    left = i + 1;
                    break;
                }
            }
            result = max(result, right - left + 1);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestValidSubstring(String word, List<String> forbidden) {
        int result = 0, left = 0, n = word.length();
        var fb = new HashSet<>(forbidden);
        for (int right = 0; right < n; right++) {
            for (int i = right; i > Math.max(right - 10, left - 1); i--) {
                if (fb.contains(word.substring(i, right + 1))) {
                    left = i + 1;
                    break;
                }
            }
            result = Math.max(result, right - left + 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def longestValidSubstring(self, word: str, forbidden: List[str]) -> int:
        result, left, forbidden, n = 0, 0, set(forbidden), len(word)
        for right in range(n):
            for i in range(right, max(right - 10, left - 1), -1):
                if word[i:right + 1] in forbidden:
                    left = i + 1
                    break
            result = max(result, right - left + 1)
        return result
```
