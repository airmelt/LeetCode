# 2506 Count Pairs Of Similar Strings 统计相似字符串对的数目

__Description:__

You are given a __0-indexed__ string array `words`.

Two strings are similar if they consist of the same characters.

- For example, `"abca"` and `"cba"` are similar since both consist of characters `'a'`, `'b'`, and `'c'`.
- However, `"abacba"` and `"bcfd"` are not similar since they do not consist of the same characters.

Return _the number of pairs_ `(i, j)` _such that_ `0 <= i < j <= word.length - 1` _and the two strings_ `words[i]` _and_ `words[j]` _are similar_.

__Example:__

Example 1:

```text
Input: words = ["aba","aabb","abcd","bac","aabc"]
Output: 2
Explanation: There are 2 pairs that satisfy the conditions:
```

- i = 0 and j = 1 : both words[0] and words[1] only consist of characters 'a' and 'b'.
- i = 3 and j = 4 : both words[3] and words[4] only consist of characters 'a', 'b', and 'c'.

Example 2:

```text
Input: words = ["aabb","ab","ba"]
Output: 3
Explanation: There are 3 pairs that satisfy the conditions:
```

- i = 0 and j = 1 : both words[0] and words[1] only consist of characters 'a' and 'b'.
- i = 0 and j = 2 : both words[0] and words[2] only consist of characters 'a' and 'b'.
- i = 1 and j = 2 : both words[1] and words[2] only consist of characters 'a' and 'b'.

Example 3:

```text
Input: words = ["nba","cba","dba"]
Output: 0
Explanation: Since there does not exist any pair that satisfies the conditions, we return 0.
```

__Constraints:__

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 100`
- `words[i]` consist of only lowercase English letters.

__题目描述:__

给你一个下标从 __0__ 开始的字符串数组 `words` 。

如果两个字符串由相同的字符组成，则认为这两个字符串 相似 。

- 例如， `"abca"` 和 `"cba"` 相似，因为它们都由字符 `'a'`、 `'b'`、 `'c'` 组成。
- 然而， `"abacba"` 和 `"bcfd"` 不相似，因为它们不是相同字符组成的。

请你找出满足字符串 `words[i]` 和 `words[j]` 相似的下标对 `(i, j)` ，并返回下标对的数目，其中 `0 <= i < j <= words.length - 1` 。

__示例:__

示例 1：

```text
输入：words = ["aba","aabb","abcd","bac","aabc"]
输出：2
解释：共有 2 对满足条件：
```

- i = 0 且 j = 1 ：words[0] 和 words[1] 只由字符 'a' 和 'b' 组成。
- i = 3 且 j = 4 ：words[3] 和 words[4] 只由字符 'a'、'b' 和 'c' 。

示例 2：

```text
输入：words = ["aabb","ab","ba"]
输出：3
解释：共有 3 对满足条件：
```

- i = 0 且 j = 1 ：words[0] 和 words[1] 只由字符 'a' 和 'b' 组成。
- i = 0 且 j = 2 ：words[0] 和 words[2] 只由字符 'a' 和 'b' 组成。
- i = 1 且 j = 2 ：words[1] 和 words[2] 只由字符 'a' 和 'b' 组成。

示例 3：

```text
输入：words = ["nba","cba","dba"]
输出：0
解释：不存在满足条件的下标对，返回 0 。
```

__提示：__

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 100`
- `words[i]` 仅由小写英文字母组成

__思路:__

```text
1. 暴力法
检查每一对字符串是否相似
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1), 不使用额外空间可以逐字符比较
2. 哈希表
使用哈希表记录每个字符串的字符集合
将字符串转化为一个二进制整数, 例如 "abc" = 0b111
使用哈希表记录每个字符串的二进制整数
遍历每个字符串, 计算其二进制整数
如果哈希表中已经存在该二进制整数, 则说明有相似字符串
累计该二进制整数的计数
并将该二进制整数的计数加一
时间复杂度为 O(MN), 空间复杂度为 O(N), 其中 M 为字符串的平均长度, N 为字符串的个数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int similarPairs(vector<string>& words) 
    {
        unordered_map<int, int> m;
        int result = 0;
        for (const auto& word : words) 
        {
            int mask = 0;
            for (const auto& c : word) mask |= 1 << (c - 'a');
            result += m[mask]++;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int similarPairs(String[] words) {
        var map = new HashMap<Integer, Integer>();
        int result = 0;
        for (var word : words) {
            int mask = 0;
            for (var c : word.toCharArray()) mask |= 1 << (c - 'a');
            result += map.getOrDefault(mask, 0);
            map.merge(mask, 1, Integer::sum);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def similarPairs(self, words: List[str]) -> int:
        return sum(set(words[i]) == set(words[j]) for i in range(len(words)) for j in range(i + 1, len(words)))
```
