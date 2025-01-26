# 2423 Remove Letter To Equalize Frequency 删除字符使频率相同

__Description:__

You are given a __0-indexed__ string `word`, consisting of lowercase English letters. You need to select __one__ index and __remove__ the letter at that index from `word` so that the __frequency__ of every letter present in `word` is equal.

Return `true` _if it is possible to remove one letter so that the frequency of all letters in_ `word` _are equal, and_ `false` _otherwise_.

Note:

- The _frequency_ of a letter `x` is the number of times it occurs in the string.
- You __must__ remove exactly one letter and cannot choose to do nothing.

__Example:__

Example 1:

```text
Input: word = "abcc"
Output: true
Explanation: Select index 3 and delete it: word becomes "abc" and each character has a frequency of 1.
```

Example 2:

```text
Input: word = "aazz"
Output: false
Explanation: We must delete a character, so either the frequency of "a" is 1 and the frequency of "z" is 2, or vice versa. It is impossible to make all present letters have equal frequency.
```

__Constraints:__

- `2 <= word.length <= 100`
- `word` consists of lowercase English letters only.

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `word` ，字符串只包含小写英文字母。你需要选择 __一个__ 下标并 __删除__ 下标处的字符，使得 `word` 中剩余每个字母出现 __频率__ 相同。

如果删除一个字母后， `word` 中剩余所有字母的出现频率都相同，那么返回 `true` ，否则返回 `false` 。

注意：

- 字母 `x` 的 __频率__ 是这个字母在字符串中出现的次数。
- 你 __必须__ 恰好删除一个字母，不能一个字母都不删除。

__示例:__

示例 1：

```text
输入：word = "abcc"
输出：true
解释：选择下标 3 并删除该字母：word 变成 "abc" 且每个字母出现频率都为 1 。
```

示例 2：

```text
输入：word = "aazz"
输出：false
解释：我们必须删除一个字母，所以要么 "a" 的频率变为 1 且 "z" 的频率为 2 ，要么两个字母频率反过来。所以不可能让剩余所有字母出现频率相同。
```

__提示：__

- `2 <= word.length <= 100`
- `word` 只包含小写英文字母。

__思路:__

```text
1. 枚举
遍历每一个字符
删除当前字符后, 统计剩余字符的频率
如果剩余字符的频率都相同, 则返回 true
否则返回 false
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1), 实际上需要小写字母个数的空间, 即 O(26)
2. 统计
先统计每个字符的频率
再统计每个频率的个数
如果只有一个频率, 则返回 true, 即所有字符串的字符都相同, 类似 "aaa"
如果出现较少的频率为 1, 且其他的频率都相同, 则返回 true, 类似 "abcc"
如果出现较多的频率为 k + 1, 且其他的频率都相同并且均为 k, 则返回 true, 类似 "aaabbcc"
否则返回 false
时间复杂度为 O(N), 空间复杂度为 O(1), 实际上时间复杂度为 O(N + 26log26), 空间复杂度为 O(26), 小写字母个数为 26
```

__代码:__

__C++__:

```C++
class Solution {
    public boolean equalFrequency(String word) {
        for (int i = 0, n = word.length(); i < n; i++) {
            Map<Character, Integer> map = new HashMap<>();
            for (int j = 0; j < n; j++) if (j != i) map.merge(word.charAt(j), 1, Integer::sum);
            if (helper(map)) return true;
        }
        return false;
    }

    private boolean helper(Map<Character, Integer> map) {
        int a = map.entrySet().iterator().next().getValue();
        for (int c : map.values()) if (c != a) return false;
        return true;
    }
}
```

__Java__:

```Java
class Solution {
    public boolean equalFrequency(String word) {
        for (int i = 0, n = word.length(); i < n; i++) {
            Map<Character, Integer> map = new HashMap<>();
            for (int j = 0; j < n; j++) if (j != i) map.merge(word.charAt(j), 1, Integer::sum);
            if (helper(map)) return true;
        }
        return false;
    }

    private boolean helper(Map<Character, Integer> map) {
        int a = map.entrySet().iterator().next().getValue();
        for (int c : map.values()) if (c != a) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def equalFrequency(self, word: str) -> bool:
        return any(len(set((word[:i] + word[i + 1:]).count(k) for k in (word[:i] + word[i + 1:]))) == 1 for i in range(len(word)))
```
