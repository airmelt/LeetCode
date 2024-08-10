# 2207 Maximize Number of Subsequences in a String 字符串中最多数目的子序列

__Description:__

You are given a __0-indexed__ string `text` and another __0-indexed__ string `pattern` of length `2`, both of which consist of only lowercase English letters.

You can add __either__ `pattern[0]` __or__ `pattern[1]` anywhere in `text` __exactly once__. Note that the character can be added even at the beginning or at the end of `text`.

Return _the __maximum__ number of times_ `pattern` _can occur as a __subsequence__ of the modified_ `text`.

A subsequence is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters.

__Example:__

Example 1:

```text
Input: text = "abdcdbc", pattern = "ac"
Output: 4
Explanation:
If we add pattern[0] = 'a' in between text[1] and text[2], we get "abadcdbc". Now, the number of times "ac" occurs as a subsequence is 4.
Some other strings which have 4 subsequences "ac" after adding a character to text are "aabdcdbc" and "abdacdbc".
However, strings such as "abdcadbc", "abdccdbc", and "abdcdbcc", although obtainable, have only 3 subsequences "ac" and are thus suboptimal.
It can be shown that it is not possible to get more than 4 subsequences "ac" by adding only one character.
```

Example 2:

```text
Input: text = "aabb", pattern = "ab"
Output: 6
Explanation:
Some of the strings which can be obtained from text and have 6 subsequences "ab" are "aaabb", "aaabb", and "aabbb".
```

__Constraints:__

- `1 <= text.length <= 10 ^ 5`
- `pattern.length == 2`
- `text` and `pattern` consist only of lowercase English letters.

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `text` 和另一个下标从 __0__ 开始且长度为 `2` 的字符串 `pattern` ，两者都只包含小写英文字母。

你可以在 `text` 中任意位置插入 __一个__ 字符，这个插入的字符必须是 `pattern[0]` _或者_  `pattern[1]` 。注意，这个字符可以插入在 `text` 开头或者结尾的位置。

请你返回插入一个字符后， `text` 中最多包含多少个等于 `pattern` 的 __子序列__ 。

子序列 指的是将一个字符串删除若干个字符后（也可以不删除），剩余字符保持原本顺序得到的字符串。

__示例:__

示例 1：

```text
输入：text = "abdcdbc", pattern = "ac"
输出：4
解释：
如果我们在 text[1] 和 text[2] 之间添加 pattern[0] = 'a' ，那么我们得到 "abadcdbc" 。那么 "ac" 作为子序列出现 4 次。
其他得到 4 个 "ac" 子序列的方案还有 "aabdcdbc" 和 "abdacdbc" 。
但是，"abdcadbc" ，"abdccdbc" 和 "abdcdbcc" 这些字符串虽然是可行的插入方案，但是只出现了 3 次 "ac" 子序列，所以不是最优解。
可以证明插入一个字符后，无法得到超过 4 个 "ac" 子序列。
```

示例 2：

```text
输入：text = "aabb", pattern = "ab"
输出：6
解释：
可以得到 6 个 "ab" 子序列的部分方案为 "aaabb" ，"aaabb" 和 "aabbb" 。
```

__提示：__

- `1 <= text.length <= 10 ^ 5`
- `pattern.length == 2`
- `text` 和 `pattern` 都只包含小写英文字母。

__思路:__

```text
贪心
如果放在开头放 pattern[0]，放在结尾放 pattern[1]
如果 pattern[0] == pattern[1]，则直接计算 pattern[0] 在 text 中出现的次数 c, 返回 c * (c + 1) / 2
否则，遍历 text，统计 pattern[0] 和 pattern[1] 的出现次数 c1 和 c2
每次遇到 pattern[0]，则将 c1 加 1
每次遇到 pattern[1]，则将 c1 加到结果中，同时将 c2 加 1
最终结果为 c1 和 c2 中的最大值加到结果中
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maximumSubsequenceCount(string text, string pattern) 
    {
        long long result = 0LL, c1 = 0LL, c2 = 0LL, c = 0LL;
        if (pattern.front() == pattern.back()) 
        {
            for (const auto& t : text) c += t == pattern.front();
            return c * (c + 1) >> 1;
        }
        for (const auto& t : text) 
        {
            if (t == pattern.front()) ++c1;
            if (t == pattern.back()) 
            {
                result += c1;
                ++c2;
            }
        }
        return result + max(c1, c2); 
    }
};
```

__Java__:

```Java
class Solution 
{
public:
    long long maximumSubsequenceCount(string text, string pattern) 
    {
        long long result = 0LL, c1 = 0LL, c2 = 0LL, c = 0LL;
        if (pattern.front() == pattern.back()) 
        {
            for (const auto& t : text) c += t == pattern.front();
            return c * (c + 1) >> 1;
        }
        for (const auto& t : text) 
        {
            if (t == pattern.front()) ++c1;
            if (t == pattern.back()) 
            {
                result += c1;
                ++c2;
            }
        }
        return result + max(c1, c2); 
    }
};
```

__Python__:

```Python
class Solution 
{
public:
    long long maximumSubsequenceCount(string text, string pattern) 
    {
        long long result = 0LL, c1 = 0LL, c2 = 0LL, c = 0LL;
        if (pattern.front() == pattern.back()) 
        {
            for (const auto& t : text) c += t == pattern.front();
            return c * (c + 1) >> 1;
        }
        for (const auto& t : text) 
        {
            if (t == pattern.front()) ++c1;
            if (t == pattern.back()) 
            {
                result += c1;
                ++c2;
            }
        }
        return result + max(c1, c2); 
    }
};
```
