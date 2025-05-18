# 2531 Make Number of Distinct Characters Equal 使字符串中不同字符的数目相等

__Description:__

You are given two __0-indexed__ strings `word1` and `word2`.

A __move__ consists of choosing two indices `i` and `j` such that `0 <= i < word1.length` and `0 <= j < word2.length` and swapping `word1[i]` with `word2[j]`.

Return `true` _if it is possible to get the number of distinct characters in_ `word1` _and_ `word2` _to be equal with __exactly one__ move._ Return `false` _otherwise_.

__Example:__

Example 1:

```text
Input: word1 = "ac", word2 = "b"
Output: false
Explanation: Any pair of swaps would yield two distinct characters in the first string, and one in the second string.
```

Example 2:

```text
Input: word1 = "abcc", word2 = "aab"
Output: true
Explanation: We swap index 2 of the first string with index 0 of the second string. The resulting strings are word1 = "abac" and word2 = "cab", which both have 3 distinct characters.
```

Example 3:

```text
Input: word1 = "abcde", word2 = "fghij"
Output: true
Explanation: Both resulting strings will have 5 distinct characters, regardless of which indices we swap.
```

__Constraints:__

- `1 <= word1.length, word2.length <= 10 ^ 5`
- `word1` and `word2` consist of only lowercase English letters.

__题目描述:__

给你两个下标从 __0__ 开始的字符串 `word1` 和 `word2` 。

一次 移动 由以下两个步骤组成：

- 选中两个下标 `i` 和 `j` ，分别满足 `0 <= i < word1.length` 和 `0 <= j < word2.length` ，
- 交换 `word1[i]` 和 `word2[j]` 。

如果可以通过 __恰好一次__ 移动，使 `word1` 和 `word2` 中不同字符的数目相等，则返回 `true` ；否则，返回 `false` 。

__示例:__

示例 1：

```text
输入：word1 = "ac", word2 = "b"
输出：false
解释：交换任何一组下标都会导致第一个字符串中有 2 个不同的字符，而在第二个字符串中只有 1 个不同字符。
```

示例 2：

```text
输入：word1 = "abcc", word2 = "aab"
输出：true
解释：交换第一个字符串的下标 2 和第二个字符串的下标 0 。之后得到 word1 = "abac" 和 word2 = "cab" ，各有 3 个不同字符。
```

示例 3：

```text
输入：word1 = "abcde", word2 = "fghij"
输出：true
解释：无论交换哪一组下标，两个字符串中都会有 5 个不同字符。
```

__提示：__

- `1 <= word1.length, word2.length <= 10 ^ 5`
- `word1` 和 `word2` 仅由小写英文字母组成。

__思路:__

```text
模拟
枚举 26 个字母成对的情况
共计 26 ^ 2 种情况
要么移动两个相同字符两个字符串中字符出现次数相等
要么移动两个不同字符的情况下
如果移动了最后一个字符
word1 中的字符数减一
word1 中需要加上另一个字符是否出现过
比较移动之后两个出现次数是否相等
时间复杂度为 O(M + N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool isItPossible(string word1, string word2) 
    {
        unordered_map<char, int> c1, c2;
        for (const auto& c : word1) c1[c]++;
        for (const auto& c : word2) c2[c]++;
        for (auto& [x, c] : c1) for (auto& [y, d] : c2) if (x == y and c1.size() == c2.size() || (x != y and c1.size() - (c == 1) + !c1.contains(y) == c2.size() - (d == 1) + !c2.contains(x))) return true;
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isItPossible(String word1, String word2) {
        Map<Character, Integer> c1 = new HashMap<>(), c2 = new HashMap<>();
        for (var c : word1.toCharArray()) c1.merge(c, 1, Integer::sum);
        for (var c : word2.toCharArray()) c2.merge(c, 1, Integer::sum);
        for (var e : c1.entrySet()) for (var f : c2.entrySet()) if (e.getKey() == f.getKey() && c1.size() == c2.size() || (e.getKey() != f.getKey() && (c1.size() - (e.getValue() == 1 ? 1 : 0) + (c1.containsKey(f.getKey()) ? 0 : 1) == c2.size() - (f.getValue() == 1 ? 1 : 0) + (c2.containsKey(e.getKey()) ? 0 : 1)))) return true;
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def isItPossible(self, word1: str, word2: str) -> bool:
        return (c1 := Counter(word1)) and (c2 := Counter(word2)) and any(x == y and len(c1) == len(c2) or (x != y and len(c1) - (v1 == 1) + (y not in c1) == len(c2) - (v2 == 1) + (x not in c2)) for x, v1 in c1.items() for y, v2 in c2.items())
```
