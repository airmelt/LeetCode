# 2185 Counting Words With a Given Prefix 统计包含给定前缀的字符串

__Description:__

You are given an array of strings `words` and a string `pref`.

Return _the number of strings in_ `words` _that contain_ `pref` _as a __prefix___.

A __prefix__ of a string `s` is any leading contiguous substring of `s`.

__Example:__

Example 1:

```text
Input:  words = ["pay","attention","practice","attend"], pref = "at"
Output:  2
Explanation:  The 2 strings that contain "at" as a prefix are: "attention" and "attend".
```

Example 2:

```text
Input:  words = ["leetcode","win","loops","success"], `pref` = "code"
Output:  0
Explanation:  There are no strings that contain "code" as a prefix.
```

__Constraints:__

- `1 <= words.length <= 100`
- `1 <= words[i].length, pref.length <= 100`
- `words[i]` and `pref` consist of lowercase English letters.

__题目描述:__

给你一个字符串数组 `words` 和一个字符串 `pref` 。

返回 `words` 中以 `pref` 作为 __前缀__ 的字符串的数目。

字符串 `s` 的 __前缀__ 就是  `s` 的任一前导连续字符串。

__示例:__

示例 1：

```text
输入: words = ["pay","_at_tention","practice","_at_tend"], pref = "at"
输出: 2
解释: 以 "at" 作为前缀的字符串有两个，分别是:"attention" 和 "attend" 。
```

示例 2：

```text
输入: words = ["leetcode","win","loops","success"], `pref` = "code"
输出: 0
解释: 不存在以 "code" 作为前缀的字符串。
```

__提示：__

- `1 <= words.length <= 100`
- `1 <= words[i].length, pref.length <= 100`
- `words[i]` 和 `pref` 由小写英文字母组成

__思路:__

```text
模拟
遍历 words 中的每个字符串，判断是否以 pref 作为前缀，如果是则计数加一
时间复杂度为 O(MN), 空间复杂度为 O(1), 其中 M 为 words 的长度，N 为 words 中字符串的平均长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int prefixCount(vector<string>& words, string pref) 
    {
        int result = 0;
        for (const auto& word : words) result += !word.find(pref);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int prefixCount(String[] words, String pref) {
        return (int)Arrays.stream(words).filter(s -> s.startsWith(pref)).count();
    }
}
```

__Python__:

```Python
class Solution:
    def prefixCount(self, words: List[str], pref: str) -> int:
        return sum(i.startswith(pref) for i in words)
```
