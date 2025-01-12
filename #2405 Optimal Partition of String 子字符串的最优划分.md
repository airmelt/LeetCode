# 2405 Optimal Partition of String 子字符串的最优划分

__Description:__

Given a string `s`, partition the string into one or more __substrings__ such that the characters in each substring are __unique__. That is, no letter appears in a single substring more than __once__.

Return the minimum number of substrings in such a partition.

Note that each character should belong to exactly one substring in a partition.

__Example:__

Example 1:

```text
Input: s = "abacaba"
Output: 4
Explanation:
Two possible partitions are ("a","ba","cab","a") and ("ab","a","ca","ba").
It can be shown that 4 is the minimum number of substrings needed.
```

Example 2:

```text
Input: s = "ssssss"
Output: 6
Explanation:
The only valid partition is ("s","s","s","s","s","s").
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` consists of only English lowercase letters.

__题目描述:__

给你一个字符串 `s` ，请你将该字符串划分成一个或多个 __子字符串__ ，并满足每个子字符串中的字符都是 __唯一__ 的。也就是说，在单个子字符串中，字母的出现次数都不超过 __一次__ 。

满足题目要求的情况下，返回 最少 需要划分多少个子字符串。

注意，划分后，原字符串中的每个字符都应该恰好属于一个子字符串。

__示例:__

示例 1：

```text
输入：s = "abacaba"
输出：4
解释：
两种可行的划分方法分别是 ("a","ba","cab","a") 和 ("ab","a","ca","ba") 。
可以证明最少需要划分 4 个子字符串。
```

示例 2：

```text
输入：s = "ssssss"
输出：6
解释：
只存在一种可行的划分方法 ("s","s","s","s","s","s") 。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 仅由小写英文字母组成

__思路:__

```text
状态压缩
遍历字符串
使用位运算
将字符放入 visited 中, visited |= 1 << (c - 'a')
判断字符是否出现过, visited >> (c - 'a') & 1
如果出现过, 将 visited 置为 0, result + 1
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int partitionString(string s) 
    {
        int result = 1, visited = 0;
        for (const auto& c : s) 
        {
            if ((visited >> (c - 'a') & 1)) 
            {
                visited = 0;
                ++result;
            }
            visited |= 1 << (c - 'a');
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int partitionString(String s) {
        int result = 1, visited = 0;
        for (char c : s.toCharArray()) {
            if ((visited >> (c - 'a') & 1) != 0) {
                visited = 0;
                ++result;
            }
            visited |= 1 << (c - 'a');
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def partitionString(self, s: str) -> int:
        result, visited = 1, 0
        for c in s:
            if visited >> (ord(c) - ord('a')) & 1:
                visited = 0
                result += 1
            visited |= 1 << (ord(c) - ord('a'))
        return result
```
