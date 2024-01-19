# 1961 Check If String Is a Prefix of Array 检查字符串是否为数组前缀

__Description:__

Given a string `s` and an array of strings `words`, determine whether `s` is a __prefix string__ of `words`.

A string `s` is a __prefix string__ of `words` if `s` can be made by concatenating the first `k` strings in `words` for some __positive__ `k` no larger than `words.length`.

Return `true` _if_ `s` _is a __prefix string__ of_ `words`_, or_ `false` _otherwise_.

__Example:__

Example 1:

```text
Input: s = "iloveleetcode", words = ["i","love","leetcode","apples"]
Output: true
Explanation:
s can be made by concatenating "i", "love", and "leetcode" together.
```

Example 2:

```text
Input: s = "iloveleetcode", words = ["apples","i","love","leetcode"]
Output: false
Explanation:
It is impossible to make s using a prefix of arr.
```

__Constraints:__

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 20`
- `1 <= s.length <= 1000`
- `words[i]` and `s` consist of only lowercase English letters.

__题目描述:__

给你一个字符串 `s` 和一个字符串数组 `words` ，请你判断 `s` 是否为 `words` 的 __前缀字符串__ 。

字符串 `s` 要成为 `words` 的 __前缀字符串__ ，需要满足: `s` 可以由 `words` 中的前 `k`（ `k` 为 __正数__ ）个字符串按顺序相连得到，且 `k` 不超过 `words.length` 。

如果 `s` 是 `words` 的 __前缀字符串__ ，返回 `true` ；否则，返回 `false` 。

__示例:__

示例 1：

```text
输入：s = "iloveleetcode", words = ["i","love","leetcode","apples"]
输出：true
解释：
s 可以由 "i"、"love" 和 "leetcode" 相连得到。
```

示例 2：

```text
输入：s = "iloveleetcode", words = ["apples","i","love","leetcode"]
输出：false
解释：
数组的前缀相连无法得到 s 。
```

__提示：__

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 20`
- `1 <= s.length <= 1000`
- `words[i]` 和 `s` 仅由小写英文字母组成

__思路:__

```text

时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution {
public:
    bool isPrefixString(string s, vector<string>& words) {

    }
};
```

__Java__:

```Java
class Solution {
    public boolean isPrefixString(String s, String[] words) {

    }
}
```

__Python__:

```Python
class Solution:
    def isPrefixString(self, s: str, words: List[str]) -> bool:
```
