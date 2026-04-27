# 2068 Check Whether Two Strings are Almost Equivalent 检查两个字符串是否几乎相等

__Description:__

Two strings `word1` and `word2` are considered __almost equivalent__ if the differences between the frequencies of each letter from `'a'` to `'z'` between `word1` and `word2` is __at most__ `3`.

Given two strings `word1` and `word2`, each of length `n`, return `true` _if_ `word1` _and_ `word2` _are __almost equivalent__, or_ `false` _otherwise_.

The __frequency__ of a letter `x` is the number of times it occurs in the string.

__Example:__

Example 1:

```text
Input: word1 = "aaaa", word2 = "bccb"
Output: false
Explanation: There are 4 'a's in "aaaa" but 0 'a's in "bccb".
The difference is 4, which is more than the allowed 3.
```

Example 2:

```text
Input: word1 = "abcdeef", word2 = "abaaacc"
Output: true
Explanation: The differences between the frequencies of each letter in word1 and word2 are at most 3:
```

- 'a' appears 1 time in word1 and 4 times in word2. The difference is 3.
- 'b' appears 1 time in word1 and 1 time in word2. The difference is 0.
- 'c' appears 1 time in word1 and 2 times in word2. The difference is 1.
- 'd' appears 1 time in word1 and 0 times in word2. The difference is 1.
- 'e' appears 2 times in word1 and 0 times in word2. The difference is 2.
- 'f' appears 1 time in word1 and 0 times in word2. The difference is 1.

Example 3:

```text
Input: word1 = "cccddabba", word2 = "babababab"
Output: true
Explanation: The differences between the frequencies of each letter in word1 and word2 are at most 3:
```

- 'a' appears 2 times in word1 and 4 times in word2. The difference is 2.
- 'b' appears 2 times in word1 and 5 times in word2. The difference is 3.
- 'c' appears 3 times in word1 and 0 times in word2. The difference is 3.
- 'd' appears 2 times in word1 and 0 times in word2. The difference is 2.

__Constraints:__

- `n == word1.length == word2.length`
- `1 <= n <= 100`
- `word1` and `word2` consist only of lowercase English letters.

__题目描述:__

如果两个字符串 `word1` 和 `word2` 中从 `'a'` 到 `'z'` 每一个字母出现频率之差都 __不超过__ `3` ，那么我们称这两个字符串 `word1` 和 `word2` __几乎相等__ 。

给你两个长度都为 `n` 的字符串 `word1` 和 `word2` ，如果 `word1` 和 `word2` __几乎相等__ ，请你返回 `true` ，否则返回 `false` 。

一个字母 `x` 的出现 __频率__ 指的是它在字符串中出现的次数。

__示例:__

示例 1：

```text
输入：word1 = "aaaa", word2 = "bccb"
输出：false
解释：字符串 "aaaa" 中有 4 个 'a' ，但是 "bccb" 中有 0 个 'a' 。
两者之差为 4 ，大于上限 3 。
```

示例 2：

```text
输入：word1 = "abcdeef", word2 = "abaaacc"
输出：true
解释：word1 和 word2 中每个字母出现频率之差至多为 3 ：
```

- 'a' 在 word1 中出现了 1 次，在 word2 中出现了 4 次，差为 3 。
- 'b' 在 word1 中出现了 1 次，在 word2 中出现了 1 次，差为 0 。
- 'c' 在 word1 中出现了 1 次，在 word2 中出现了 2 次，差为 1 。
- 'd' 在 word1 中出现了 1 次，在 word2 中出现了 0 次，差为 1 。
- 'e' 在 word1 中出现了 2 次，在 word2 中出现了 0 次，差为 2 。
- 'f' 在 word1 中出现了 1 次，在 word2 中出现了 0 次，差为 1 。

示例 3：

```text
输入：word1 = "cccddabba", word2 = "babababab"
输出：true
解释：word1 和 word2 中每个字母出现频率之差至多为 3 ：
```

- 'a' 在 word1 中出现了 2 次，在 word2 中出现了 4 次，差为 2 。
- 'b' 在 word1 中出现了 2 次，在 word2 中出现了 5 次，差为 3 。
- 'c' 在 word1 中出现了 3 次，在 word2 中出现了 0 次，差为 3 。
- 'd' 在 word1 中出现了 2 次，在 word2 中出现了 0 次，差为 2 。

__提示：__

- `n == word1.length == word2.length`
- `1 <= n <= 100`
- `word1` 和 `word2` 都只包含小写英文字母。

__思路:__

```text
模拟
用哈希表或者数组记录出现过的字符的频
然后遍历两个字符串
在哈希表中记录字符出现的频率的差值
最后判断是否所有字符的频率差值都小于等于 3
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool checkAlmostEquivalent(string word1, string word2) 
    {
        unordered_map<char, int> freq;
        for (const auto& c: word1) ++freq[c];
        for (const auto& c: word2) --freq[c];
        return all_of(freq.cbegin(), freq.cend(), [](auto&& x) { return abs(x.second) <= 3; });
    }
};
```

__Java__:

```Java
class Solution {
    public boolean checkAlmostEquivalent(String word1, String word2) {
        int[] count = new int[26];
        for (int i = 0, n = word1.length(); i < n; i++) {
            ++count[word1.charAt(i) - 'a'];
            --count[word2.charAt(i) - 'a'];
        }
        return Arrays.stream(count).allMatch(n -> Math.abs(n) <= 3);
    }
}
```

__Python__:

```Python
class Solution:
    def checkAlmostEquivalent(self, word1: str, word2: str) -> bool:
```
